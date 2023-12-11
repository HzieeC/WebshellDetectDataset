<%@ page language="java" import="java.util.*" pageEncoding="GB2312"%>
  <%@ taglib prefix="s" uri="/struts-tags" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
 
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
   您的用户名：<s:iterator  value="#request.user"  id="user"/>
    <s:property value="%{#user.username}"/><br/>
    您的照片 ：<img src="<s:property value="%{#user.picture}"/>"/><br/>
    您的性别：<s:set name="sex" value="%{#user.sex}" />
       <s:if test="sex==1">
       男
      </s:if>
      <s:elseif test="sex==0">
      女</s:elseif><br>
       您的朋友：<s:property value="%{#user.friends}"/><br/>
    您的信息：<s:property value="%{#user.message}"/><br/>
    省份：<s:property value="%{#user.province}"/><br/>
  城市：<s:property value="%{#user.city}"/><br/>
        地址：<s:property value="%{#user.address}"/><br/>
            邮编：<s:property value="%{#user.post}"/><br/>
              电话：<s:property value="%{#user.telphone}"/><br/>
                   手机：<s:property value="%{#user.mobilephone}"/><br/>
  </body>
</html>
