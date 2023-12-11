<%@ page language="java" import="java.util.*" pageEncoding="GB2312"%>
  <%@ taglib prefix="s" uri="/struts-tags" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
  <s:set id="user" value="#request.user"/>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title><s:property value="%{#user.username}"/>个人详细信息</title>
    
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
	<link href="./css/style.css" rel="stylesheet" type="text/css">

  </head>
  
  <body>
    <s:property value="%{#user.username}"/><br/>
      <img src="<s:property value="%{#user.picture}"/>"/><br/>
      <s:property value="%{#user.sex}"/><br/>
       <s:property value="%{#user.friends}"/><br/>
        <s:property value="%{#user.message}"/><br/>
  </body>
</html>
