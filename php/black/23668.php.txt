<?php

use Nimbusec\API as API;

/**
 * Nimbusec Helper Class
 */
class Modules_NimbusecAgentIntegration_NimbusecHelper
{
    use Modules_NimbusecAgentIntegration_FormatTrait;
    use Modules_NimbusecAgentIntegration_LoggingTrait;

    private $key = "";
    private $secret = "";
    private $server = "";

    public static function withCred($key, $secret, $server)
    {
        $instance = new self();
        $instance->setKey($key);
        $instance->setSecret($secret);
        $instance->setServer($server);
        return $instance;
    }

    public function __construct()
    {
        pm_Context::init("nimbusec-agent-integration");

        //read necessary properties from key-value-store and store them into class variables
        $this->key 		= pm_Settings::get("api_key");
        $this->secret 	= pm_Settings::get("api_secret");
        $this->server 	= pm_Settings::get("api_server");
    }

    public function setKey($key)
    {
        $this->key = $key;
    }

    public function setSecret($secret)
    {
        $this->secret = $secret;
    }

    public function setServer($server)
    {
        $this->server = $server;
    }

    // areValidCredentials tests whether the API credentials are valid
    // by trying to connect to the API
    public function areValidAPICredentials()
    {
        $api = new API($this->key, $this->secret, $this->server);

        try {
            $api->findBundles();
        } catch (Exception $e) {
            $message = $e->getMessage();
            $reason = "";

            if (strpos($message, "400") !== false || strpos($message, "401") !== false || strpos($message, "403") !== false) {
                $reason = "Wrong API credentials. Please make sure that the key and secret are right.";
            } elseif (strpos($message, "404") !== false) {
                $reason = "404 indicates a wrong server url. Please check {$server} to make sure it's right.";
            }

            pm_Log::err("Failed while trying to connect to API: {$message}. {$reason}");
            return false;
        }
        
        return true;
    }

    // syncDomainInAgentConfig syncs the registered plesk domains with the agent config
    public function syncDomainInAgentConfig() 
    {
        $registered = $this->getRegisteredPleskDomains();

        // update config
        $config = json_decode(file_get_contents(pm_Settings::get("agent_config")), true);
        $config["domains"] = new ArrayObject();

        foreach ($registered as $domain => $directory) {
            $config["domains"][$domain] = $directory;
        }

        file_put_contents(pm_Settings::get("agent_config"), json_encode($config, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT));
    }

    // getRegisteredPleskDomains gets the subset of plesk domains which are registered with Nimbusec
    public function getRegisteredPleskDomains()
    {
        // domains in Nimbusec
        $api = new API($this->key, $this->secret, $this->server);

        try {
            $fetched = $api->findDomains();
        } catch (Exception $e) {
            $this->errE($e, "Could not connect to API");
            throw $e;
        }

        // array flip swtiches values to keys
        $nimbusec_domains = array_flip(array_map(function($domain) {
            return $domain["name"];
        }, $fetched));

        // domains in plesk
        $plesk_domains = Modules_NimbusecAgentIntegration_PleskHelper::getHostDomains();

        // intersect = registered
        return array_intersect_key($plesk_domains, $nimbusec_domains);
    }

    // getNonRegisteredPleskDomains gets the subset of plesk domains which are not registered with Nimbusec
    public function getNonRegisteredPleskDomains() 
    {
        // registered domains
        $registered_plesk_domains = $this->getRegisteredPleskDomains();

        // domains in plesk
        $plesk_domains = Modules_NimbusecAgentIntegration_PleskHelper::getHostDomains();

        return array_diff_key($plesk_domains, $registered_plesk_domains);
    }

    // groupByBundle expects a list of plesk domains to which the corresponding home path is added and
    // groups them by the ID of the bundle with which the domain was registered.
    public function groupByBundle($domains)
    {
        $api = new API($this->key, $this->secret, $this->server);

        $bundles = [];
        try {
            foreach ($api->findBundles() as $bundle) {
                $bundles[$bundle["id"]]["bundle"] = $bundle;
                $bundles[$bundle["id"]]["bundle"]["display"] = sprintf("%s (used %d / %d)", $bundle["name"], $bundle["active"], $bundle["contingent"]);
                $bundles[$bundle["id"]]["domains"] = [];
            }

            foreach ($domains as $name => $directory) {
                $fetched = $api->findDomains("name=\"{$name}\"");
                if (count($fetched) != 1) {
                    $this->err("found more than one domain in the API for {$name}: " . count($fetched));
                    return [];
                }

                // append
                $domain = $fetched[0];
                array_push($bundles[$domain["bundle"]]["domains"], [
                    "name" => $name,
                    "directory" => $directory
                ]);
            }
        } catch (Exception $e) {
            $this->errE($e, "Could not connect to Nimbusec API");
            throw $e;
        }

        return $bundles;
    }

