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
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<jsp:useBean id="com" scope="page" class="com.weishang.bean.Common"/>
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
		        	<form name="messageForm" id="messageForm">
		        		<input type="hidden" name="cid" value="${cid}" />
						        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
							        <tbody>
							        	<tr>
											<td class="label_c" width="20%"><label><fmt:message key="links" bundle="${messagesBundle}"/><fmt:message key="theway" bundle="${messagesBundle}"/>：</label></td>
											<td width="80%">
									            <select name="type" id="type">
									            	<option value="">--请选择--</option>
									            	<c:forEach items="${com.conList}" var="con" varStatus="status">
									            		<option value="${con.name}">${con.name}</option>
													</c:forEach>
									            </select>
											</td>
										</tr>
										<tr>
								            <td class="label_c"><label><fmt:message key="customer" bundle="${messagesBundle}"/><fmt:message key="state" bundle="${messagesBundle}"/>：</label></td>
								            <td>
								            	<select name="state" id="state">
									            	<option value="">--请选择--</option>
									            	<c:forEach items="${com.stateList}" var="state" varStatus="status">
									            		<option value="${state.id}">${state.name}</option>
													</c:forEach>
									            </select>
								            </td>
								        </tr>
								        <tr>
								            <td class="label_c"><label><fmt:message key="conclusion" bundle="${messagesBundle}"/>：</label></td>
								            <td>
								            	<input type="text" value="" name="contact" id="contact" class="wid_80"/>
								            </td>
								        </tr>
									</tbody>
									
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
			var type=document.getElementById("type").value;
			var state=document.getElementById("state").value;
			var contact=requree_name("contact") && requree_length("contact",2,80);
			
            if(type==""||type==null){
             	layer.alert("请选择联系方式");
            }else if(state==""||state==null){
             	layer.alert("请选择客户状态");
            }else if(!contact){
             	layer.alert("请填写结论，字数限制为2~80字");
            }else{
             	var params= $('#messageForm').serialize()
		        $.ajax({
					 url:"<%=basePath%>admin/addMessage", //后台处理程序
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