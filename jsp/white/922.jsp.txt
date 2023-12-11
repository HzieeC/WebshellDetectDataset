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
		<div class="title_c"><span>内容信息管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<a href="javascript:void(0)" onclick="javascript:deleteCms()" class="button-1 button-add">-<fmt:message key="delete" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="<%=basePath%>admin/goAddCms?modId=${modId}"  class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
					</ul> 
				</div>
				<form name="cmsForm" id="cmsForm" action="javascript:void(0)">
					<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
				      <thead>
				          <tr>
				              <th width="5%"><input type="checkbox" name="chekall" id="chekall" class="ckAll"/></th>
				              <th align="left" width="45%"><fmt:message key="title" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="upload" bundle="${messagesBundle}"/><fmt:message key="person" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="source" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="recommend" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="upload" bundle="${messagesBundle}"/><fmt:message key="time" bundle="${messagesBundle}"/></th>
				              <th width="10%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
				          </tr>
				      </thead>
				      <tbody id="list_chckbox">
				      <c:forEach items="${cmsList}" var="cms" varStatus="status">
					      <tr>
					      	<td><input type="checkbox" name="cms_id" value="${cms.id}"></td>
					        <td align="left" style="${cms.style}">${cms.name}</td>
					        <td>${cms.person}</td>
					        <td>${cms.suroce}</td>
					        <td>
						        <c:if test="${cms.flag==0}">
						        	普通
						        </c:if>
					         	<c:if test="${cms.flag==1}">
						        	推荐
						        </c:if>
					        	<c:if test="${cms.flag==2}">
						        	热文
						        </c:if>
					        </td>
					        <td>${cms.time}</td>      
					        <td>
					        	<div class="tablehand">
					        		<a href="<%=basePath%>admin/goAddCms?id=${cms.id}&modId=${modId}" class="red"><fmt:message key="modify" bundle="${messagesBundle}"/></a>&nbsp;
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
				<jsp:setProperty property="url" value="/admin/modularCms?id=${modId}" name="paging"/>
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
		
		function deleteCms(){
			var params= $('#cmsForm').serialize()
			if(confirm("请确认是否删除此数据")){
				 $.ajax({
					url:"<%=basePath%>admin/deleteCms", //后台处理程序
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