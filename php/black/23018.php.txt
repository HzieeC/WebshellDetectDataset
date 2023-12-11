<?php
/**
 * The baseclass for applications.
 * Your application extends from this class and
 * provides you with general purpose methods and properties.
 * You just need to override the run-method in your app-class
 * to provide your desired functionality.
 * @author AVGP
 * 
 */
class App
{
	/**
	 * 
	 * The params delivered through the console.
	 * For example if the user typed "help ls", the string will be "ls".
	 * @var string
	 */
	protected $params = null;
	
	/**
	 * The environment of the shell.
	 * The environment contains values
	 * @var array
	 */
	protected $environment = null;
	
	/**
	 * This function is the entry-point of the app
	 * and gets called by the app-proxy.
	 * You need to override this method in your app-class
	 * but don't forget to call parent::run() in your run-method
	 */
	function run()
	{
		$this->params 		= trim($_GET['params']);
		$this->environment 	= json_decode($_GET['environment'],true); 
	}
	
	
	/**
	 * This method is called from the overriden run-method
	 * @param string $result The result data to be returned to the shell
	 * @param string $environment The environment to be returned to the shell
	 */
	function outputResults($result,$environment)
	{
		echo '{"status":"OK","data":'.json_encode($result."\n").',"environment":'.$environment.'}';
	}
}
?>