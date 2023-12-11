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
		        	<form name="signForm" id="signForm">
		        		<input type="hidden" name="cid" value="${cid}" />
					        
						        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
							        <tbody>
								        <tr>
								            <td class="label_c" width="20%"><label><fmt:message key="price" bundle="${messagesBundle}"/>：</label></td>
											<td width="80%">
									            <input type="text" value="" name="price" class="wid_80"/>
											</td>
								        </tr>
								        <tr>
								            <td class="label_c"><label><fmt:message key="sign" bundle="${messagesBundle}"/><fmt:message key="desc" bundle="${messagesBundle}"/>：</label></td>
											<td>
									            <input type="text" value="" name="content" class="wid_80"/>
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
			    	<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="sign()"/>
			    </li>
		    </ul>	
		</div>
	</div>
	</fmt:bundle>
	<script>
		function sign(){
			var price=requree_double("price");
			var content=requree_name("content") && requree_length("content",2,80);
			if(!price){
             	layer.alert("请输入正确价格！数字和小数点组成");
            }else if(!content){
             	layer.alert("请填写描述，字数限制为2~80字");
            }else{
			    var params= $('#signForm').serialize()
			    $.ajax({
					 url:"<%=basePath%>admin/signAction", //后台处理程序
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