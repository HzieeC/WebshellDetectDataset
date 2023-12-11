<%@page language="java" contentType="text/html; charset=utf-8" pageEncoding="UTF-8"%>
<%@include file="common.jsp"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8" />
<title>SHOP++安装程序 - Powered By SHOP++</title>
<meta name="Author" content="SHOP++ Team" />
<meta name="Copyright" content="SHOP++" />
<link href="css/install.css" rel="stylesheet" type="text/css" />
<script type="text/javascript" src="js/jquery.js"></script>
<script type="text/javascript">
$().ready( function() {
	
	var $isAgreeAgreement = $("input[name='isAgreeAgreement']");
	var $submitButton = $("#inputForm :submit");
	
	$isAgreeAgreement.click( function() {
		if ($isAgreeAgreement.prop("checked") == true) {
			$submitButton.prop("disabled", false);
		} else {
			$submitButton.prop("disabled", true);
		}
	});

});
</script>
</head>
<body>
	<div class="header">
		<div class="title">SHOP++ 安装程序 - 许可协议</div>
		<div class="banner"></div>
	</div>
	<div class="body">
		<form id="inputForm" action="check.jsp" method="get">
			<div class="bodyLeft">
				<ul class="step">
					<li class="current">许可协议</li>
					<li>环境检测</li>
					<li>系统配置</li>
					<li>系统安装</li>
					<li>完成安装</li>
				</ul>
			</div>
			<div class="bodyRight">
				<iframe scrolling="yes" src="../license.html"></iframe>
				<input type="checkbox" id="isAgreeAgreement" name="isAgreeAgreement" value="true" />
				<label for="isAgreeAgreement">我已经仔细阅读,并同意上述条款中的所有内容</label>
			</div>
			<div class="buttonArea">
				<input type="submit" class="button" value="下 一 步" disabled="disabled" />
			</div>
		</form>
	</div>
	<div class="footer">
		<p class="copyright">Copyright © 2005-2013 shopxx.net All Rights Reserved.</p>
	</div>
</body>
</html>