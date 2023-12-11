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
	<script src="<%=basePath%>js/layer/layer.js"></script>
   
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>会员角色管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
			
				<form action="javascript:void(0)" name="roleForm" id="roleForm">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<input type="button" onclick="add()" value="<fmt:message key="add" bundle="${messagesBundle}"/>" class="button-4 button-search"/>
						</li>
						<li class="rightchild">
							<input type="button" onclick="update()" value="<fmt:message key="modify" bundle="${messagesBundle}"/>" class="button-4 button-search"/>
						</li>
						<li class="rightchild">
							<input type="button" onclick="deleteRole()" value="<fmt:message key="delete" bundle="${messagesBundle}"/>" class="button-4 button-search"/>
						</li>
					   
					</ul> 
				</div>
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
			      <thead>
			          <tr>
			              <th width="10%"><input type="checkbox" name="chekall" id="chekall" class="ckAll"/></th>
			              <th width="90%" align="left"><fmt:message key="role" bundle="${messagesBundle}"/><fmt:message key="name" bundle="${messagesBundle}"/></th>
			          </tr>
			      </thead>
			      <tbody id="list_chckbox">
			      <c:forEach items="${roleList}" var="role" varStatus="status">
				      <tr>
				        <td><input type="checkbox" name="role_id" value="${role.id}"></td>
				        <td align="left">${role.name }</td>
				      </tr>
			      </c:forEach>
			      </tbody>
			    </table>
			</div>
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
			// 添加
			function add(){
		  		layer.open({
				  type: 2,
				  title: '添加角色',
				  shadeClose: true,
				  shade: 0.8,
				  area: ['600px', '400px'],
				  content: '<%=basePath%>admin/goAddUserType',
					end:function(index){
						window.location.reload();
					}
				}); 
		  	}
			// 修改
			function update(){
				var chkbs = document.getElementsByName("role_id");
				var j = 0; // 用户选中的选项个数
			    for(var i=0;i<chkbs.length;i++){
			      if(chkbs[i].checked){
			          j++;
			      }
			    }
			   
			   if(j>1){
			   		layer.alert("一次只能选中一行");
			   }if(j==0){
			   		layer.alert("请选择您要修改的数据");
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
					  title: '修改',
					  shadeClose: true,
					  shade: 0.8,
					  area: ['600px', '400px'],
					  content: '<%=basePath%>admin/goAddUserType?tem='+chestr+'',
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
			    	layer.alert("请选择您要删除的数据");
			    }else{
			    	if(confirm("删除分类之后，分类下的用户将不能正常运行")){
						var params= $('#roleForm').serialize()
					    $.ajax({
							 url:"<%=basePath%>admin/deleteUserType", //后台处理程序
							 type:'post',         //数据发送方式
							 dataType:'json',
							 data:params,         //要传递的数据
							 success:function(data){
								 layer.alert(data.tip);
								 window.location.reload();
							 }
						}); 
					}
			    }
				
			}

	</script>