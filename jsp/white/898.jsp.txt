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
	  <div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
		        	<form name="orderForm" id="orderForm">
				        
						        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
							        <tbody>
								        <tr>
								            <td class="label_c" width="20%"><label>门店：</label></td>
								            <td width="80%">
								            	<select name="store" id="store" onblur="getEmployeeBySotre()">
								            		<c:forEach items="${storeList}" var="store" varStatus="status">
									            		<option value="${store.id}">${store.name}</option>
									            	</c:forEach>
									            </select>
								            </td>
								            <tr>
												<td class="label_c"><label>销售：</label></td>
												<td>
										            <select name="employee" id="employee">
									            	
									            	</select>
												</td>
											</tr>
								        </tr>
							        </tbody>
						        </table>
					        
				        <input name="order_id" value="${order_id}" type="hidden"/>
			        </form>
		       </div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
			    <li>
			    	<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="distribute()"/>
			    </li>
		    </ul>	
		</div>
	</div>
	<script>
	     
	        
	        //根据门店id获取雇员信息
	        function getEmployeeBySotre(){
	        	var store_id=$("#store").val();
	        	$.ajax({
					url:"<%=basePath%>admin/myAdmin?tag=goEmployeeByStore", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:{store_id:store_id},         //要传递的数据
					success:function(data){
						 $("#employee").empty();
						 $("#employee").append(""+data.tip+"");
					}
				});
	        }

			function distribute(){
				var params= $('#orderForm').serialize();
			 	$.ajax({
					 url:"<%=basePath%>admin/myAdmin?tag=distributGoodsOrder", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
					 	alert(data.tip);
					 	parent.window.location.reload();
						//$("#order_attr").html(data.tip);
					 }
				}); 
			}
			//设置高度
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-80);
			$(window).resize(function () {          //当浏览器大小变化时
				var height = $(window).height();
				$(".middle_cnt_c2").height(height-80);
			});
	    </script>
	</fmt:bundle>
<!--end right-->