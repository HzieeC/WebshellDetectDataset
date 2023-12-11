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
		<div class="title_c"><span>品牌管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<a href="javascript:void(0)" onclick="javascript:deleteGoodsBrand()" class="button-1 button-add">-<fmt:message key="delete" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="<%=basePath%>/admin/goAddGoodsBrand"  class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
					</ul> 
				</div>
				<form name="brandForm" id="brandForm" action="javascript:void(0)">
					<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
				      <thead>
				          <tr>
				              <th width="5%"><input type="checkbox" name="chekall" id="chekall" class="ckAll" value="${type.id}"/></th>
				              <th width="15%"><fmt:message key="name" bundle="${messagesBundle}"/></th>
				              <th width="30%"><fmt:message key="img" bundle="${messagesBundle}"/></th>
				              <th width="15%"><fmt:message key="net" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="order" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="sentences16" bundle="${messagesBundle}"/></th>
				              <th width="20%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
				          </tr>
				      </thead>
				      <tbody id="list_chckbox">
				      <c:forEach items="${brandList}" var="brand" varStatus="status">
					      <tr>
					      	<td><input type="checkbox" name="brand_id" value="${brand.id}"/></td>
					        <td>${brand.name}</td>
					        <td><img src="<%=basePath%>${brand.logo}" width="100px" height="100px"/></td>
					        <td>${brand.url}</td>
					        <td>${brand.order}</td>
					        <td>
					        	<c:if test="${brand.is_show==1}">显示</c:if>
					        	<c:if test="${brand.is_show!=1}">不显示</c:if>
					        </td>
					        <td>
					        	<div class="tablehand">
					        		<a href="<%=basePath%>/admin/goAddGoodsBrand?brand_id=${brand.id}" onclick="" class="red"><fmt:message key="modify" bundle="${messagesBundle}"/></a>
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
				<jsp:setProperty property="url" value="/admin/goGoodsBrand" name="paging"/>
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
		
		
		function deleteGoodsBrand(){
			var params= $('#brandForm').serialize()
			if(confirm("请确认是否删除此数据")){
				 $.ajax({
					url:"<%=basePath%>admin/deleteGoodsBrand", //后台处理程序
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