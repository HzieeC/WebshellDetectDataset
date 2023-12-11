<%@ page language="java" import="java.util.*" pageEncoding="gb2312"%>
<%@ taglib prefix="s" uri="/struts-tags" %>  
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'listbigtype.jsp' starting page</title>
    
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
  产品类别--选择小类<br>
  <select name="smallcategory">
<s:iterator value="#request.bigtype"  status="status" > 
<option><s:property value="bigtypename"/></option>
  <s:iterator value="#request.smalltype"> 
<s:if test="bigtypeid==(#status.index+1)"> <option value="<s:property value="smalltypename"/>">--<s:property value="smalltypename"/></option></s:if> 
  </s:iterator>
</s:iterator>
</select>
  </body>
</html>
