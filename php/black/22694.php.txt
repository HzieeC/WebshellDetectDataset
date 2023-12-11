<?php
class lsApplication extends App
{
	function run()
	{
		parent::run();
		if(!empty($this->params)) //Path was specified
		{
			$path = $this->getPath();
		}
		else //Current directory
		{
			$path = $this->getCurrentPath();
		}
		
		$result = scandir(getcwd().'/data/'.trim($path,'/'));
		$output = '';
		for($i=0;$i<count($result);$i++)
		{
			$output .= $result[$i]."\t\t\t";
			if($i%3 == 0 && $i > 0 && $i<count($result)-1) $output .= "\n";
		}
		
		$this->outputResults($output,$_GET['environment']);
	}
	
	function getPath()
	{
		$path = strtok($this->params,' ');
		$path_tokens = explode('/', rtrim($path,'/'));
		$path_new    = array();

		//Relative Paths:
		if($path_tokens[0] == '')
		{
			$path_new = explode('/', rtrim($this->environment['path'],'/'));
		}

		//Build new path from tokens:
		foreach($path_tokens as $dir)
		{
			if($dir != '..')
			{
				$path_new[] = $dir;
			}
			else if($dir != '.')
			{
				array_pop($path_new);
			}
		}
		return '/'.implode('/', $path_new);
	}
	
	function getCurrentPath()
	{
		return $this->environment['path'];
	}	
}
?>