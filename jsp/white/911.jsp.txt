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
	<script src="<%=basePath%>js/validate.js"></script>
	<script src="<%=basePath%>js/public.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>雇员信息</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c">
		        	<form name="employeeForm" id="employeeForm">
		        		<input type="hidden" name="employee_id" value="${employee_id}" />
				        <ul>
							<li>
						        <table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
						        	<tr>
										<td class="label_c" width="15%"><label><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<input type="text" value="${employee.name}" name="name" id="name" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="shenfen" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${employee.shenfen}" name="shenfen" id="shenfen" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="tel" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${employee.tel}" name="tel" id="tel" class="wid_60"/>
										</td>
									</tr>
							       	<tr>
										<td class="label_c"><label><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="email" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${employee.email}" name="email" id="email" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="no" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<c:if test="${employee_id==null || employee_id==''}">
								            		<input type="text"  value="${no}" name="no" id="no" class="wid_60"/>
								            	</c:if>
								            	<c:if test="${employee_id!=null && employee_id!=''}">
								            		<input type="text"  value="${employee.no}" name="no" id="no" class="wid_60" readonly/>
								            	</c:if>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="password" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${employee.pass}" name="pass" id="pass" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="grading" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<select name="grading" id="grading">
									            	<option value="">请选择</option>
									            	<option value="0">销售</option>
									            	<option value="1">店长</option>	
									            </select>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="state" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<select name="state" id="state">
									            	<option value="">请选择</option>
									            	<option value="1">运行</option>
									            	<option value="2">关闭</option>	
									            </select>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="store" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<select name="store_id" id="store_id">
									            	<option value="">请选择</option>
									            	<c:forEach items="${storeList}" var="store" varStatus="status">
									            		<option value="${store.id}">${store.name}</option>
									            	</c:forEach>
									            </select>
										</td>
									</tr>
						        </table>
					        </li>
			        </form>
			    </div>
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<li>
			    	<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="add()"/>
			    </li>
			     <li>
			    	<input type="button" onclick="back()" value="<fmt:message key="back" bundle="${messagesBundle}"/>" class="button-2 vcenter"/>
			    </li>
			</ul>	
		</div>
		
	</div>
	<!--本页主内容结束 -->
		
	</fmt:bundle>
	<script>
		function add(){
			 var title=requree_name("name") && requree_length("name",2,10);
			 var shenfen=requree_cer("shenfen");
			 var tel=requree_iphone("tel");
			 var email=requree_email("email");
			 var grading=document.getElementById("grading").value;
			 var state=document.getElementById("state").value;
			 var store_id=document.getElementById("store_id").value;
             if(!title){
             	layer.alert("雇员名称为必填项");
             }else if(!shenfen){
             	layer.alert("请输入正确的身份证号");
             }else if(!tel){
             	layer.alert("请输入电话号码");
             }else if(!email){
             	layer.alert("请输入电子邮件");
             }else if(grading==""||grading==null){
             	layer.alert("请设置雇员级别！");
             }else if(state==""||state==null){
             	layer.alert("请设置状态！");
             }else if(store_id==""||store_id==null){
             	layer.alert("请选择所属门店！");
             }else{
             	var params= $('#employeeForm').serialize();
			    $.ajax({
					 url:"<%=basePath%>admin/addEmployee", //后台处理程序
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
		
		function back(){
			window.location.href="<%=basePath%>admin/goEmployee?id=66"		
		}
		document.getElementById("state").value="${employee.state}";
		document.getElementById("grading").value="${employee.flag}";
		document.getElementById("store_id").value="${employee.store_id}";
	</script>