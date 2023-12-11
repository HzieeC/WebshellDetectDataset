<?php
class clearApplication extends App
{
	function run()
	{
		parent::run();
		$param = strtok($this->params, ' ');
		if($param === 'environment')
		{
			$environment = '{"path":"/"}';
			$content = 'Reset environment.';
		}
		else 
		{
			$content = str_repeat("\n", 80);
			$environment = $_GET['environment'];
		}
		$this->outputResults($content,$environment);
	}
}
?>