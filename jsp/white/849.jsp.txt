<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
 <jsp:include  page="/WEB-INF/jsp/head.jsp"/>
 <!--整体内容层-->
    <div id="middle">
  		<jsp:include  page="/WEB-INF/jsp/left.jsp"/>
		<!--right-->
     	<div id="m_center">
			<iframe src="<%=basePath%>admin/adminInit" width="100%" height="100%" frameborder="0" scrolling="no" name="main" id="main" allowtransparency="true" ></iframe>
		</div>
		<!--end right-->
	</div>
	<!--整体内容层结束-->
 <jsp:include  page="/WEB-INF/jsp/footer.jsp"/>