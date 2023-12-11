<?php
	session_start();

	$exec_binary = "exec_cmd";
	
	if (isset($_POST["Reset"])) {
		unset($_SESSION["cmds"]);
		unset($_SESSION["dir"]);
	} else {
		if (!isset($_SESSION["exec_binary_path"])){
			$_SESSION["exec_binary_path"] = exec("pwd");
		}
		
		// Assuming a home directory exist and it is /home/<username>
		if (!isset($_SESSION["HOME"])){
			$_SESSION["HOME"] = "/home/".$_SERVER['REMOTE_USER'];
		}
		
		if (!isset($_SESSION["dir"])){
			$_SESSION["dir"] = $_SESSION["HOME"];
		}
		
		chdir($_SESSION["dir"]);
		if (isset($_POST["cmd"])) {
			$command = explode(" ", filter_input(INPUT_POST, "cmd"));
			if ($command[0] === "cd") {
				if (count($command) > 1){
					$_SESSION["dir"] = $command[1];
				}
				else {
					$_SESSION["dir"] = $_SESSION["HOME"];
				}
				$dirChanged = true;
				chdir($_SESSION["dir"]);
			}

			$command=filter_input(INPUT_POST, "cmd");
			
			// binary_path <user> <command>
			$resultat = shell_exec($_SESSION["exec_binary_path"]."/".$exec_binary." ".$_SERVER['REMOTE_USER']." \"".$command."\" 2>&1");

			if (isset($_SESSION["cmds"])){
				if(isset($dirChanged) && $dirChanged == true) {
					$_SESSION["cmds"] = $_SERVER['REMOTE_USER']."\$ ".$command."<br />".
							    "Directory changed to " . $_SESSION["dir"] . "<br />".
							    str_replace(" ", "&nbsp;", htmlentities($resultat)) . "<br /><hr/>" . $_SESSION["cmds"];
				} else {
					$_SESSION["cmds"] = $_SERVER['REMOTE_USER']."\$ ".$command."<br />".
                                                            str_replace(" ", "&nbsp;", htmlentities($resultat)) . "<br /><hr/>" . $_SESSION["cmds"];
				}
			}
			else {
				if(isset($dirChanged) && $dirChanged == true) {
					$_SESSION["cmds"] = $_SERVER['REMOTE_USER']."\$ ".substr($command,0,-5)."<br />".
							    "Directory changed to " . $_SESSION["dir"] . "<br />".
							    str_replace(" ", "&nbsp;", htmlentities($resultat));
				} else {
					$_SESSION["cmds"] = $_SERVER['REMOTE_USER']."\$ ".substr($command,0,-5)."<br />".
                                                            str_replace(" ", "&nbsp;", htmlentities($resultat));
				}
			}
		}
	}
?>
<!DOCTYPE html>
<html>
<head>
</head>
<body bgcolor="#000000" style="color:#19DA00">
	<div style="margin-top:20px; font-family: monospace; font-size:14px;">
		<?php
			if(isset($_SESSION['cmds'])) {
				echo nl2br($_SESSION['cmds']);
			}
		?>
	</div>
</body>
</html>
