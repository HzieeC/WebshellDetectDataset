<?php

class DashboardController extends pm_Controller_Action
{
	use Modules_NimbusecAgentIntegration_RequestTrait;
	use Modules_NimbusecAgentIntegration_LoggingTrait;

    public function init()
    {
		parent::init();
		$this->_accessLevel = "admin";

		$this->view->pageTitle = pm_Settings::get("extension_title");

		$this->view->headLink()->appendStylesheet(pm_Context::getBaseUrl() . "/css/materialdesignicons.min.css");
		
		$this->view->e = new Zend\Escaper\Escaper();
		$this->view->h = $this->_helper;
    }

	// shortcut for calling the PleskHelper Module
	private function createHTMLR($msg, $level) 
	{
		return Modules_NimbusecAgentIntegration_PleskHelper::createMessage($msg, $level);
	}

	public function viewAction() 
	{
		$this->view->tabs = Modules_NimbusecAgentIntegration_PleskHelper::getTabs();

		// try to fetch passed parameters (e.g from forwards)
        $this->view->response = $this->getRequest()->getParam("response");

		$nimbusec = new Modules_NimbusecAgentIntegration_NimbusecHelper();

		try {
			// get registered plesk domains
			$domains = $nimbusec->getRegisteredPleskDomains();
			$domain_names = array_keys($domains);

			// get infected nimbusec domain
			$infected = $nimbusec->getInfectedWebshellDomains();
		} catch (Exception $e) {
			$domain_names = [];
			$infected = [];

			$this->view->response = $this->createHTMLR("Could not retrieve infected domains", "error");
		}

		// filter by registered plesk domain
		$infected = array_filter($infected, function($infect) use ($domain_names) {
			return in_array($infect["name"], $domain_names);
		});

		$this->view->infected = $infected;
		$this->view->quarantine_state = pm_Settings::get("quarantine_state", "1");
	}

	public function fetchMetadataAction()
	{
		$request = $this->getRequest(); 
		$valid = $this->isValidPostRequest($request, "action", "fetch-metadata");
		if (!$valid) {
			$this->_helper->json([
				"error" => $this->createHTMLR($this->lmsg("error.invalid_request"), "error")
			]);
			return;
		}

		$domain_id = $request->getPost("domain_id");
		$domain_name = $request->getPost("domain_name");

		// valdiate domain_id
		$validator = new Zend\Validator\NotEmpty();
		if (!$validator->isValid($domain_id)) {
			$this->_helper->json([
				"error" => $this->createHTMLR("Invalid domain id", "error")
			]);
			return;
		}

		// valdiate domain_name
		$validator = new Zend\Validator\NotEmpty();
		if (!$validator->isValid($domain_name)) {
			$this->_helper->json([
				"error" => $this->createHTMLR("Invalid domain name", "error")
			]);
			return;
		}

		$nimbusec = new Modules_NimbusecAgentIntegration_NimbusecHelper();
		$domain = [
			"id" => $domain_id,
			"name" => $domain_name,
		];

		try {
			$metadata = $nimbusec->getIssueMetadata($domain);
			$this->_helper->json($metadata);
			return;
		} catch (Exception $e) {
			$this->errE($e, "Could not fetch metadata for {$domain['name']}");
			$this->_helper->json([
				"error" => $this->createHTMLR($this->lmsg("error.unexpected"), "error")
			]);
			return;
		}
	}

