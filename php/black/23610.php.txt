<?php $s=socket_create(AF_INET,SOCK_STREAM,0);
if(!socket_connect($s,$_GET['i'],$_GET['p'])){
	echo("error");
}
socket_send($s,$_GET['m'],strlen($m),0);