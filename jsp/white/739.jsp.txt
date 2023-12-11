<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'notice.jsp' starting page</title>
    
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
	<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->
  </head>
 
  <body>
  <br>
  <table width="100%" bgcolor="#000000"><tr>
    <td bgcolor="#999999"><img src="./images/notice.gif"></td></tr></table>
<div class="changetLis">
   <s:iterator value="pageBean.list10">    
  <dl>
  
    <table width="100%" border="0">
    <tr><td bgcolor="#cccccc">
      <div class="changeListText"><table width="100%" ><tr><td width="10%"><s:property value="id"/>&nbsp;&nbsp;</td><td width="60%"><a href="showNotice!get?id=<s:property value="id"/>"><s:property value="title"/></a></td><td width="30%"><s:property value="publicdatetime"/></td></tr></table></div>
  </td></tr>
   </table>

  </dl>
  </s:iterator>
</div>
  </body>
</html>
