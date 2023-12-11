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
	<script src="<%=basePath%>js/public.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	  <fmt:setLocale value="zh_CN"/>
	  <fmt:setBundle basename="messages" var="messagesBundle"/>
	  <fmt:bundle basename="messagesAllMessage">
	  <div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>废弃库</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
			<div class="toolbar clear">
				<ul class="handlist-left">
					<li>
						<input type="text" placeholder="<fmt:message key="tel" bundle="${messagesBundle}"/>" value="" class="search_inputa" />
					</li>
					<li>
						<input type="button" value="<fmt:message key="retrieval" bundle="${messagesBundle}"/>"class="button-4"/>
					</li>
					<li class="rightchild">
						<a href="javascript:void(0)" onclick="add()"  class="button-2 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
					</li>
				</ul> 
			</div>
			<table border="0" cellspacing="0" cellpadding="0" class="tablelist">
		      <thead>
		          <tr>
		              <th><fmt:message key="customer" bundle="${messagesBundle}"/><fmt:message key="name" bundle="${messagesBundle}"/></th>
		               <th><fmt:message key="contact" bundle="${messagesBundle}"/></th>
		              <th><fmt:message key="tel" bundle="${messagesBundle}"/></th>
		              <th><fmt:message key="qq" bundle="${messagesBundle}"/></th>
		              <th><fmt:message key="like" bundle="${messagesBundle}"/><fmt:message key="the" bundle="${messagesBundle}"/><fmt:message key="product" bundle="${messagesBundle}"/></th>
		              <th><fmt:message key="state" bundle="${messagesBundle}"/></th>
		              
		              <th><fmt:message key="giveup" bundle="${messagesBundle}"/><fmt:message key="person" bundle="${messagesBundle}"/></th>
		              <th><fmt:message key="giveup" bundle="${messagesBundle}"/><fmt:message key="time" bundle="${messagesBundle}"/></th>
		              
		              <th><center><fmt:message key="links" bundle="${messagesBundle}"/><fmt:message key="record" bundle="${messagesBundle}"/></center></th>
		          </tr>
		      </thead>
		      <c:forEach items="${cusList}" var="cus" varStatus="status">
			      <tr>
			        <td>${cus.name}</td>
			         <td>${cus.person}</td>
			        <td>${cus.tel}</td>
			        <td>${cus.qq} </td>
			        <td>${cus.product}</td>
			        
			        <td>
			        		<jsp:useBean id="admin" scope="page" class="com.weishang.bean.Admin"/>
			        		<jsp:setProperty property="state" value="${cus.state}" name="admin"/>
			        		${admin.stateString}
			        </td>
			        <td>${cus.user3} </td>
			        <td>${cus.time3}</td>
			        <td>
			        	<div class="tablehand">
			        		<a href="javascript:void(0)" onclick="look(${cus.id})" class="green"><fmt:message key="look" bundle="${messagesBundle}"/></a>&nbsp;
			        		<a href="javascript:void(0)" onclick="link_add_message(${cus.id})" class="orang"><fmt:message key="add" bundle="${messagesBundle}"/></a>&nbsp;
			        	</div>
			        </td>
			      </tr>
		      </c:forEach>
		    </table>
			</div>
		</div>
		
		<!--分页 -->
		<div class="right_bottom_btnlist">
		  <ul class="page_num f_l">
				<jsp:useBean id="paging" scope="page" class="com.weishang.bean.Page"/>
				<jsp:setProperty property="user" value="admin" name="paging"/>
				<jsp:setProperty property="crrent" value="${pageNo}" name="paging"/>
				<jsp:setProperty property="suffix" value="" name="paging"/>
				<jsp:setProperty property="sumPage" value="${sum}" name="paging"/>
				<jsp:setProperty property="url" value="/admin/goGiveUpByUser" name="paging"/>
				${paging.pageString}
		     </ul>
		</div>
	</div>
	
	  </fmt:bundle>
	  <script language="javascript">
	  	function add(){
			layer.open({
		  		type: 2,
				title: "添加客户",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['600px', '400px'],
				content: '<%=basePath%>admin/goAddCustomer',
				end:function(index){
					window.location.reload();
				}
		  	});
		}
		function look(id){
			layer.open({
		  		type: 2,
				title: "查看",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['600px', '400px'],
				content: '<%=basePath%>admin/goLookMessage?cid='+id+''
		  	});
		}
		function link_add_message(id){
			layer.open({
		  		type: 2,
				title: "添加联系记录",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['600px', '400px'],
				content: '<%=basePath%>admin/goAddMessage?cid='+id+'',
				end:function(index){
					window.location.reload();
				}
		  	});
		}
	  </script>