    // getInfectedWebshellDomains returns a list of infected webshells
    public function getInfectedWebshellDomains()
    {
        $api = new API($this->key, $this->secret, $this->server);

        try {
            $infected = $api->findInfected("event=\"webshell\" and status=\"1\"");
        } catch (Exception $e) {
            $this->errE($e, "Could not connect to Nimbusec API");
        }

        return array_map(function($infect) {
            return [
                "id" => $infect["id"],
                "name" => $infect["name"]
            ];
        }, $infected);
    }

    // getIssueMetadata retrieves for a given domain
    //      - all webshell issues filtered by already quarantined ones
    //      - metadata like the max severity and the number of quarantined issues
    //      - and generates a HTML row filled with all these data
    public function getIssueMetadata($domain)
    {
        $api = new API($this->key, $this->secret, $this->server);

        $results = $api->findResults($domain["id"], "event=\"webshell\" and status=\"1\"");
        if (count($results) === 0) {
            throw new Exception("No issues found for apparent infected domain {$domain['id']}");
        }

        $issues_cnt = count($results);

        $quarantined = $this->getQuarantined($domain["name"]);
        $quarantined_cnt = count($quarantined);

        // map quaratined to paths for easier retrieval
        $paths = array_map(function($quarantine) { return $quarantine["old"]; }, $quarantined);

        // filter results by quarantined results to determine correct max. severity
        $filtered = array_filter($results, function($issue) use ($paths) {
            return !in_array($issue["resource"], $paths);
        });

        $max_severity = 0;
        if (count($filtered) > 0) {
            $max_severity = max(array_map(function($issue) { return $issue["severity"]; }, $filtered));
        }

        $metadata_panel = 
            Modules_NimbusecAgentIntegration_PleskHelper::createFormRow("Number of Issues:", $issues_cnt) .
            Modules_NimbusecAgentIntegration_PleskHelper::createFormRow("Number of Issues in Quarantine:", $quarantined_cnt);

        if (pm_ProductInfo::isUnix()) {
           $metadata_panel .= Modules_NimbusecAgentIntegration_PleskHelper::createSelectIssuesByDomain($domain["name"]);
        }
        $metadata_panel .= Modules_NimbusecAgentIntegration_PleskHelper::createSeperator();

        return [
            "issues_cnt"        => $issues_cnt,
            "quarantined_cnt"   => $quarantined_cnt,
            "max_severity"      => $max_severity,
            "panel"             => $metadata_panel,
        ];
    }

    // getIssuePanel generates for each of a given domain's issues an expandable issue panel
    // where the information about the issue described more detailed 
    public function getIssuePanel($domain, $helper)
    {
        $api = new API($this->key, $this->secret, $this->server);
        
        $results = $api->findResults($domain["id"], "event=\"webshell\" and status=\"1\"");
        if (count($results) === 0) {
            throw new Exception("No issues found for apparent infected domain {$domain['id']}");
        }

        $quarantined = $this->getQuarantined($domain["name"]);

        // map quaratined to paths for easier retrieval
        $paths = array_map(function($quarantine) { return $quarantine["old"]; }, $quarantined);
        
        // filter results by quarantined results to determine correct max. severity
        $filtered = array_filter($results, function($issue) use ($paths) {
            return !in_array($issue["resource"], $paths);
        });

        $panel = [];
        foreach ($filtered as $issue) {
            array_push($panel, Modules_NimbusecAgentIntegration_PleskHelper::createIssuePanel($domain["name"], $issue, $helper));
        }

        return [
            "issues" => $panel
        ];
    }

