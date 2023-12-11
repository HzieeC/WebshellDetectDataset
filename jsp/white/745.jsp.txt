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
<table><tr><td background="./images/catelogebg.jpg" align="center"><b>产品类别</b></td></tr>
<s:iterator value="#request.bigtype"  status="status" > 
<tr><td background="./images/typebg.jpg" align="center" valign="middle"><a href="listSmalltype!list?id=<s:property value="bigtypeid"/>"><b><s:property value="bigtypename" /></b></a></td></tr>
<s:iterator value="#request.smalltype"> 
<s:if test="bigtypeid==(#status.index+1)"><tr><td bgcolor="" align="center"> <a href="listProducts!show?smallcategory=<s:property value="smalltypename"/>"><s:property value='smalltypename'/></a> </td></tr></s:if> 
  </s:iterator>
</s:iterator>

</table>
  </body>
</html>
