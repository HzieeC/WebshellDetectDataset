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
		<div class="header_rg_bg">
			<div class="clear regist_box">
				<div class="left header_rg_form">
					<h3 class="re_til">注册新用户</h3>
					<form class="regist_form" name="registeForm" id="registeForm" action="javscript:void(0)" method="post">
						<ul class="regist_now">
							<li><input type="text" class="rmobile_icon" name="tel" id="tel" placeholder="请输入您的手机号码" /></li>
							<li>
								<input type="text" class="rphone_code" name="telcode" placeholder="请输入动态码" />
								<input type="button" class="rget_code" onclick="settime(this)" value="获取手机动态码" />
							</li>
							<li><input type="password" name="pass" class="rpass_icon" placeholder="请输入6-12位数字、字母、符号组合" /></li>
							<li><input type="password" name="re_pass" class="rpass_icon" placeholder="请重复输入6-12位数字、字母、符号组合" /></li>
							<li class="pass_regist">
								<input type="checkbox"/>我已阅读并同意  <a href="javascript:void(0)" class="regist_terms">《会员注册服务条款》</a></li>
							<li><div onclick="userRegiste()" class="regist_btn">注&nbsp;&nbsp;册</div></li>
						</ul>
					</form>
				</div>
				<!-- <div class="right regist_other">
					<h3 class="re_til">第三方账号注册</h3>
					<div class="regist_other_qq">QQ账号登录</div>
					<div class="regist_other_wx">微信账号登录</div>
				</div>
				 -->
			</div>
		</div>
	</div>
	
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
    <script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
    <script type="text/javascript" src="<%=basePath%>js/layer/layer.js"></script>
	<script type="text/javascript">
		function userRegiste(){
			var params= $('#registeForm').serialize();
			$.ajax({
				url:"<%=basePath%>front?tag=userRegiste", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					layer.alert(data.tip,function(){
						if(data.status==200){
							window.location.href="<%=basePath%>goPetition/9.html";
						}else{
							close(index);
						}
					});
				}
			}); 
		}
		function getTelCode(){
		
			var tel=$("#tel").val();
			$.ajax({
				url:"<%=basePath%>front?tag=getTelCode", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:{tel:tel},         //要传递的数据
				success:function(data){
					layer.alert(data.tip);
				}
			});
			 
		}
		
		var countdown=60;
		function settime(val) {
			if(countdown==60){
				getTelCode();
			}
			if (countdown == 0) { 
				val.removeAttribute("disabled");    
				val.value="获取手机动态码";
				countdown = 60;
				return;
			}else { 
				val.setAttribute("disabled", true); 
				val.value="重新发送(" + countdown + ")"; 
				countdown--; 
			}
			setTimeout(function() { 
				settime(val);
			},1000)
		} 
	</script>
  </body>
</html>