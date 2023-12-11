<%@page pageEncoding="UTF-8" contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="s" uri="/struts-tags" %>  
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">   
   
<html>   
   
    <head>   
   
       <meta http-equiv="Content- Type" content="text/html; charset=GBK"/>     
       <link href="../css/style.css" rel="stylesheet" type="text/css">
       <title>用户购物系统</title>   
     
    </head>   
   
    <body>   
    <s:set var="adminname" value="#session.adminname"></s:set> 
    <s:if test="adminname!=null"> 
欢迎您<s:property value="#session.adminname" />！<br>  
 
</s:if>  
  
 <b>用户管理</b><br> 
   <a href="adminUser!list" target="right">用户列表</a><br> 
<b>分类管理</b><br> 
<a href="type/addbigtype.jsp" target="right">增加大类</a><br> 
<a href="listBigtype2!list" target="right">增加小类</a><br> 
<b>产品管理</b><br>  
     <a href="addProduct" target="right">增加产品</a><br> 
   <a href="adminProducts!list" target="right">产品列表</a><br><b>新闻管理</b><br> 
 <a href="news/addnews.jsp" target="right">添加新闻</a><br> 
<b>退出</b><br> 
   <a href="logoutAdmin!logout" target="_top">退出</a> 
    </body>   
   
</html>   

