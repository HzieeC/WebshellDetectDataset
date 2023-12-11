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
	<jsp:useBean id="com" scope="page" class="com.weishang.bean.Common"/>
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
		        	<form name="roleForm" id="roleForm">
		        		<input type="hidden" name="roleId" value="${cid}" />
				        
						       <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
						       		<tr>
										<td class="label_c" width="15%"><label><fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
								           	<input type="text" value="${role.name}" name="name" id="name" class="wid_80"/>
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
		 	 var title=requree_name("name") && requree_length("name",2,80);
             if(!title){
             	layer.alert("名称为必填项");
             }else{
	            var params= $('#roleForm').serialize()
			    $.ajax({
					 url:"<%=basePath%>admin/addRole", //后台处理程序
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
	</script>