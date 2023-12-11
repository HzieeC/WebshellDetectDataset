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
		<div class="title_c"><span>角色管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c">
			<div class="middle_cnt_cont_d">
				<form action="javascript:void(0)" name="roleForm" id="roleForm">
					<div class="toolbar clear">
						<ul class="handlist-left">
							<li class="rightchild">
								<a href="javascript:void(0);" onclick="add()" class="button-1"/><fmt:message key="add" bundle="${messagesBundle}"/></a>
							</li>
							<li class="rightchild">
								<a href="javascript:void(0);" onclick="update()" class="button-1"/><fmt:message key="modify" bundle="${messagesBundle}"/></a>
							</li>
							<li class="rightchild">
								<a href="javascript:void(0);" onclick="deleteRole()" class="button-1"/><fmt:message key="delete" bundle="${messagesBundle}"/></a>
							</li>
						   
						</ul> 
					</div>
					<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
				      <thead>
				          <tr>
				              <th width="5%"><input type="checkbox" name="chekall" id="chekall" class="ckAll" value="${role.id}"/></th>
				              <th width="95%" style="text-align:left"><fmt:message key="role" bundle="${messagesBundle}"/><fmt:message key="name" bundle="${messagesBundle}"/></th>
				          </tr>
				      </thead>
				      <tbody id="list_chckbox">
				      <c:forEach items="${roleList}" var="role" varStatus="status">
					      <tr>
					        <td><input type="checkbox" name="role_id" value="${role.id}"></td>
					        <td style="text-align:left">${role.name }</td>
					      </tr>
				      </c:forEach>
				      </tbody>
				    </table>
				</form>
			</div>
		</div>
	</div>
		
	</fmt:bundle>
	<script language="javascript">
		//全选   
		 $('.ckAll').click(function(){
			$("#list_chckbox :checkbox").each(function(){
				$(this).prop("checked",!!$("#chekall").prop("checked"));
			});
		});
			function add(){
				layer.open({
					  type: 2,
					  title: '添加角色',
					  shadeClose: true,
					  shade: 0.8,
					  area: ['400px', '220px'],
					  content: '<%=basePath%>admin/goAddRole',
					end:function(index){
						window.location.reload();
					}
				}); 
			}
			
			function update(){
				var chkbs = document.getElementsByName("role_id");
				var j = 0; // 用户选中的选项个数
			    for(var i=0;i<chkbs.length;i++){
			      if(chkbs[i].checked){
			          j++;
			      }
			    }
			   
			   if(j>1){
			   		alert("一次只能选中一行");
			   }if(j==0){
			   		alert("请选择您要修改的数据");
			   }else{
				   	var objarray=chkbs.length;
					var chestr="";
					for (i=0;i<objarray;i++)
					{
						if(chkbs[i].checked == true)
						{
						  chestr+=chkbs[i].value;
						}
					}
					layer.open({
						  type: 2,
						  title: '修改角色',
						  shadeClose: true,
						  shade: 0.8,
						  area: ['400px', '220px'],
						  content: '<%=basePath%>admin/goAddRole?cid='+chestr+'',
						end:function(index){
							window.location.reload();
						}
					}); 
			   }
				
			}
			
			function deleteRole(){
				var chkbs = document.getElementsByName("role_id");
				var j = 0; // 用户选中的选项个数
			    for(var i=0;i<chkbs.length;i++){
			      if(chkbs[i].checked){
			          j++;
			      }
			    }
			    if(j==0){
			    	alert("请选择您要删除的数据");
			    }else{
			    	if(confirm("请确认是否要删除此角色")){
						var params= $('#roleForm').serialize()
					    $.ajax({
							 url:"<%=basePath%>admin/deleteRole", //后台处理程序
							 type:'post',         //数据发送方式
							 dataType:'json',
							 data:params,         //要传递的数据
							 success:function(data){
								 alert(data.tip);
								 window.location.reload();
							 }
						}); 
					}
			    }
				
			}
			

	</script>