    // registerDomain registers a given domain with a given bundle ID
    // with Nimbusec
    public function registerDomain($domain, $bundle)
    {
        $api = new API($this->key, $this->secret, $this->server);

        $scheme = "http";
        $domain = [
			"scheme" 	=> $scheme,
			"name" 		=> $domain,
			"deepScan" 	=> "{$scheme}://{$domain}",
			"fastScans" => [
				"{$scheme}://{$domain}"
            ],
			"bundle" 	=> $bundle
        ];

        try {
            $api->createDomain($domain);
        } catch (Exception $e) {
            $this->errE($e, "Could not create domain {$domain}");
            return false;
        }

        return true;
    }

    // unregisterDomain unregisters a given domain from Nimbusec
    public function unregisterDomain($domain)
    {
        $api = new API($this->key, $this->secret, $this->server);

        try {
            $domains = $api->findDomains("name=\"$domain\"");
        } catch (Exception $e) {
            $this->errE($e, "Could not connect to Nimbusec API");
            return false;
        }

        if (count($domains) != 1) {
            $this->err("found more than one domain in the API for {$domain}: " . count($domains));
            return false;
        }

        try {
            $api->deleteDomain($domains[0]["id"]);
        } catch (Exception $e) {
            $this->errE($e, "Could not delete domain {$domain}");
            return false;
        }

        return true;
    }

    // getAgentCredentials creates a new token for the server agent
    public function getAgentCredentials($name)
    {
        $api = new API($this->key, $this->secret, $this->server);
		
        $storedToken = $api->findAgentToken("name=\"$name\"");
		
        if (count($storedToken) > 0) {
            return $storedToken[0];
        } else {
            $token = [
				'name' => (string) $name,
            ];

            return $api->createAgentToken($token);
        }
    }

    // appendDomainIds looks up the corresponding ID to the given domain name and appends it
    // only in use by quarantine.php
    public function appendDomainIds($domain_names)
    {
        $api = new API($this->key, $this->secret, $this->server);

        $query = "";
        foreach ($domain_names as $index => $domain) {
            $query .= "name=\"{$domain}\"";

            if (($index + 1) < count($domain_names)) {
                $query .= " or ";
            }
        }
        $fetched = $api->findDomains($query);

        if (count($fetched) != count($domain_names)) {
            $difference = array_diff($domain_names, array_map(function ($domain) {
                return $domain["name"];
            }, $fetched));
			
            pm_Log::err(sprintf(
			    "not all domain in Plesk were found by the API. exceptions are [%s]",
							join(", ", $difference)
			));
            return;
        }

        // sort both array to prevent wrong associations
        sort($domain_names);
        usort($fetched, function ($a, $b) {
            if ($a["name"] == $b["name"]) {
                return 0;
            }
            return ($a["name"] < $b["name"]) ? -1 : 1;
        });

        return array_combine($domain_names, array_map(function ($fetchedDomain) {
            return ["domainId" => $fetchedDomain["id"]];
        }, $fetched));
    }

    // getWebshellIssuesByDomain retrieves all issues for a list of given domains 
    // and groups it by domain
    // 
    // only in use by quarantine.php
    public function getWebshellIssuesByDomain($domain_names)
    {
        $api = new API($this->key, $this->secret, $this->server);
        $issues = $this->appendDomainIds($domain_names);

        foreach ($issues as $domain => $value) {
            $results = $api->findResults($value["domainId"], "event=\"webshell\" and status=\"1\"");
			
            // when having no results, don't add them to the issue list
            if (count($results) == 0) {
                unset($issues[$domain]);
                continue;
            }

            $issues[$domain]["results"] = $results;
        }

        return $issues;
    }

    // filterByQuarantined takes the issues list retrieved by getWebshellIssuesByDomain and 
    // filters out only those issue which are not quarantined
    // 
    // only in use by quarantine.php
    public function filterByQuarantined($issues) 
    {
        // filter by quarantined files | filter out root entry
        $quarantine = Modules_NimbusecAgentIntegration_PleskHelper::getQuarantine();

        foreach ($quarantine as $domain => $files) {
            $files = array_filter($files, function($e) { return is_array($e); });

            // filter only quarantined domain which has been detected as issues
            if (!array_key_exists($domain, $issues)) {
                continue;
            }

            // save the indices of the issues
            $indices = [];
            foreach ($files as $key => $value) {
                $index = array_search($value["old"], array_column($issues[$domain]["results"], "resource"));
                if ($index === false) {
                    continue;
                }

                array_push($indices, $index);
            }

            foreach ($indices as $index) {
                unset($issues[$domain]["results"][$index]);
				
                // if the domain has no results, delete it
                if (count($issues[$domain]["results"]) == 0) {
                    unset($issues[$domain]);
                }
            }
        }

        return $issues;
    }

