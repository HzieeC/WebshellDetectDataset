<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE html>
<html>
  <head>
    <base href="<%=basePath%>">
    <jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/>
    <jsp:useBean id="net" scope="page" class="com.weishang.bean.ReceptBean"/><!--网站基本信息-->
    <title>${net.net.company}</title>
    
	<link href="<%=basePath%>template/${folder.tpl.folder}/css/style_pc.css" rel="stylesheet">
  </head>
  
  <body>
    <!-- 地址列表 -->
    <div class="user-myOrder">
		<div class="user-bd-main" style="padding:10px; border:none">
	    <table class="content">
		    <tr>
		         <th>商品名称</th>
		         <th>编号</th>
		    </tr>
		    <c:forEach items="${goodsList}" var="goods" varStatus="status">
			    <tr>
			       <td>${goods.goodsName}</td>
			       <td>${goods.goodsSn}</td>                           
			    </tr>
		    </c:forEach> 
	     </table>
	    </div>
     </div>
  </body>
</html>