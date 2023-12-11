<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!--down-->
<jsp:useBean id="admin" scope="page" class="com.weishang.bean.Admin"></jsp:useBean>
<jsp:setProperty property="roleId" value="${user.role}" name="admin"/>


		<div id="m_left">
	    	<!--left-->
	        <div class="left" id="leftMenu">
	            <div class="wrap-menu">
			
		        </div>
	            <script>
					var myMenu=${admin.menuCode};
					$(function(){
			  	    	new AccordionMenu({menuArrs:myMenu});
					});
	            </script>
	        </div>
	        <!--end left-->
		</div>
	    
    
