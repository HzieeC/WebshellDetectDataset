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
			
		        	<form name="customerForm" id="customerForm">
		        		<input type="hidden" name="item" value="${item}" />
				        
						        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
							        <tbody>
							        	<tr>
											<td class="label_c" width="20%"><label><fmt:message key="customer" bundle="${messagesBundle}"/><fmt:message key="type" bundle="${messagesBundle}"/>：</label></td>
											<td width="80%">
									            <select name="type" id="type">
									            	<option value="">--请选择--</option>
									            	<c:forEach items="${com.typeList}" var="type" varStatus="status">
									            		<option value="${type.name}">${type.name}</option>
													</c:forEach>
									            </select>
											</td>
										</tr>
								        <tr>
								            <td class="label_c"><label><fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
								            <td>
								            	<input type="text" name="name" id="name" class="wid_80"/>
								            </td>
								        </tr>
								        <tr>
								            <td class="label_c"><label><fmt:message key="contact" bundle="${messagesBundle}"/>：</label></td>
								            <td>
								            	<input type="text" name="person" id="person" class="wid_80"/>
								            </td>
								        </tr>
								        <tr>
								            <td class="label_c"><label><fmt:message key="tel" bundle="${messagesBundle}"/>：</label></td>
								            <td>
								            	<input type="text" name="tel"  value="" id="tel" class="wid_80"/>
								            </td>
								        </tr>
								        <tr>
								            <td class="label_c"><label><fmt:message key="qq" bundle="${messagesBundle}"/>：</label></td>
								            <td>
								            	<input type="text" name="qq"  value="" id="qq" class="wid_80"/>
								            </td>
								        </tr>
								        <tr>
								            <td class="label_c"><label><fmt:message key="like" bundle="${messagesBundle}"/><fmt:message key="the" bundle="${messagesBundle}"/><fmt:message key="product" bundle="${messagesBundle}"/>：</label></td>
								            <td>
									            <input type="text" name="product" id="product"  value="" class="wid_80"/>
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
			var title=requree_name("name") && requree_length("name",2,30);
			var person=requree_name("person") && requree_length("person",2,10);
			var tel=requree_iphone("tel");
			var qq=requree_integer("qq");
			
			if(type==""||type==null){
             	layer.alert("请设置客户类型");
            }else if(!title){
             	layer.alert("请填写客户名称，字数限制为2~30字");
            }else if(!person){
             	layer.alert("请填写联系人，字数限制为2~10字");
            }else if(!tel){
             	layer.alert("请填写手机号码");
            }else if(!qq){
             	layer.alert("请填写QQ号码");
            }else{
		        var params= $('#customerForm').serialize()
				$.ajax({
					url:"<%=basePath%>admin/addCustomer", //后台处理程序
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