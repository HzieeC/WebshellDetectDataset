<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE html>
<html>
  <head>
    <base href="<%=basePath%>">
    <jsp:useBean id="net" scope="page" class="com.weishang.bean.Common"></jsp:useBean>
    
     <title>${net.net.title }</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="${net.net.keywords}">
	<meta http-equiv="description" content="${net.net.descript}">
	
	<link rel="stylesheet" href="<%=basePath%>css/admin_lay.css" />
	<script src="<%=basePath%>js/jquery-1.11.2.min.js"></script>
	<script src="<%=basePath%>js/public.js"></script>
	<script src="<%=basePath%>js/jquery.nicescroll.js"></script>
  </head>
  
<body>
<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
		<fmt:bundle basename="messagesAllMessage">
	  	<!--head-->
	    <div id="header">
	    	<div class="header_logo">
				<a href="<%=basePath%>admin/adminInit" target="main"><img src="<%=basePath%>${net.net.logo}"></a>
			</div>
			<div class="header_title"><a href="<%=basePath%>admin/adminInit" target="main">${net.net.company}<fmt:message key="manage" bundle="${messagesBundle}"/><fmt:message key="system" bundle="${messagesBundle}"/></a></div>
			<div class="header_rg">
				<div class="header_rg_ct"><span class="time"></span></div>
				<div class="header_right">
					<ul>
						<li><a href="<%=basePath%>admin/adminLoginOut" class="tuichu"><fmt:message key="loginout" bundle="${messagesBundle}"/></a></li>
						<li>
							<img src="<%=basePath%>images/touxiang.png">
							你好，${user.adminName}&nbsp;&nbsp;|<a href="" class="xiugai_password">修改密码</a>|
						</li>
					</ul>
				</div>
			</div>
		</div>
		<!--head结束-->
		</fmt:bundle>