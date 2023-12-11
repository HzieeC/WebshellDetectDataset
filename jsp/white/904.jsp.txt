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
		<div class="title_c"><span>友情链接/合作商管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c">
			<div class="middle_cnt_cont_d">
		  		<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<a href="javascript:void(0)" onclick="deleteCoo()" class="button-1 button-add">-<fmt:message key="delete" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="<%=basePath%>admin/goAddCooperation?flag=${flag}" class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
					</ul> 
				</div>
			
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
			      <thead>
			          <tr>
			              <th width="5%"><input type="checkbox" name="chekall" id="chekall" value="${type.id}" class="ckAll"/></th>
			              <th width="20%" align="left"><fmt:message key="url" bundle="${messagesBundle}"/></th>
			              <th width="50%" align="left"><fmt:message key="www" bundle="${messagesBundle}"/></th>
			              <th width="25%" align="left"><fmt:message key="logo" bundle="${messagesBundle}"/></th>
			              
			          </tr>
			      </thead>
			      <tbody id="list_chckbox">
			      <form name="cooperationForm" id="cooperationForm">
				      <c:forEach items="${cooList}" var="coo" varStatus="status">
					      <tr>
					        <td><input type="checkbox" name="id" value="${coo.id}"></td>
					        <td align="left">${coo.url}</td>
					        <td align="left">${coo.name}</td>
					        <td align="left"><img src="<%=basePath%>${coo.logo}" width="200px" height="100px"/></td>
					      </tr>
					      
				      </c:forEach>
			      </form>
			      </tbody>
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
				<jsp:setProperty property="url" value="/admin/goCooperation?flag=${flag}" name="paging"/>
				${paging.pageString}
		     </ul>
		</div>
	</div>
	</fmt:bundle>
	<script>
		 //全选   
		 $('.ckAll').click(function(){
			$("#list_chckbox :checkbox").each(function(){
				$(this).prop("checked",!!$("#chekall").prop("checked"));
			});
		});
		function deleteCoo(){
			if(confirm("请确认是否删除此数据")){
				var params= $('#cooperationForm').serialize()
				$.ajax({
					url:"<%=basePath%>admin/deleteCooperation", //后台处理程序
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
	</script>