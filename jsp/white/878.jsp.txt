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
		<div class="title_c"><span>邮件设置</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c">
					<form name="smtpForm" id="smtpForm">
						<ul>
							<li>
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
									<tr>
										<td class="label_c" width="15%"><label><fmt:message key="smtp" bundle="${messagesBundle}"/><fmt:message key="address" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<input type="text" name="address" value="${smtp.address }" class="wid_60" placeholder="请输入邮件地址"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="smtp_from" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" name="from" value="${smtp.from}" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="smtp_user" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" name="user" value="${smtp.user}" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="smtp_pass" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" name="pass" value="${smtp.pass}" class="wid_60"/>
										</td>
									</tr>
	  							</table>
	         				</li>
						</ul>
					</form>
				</div>
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<li>
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="update()"/>
				</li>
			</ul>	
		</div>
	  <script>

	        //修改smtp
			function update(){
		    	var params= $('#smtpForm').serialize()
		        $.ajax({
					 url:"<%=basePath%>admin/updateSmtp", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
					 	layer.alert(data.tip);
					 }
				}); 
			}
			
		
	    </script>
	  
	</fmt:bundle>