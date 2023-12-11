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
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>管理员管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<a href="javascript:void(0);" onclick="add()" target="main" class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
					</ul> 
				</div>
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
			      <thead>
			          <tr>
			              
			              <th width="15%"><fmt:message key="name" bundle="${messagesBundle}"/></th>
			              <th width="17%"><fmt:message key="username" bundle="${messagesBundle}"/></th>
			              <th width="10%"><fmt:message key="sex" bundle="${messagesBundle}"/></th>
			              <th width="17%"><fmt:message key="role" bundle="${messagesBundle}"/></th> 
			              <th width="23%"><fmt:message key="qq" bundle="${messagesBundle}"/></th> 
			              <th width="17%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
			          </tr>
			      </thead>
			      <c:forEach items="${adminList}" var="admin" varStatus="status">
				      <tr>
				        
				        <td>${admin.adminName}</td>
				        <td>${admin.username }</td>
				        <td>${admin.adminSex} </td>
				        <td>${admin.roleName}</td>
				        <td>${admin.qq}</td>
				        <td>
				        	<div class="tablehand">
					        	<a href="javascript:void(0)" onclick="update(${admin.id})" class="red">
					        		<fmt:message key="modify" bundle="${messagesBundle}"/>
					        	</a>&nbsp;
					        	<a href="javascript:void(0)" onclick="deleteAdmin(${admin.id})" class="blue">
					        		<fmt:message key="delete" bundle="${messagesBundle}"/>
					        	</a>&nbsp;
				        	</div>
				        </td>
				      </tr>
			      </c:forEach>
		    	</table>
			</div>
		</div>
	</div>
	</fmt:bundle>
	<script language="javascript">
		
		function add(){
			layer.open({
				  type: 2,
				  title: '添加',
				  shadeClose: true,
				  shade: 0.8,
				  area: ['600px', '400px'],
				  content: '<%=basePath%>admin/goAddAdmin',
				end:function(index){
					window.location.reload();
				}
			}); 
		}
		
		function update(id){
			layer.open({
				  type: 2,
				  title: '修改',
				  shadeClose: true,
				  shade: 0.8,
				  area: ['600px', '400px'],
				  content: '<%=basePath%>admin/goSingleAdmin?item='+id+'',
				end:function(index){
					window.location.reload();
				}
			}); 
		}
		
		function deleteAdmin(id){
			if(confirm("请确认是否删除此管理员")){
				$.ajax({
					 url:"<%=basePath%>admin/deleteAdmin", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:{id:id},         //要传递的数据
					 success:function(data){
					 	alert(data.tip);
					 	window.location.reload();
					 }
				}); 
			}
			
			
		}
		
	</script>