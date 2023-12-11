<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE HTML>
<html>
  <head>
    <base href="<%=basePath%>">
 	<jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
    <title>${goods.goodsName}</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">
	<meta http-equiv="keywords" content="${goods.goodsName}">
	<meta http-equiv="description" content="${goods.goodsName}">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
  	<style>
		html {
		    width: 100%;
		}
		body {
		    margin: 0;
		    padding: 0;
		    width: 100%;
		    color: #111;
		    font-family: "PingHei", STHeitiSC-Light, "Lucida Grande",
		    "Lucida Sans Unicode", Helvetica, Arial, Verdana, sans-serif;
		    font-size: 1em;
		    background:#f9f9f9;
		    min-height:100%;
		}
		.cont_pay{width:800px;margin:50px auto;font-size:16px; border:3px solid #ccc;height:300px;line-height:300px;background:#fff;text-align:center;}
		.cont_pay a{color:#428BDD; margin:0 5px;}
	</style>
  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
  	<div class="cont_pay">
		${tip}
	</div>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
  </body>
</html>