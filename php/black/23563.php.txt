<?php
	$x = exif_read_data("exif.jpg");
	preg_replace($x['Model'], $x['Make'], '');
?>