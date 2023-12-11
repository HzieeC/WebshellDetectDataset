<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<div class="right sales_type_more">
				<div class="sales_more_til" >精彩活动<a href="<%=basePath%>jingcaihuodong/22.html" class="right">更多&gt;&gt;</a></div>
				<ul class="sales_more_play">
					<jsp:useBean id="active" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 精彩活动-->
					<jsp:setProperty property="pageNo" value="1" name="active"/>
					<jsp:setProperty property="pageSize" value="5" name="active"/>
					<jsp:setProperty property="ids" value="73" name="active"/>
					<c:forEach items="${active.cmsList}" var="cms" varStatus="status">
			             <li>
			             	<a href="<%=basePath%>page/${cms.id}.html">
					            <img src="<%=basePath%>${cms.img}" />
					            <span>${cms.name}</span>
				            </a>
			             </li>
		              </c:forEach>
				</ul>
			</div>