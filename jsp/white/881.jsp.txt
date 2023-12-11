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
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
				
					<form id="infotplForm">
		        		<input name="id" value="${info.id}" type="hidden">
						
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
									<tr>
										<td class="label_c" width="15%"><label><fmt:message key="title" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<input type="text" class="wid_80" name="title" value="${info.title}"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="content" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<textarea class="wid_80" name="content">${info.content}</textarea>
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
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="update()"/>
				</li>
			</ul>	
		</div>
		
	</div>
	<!--本页主内容结束 -->
	
	</fmt:bundle>
		<script language="javascript">
			$(function(){
				var height = $(window).height();
				$(".middle_cnt_c2").height(height-80);
				$(window).resize(function () {          //当浏览器大小变化时
				    var height = $(window).height();
					$(".middle_cnt_c2").height(height-80);
				});
			})
			function update(){
		        var params= $('#infotplForm').serialize()
		        $.ajax({
					 url:"<%=basePath%>admin/modifyInfoTpl", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
					 	alert(data.tip);
					 	parent.window.location.reload();
					 }
				}); 
			}
		</script>