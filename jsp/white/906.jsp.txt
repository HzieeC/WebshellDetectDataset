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
		        	<form name="userForm" id="userForm">
		        		<input type="hidden" name="item" value="${item}" />

						        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
						        	<tr>
										<td class="label_c" width="15%"><label><fmt:message key="username" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<c:if test="${item==null}">
								            	<input type="text"  value="${user2.username}" name="username" id="username" class="wid_80"/>
								            </c:if>
								            <c:if test="${item!=null}">
								            	<input type="text" readonly value="${user2.username}" name="username" id="username" class="wid_80"/>
								            </c:if>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="password" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <c:if test="${item==null}">
								            		<input type="text"  value="123456" name="password" id="password" class="wid_80"/>
								            	</c:if>
								            	<c:if test="${item!=null}">
								            		<input type="text" readonly value="${user2.pass}" name="password" id="password" class="wid_80"/>
								            	</c:if>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>姓名：</label></td>
										<td>
											<c:if test="${item==null}">
								            		<input type="text" value="${user2.name}" name="name" id="name" class="wid_80"/>
								            	</c:if>
								            	<c:if test="${item!=null}">
								            		<input type="text" readonly value="${user2.name}" name="name" id="name" class="wid_80"/>
								            	</c:if>
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
										<td class="label_c"><label><fmt:message key="state" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<select name="state" id="state">
									            	<option value="">--请选择--</option>
									            	<option value="1">运行</option>
									            	<option value="2">冻结</option>
									            </select>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="tel" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<c:if test="${item==null}">
								            		<input type="text" name="tel" id="tel"  value="${user2.tel}" class="wid_80"/>
								            	</c:if>
								            	<c:if test="${item!=null}">
								            		<input type="text" readonly name="tel" id="tel"  value="${user2.tel}" class="wid_80"/>
								            	</c:if>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="email" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<c:if test="${item==null}">
								            		<input type="text" name="email" id="email"  value="${user2.email}" class="wid_80"/>
								            	</c:if>
								            	<c:if test="${item!=null}">
								            		<input type="text" readonly name="email" id="email"  value="${user2.email}" class="wid_80"/>
								            	</c:if>
										</td>
									</tr>
									<tr>
								        <td class="label_c"><label>余额：</label></td>
								        <td>
								    		<input type="text" name="money" id="money"  value="${user2.money}" class="wid_80"/>
								        </td>
								    </tr>
									<tr>
								      <td class="label_c"><label>积分：</label></td>
								      <td>
								    	<input type="text" name="intagel" id="intagel"  value="${user2.integel}" class="wid_80"/>
								      </td>
								    </tr>
								    <tr>
								        <td class="label_c"><label>卡号：</label></td>
								        <td>
								    		<input type="text" name="card" id="card"  value="${user2.card}" class="wid_80"/>
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
		
		$(function(){
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-80);
			$(window).resize(function () {          //当浏览器大小变化时
			    var height = $(window).height();
				$(".middle_cnt_c2").height(height-80);
			});
		})
		
		function add(){
			 var title=requree_name("username") && requree_length("username",2,80);
			 var password=requree_length("password",6,20);
			 var role=document.getElementById("role").value;
			 var state=document.getElementById("state").value;
			 var tel=requree_iphone("tel");
			 var email=requree_email("email");
			 if(!title){
             	layer.alert("用户名为必填项");
             }else if(!password){
             	layer.alert("请设置密码");
             }else if(role==""||role==null){
             	layer.alert("请设置所属角色！");
             }else if(state==""||state==null){
             	layer.alert("请设置状态！");
             }else if(!tel){
             	layer.alert("请填写电话！");
             }else if(!email){
             	layer.alert("请填写电子邮件！");
             }else{
             	var params= $('#userForm').serialize();
			    $.ajax({
					 url:"<%=basePath%>admin/addUser", //后台处理程序
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
		document.getElementById("role").value="${user2.type}";
		document.getElementById("state").value="${user2.flag}";
	</script>