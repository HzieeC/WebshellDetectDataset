<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
<meta http-equiv="pragma" content="no-cache"/>
<meta http-equiv="cache-control" content="no-cache"/>
<meta http-equiv="expires" content="0"/>    
	<link rel="stylesheet" href="<%=basePath%>css/admin_lay.css" />
	<script src="<%=basePath%>js/jquery-1.11.2.min.js"></script>
	<script src="<%=basePath%>js/validate.js"></script>
	<script src="<%=basePath%>js/My97DatePicker/WdatePicker.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
				
					<form name="auntForm" id="auntForm">
		        		<input type="hidden" name="aunt_id" value="${aunt_id}" />
						
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
									<tr>
										<td class="label_c" width="35%"><label>员工状态：</label></td>
										<td width="65%">
											<select name="flag" id="flag">
									            	<option value="">--请选择--</option>
									            	<option value="1">已经签订合同</option>
									            	<option value="2">适合的</option>
									            	<option value="3">未联系</option>
									            	<option value="4">不适合</option>
									            </select>
										</td>
									</tr>
									<tr style="display:none">
										<td class="label_c"><label>合同开始时间：</label></td>
										<td>
											<input type="text" class="wid_60" onClick="WdatePicker()" name="con_stardate" id="con_stardate"  value="${aunt.con_stardate}"/>
										</td>
									</tr>
									<tr style="display:none">
										<td class="label_c"><label>合同结束时间：</label></td>
										<td>
											<input type="text" class="wid_60" onClick="WdatePicker()" name="con_enddate" id="con_enddate"  value="${aunt.con_enddate}"/>
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
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="add()"/>
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
		
		function add(){
			var params= $('#auntForm').serialize();
			   $.ajax({
				url:"<%=basePath%>admin/addAunt", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					alert(data.tip);
					parent.window.location.reload();
				}
			}); 
		}
		
		document.getElementById("flag").value="${aunt.flag}";
	</script>