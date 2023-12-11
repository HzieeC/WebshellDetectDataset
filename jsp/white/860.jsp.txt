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
			<form name="auntForm" id="auntForm" action="javascript:void(0)">
				<input name="order_id" value="${order_id}" type="hidden"/>
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
			      <thead>
			          <tr>
			              <th><input type="checkbox" name="chekall" value="${type.id}"/></th>
			              <th><fmt:message key="aunt" bundle="${messagesBundle}"/><fmt:message key="name" bundle="${messagesBundle}"/></th>
			              <th><fmt:message key="shenfen" bundle="${messagesBundle}"/><fmt:message key="no" bundle="${messagesBundle}"/></th>
			              <th><fmt:message key="tel" bundle="${messagesBundle}"/></th>
			              <th>状态</th>
			              <th>合同开始时间</th>
			              <th>合同结束时间</th>
			          </tr>
			      </thead>
			      <c:forEach items="${auntList}" var="aunt" varStatus="status">
				      <c:if test="${aunt.flag ==1}">
				      	<tr style="color:green">
				      </c:if>
				      <c:if test="${aunt.flag ==2}">
				        	<tr style="color:blue">
				      </c:if>
				      <c:if test="${aunt.flag ==3}">
				        	<tr>
				      </c:if>
				      <c:if test="${aunt.flag ==4}">
				        	<tr style="color:red">
				      </c:if>
				      	<td><input type="checkbox" name="aunt_id" value="${aunt.id}"/></td>
				        <td>${aunt.name}</td>
				        <td>${aunt.no}</td>
				        <td>${aunt.tel}</td>
				        <td>
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
				        <td>
					    	 ${aunt.con_stardate}   
				        </td>
				        <td>
					    	 ${aunt.con_enddate}           
				        </td>
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
				<jsp:setProperty property="sumPage" value="${sum}" name="paging"/>
				<jsp:setProperty property="url" value="/amin/goDistributeAunt?order_id=${order_id }" name="paging"/>
				${paging.pageString}
		     </ul>
		  <button class="f_r" onclick="distributeAunt()">提交</button>
		</div>
	</div>
	  </fmt:bundle>
	  <script language="javascript">
	  	var height = $(window).height();
		$(".middle_cnt_c2").height(height-80);
		$(window).resize(function () {          //当浏览器大小变化时
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-80);
		});
		
		function distributeAunt(){
			var params= $('#auntForm').serialize()
			$.ajax({
					url:"<%=basePath%>admin/distributeAunt", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:params,         //要传递的数据
					success:function(data){
						alert(data.tip);
						parent.window.location.reload();
					}
			});
		}
	  </script>