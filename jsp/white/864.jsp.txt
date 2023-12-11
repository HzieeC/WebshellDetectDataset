<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<meta http-equiv="pragma" content="no-cache"/>
<meta http-equiv="cache-control" content="no-cache"/>
<meta http-equiv="expires" content="0"/>    
	<link rel="stylesheet" href="<%=basePath%>css/admin_lay.css" />
	<script src="<%=basePath%>js/jquery-1.11.2.min.js"></script>
	<script src="<%=basePath%>js/public.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
			
			<table border="0" cellspacing="0" cellpadding="0" class="tablelist">
		      <thead>
		          <tr>
		              <th width="15%"><fmt:message key="links" bundle="${messagesBundle}"/><fmt:message key="theway" bundle="${messagesBundle}"/></th>
		              <th width="15%"><fmt:message key="input" bundle="${messagesBundle}"/><fmt:message key="person" bundle="${messagesBundle}"/></th>
		              <th width="25%"><fmt:message key="time" bundle="${messagesBundle}"/></th>
		              <th width="45%"><fmt:message key="conclusion" bundle="${messagesBundle}"/></th>
		          </tr>
		      </thead>
		      <c:forEach items="${conList}" var="con" varStatus="status">
			      <tr>
			        <td>${con.type}</td>
			        <td>${con.name}</td>
			        <td>${con.time}</td>
			        <td class="text_align">${con.conclusion} </td>
			      </tr>
		      </c:forEach>
		    </table>
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<jsp:useBean id="paging" scope="page" class="com.weishang.bean.Page"/>
				<jsp:setProperty property="user" value="admin" name="paging"/>
				<jsp:setProperty property="crrent" value="${pageNo}" name="paging"/>
				<jsp:setProperty property="suffix" value="" name="paging"/>
				<jsp:setProperty property="sumPage" value="${sum}" name="paging"/>
				<jsp:setProperty property="url" value="/admin/goLookMessage" name="paging"/>
				${paging.pageString}
		     </ul>
		</div>
	</div>
	</fmt:bundle>