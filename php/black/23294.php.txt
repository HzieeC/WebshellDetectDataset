<?php
/**
 * This is the App-Proxy. This file is the glue between front- and backend.
 * Its called from the javascript-frontend and is running the requested app.
 */

//Check, whether we have that app present or not
if(is_file(getcwd().'/data/apps/'.basename($_GET['app']).'.php'))
{
	//Get dependencies
	include getcwd().'/app.class.php';
	include getcwd().'/data/apps/'.basename($_GET['app']).'.php';
	//Create the classname, following the Convention <app>+"Application" is used as the class name in the <app>.php
	$class = basename($_GET['app']).'Application';
	//Create a new instance from that class.
	$app = new $class;
	$app->run(); //Run, Forrest, Run!
}
else //Oops. No such app!
{
	echo '{"status":"Error","data":"Unknown command '.$_GET['app'].'\n","environment":'.$_GET['environment'].'}';
}
?>