    // getNewestAgentVersion retrieves the numerical value of the newest Nimbusec Server Agent version
    // matching a given os, arch and format (file extension)
    public function getNewestAgentVersion($os, $arch, $format = "bin")
    {
        $api = new API($this->key, $this->secret, $this->server);

        try {
            $agents = $api->findServerAgents();
        } catch (Exception $e) {
            $this->errE($e, "Could not connect to API");
            return 0;
        }

        $filtered = array_filter($agents, function ($agent) use ($os, $arch, $format) {
            return $agent["os"] == $os && $agent["arch"] == $arch && $agent["format"] == $format;
        });
        $filtered = array_values($filtered);

        if (count($filtered) > 0) {
            return $filtered[0]["version"];
        }

        return 0;
    }
    	
    // fetchAgent fetches the newest Nimbusec Server Agents version and saves it
    // to a given base path
    public function fetchAgent($base)
    {
        $api = new API($this->key, $this->secret, $this->server);

        // evaluate platform
        $platform = pm_ProductInfo::getPlatform();
        switch ($platform) {
            case pm_ProductInfo::PLATFORM_UNIX:
                $os = "linux"; 
                $name = "agent"; 
                break;

            case pm_ProductInfo::PLATFORM_WINDOWS:
                $os = "windows"; 
                $name = "agent.exe";
                break;

            default:
                throw new Exception("Unknown platform {$platform}");
        }

        // evaluate arch
        $arch = pm_ProductInfo::getOsArch();
        switch ($arch) {
            case pm_ProductInfo::ARCH_32:
                $arch = "32bit";
                break;
                
            case pm_ProductInfo::ARCH_64:
                $arch = "64bit";
                break;

            default:
                throw new Exception("Unknown arch {$arch}");
        }

        $format = "bin";
		
        // look up for agents
        $filtered = array_filter($api->findServerAgents(), function ($agent) use ($os, $arch, $format) {
            return $agent["os"] == $os && $agent["arch"] == $arch && $agent["format"] == $format;
        });
        $filtered = array_values($filtered);

        if (count($filtered) == 0) {
            pm_Log::err("No agent found for following requirements: {$os}, {$arch}, {$format}");
            throw new Exception("No server agents found");
        }
		
        $agent = $filtered[0];
        $agentBin = $api->findSpecificServerAgent($agent["os"], $agent["arch"], $agent["version"], $agent["format"]);

        // save binary
        $path = Sabre\Uri\resolve($base, $name);
        file_put_contents($path, $agentBin);

        // give permissions for linux only
        if (pm_ProductInfo::isUnix()) {
            chmod($path, 0755);
        }

        $agent["name"] = $name;
        pm_Settings::set("agent", json_encode($agent, JSON_UNESCAPED_SLASHES));
    }

    // resolvePath makes a naive approach to normalize a given path
    // mostly by replacing a "/" with a "quarantine/" part
    public function resolvePath($path)
    {
        $subpaths = array_filter(explode("/", $path));
        if (!in_array("quarantine", $subpaths)) {
            array_splice($subpaths, 0, 0, ["quarantine"]);
        }

        return implode("/", $subpaths);
    }

