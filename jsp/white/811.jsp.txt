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
    <jsp:useBean id="active" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 精彩活动-->
 	<jsp:useBean id="news" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 新闻-->
 	<jsp:useBean id="brand" scope="page" class="com.weishang.my.bean.MyBean"/><!-- 品牌-->
 	<jsp:useBean id="type" scope="page" class="com.weishang.my.bean.MyBean"/><!-- 类型-->
 	<jsp:useBean id="cat" scope="page" class="com.weishang.my.bean.MyBean"/><!-- 分类-->
 	
    <title>${recept.title}</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="${recept.keywords}">
	<meta http-equiv="description" content="${recept.descript}">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
		<div class="header_banner">
			<div class="header_lg_bg"></div>
				<div class="header_lg_form">
					<div class="login_fun">
						<div class="left login_action" id="login_tab">登录</div>
					</div>
					<form  class="login_form" name="loginForm" id="loginForm" action="javscript:void(0)" method="post">
						<ul class="login_now">
							<li><input type="text" class="lname_icon" name="tel" placeholder="请输入手机号" /></li>
							<li><input type="password" class="lpass_icon" name="pass" placeholder="请输入登录密码" /></li>
							<li class="pass_regist"><a>忘记登录密码</a>  <a href="<%=basePath%>goPetition/10.html" class="right">免费注册&gt;&gt;</a></li>
							<li><div onclick="userLogin()" class="login_btn">登&nbsp;&nbsp;录</div></li>
							<!-- <li class="other_login"><a href="#" class="login_qq">QQ登录</a><a href="#" class="login_wx">微信登录</a></li> -->
						</ul>
					</form>
					<div class="login_mobiled"></div>
				</div>
			</div>
		</div>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
    <script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
	<script type="text/javascript">
		function userLogin(){
			var params= $('#loginForm').serialize();
			$.ajax({
				url:"<%=basePath%>front?tag=userLogin", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					alert(data.tip);
					if(data.status==200){
						window.location.reload();
					}
				}
			}); 
		}
	</script>
  </body>
</html>