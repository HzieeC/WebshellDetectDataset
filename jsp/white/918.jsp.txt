<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html>
<meta http-equiv="pragma" content="no-cache"/>
<meta http-equiv="cache-control" content="no-cache"/>
<meta http-equiv="expires" content="0"/>    
	<link rel="stylesheet" href="<%=basePath%>css/admin_lay.css" />
	<script src="<%=basePath%>js/jquery-1.11.2.min.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>信息模板</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c">
			<div class="middle_cnt_cont_d">
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
			      <thead>
			          <tr> 
			              <th width="27%"><fmt:message key="modular" bundle="${messagesBundle}"/></th>
			              <th width="17%"><fmt:message key="title" bundle="${messagesBundle}"/></th>
			              <th width="29%"><fmt:message key="desc" bundle="${messagesBundle}"/></th>
			              <th width="17%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
			          </tr>
			      </thead>
			      <tbody>
			      <c:forEach items="${infoList}" var="info" varStatus="status">
				      <tr>			        
				        <td>${info.name }</td>
				        <td>${info.title }</td>
				        <td>${info.content}</td>
				        <td>
				     		<a href="javascript:void(0)"  onclick="Mask(${info.id})"  class="red"><fmt:message key="modify" bundle="${messagesBundle}"/></a>&nbsp;
				        </td>
				      </tr>
			      </c:forEach>
			      </tbody>
			    </table>
		    	
			</div>
		</div>
	</div>
	
	</fmt:bundle>
	<script language="javascript">
		function Mask(id){
	  		layer.open({
			  type: 2,
			  title: '修改模板',
			  shadeClose: true,
			  shade: 0.8,
			  area: ['450px', '320px'],
			  content: '<%=basePath%>admin/goModifyInfoTpl?id='+id+'',
				end:function(index){
					window.location.reload();
				}
			}); 
	  	}
	</script>