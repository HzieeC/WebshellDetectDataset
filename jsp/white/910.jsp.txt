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
		        	<form name="examineForm" id="examineForm">
		        		<input type="hidden" name="cid" value="${cid}" />
		        		<input type="hidden" name="aid" value="${aid}" />
		        		<input type="hidden" name="endTiem" value="${endTiem}" />
		        		
						        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
							        <tbody>
								        <tr>
								        	<td class="label_c" width="20%"><label><fmt:message key="conclusion" bundle="${messagesBundle}"/>：</label></td>
											<td width="80%">
									            <select name="conclusion" id="conclusion">
									            	<option value="">--请选择--</option>
									            	<c:forEach items="${com.conclusionList}" var="con" varStatus="status">
									            		<option value="${con.id}">${con.name}</option>
													</c:forEach>
									            </select>
											</td>
								        </tr>
								        <tr>
								        	<td class="label_c"><label><fmt:message key="delay" bundle="${messagesBundle}"/><fmt:message key="time" bundle="${messagesBundle}"/>：</label></td>
											<td>
									            <select name="days" id="days">
									            	<option value="">--请选择--</option>
									            	<c:forEach items="${com.delayList}" var="day" varStatus="status">
									            		<option value="${day.id}">${day.name}</option>
													</c:forEach>
									            </select>
											</td>
								        </tr>
								        <tr>
								        	<td class="label_c"><label><fmt:message key="conclusion" bundle="${messagesBundle}"/>：</label></td>
											<td>
									            <input type="text" name="content"  value="" class="wid_80"/>
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
			    	<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="examine()"/>
			    </li>
		    </ul>	
		</div>
	</div>
	</fmt:bundle>
	<script>
		function examine(){
			var conclusion=document.getElementById("conclusion").value;
			var days=document.getElementById("days").value;
			var content=requree_name("content") && requree_length("content",2,80);
			
			if(conclusion==""||conclusion==null){
             	layer.alert("请选择结论");
            }else if(days==""||days==null){
             	layer.alert("请选择延期时间");
            }else if(!content){
             	layer.alert("请填写结论内容，字数限制为2~80字");
            }else{
		    	var params= $('#examineForm').serialize()
		        $.ajax({
				 url:"<%=basePath%>admin/examine", //后台处理程序
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