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
	<script src="<%=basePath%>js/public.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>网站模板</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
					<thead width=100%>
						<tr>
							<th width="40%"><fmt:message key="preview" bundle="${messagesBundle}"/></th>
		              		<th width="25%"><fmt:message key="folder" bundle="${messagesBundle}"/></th>
		              		<th width="25%"><fmt:message key="name" bundle="${messagesBundle}"/></th>
		              		<th width="10%"><fmt:message key="operation" bundle="${messagesBundle}"/></th>
		              		
						</tr>
					</thead>
					<tbody width=100%>
						<c:forEach items="${tplList}" var="tpl" varStatus="status">
						<tr>
							<td><img width="100px" height="100px" src="<%=basePath%>template/${tpl.folder}/index.jpg"></td>
							<td class="text_align">${tpl.folder}</td>
							<td class="text_align">${tpl.name}</td>
							<td class="text_align">
								<c:if test="${tpl.flag==2}">
						        	 <div class="tablehand"><img style="cursor:pointer" src="<%=basePath%>/images/off.png" border=0 onclick="update(${tpl.flag},${tpl.id})"></div>
						        </c:if>
						       	<c:if test="${tpl.flag==1}">
						        	 <div class="tablehand"><img style="cursor:pointer" src="<%=basePath%>/images/no.png" onclick="update(${tpl.flag},${tpl.id})" border=0></div>
						        </c:if>
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
		//修改网站基本信息
			function update(flag,id){
		        $.ajax({
					 url:"<%=basePath%>admin/operaTpl", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:{flag:flag,id:id},         //要传递的数据
					 success:function(data){
					 	window.location.reload();
					 }
				}); 
			}
			
	</script>