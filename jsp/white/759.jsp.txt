<%@ page language="java" import="java.util.*" contentType = "text/html;charset=GB2312" pageEncoding ="GB2312"%>
<%@ taglib prefix = "s" uri = "/struts-tags" %> 

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    
    <title>My JSP 'ShowUpload.jsp' starting page</title>
    
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
  <div style ="padding:3px;border:solid 1px #cccccc;text-align:center" > 
        <img src='UploadImages/<s:property value ="imageFileName" />'/>
        <br/> 
        <s:property value ="caption"/> 
    </div> 
  
  </body>
</html>