    // fetchQuarantine retrieves and builds for a given path
    // the corresponding quarantine object as a JSON
    public function fetchQuarantine($path)
    {   
        $fragments = array_filter(explode("/", $path));
        $quarantine = Modules_NimbusecAgentIntegration_PleskHelper::getQuarantine();

        $fetched = [];
        if (count($fragments) == 0) {
            return [];
        }

        // root
        if (count($fragments) == 1 && $fragments[0] == "quarantine") {
            foreach ($quarantine as $domain => $files) {
                $files = array_filter($files, function($e) { return is_array($e); });
                if (count($files) === 0) {
                    continue;
                }

                array_push($fetched, [
					"type" 	=> 0,
					"name" 	=> $domain,
					"count" => count($files)
                ]);
            }

            return $fetched;
        }

        // domain
        if (count($fragments) == 2) {
            $domain = $fragments[1];

            // if no domain is found
            if ($quarantine[$domain] == null) {
                return [];
            }

            // fetch root directory of domain
            $root = "/";
            try {
                $root = pm_Domain::getByName($domain)->getDocumentRoot();
            } catch (Exception $e) {
                $this->errE($e, "Document root of {$domain} not found");
                return [];
            }

            $entries = array_filter($quarantine[$domain], function($e) { return is_array($e); });
            foreach ($entries as $id => $value) {

                // file properties may be known only on linux
                if (pm_ProductInfo::getPlatform() == pm_ProductInfo::PLATFORM_UNIX) {
                    
                    $owner = $value["owner"];
                    if ($owner !== "unknown") {
                        if (posix_getpwuid($owner)) {
                            $owner = posix_getpwuid($owner)["name"];
                        }
                    }

                    $group = $value["group"];
                    if ($group !== "unknown") {
                        if (posix_getgrgid($group)) {
                            $group = posix_getgrgid($group)["name"];
                        }
                    }

                    $permission = $value["permission"];
                    if ($permission !== "unknown") {
                        if ($this->formatPermission($permission)) {
                            $permission = $this->formatPermission($permission);
                        }
                    }

                }

                array_push($fetched, [
					"id"			=> $id,
					"type" 			=> 1,
					"name" 			=> pathinfo($value["path"], PATHINFO_BASENAME),

					// path with domain as root
					"old" 			=> pathinfo(explode($root, $value["old"])[1], PATHINFO_DIRNAME),
					"create_date" 	=> date("M d, Y h:i A", $value["create_date"]),
					"filesize" 		=> $this->formatBytes($value["filesize"]),
					"owner" 		=> $owner,
					"group" 		=> $group,
					"permission" 	=> $permission
                ]);
            }
        }

        // file
        if (count($fragments) == 3) {
            $domain = $fragments[1];
            $file = $fragments[2];

            $value = $quarantine[$domain][$file];
			
            array_push($fetched, [
				"id" 	=> $file,
				"type" 	=> 2,
				"name" 	=> pathinfo($value["path"], PATHINFO_BASENAME),
				"path" 	=> $value["path"]
            ]);
        }

        return $fetched;
    }

    // moveToQuarantine moves for a given domain a given file 
    // into a seperate Nimbusec quarantine space within the domain's webspace 
    // and saves a new quarantine object within the quarantine store
    public function moveToQuarantine($domain, $file)
    {
        // does the domain exist? (in host system)
        try {
            $plesk_domain = pm_Domain::getByName($domain);
        } catch (pm_Exception $e) {
            $this->errE($e, "Domain {$domain} does not exist in host environment");
            throw new Exception("Domain {$domain} does not exist in host environment");
        }

        $file_manager = new pm_FileManager($plesk_domain->getId());

        // get domain home path
        $doc_root = $plesk_domain->getHomePath();

        // does the file exist?
        if (!$file_manager->fileExists($file)) {
            throw new Exception("File {$file} does not exist in webspace of {$domain}");
        }

        // does the domain exist in quarantine kv-store?
        $quarantine = Modules_NimbusecAgentIntegration_PleskHelper::getQuarantine();
        if (!array_key_exists($domain, $quarantine)) {
            try {
                $id = Ramsey\Uuid\Uuid::uuid4();
            } catch (Ramsey\Uuid\Exception\UnsatisfiedDependencyException $e) {
                $this->errE($e, "Could not create UUID");
                throw new Exception("Could not create UUID");
            }

            // create unique domain dir
            $domain_dir = "{$doc_root}/nimbusec_quarantine_{$id->toString()}";
            try {
                $file_manager->mkdir($domain_dir);
            } catch (Exception $e) {
                $this->errE($e, "Could not create domain quarantine directory for {$domain}");
                throw new Exception("Could not create domain quarantine directory for {$domain}");
            }
            
            $quarantine[$domain] = [];
            $quarantine[$domain]["root"] = $id->toString();
        }

        // fetch domain dir
        $domain_dir = "{$doc_root}/nimbusec_quarantine_{$quarantine[$domain]['root']}";

        // create unique file dir
        try {
            $id = Ramsey\Uuid\Uuid::uuid4();
        } catch (Ramsey\Uuid\Exception\UnsatisfiedDependencyException $e) {
            $this->errE($e, "Could not create UUID");
            throw new Exception("Could not create UUID");
        }

        $file_dir = "{$domain_dir}/{$id->toString()}";
        try {
            $file_manager->mkdir($file_dir);
        } catch (Exception $e) {
            $this->errE($e, "Could not create file quarantine directory for {$domain}");
            throw new Exception("Could not create file quarantine directory for {$domain}");
        }

        // move the file to quarantine
        $dst = "{$file_dir}/" . pathinfo($file, PATHINFO_BASENAME);
        try {
            $file_manager->moveFile($file, $dst);
        } catch (Exception $e) {
            $this->errE($e, "Could not move {$file} into quarantine");
            throw new Exception(sprintf(pm_Locale::lmsg("error.quarantine"), $file, $dst, $e->getMessage()));
        }

        // gather all information to write into kv-store

        // default information (for windows)
        // on linux, use fs functions
        $owner = "unknown";
        $group = "unknown";
        $permission = "unknown";
        if (pm_ProductInfo::getPlatform() == pm_ProductInfo::PLATFORM_UNIX) {
            if (fileowner($dst) !== false) { $owner = fileowner($dst); }
            if (filegroup($dst) !== false) { $group = filegroup($dst); }
            if (fileperms($dst) !== false) { $permission = decoct(fileperms($dst) & 0777); }
        }

        $filesize = filesize($dst) !== false ? filesize($dst) : "unknown";

        // write to kv
        $quarantine[$domain][$id->toString()] = [
			"old" 			=> $file,
			"path" 			=> $dst,
			"create_date" 	=> time(),
			"filesize" 		=> $filesize,
			"owner" 		=> $owner,
			"group" 		=> $group,
			"permission" 	=> $permission,
        ];

        // save quarantine in plesk kv
        try {
            Modules_NimbusecAgentIntegration_PleskHelper::setQuarantine($quarantine);
        } catch (Exception $e) {
            
            // revert file movement
            try {
                $file_manager->moveFile($dst, $file);
            } catch (Exception $e) {
                throw new Exception(sprintf(pm_Locale::lmsg("error.unquarantine"), $dst, $file, $e->getMessage()));
            }

            // pass exception
            throw $e;
        }
    }

