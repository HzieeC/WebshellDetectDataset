<%@ page language="java" contentType = "text/html;charset=utf-8" import="java.util.*" pageEncoding="UTF-8"%>
<%@ taglib prefix ="s" uri ="/struts-tags"%> 

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    
    <title>Struts 2 File Upload </title>
    
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
  <s:fielderror></s:fielderror>
  <s:form action ="fileUpload" method ="POST" enctype ="multipart/form-data"> 
    <s:file name ="myFile" label ="Image File"/> 
        <s:textfield name ="caption" label ="Caption"/>        
        <s:submit/> 
    </s:form > 

  </body>
</html>
