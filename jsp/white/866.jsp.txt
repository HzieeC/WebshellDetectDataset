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
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
				
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
									<tr>
										<td class="label_c" width="15%"><label>员工姓名：</label></td>
										<td class="cont_z" width="85%">
											${aunt.name}
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>身份证编号：</label></td>
										<td class="cont_z">
											${aunt.no}
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="tel" bundle="${messagesBundle}"/>：</label></td>
										<td class="cont_z">
											${aunt.tel}
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>生日：</label></td>
										<td class="cont_z">
											${aunt.birthday}
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>员工简述：</label></td>
										<td class="cont_z">
											${aunt.desc}
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>状态：</label></td>
										<td class="cont_z">
											<c:if test="${aunt.flag ==1}">
									        		已经签订合同
									        	</c:if>
									        	<c:if test="${aunt.flag ==2}">
									        		适合
									        	</c:if>
									        	<c:if test="${aunt.flag ==3}">
									        		未联系
									        	</c:if>
									        	<c:if test="${aunt.flag ==4}">
									        		不合适
									        	</c:if>
										</td>
									</tr>
								</table>
							
			</div>
		</div>
	</div>
	<!--本页主内容结束 -->
	</fmt:bundle>
	<script>
		$(function(){
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-32);
		})
	</script>