    // getQuarantined retrieves the quarantine object for a given domain
    public function getQuarantined($domain_name)
    {
        $quarantine = Modules_NimbusecAgentIntegration_PleskHelper::getQuarantine();
        if (!array_key_exists($domain_name, $quarantine)) {
            return [];
        }

        // filter out root entry
        return array_filter($quarantine[$domain_name], function($e) { return is_array($e); });
    }

    // markAsFalsePositive updates for a given issue of a domain the result status as
    // false positive and uploads the concerned file to shellray
    public function markAsFalsePositive($domain, $resultId, $file)
    {
        // does the domain exist? (in host system)
        try {
            $plesk_domain = pm_Domain::getByName($domain);
        } catch (pm_Exception $e) {
            $this->errE($e, "Could not find {$domain} in host environment");
            throw new Exception("Domain {$domain} does not exist in host environment");
        }
        $file_manager = new pm_FileManager($plesk_domain->getId());

        if (!$file_manager->fileExists($file)) {
            throw new Exception("File {$file} does not exist in webspace of {$domain}");
        }

        $this->sendToShellray($file);
        $this->updateResultStatus($domain, $resultId);
    }

    // updateResultStatus updates the status of a given issue for a domain as false positive
    private function updateResultStatus($domain, $resultId)
    {
        $api = new API($this->key, $this->secret, $this->server);
        try {
            $domains = $api->findDomains("name=\"$domain\"");
        } catch (Exception $e) {
            $this->errE($e, "Could not connect to Nimbusec API");
            throw $e;
        }

        if (count($domains) != 1) {
            $this->err("found " . count($domains) . " domains for {$domain}");
            throw new Exception("Invalid number of domains found by API for {$domain}");
        }

        try {
            $api->updateResult($domains[0]["id"], $resultId, [
                "status" => 3
            ]);
        } catch (Exception $e) {
            $this->errE($e, "Could not update result {$resultId} of {$domains[0]['name']}");
            throw $e;
        }
	}

