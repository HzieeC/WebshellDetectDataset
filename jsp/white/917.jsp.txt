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
			<form name="auntForm" id="auntForm" action="<%=basePath%>admin/goCapitalTransactions" method="get">
				<input name="store_id" type="hidden" value="${store_id}"/>
				<input name="direction" type="hidden" value="${direction}"/>
				<div class="toolbar clear">
					<div class="c_pp_l">
						总金额：<span>${sum}</span>元
					</div>
					<div class="clear"></div>
					<ul class="handlist-left">
						<li>
							<input name="stratDate" type="text" value="${stratDate}" placeholder="开始时间" class="search_inputa" onClick="WdatePicker({dateFmt: 'yyyy-MM-dd'})"/>
							<input name="endDate" type="text" value="${endDate}" placeholder="结束时间" class="search_inputa" onClick="WdatePicker({dateFmt: 'yyyy-MM-dd'})"/>
						</li>
						<li>
							<select name="source" id="source">
								<option value="">--请选择--</option>
								<option value="1">余额</option>
								<option value="2">微信支付</option>
								<option value="3">支付宝支付</option>
								<option value="4">现金支付</option>
								<option value="5">pos刷卡支付</option>
							</select>
						</li>
						<li>
							<input type="submit"  value="<fmt:message key="search" bundle="${messagesBundle}"/>" class="button-4"/>
						</li>
					</ul>
					
				</div>
				
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
			      <thead>
			          <tr>
			              <th>交易方向</th>
			              <th>交易来源</th>
			              <th>金额</th>
			              <th>日期</th>
			              <th>操作方向</th>
			              <th>操作人</th>
			             
			          </tr>
			      </thead>
			      <c:forEach items="${moneyList}" var="record" varStatus="status">
				      <tr>
				        <td width="8%">
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
				        <td width="8%">${record.money}</td>
				        <td width="8%">${record.year}-${record.month}-${record.day}</td>
				        <td width="5%">
				        	<c:if test="${record.deriction==1}">
				        		管理员
				        	</c:if>
				        	<c:if test="${record.deriction==2}">
				        		门店
				        	</c:if>
				        </td>
				        <td width="5%">${record.admin}</td>
				      </tr>
			      </c:forEach>
			    </table>
		    </form>
		   </div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			  <ul class="page_num f_l">
					<jsp:useBean id="paging" scope="page" class="com.weishang.bean.Page"/>
					<jsp:setProperty property="user" value="admin" name="paging"/>
					<jsp:setProperty property="crrent" value="${pageNo}" name="paging"/>
					<jsp:setProperty property="suffix" value="" name="paging"/>
					<jsp:setProperty property="sumPage" value="${row}" name="paging"/>
					<jsp:setProperty property="url" value="/admin/goCapitalTransactions?store_id=${store_id}&direction=${direction}&stratDate=${stratDate}&endDate=${endDate}" name="paging"/>
					${paging.pageString}
			    </ul>
			</div>
		</div>
		<script>
		$(function(){
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-80);
			$(window).resize(function () {          //当浏览器大小变化时
			    var height = $(window).height();
				$(".middle_cnt_c2").height(height-80);
			});
		})
		</script>
	  </fmt:bundle>
	  
	 