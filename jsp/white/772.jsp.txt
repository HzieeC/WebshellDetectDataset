<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>支付跳转中...</title>
</head>
<body style="background:#F3F3F4">
<br />
<br />
<form id='paysubmit' name='paysubmit' action='${gateway_new}' accept-charset='utf-8' method='post'>
	<c:forEach var="item" items="${data}">
	   	<input type='hidden' name='${item.key}' value='${item.value}'/>
	</c:forEach>
	<input type='submit'  value='支付进行中...' style='display:none;'/>
</form>

<script type="text/javascript">
	document.forms['paysubmit'].submit();
</script>
</body>
</html>
