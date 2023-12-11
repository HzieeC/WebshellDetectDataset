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
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>系统幻灯片</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<a href="javascript:void(0)" onclick="deleteSlide()"  class="button-1 button-add">-<fmt:message key="delete" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="<%=basePath%>admin/goAddSlide?id="  class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
					</ul> 
				</div>
				<form name="slideForm" id="slideForm">
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
			      <thead>
			          <tr>
			              <th width="3%"><input type="checkbox" name="chekall" id="chekall" class="ckAll"/></th>
			              <th width="17%" align="left"><fmt:message key="preview" bundle="${messagesBundle}"/></th>
			              <th width="29%" align="left"><fmt:message key="title" bundle="${messagesBundle}"/></th>
			              <th width="17%" align="left">url</th>
			              <th width="17%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
			          </tr>
			      </thead>
			      <tbody id="list_chckbox">
			      <c:forEach items="${slideList}" var="slide" varStatus="status">
				      <tr>
				        <td><input type="checkbox"  name="slide_id" value="${slide.id}"></td>
				        <td><img width="100px" height="100px" src="<%=basePath%>${slide.img}"></td>
				        <td>${slide.title}</td>
				        <td>${slide.url}</td>
				        <td>
				        	<div class="tablehand">
					        	<a href="<%=basePath%>admin/goAddSlide?id=${slide.id}" class="red">
					        		<fmt:message key="modify" bundle="${messagesBundle}"/>
					        	</a>
				        	</div>
				        </td>
				      </tr>
			      </c:forEach>
			      </tbody>
			    </table>
		    	</form>
			</div>
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
		//删除幻灯片
			function deleteSlide(){
				if(confirm("请确认是否要删除此幻灯片")){
					var params= $('#slideForm').serialize();
			        $.ajax({
						 url:"<%=basePath%>admin/deleteSlide", //后台处理程序
						 type:'post',         //数据发送方式
						 dataType:'json',
						 data:params,         //要传递的数据
						 success:function(data){
						 	window.location.reload();
						 }
					}); 
				}
			}
			
	</script>