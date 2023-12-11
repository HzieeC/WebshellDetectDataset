<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<fmt:setLocale value="zh_CN"/>
<fmt:setBundle basename="messages" var="messagesBundle"/>
<fmt:bundle basename="messagesAllMessage">
<html>
  <head>
    <base href="<%=basePath%>">
    <title><fmt:message key="file" bundle="${messagesBundle}"/><fmt:message key="upload" bundle="${messagesBundle}"/></title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
	<link rel="stylesheet" href="<%=basePath%>css/admin_lay.css" />
	<script src="<%=basePath%>/js/upload/uploadPreview.js" type="text/javascript"></script>
    <script>
       window.onload = function () { 
            new uploadPreview({ UpBtn: "up_img", DivShow: "imgdiv", ImgShow: "imgShow" });
        }
    </script>
    <style>
		form{ padding:30px; line-height:20px}
	</style>
  </head>
  
  <body>
  	
	  	 <form action="<%=basePath%>admin/uploadHandleServlet" enctype="multipart/form-data" method="post">
		    <div id="imgdiv"><img id="imgShow" width="100" height="100" /></div><br>
		    <input type="file" id="up_img" name="file1"/><br><br>
		    <input type="submit" class="button-3" value="<fmt:message key="submit" bundle="${messagesBundle}"/>"/>
	    </form>
   
  </body>
</html>
 </fmt:bundle>