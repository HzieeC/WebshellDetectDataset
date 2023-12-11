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
	<script src="<%=basePath%>js/public.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>门店信息</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c">
		        	<form name="storeForm" id="storeForm">
		        		<input type="hidden" name="store_id" value="${store_id}" />
				        <ul>
							<li>
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
									<tr>
										<td class="label_c" width="15%"><label><fmt:message key="store" bundle="${messagesBundle}"/><fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<input type="text" value="${store.name}" name="name" id="name" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="store" bundle="${messagesBundle}"/><fmt:message key="address" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${store.address}" name="address" id="address" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="store" bundle="${messagesBundle}"/><fmt:message key="tel" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${store.tel}" name="tel" id="tel" class="wid_60" placeholder="座机填写方式如：02885980903"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="store" bundle="${messagesBundle}"/><fmt:message key="state" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<select name="state" id="state">
									            	<option value="1">运行</option>
									            	<option value="2">关闭</option>	
									            </select>
										</td>
									</tr>
								</table>
					        </li>
				        </ul>
			        </form>
			     </div>
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<li>
			    	<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="add()"/>
			    </li>
			     <li>
			    	<input type="button" onclick="back()" value="<fmt:message key="back" bundle="${messagesBundle}"/>" class="button-2 vcenter"/>
			    </li>
			</ul>	
		</div>
		
	</div>
	<!--本页主内容结束 -->

		
	</fmt:bundle>
	<script>
		function add(){
			 var title=requree_name("name") && requree_length("name",2,80);
			 var address=requree_name("address") && requree_length("address",2,80);
			 var tel=requree_iphone("tel");
			 var state=document.getElementById("state").value;
			 
             if(!title){
             	layer.alert("门店名称为必填项！");
             }else if(!address){
             	layer.alert("请填写门店详细地址");
             }else if(!tel){
             	layer.alert("请填写电话号码");
             }else if(state==""||state==null){
             	layer.alert("请设置门店状态！");
             }else{
             	var params= $('#storeForm').serialize();
			    $.ajax({
					 url:"<%=basePath%>admin/addStore", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
						 alert(data.tip);
						 window.location.reload();
					 }
				}); 
             }
		    
		}
		
		function back(){
			window.location.href="<%=basePath%>admin/goStore?id=65"		
		}
		
		document.getElementById("state").value="${store.flag}";
	</script>