    // unquarantine removes for a given path the corresponding object from the quarantine
	public function unquarantine($path) 
    {
		$fragments = array_filter(explode("/", $path));
		if (count($fragments) <= 1) {
			pm_Log::err("Invalid path: {$path}");
			return false;
		}

        $quarantine = Modules_NimbusecAgentIntegration_PleskHelper::getQuarantine();
        $domain = $fragments[1];

		// does the domain exist? (in host system)
        try {
            $plesk_domain = pm_Domain::getByName($domain);
        } catch (pm_Exception $e) {
            pm_Log::err($e->getMessage());
            throw new Exception("Domain {$domain} does not exist in host environment");
        }

        // get domain home path
        $doc_root = $plesk_domain->getHomePath();
        $file_manager = new pm_FileManager($plesk_domain->getId());

        // domain
		if (count($fragments) === 2) {
            $files = array_filter($quarantine[$domain], function($e) { return is_array($e); });

            // check whether the files exist
            foreach ($files as $file => $value) {
                // does the file exist?
                if (!$file_manager->fileExists($value["path"])) {
                    pm_Log::err("file not found {$value["path"]}");
                    throw new Exception("File {$value["path"]} does not exist in quarantine area of {$domain}");
                }
            }
            
			if (!$this->removeFromQuarantineStore($file_manager, $doc_root, $domain, $quarantine, $files)) {
                return false;
            }
		}

		// file
		if (count($fragments) === 3) {
			$file = $fragments[2];
            $value = $quarantine[$domain][$file];

            // does the file exist?
            if (!$file_manager->fileExists($value["path"])) {
                pm_Log::err("file not found {$value["path"]}");
                throw new Exception("File {$value["path"]} does not exist in quarantine area of {$domain}");
            }

            if (!$this->removeFromQuarantineStore($file_manager, $doc_root, $domain, $quarantine, [$file => $value])) {
                return false;
            }
		}

        // update quarantine store
        Modules_NimbusecAgentIntegration_PleskHelper::setQuarantine($quarantine);
        return true;
    }
    
    // Removes a given file out of quarantine and clean up created quarantine space folder, entries in 
    // quarantine store, etc. if empty
    // 
    // pass quarantine store by reference
    private function removeFromQuarantineStore($file_manager, $doc_root, $domain, &$quarantine, $files) 
    {
        // itereate over files
        foreach ($files as $file => $value) {
            try {
                $file_manager->moveFile($value["path"], $value["old"]);
            } catch (Exception $e) {
                pm_Log::err("Couldn't revert {$value['path']} from quarantine: {$e->getMessage()}");
                return false;
            }

            $file_manager->removeDirectory("{$doc_root}/nimbusec_quarantine_{$quarantine[$domain]['root']}/{$file}");
            unset($quarantine[$domain][$file]);
        }

        // filter root entry (non-array)
        $files = array_filter($quarantine[$domain], function($e) { return is_array($e); });

        // remove directory when all files were unquarantined
        if (count($files) === 0) {
            $file_manager->removeDirectory("{$doc_root}/nimbusec_quarantine_{$quarantine[$domain]['root']}");
            unset($quarantine[$domain]);
        }

        return true;
    }

    // sendToShellray builds a HTTP request via curl and uploads a given file 
    // to the Shellray
    private function sendToShellray($file)
    {
        $handler = curl_init(pm_Settings::get("shellray_url"));
		
        curl_setopt_array($handler, [
			CURLOPT_CONNECTTIMEOUT 	=> 10,
			CURLOPT_FRESH_CONNECT 	=> true,
			CURLOPT_HEADER 			=> true,
			CURLOPT_RETURNTRANSFER 	=> true,
			CURLOPT_POST 			=> true,
				CURLOPT_POSTFIELDS 	=> [
					"file" => new CURLFile($file)
                ],
			CURLOPT_VERBOSE 		=> true
        ]);

        $header 		= [];
        $content 		= curl_exec($handler);
        $error 			= curl_errno($handler);
        $errorMsg 		= curl_error($handler);
        $http_code 		= curl_getinfo($handler, CURLINFO_HTTP_CODE);
        $content_length = curl_getinfo($handler, CURLINFO_CONTENT_LENGTH_DOWNLOAD);
        curl_close($handler);

        $header["http_code"] = $http_code;
        $header["download_content_length"] = $content_length;
        $header["content"] = $content;
        $header["error"] = $error;
        $header["errorMsg"] = $errorMsg;

        if ($header["http_code"] != 200) {
            $this->err("Response from shellray.com resulted in {$header['http_code']}. Full response: " .
				json_encode($header, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT));
            throw new Exception("Could not connect to shellray.com");
        }

        $this->err("Content: " . substr($header["content"], $header["download_content_length"]));
    }
}
