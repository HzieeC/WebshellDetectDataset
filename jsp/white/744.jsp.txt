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
  <table width="100%" height="25" align="center" \>
  <tr>
  <td background="./images/bg.jpg" ><s:a href="index.jsp">
    <div align="center"><strong>首页</strong></div>
  </s:a></td>  
    <td background="./images/bg.jpg" ><s:a href="productsAction!list">
      <div align="center"><strong>产品中心</strong></div>
    </s:a></td>     
    <td background="./images/bg.jpg" > <s:a href="cart/viewcart.jsp">
      <div align="center"><strong>查看购物车</strong></div>
    </s:a></td><td background="./images/bg.jpg" ><s:a href="order/order.jsp">
      <div align="center"><strong>去收银台</strong></div>
    </s:a></td>
	  <td background="./images/bg.jpg" > <s:a href="newsAction!list">
	    <div align="center"><strong>新闻</strong></div>
    </s:a></td>
</tr>
</table>
  </body>
</html>
