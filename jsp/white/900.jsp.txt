<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>

<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
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
	<script src="<%=basePath%>js/public.js"></script>
	<!--[if IE 6]> 
	    <script src="<%=basePath%>js/DD_belatedPNG.js"></script> 
	    <script> 
	        DD_belatedPNG.fix('.png, .png img'); 
	    </script> 
	<![endif]--> 
  </head>
  
  <body>
	  <fmt:setLocale value="zh_CN"/>
	  <fmt:setBundle basename="messages" var="messagesBundle"/>
	  <fmt:bundle basename="messagesAllMessage">
	    <div class="loginbg">
			<div class="loginbox png">
		    	<h1 title="${net.net.company}<fmt:message key="manage" bundle="${messagesBundle}"/><fmt:message key="system" bundle="${messagesBundle}"/>">${net.net.company} <fmt:message key="manage" bundle="${messagesBundle}"/><fmt:message key="system" bundle="${messagesBundle}"/></h1>
		    	<div class="logo_n"><img src="<%=basePath%>${net.net.logo}"></div>
		    	<div class="logonform png">
		            <div class="loginright f_l">
		            	<div class="tips_t">${tip}</div>
		            	<form action="<%=basePath%>admin/adminLogin" method="post">
		            	<ul>
		                	<li>
		                    	<input type="text" name="user" id="user" class="input_user" placeholder="<fmt:message key="account" bundle="${messagesBundle}"/>"/>   
		                    </li>
		                    <li>
		                    	<input type="password" name="password" id="password" class="input_pass" placeholder="<fmt:message key="password" bundle="${messagesBundle}"/>" /> 
		                    </li>
		                    <li>
		                    	<input type="text" class="input_pass" name="verif_code" id="verif_code" style="float:left;width:50%;background:#fff;padding:0 0 0 15px" placeholder="验证码" />
		                    	<div style="float:right;width:80px;margin:0 24px 0 0"><img id="safecode" onclick="reloadcode()" src="<%=basePath%>admin/myAdmin?tag=getCode"/></div>
		                    </li>
		                    <li><input type="submit" value="<fmt:message key="login" bundle="${messagesBundle}"/>" class="input_submit" /></li>
		                </ul>
		                </form>
		            </div>
		        </div>
		    </div>
		</div>
	 </fmt:bundle> 
	  <script>
	  	function reloadcode(){
			var verify=document.getElementById('safecode');
			verify.setAttribute('src','<%=basePath%>admin/myAdmin?tag=getCode&random='+Math.random());
			//这里必须加入随机数不然地址相同我发重新加载
		}
	  </script>
  </body>
</html>
