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
	<script src="<%=basePath%>js/validate.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
		        	<form name="adminForm" id="adminForm">
		        		<input type="hidden" name="item" value="${item}" />
						        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
						        	<tr>
										<td class="label_c" width="15%"><label><fmt:message key="username" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<c:if test="${item==null}">
								            	<input type="text"  value="${admin.username}" name="username" id="username" class="wid_80"/>
								            </c:if>
								            <c:if test="${item!=null}">
								            	<input type="text" readonly value="${admin.username}" name="username" id="username" class="wid_80"/>
								            </c:if>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>姓名：</label></td>
										<td>
								            <input type="text" value="${admin.adminName}" name="name" id="name" class="wid_80"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="sex" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <select name="sex" id="sex">
									            	<option value="">--请选择--</option>
									            	<option value="男">男</option>
													<option value="女">女</option>
									            </select>
										</td>
									</tr>
					 				<tr>
										<td class="label_c"><label><fmt:message key="role" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <select name="role" id="role">
									            	<option value="">--请选择--</option>
									            	<c:forEach items="${roleList}" var="role" varStatus="status">
									            		<option value="${role.id }">${role.name}</option>
													</c:forEach>
									            </select>
										</td>
									</tr>
					       			<tr>
										<td class="label_c"><label><fmt:message key="qq" bundle="${messagesBundle}"/>：</label></td>
										<td>
								        	<input type="text" name="qq" id="qq"  value="${admin.qq}" class="wid_80"/>
										</td>
									</tr>
						        </table>
					      
			        </form>
			    
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<li>
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="add()"/>
				</li>
			</ul>	
		</div>
		
	</div>
	<!--本页主内容结束 -->
		
	</fmt:bundle>
	<script>
		function add(){
			 var username=requree_name("username") && requree_length("username",6,20);
			 var name=requree_name("name") && requree_length("name",2,20);
			 var role=document.getElementById("role").value;
			 
             if(!username){
             	layer.alert("用户名为必填项，限制6~20位");
             }else if(!name){
             	layer.alert("请输入姓名，限制2~20位");
             }else if(role==""||role==null){
             	layer.alert("请设置所属角色！");
             }else{
             	var params= $('#adminForm').serialize();
			    $.ajax({
					 url:"<%=basePath%>admin/addAdmin", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
						 alert(data.tip);
						 parent.window.location.reload();
					 }
				}); 
             }
		    
		}
		document.getElementById("role").value="${admin.role}";
		document.getElementById("sex").value="${admin.adminSex}";
		var height = $(window).height();
		$(".middle_cnt_c2").height(height-80);
		$(window).resize(function () {          //当浏览器大小变化时
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-80);
		});
	</script>