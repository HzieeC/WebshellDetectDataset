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

	  <fmt:setLocale value="zh_CN"/>
	  <fmt:setBundle basename="messages" var="messagesBundle"/>
	  <fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
		
			<form name="auntForm" id="auntForm" action="javascript:void(0)">
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
			      <thead>
			          <tr>
			              <th width="25%">交易方向</th>
			              <th width="25%">交易来源</th>
			              <th width="15%">金额</th>
			              <th width="10%">状态</th>
			              <th width="25%">日期</th>
			          </tr>
			      </thead>
			      <c:forEach items="${recordList}" var="record" varStatus="status">
				      <tr>
				        <td>
				        	<c:if test="${record.direction==1}">
				        		充值
				        	</c:if>
				        	<c:if test="${record.direction==2}">
				        		消费
				        	</c:if>
				        </td>
				        <td width="12%">
				        	<c:if test="${record.source==1}">
				        		余额
				        	</c:if>
				        	<c:if test="${record.source==2}">
				        		微信
				        	</c:if>
				        	<c:if test="${record.source==3}">
				        		支付宝
				        	</c:if>
				        	<c:if test="${record.source==4}">
				        		现金
				        	</c:if>
				        	<c:if test="${record.source==5}">
				        		pos机
				        	</c:if>
				        </td>
				        <td>${record.money}</td>
				        <c:if test="${record.flag==1}">
				        	<td>正常</td>
				        </c:if>
				        <c:if test="${record.flag==2}">
				        	<td>退款</td>
				        </c:if>
				        <td>${record.year}-${record.month}-${record.day}</td>
				      </tr>
			      </c:forEach>
			    </table>
		    </form>
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