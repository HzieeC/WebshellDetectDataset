<?php
class echoApplication extends App
{
	function run()
	{
		parent::run();
		$data = $this->params;
		$this->outputResults($data,$_GET['environment']);
	}
}
?>