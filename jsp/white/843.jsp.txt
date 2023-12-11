<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<jsp:useBean id="navigat" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 主导航 -->
<jsp:useBean id="net" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 主导航 -->
<jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
<div class="header">
	<div class="header_fun_bg"> 
		<div class="mainbox header_fun_info"> 
			尊敬的客户，欢迎您来到${net.net.company}
			<div class="right">
				<c:if test="${ordinary_user==null}">
					<a href="<%=basePath%>goPetition/9.html">登陆</a>|
					<a href="<%=basePath%>goPetition/10.html">注册</a>
				</c:if>
				<c:if test="${ordinary_user!=null}">
					<a href="<%=basePath%>pc/pcUserIndex">用户中心</a>|
					<a href="<%=basePath%>pc/pcLoginOut">退出</a>
				</c:if>
				|<a href="#">网站导航</a>
				|<a href="<%=basePath%>auto/19.html">帮助中心</a>
			</div>
		</div>
	</div>	
	<div class="header_menu_bg">
		<div class="mainbox header_menu_info">
			<a href="<%=basePath%>" class="header_logo"><img src="<%=basePath%>${net.net.logo}" /></a>
			<ul class="header_menu">
				<jsp:setProperty property="addr" value="1" name="navigat"/>
				<jsp:setProperty property="host" value="<%=basePath%>" name="navigat"/>
				<c:forEach items="${navigat.receptList}" var="recept" varStatus="status">
					<li>
						<a href="${recept.modUrl}">${recept.name}</a>
					</li>
				</c:forEach>
				<li class="app"><span class="header_menu_mobile"><a href="javascript:void(0)">手机版</a></span></li>
				<li class="phone"><img src="<%=basePath%>template/${folder.tpl.folder}/images/header_logo_phone.jpg" /></li>
				
			</ul>
		</div>
	</div>