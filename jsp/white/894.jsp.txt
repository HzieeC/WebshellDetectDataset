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
		<div class="title_c"><span>门店员工管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<a href="javascript:void(0)" onclick="javascript:deleteEmployee()" class="button-1 button-add">-<fmt:message key="delete" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="<%=basePath%>/admin/goAddEmployee"  class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
					</ul> 
				</div>
				<form name="brandForm" id="brandForm" action="javascript:void(0)">
					<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
				      <thead>
				          <tr>
				              <th width="5%"><input type="checkbox" name="chekall" id="chekall" class="ckAll" value="${type.id}"/></th>
				              <th width="10%"><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="name" bundle="${messagesBundle}"/></th>
				              <th width="20%"><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="shenfen" bundle="${messagesBundle}"/></th>
				              <th width="15%"><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="no" bundle="${messagesBundle}"/></th>
				              <th width="12%"><fmt:message key="tel" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="grading" bundle="${messagesBundle}"/></th>
				              <th width="12%"><fmt:message key="employee" bundle="${messagesBundle}"/><fmt:message key="store" bundle="${messagesBundle}"/></th>
				              <th width="16%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
				          </tr>
				      </thead>
				      <tbody id="list_chckbox">
				      <c:forEach items="${employeeList}" var="employee" varStatus="status">
					      <tr>
					      	<td><input type="checkbox" name="employee_id" value="${employee.id}"/></td>
					        <td>${employee.name}</td>
					        <td>${employee.shenfen }</td>
					        <td>${employee.no}</td>
					        <td>${employee.tel}</td>
					        <c:if test="${employee.flag==0}">
					        	<td>销售</td>
					        </c:if>
					        <c:if test="${employee.flag==1}">
					        	<td>店长</td>
					        </c:if>
					        <td>${employee.store_name}</td>
					        <td>
					        	<div class="tablehand">
					        		<a href="<%=basePath%>admin/goAddEmployee?employee_id=${employee.id}" class="red">
					        			<fmt:message key="modify" bundle="${messagesBundle}"/>
					        		</a>
					        		 <c:if test="${employee.flag==1}">
					        		 	<a onclick="distribute('<%=basePath%>admin/goDistructData?employee_id=${employee.id}')" href="javascript:void(0)" class="green">
						        			<fmt:message key="distribut" bundle="${messagesBundle}"/><fmt:message key="data" bundle="${messagesBundle}"/>
						        		</a>
					        		 </c:if>
					        	</div>
					        </td>
					      </tr>
				      </c:forEach>
				      </tbody>
				    </table>
			    </form>
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
				<jsp:setProperty property="url" value="/admin/goEmployee" name="paging"/>
				${paging.pageString}
		     </ul>
		</div>
	</div>
		
	  </fmt:bundle>
	  <script language="javascript">
	  	//全选   
		 $('.ckAll').click(function(){
			$("#list_chckbox :checkbox").each(function(){
				$(this).prop("checked",!!$("#chekall").prop("checked"));
			});
		});
		
	  	function distribute(url){
	  		layer.open({
				  type: 2,
				  title: '分配数据',
				  shadeClose: true,
				  shade: 0.8,
				  area: ['600px', '400px'],
				  content:'url'
			});
	  	}
		
		function deleteEmployee(){
			var params= $('#brandForm').serialize()
			if(confirm("请确认是否删除此数据")){
				 $.ajax({
					url:"<%=basePath%>admin/deleteEmployee", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:params,         //要传递的数据
					success:function(data){
						layer.alert(data.tip);
						window.location.reload();
					}
				});
			}
		}
	  </script>