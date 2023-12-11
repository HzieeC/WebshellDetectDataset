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
		        	<form name="orderForm" id="orderForm">
		        		<input type="hidden" name="order_id" value="${order_id}" />
		        		<input type="hidden" name="sure_sum" id="sure_sum" value="${sum}" />
				        
						        <table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
						        	<tr>
										<td class="label_c" width="20%"><label>已收金额：</label></td>
										<td width="80%">
											${sum}
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>实收金额：</label></td>
										<td>
											<input type="text" value="" name="money" id="money" class="wid_80"/>
										</td>
									</tr>
							       <tr>
										<td class="label_c"><label>是否付款：</label></td>
										<td>
											<select name="payFlag" id="payFlag">
									            	<option value="1">是</option>
									            	<option value="2">否</option>
									            </select>
										</td>
									</tr>
					        	</table>
				        <input type="hidden" value="" name="orderFalg" id="orderFalg"/>
			        </form>
		        </div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<li>
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="endOrder(3)"/>
				</li>
				<li>
			    	<input type="submit" value="异常结束" class="button-2 vcenter" onclick="endOrder(7)"/>
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
		
		function endOrder(flag){
			var sure_sum=$("#sure_sum").val();
			var money=$("#money").val();
			if(sure_sum!=money){
				layer.alert('操作错误，您填写的“实收金额”和“已收金额”不相等');
			}else{
				layer.confirm('请再次确认此订单是否马上结束？', {
				    btn: ['我确定信息无误','我不敢确定'] //按钮
				}, function(){
					document.getElementById("orderFalg").value=flag;
			  		var params= $('#orderForm').serialize()
					$.ajax({
							url:"<%=basePath%>admin/endOrder", //后台处理程序
							type:'post',         //数据发送方式
							dataType:'json',
							data:params,         //要传递的数据
							success:function(data){
								alert(data.tip);
								parent.window.location.reload();
							}
					});
				},function(){
				    layer.msg('那请您在好好检查一下', {
				        time: 5000, //5s后自动关闭
				    });
				});
			}
	  	}

	</script>