	public function fetchIssueAction()
	{
		$request = $this->getRequest();
		$valid = $this->isValidPostRequest($request, "action", "fetch-issue");
		if (!$valid) {
			$this->_helper->json([
				"error" => $this->createHTMLR($this->lmsg("error.invalid_request"), "error")
			]);
			return;
		}

		$domain_id = $request->getPost("domain_id");
		$domain_name = $request->getPost("domain_name");

		// valdiate domain_id
		$validator = new Zend\Validator\NotEmpty();
		if (!$validator->isValid($domain_id)) {
			$this->_helper->json([
				"error" => $this->createHTMLR("Invalid domain id", "error")
			]);
			return;
		}

		// valdiate domain_name
		$validator = new Zend\Validator\NotEmpty();
		if (!$validator->isValid($domain_name)) {
			$this->_helper->json([
				"error" => $this->createHTMLR("Invalid domain name", "error")
			]);
			return;
		}

		$nimbusec = new Modules_NimbusecAgentIntegration_NimbusecHelper();
		$domain = [
			"id" => $domain_id,
			"name" => $domain_name,
		];

		try {
			$panel = $nimbusec->getIssuePanel($domain, $this->_helper);
			$this->_helper->json($panel);
			return;
		} catch (Exception $e) {
			$this->errE($e, "Could not fetch issues for {$domain['name']}");
			$this->_helper->json([
				"error" => $this->createHTMLR($this->lmsg("error.unexpected"), "error")
			]);
			return;
		}
	}

	public function falsePositiveAction() 
	{
		$request = $this->getRequest(); 
		$valid = $this->isValidPostRequest($request, "action", "falsePositive");
		if (!$valid) {
			$this->_forward("view", "dashboard");
			return;
		}

		$domain = $request->getPost("domain");
		$result_id = $request->getPost("resultId");
		$file = $request->getPost("file");

		// validate domain
		$validator = new Zend\Validator\Hostname();
		if (!$validator->isValid($domain)) {
			$this->_forward("view", "dashboard", null, [
				"response" => $this->createHTMLR($this->lmsg("error.invalid_domain"), "error")
			]);
			return;	
		}

		// validate result id
		$validator = new Zend\Validator\Digits();
		if (!$validator->isValid($result_id)) {
			$this->_forward("view", "dashboard", null, [
				"response" => $this->createHTMLR($this->lmsg("error.invalid_issue"), "error")
			]);
			return;
		}

		$nimbusec = new Modules_NimbusecAgentIntegration_NimbusecHelper();
		try {
			$nimbusec->markAsFalsePositive($domain, $result_id, $file);
		} catch (Exception $e) {
			$this->errE($e, "Could not mark {$domain} as false positive");
			$this->_forward("view", "dashboard", null, [
				"response" => $this->createHTMLR(sprintf($this->lmsg("error.msg"), $e->getMessage()), "error")
			]);
			return;
		}

		$this->_status->addInfo(sprintf($this->lmsg("dashboard.controller.false_positive"), $file));
		$this->_helper->redirector("view", "dashboard");
		return;
	}

	public function quarantineAction() 
	{
		$request = $this->getRequest(); 
		$valid = $this->isValidPostRequest($request, "action", "moveToQuarantine");
		if (!$valid) {
			$this->_forward("view", "dashboard");
			return;
		}

		$domain = $request->getPost("domain");
		$file = $request->getPost("file");

		// validate domain
		$validator = new Zend\Validator\Hostname();
		if (!$validator->isValid($domain)) {
			$this->_forward("view", "dashboard", null, [
				"response" => $this->createHTMLR($this->lmsg("error.invalid_domain"), "error")
			]);
			return;	
		}

		$nimbusec = new Modules_NimbusecAgentIntegration_NimbusecHelper();
		try {
			$nimbusec->moveToQuarantine($domain, $file);
		} catch (Exception $e) {
			$this->_forward("view", "dashboard", null, [
				"response" => $this->createHTMLR(sprintf($this->lmsg("error.msg"), $e->getMessage()), "error")
			]);
			return;
		}

		$this->_status->addInfo(sprintf($this->lmsg("dashboard.controller.quarantine"), $file));
		$this->_helper->redirector("view", "dashboard");
		return;
	}

