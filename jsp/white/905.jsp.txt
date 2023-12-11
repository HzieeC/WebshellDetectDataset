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
	<script src="<%=basePath%>js/My97DatePicker/WdatePicker.js"></script>
	
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">

		        	<form name="adminForm" id="adminForm">
		        		<input type="hidden" name="coupon_id" value="${coupon_id}" />
		        		<input type="hidden" name="saveFlag" id="saveFlag" value="" />
				       
						        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
						        	<tr>
										<td class="label_c" width="15%"><label><fmt:message key="title" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
								            <input type="text" value="${coupon.title}" name="title" id="title" class="wid_80"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="money" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <input type="text" value="${coupon.price}" name="money" id="money" class="wid_80"/>
										</td>
									</tr>
							        <tr>
										<td class="label_c"><label><fmt:message key="range" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <select name="range" id="range">
									            	<option value="">--选择使用范围--</option>
									            	<c:forEach items="${goodsType}" var="type" varStatus="status">
									            		<option value="${type.id }">${type.name}</option>
									            	</c:forEach>
									            </select>
										</td>
									</tr>
					       			<tr>
										<td class="label_c"><label><fmt:message key="sentences25" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <select name="guoqi" id="guoqi">
									            	<option value="2">根据过期时间过期</option>
									            </select>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="end" bundle="${messagesBundle}"/><fmt:message key="time" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <input type="text" name="endTime" id="endTime"  value="${coupon.enddate}" onClick="WdatePicker({dateFmt: 'yyyy-MM-dd'})" class="wid_80"/>
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
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="add(1)"/>
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
		
		function add(saveFlag){
			 var title=requree_name("title") && requree_length("title",2,80);
			 var money=requree_double("money");
			 var range=document.getElementById("range").value;
			 var guoqi=document.getElementById("guoqi").value;
			 var endTime=requree_name("endTime");
			 
             if(!title){
             	layer.alert("请输入优惠券名称");
             }else if(!money){
             	layer.alert("请正确输入金额");
             }else if(range==""||range==null){
             	layer.alert("请设置优惠券使用范围！");
             }else if(guoqi==""||guoqi==null){
             	layer.alert("请设置结束方式！");
             }else if(!endTime){
             	layer.alert("请设置结束时间！");
             }else{
             	document.getElementById("saveFlag").value=saveFlag;
             	var params= $('#adminForm').serialize();
			    $.ajax({
					 url:"<%=basePath%>admin/addCoupon", //后台处理程序
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
		document.getElementById("range").value="${coupon.service_id}";
		
	</script>