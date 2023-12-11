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
		        		<input type="hidden" name="order_id" value="${order_id}" />
						        <table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
						        	<tr>
										<td class="label_c" width="20%"><label>付款金额：</label></td>
										<td width="80%">
											<input type="text" value="" name="money" id="money" class="wid_80"/>
										</td>
									</tr>
							       	<tr>
										<td class="label_c"><label>付款方式：</label></td>
										<td>
											<select name="source" id="source">
									            	<option value="1">余额</option>
									            	<option value="2">微信支付</option>
									            	<option value="3">支付宝支付</option>
									            	<option value="4">现金支付</option>
									            	<option value="5">pos刷卡支付</option>
									            </select>
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
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="recevi()"/>
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
		
		function recevi(){
			var money=requree_double("money");
			if(!money){
             	layer.alert("请输入收款金额");
             }else{
             	var params= $('#orderForm').serialize()
				$.ajax({
						url:"<%=basePath%>admin/receivAbles", //后台处理程序
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