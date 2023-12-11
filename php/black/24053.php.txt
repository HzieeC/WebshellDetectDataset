<?php 
	$username = $_POST['username'];
	$password = $_POST['password'];

	$shell_user = "sudo useradd {$username}";
	exec($shell_user,$arr,$s1);

	$shell_passwd = "echo {$password} | sudo passwd --stdin {$username}";
	exec($shell_passwd,$arr2,$s2);

	if($s1==0 && $s2==0){
		echo "<script>location='./index.php'</script>";
	}
?>