<?php
class lessApplication extends App
{
	function run()
	{
		parent::run();
		$filepath = $this->params;
		$data = '';
		$path = $this->prepareFilePath($filepath);
		if(is_file(getcwd().'/data/'.ltrim($path,'/')))
		{
			$content = file_get_contents(getcwd().'/data/'.ltrim($path,'/'));
			$data = $content;
		}
		else
		{
			$data = 'This is not a file!';
		}
		$this->outputResults($data,$_GET['environment']);
	}
	
	function prepareFilePath($path)
	{
		$path_tokens = explode('/', trim($path,'/'));
		$filename 	 = array_pop($path_tokens); //Remove the filename!
		$path_new    = array();

		//Relative Paths:
		if(empty($path_tokens[0]))
		{
			$path_new = explode('/', rtrim($this->environment['path'],'/'));
		}

		//Build new path from tokens:
		foreach($path_tokens as $dir)
		{
			switch($dir)
			{
				case null:
				case '':
					break;
				case '..':
					break;
				case '.':
					array_pop($path_new);
					break;
				default:
					$path_new[] = $dir;
					break;
			}
		}
		return rtrim(implode('/', $path_new),'/').'/'.$filename;
	}
	
}
?>