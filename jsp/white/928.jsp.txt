<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html>
<html>
 <jsp:useBean id="json" scope="page" class="com.weishang.bean.Authoriz"/><!-- 系统授权 -->
 <jsp:setProperty property="host" value="<%=request.getServerName()+path%>" name="json"/>
  <head>
    <base href="<%=basePath%>">

	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">
	<link rel="stylesheet" href="<%=basePath%>css/admin_lay.css" />
	<link rel="stylesheet" href="<%=basePath%>css/init.css" />
	<script src="<%=basePath%>js/jquery-1.11.2.min.js"></script>
	<script src="<%=basePath%>js/public.js"></script>
  </head>
  <body>
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>我们的服务</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c">
		    <div class="user_c">
				<div class="user_c_rg">
					<div class="user_account">
						<span id="authChar"></span>
						<span id="authImg"></span>
					</div>
					<div class="user_main_tip">
						<div class="title">我们的服务<span>(本页面的所有内容受到法律的保护，未经允许不得删除或修改)</span></div>
						<div class="user_li_0">
							<ul>
								<li><a target="_blank" href="http://wpa.qq.com/msgrd?v=3&uin=1919594905&site=qq&menu=yes" class="a1"><span><b>短信购买、软件授权</b><br><font class="cor_1">咨询QQ：1919594905</font></span></a></li>
								<li><a target="_blank" href="http://wpa.qq.com/msgrd?v=3&uin=694510804&site=qq&menu=yes" class="a2"><span><b>订制开发</b><br><font class="cor_1">咨询QQ：694510804</font></span></a></li>
								<li><a target="_blank" href="http://wpa.qq.com/msgrd?v=3&uin=1518912563&site=qq&menu=yes" class="a3"><span><b>支付代申请/备案/服务器租赁</b><br><font class="cor_1">咨询QQ：1518912563</font></span></a></li>
							</ul>
						</div>
						<div class="tips_cr">技术支持，请加入QQ群125836898</div>
					</div>
				</div>
			</div>
		</div>
		<!--右侧内容部分结束-->
	</div>
  </body>
  <script>
  	var dataObj=${json.json};
  	document.getElementById("authImg").innerHTML="<img width=100px height=100px src=\""+dataObj.img+"\" border=0/>";
  </script>
</html>
