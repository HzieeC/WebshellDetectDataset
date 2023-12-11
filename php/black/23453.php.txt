<?php

/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */

set_time_limit(0);
pm_Context::init("nimbusec-agent-integration");

$varDir = pm_Context::getVarDir();
$agent = json_decode(pm_Settings::get("agent"), true)["name"];

$config_path = pm_Settings::get("agent_config");
$cmd = "{$varDir}/{$agent} -config {$config_path}";

if (pm_Settings::get("agent_yara") === "true") {
    $cmd .= " -yara";
}

$log_path = pm_Settings::get("agent_log");
$cmd .= " > {$log_path} 2>&1";

system($cmd);