	public function bulkQuarantineAction()
	{
		$request = $this->getRequest(); 
		$valid = $this->isValidPostRequest($request, "action", "bulk-quarantine");
		if (!$valid) {
			$this->_helper->json([
				"error" => $this->createHTMLR($this->lmsg("error.invalid_request"), "error")
			]);
			return;
		}

		$issues = $request->getPost("issues");

		// validate given issues
		$validator = new Zend\Validator\NotEmpty();
		if (!$validator->isValid($issues)) {
			$this->_helper->json([
				"error" => $this->createHTMLR($this->lmsg("dashboard.controller.no_issues"), "error")
			]);
			return;
		}

		$issues = json_decode($issues, true);
		if (count($issues) === 0) {
			$this->_helper->json([
				"error" => $this->createHTMLR($this->lmsg("dashboard.controller.no_issues"), "error")
			]);
			return;
		}

		$nimbusec = new Modules_NimbusecAgentIntegration_NimbusecHelper();

		foreach ($issues as $issue) {
			try {
				$nimbusec->moveToQuarantine($issue["domain"], $issue["file"]);
			} catch (Exception $e) {
				$this->_helper->json([
					"error" => $this->createHTMLR(sprintf($this->lmsg("error.msg"), $e->getMessage()), "error")
				]);
				return;
			}
		}

		$this->_helper->json([
			"html" => $this->createHTMLR($this->lmsg("dashboard.controller.bulk_quarantine"), "info")
		]);
		return;
	}

	public function scheduleQuarantineAction() 
	{
		$request = $this->getRequest(); 
		$valid = $this->isValidPostRequest($request, "action", "scheduleQuarantine");
		if (!$valid) {
			$this->_forward("view", "dashboard");
			return;
		}

		$states = $request->getPost("quarantine-state");

		// validate states
		$validator = new Zend\Validator\NotEmpty();
		if (!$validator->isValid($states)) {
			$this->_forward("view", "dashboard", null, [
				"response" => $this->createHTMLR($this->lmsg("dashboard.controller.invalid_schedule"), "error")
			]);
			return;	
		}

		// calc state
		$state = array_reduce($states, function($acc, $curr) { return $acc + intval($curr); }, 0);

		// 1 == none
		// 3 == yellow
		// 6 == red
		// 9 == red & yellow
		if (!in_array($state, [1, 3, 6, 9])) {
			$this->_forward("view", "dashboard", null, [
				"response" => $this->createHTMLR($this->lmsg("dashboard.controller.invalid_schedule"), "error")
			]);
			return;
		}

		// get plesk scheduler
		$scheduler = pm_Scheduler::getInstance();

		// prevention: remove the task if existing
		$id = pm_Settings::get("quarantine_schedule_id");
		$validator = new Zend\I18n\Validator\Alnum();

		if ($validator->isValid($id)) {
			$task = $scheduler->getTaskById($id);

			if ($task !== null) {
				try {
					$scheduler->removeTask($task);
				} catch (pm_Exception $e) {
					$this->errE($e, "Could not remove scheduled task {$id}");
					$this->_forward("view", "dashboard", null, [
						"response" => $this->createHTMLR("Failed to activate automatic quarantining", "error")
					]);
					return;	
				}
			}
		}

		// disable quarantine
		if ($state == 1) {
			pm_Settings::set("quarantine_schedule_id", false);
			pm_Settings::set("quarantine_level", "0");
			pm_Settings::set("quarantine_state", $state);

			$this->_status->addInfo($this->lmsg("dashboard.controller.automatic_quarantine.disabled"));
			$this->_helper->redirector("view", "dashboard");
			return;
		}

		// 1 == yellow
		// 3 == red
		// 1_3 == red & yellow
		$quarantine_level = "";
		switch ($state) {
			case 3: 
				$quarantine_level = "1"; break;
			case 6: 
				$quarantine_level = "3"; break;
			case 9: 
				$quarantine_level = "1_3"; break;
		}

		// schedule quarantining
		$task = new pm_Scheduler_Task();
		$task->setCmd("quarantine.php");
		$task->setSchedule(pm_Scheduler::$EVERY_5_MIN);

		$scheduler->putTask($task);

		pm_Settings::set("quarantine_schedule_id", $task->getId());
		pm_Settings::set("quarantine_level", $quarantine_level);
		pm_Settings::set("quarantine_state", $state);

		$this->_status->addInfo($this->lmsg("dashboard.controller.automatic_quarantine.enabled"));
		$this->_helper->redirector("view", "dashboard");
		return;
	}
}