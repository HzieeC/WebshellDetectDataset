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
		        	<form name="roleForm" id="roleForm">
		        		<input type="hidden" name="action" value="${action}" />
				        
						        <table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
						        	<tr>
										<td class="label_c" width="25%"><label>用户电话：</label></td>
										<td width="75%">
								            <input type="text" value="" name="tel" id="tel" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><c:if test="${action=='add'}">
								            	充值<fmt:message key="money" bundle="${messagesBundle}"/>：
								            </c:if>
								            <c:if test="${action=='reduce'}">
								            	扣除<fmt:message key="money" bundle="${messagesBundle}"/>：
								            </c:if></label></td>
										<td>
								            <input type="text" value="" name="money" id="money" class="wid_60"/>
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
			    	<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="updateMoney()"/>
			    </li>
		    </ul>	
		</div>
	</fmt:bundle>
	<script>
		$(function(){
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-80);
		})
		function updateMoney(){
			var tel=requree_iphone("tel");
			var money=requree_double("money");
			if(!tel){
             	layer.alert("请输入正确手机号");
            }else if(!money){
             	layer.alert("请输入金额数");
            }else{
            	if(confirm("请确认您的操作是否准确")){
			 		var params= $('#roleForm').serialize()
				    $.ajax({
						 url:"<%=basePath%>admin/updateUserMoney", //后台处理程序
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
		}
	</script>