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
		<div class="title_c"><span>属性管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild"> 
							<a href="javascript:void(0)" onclick="javascript:deleteGoodsAttribute()" class="button-1 button-add">-<fmt:message key="delete" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="javascript:void(0)" onclick="javascript:addGoodsAttribute(${type.id})"  class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
					</ul> 
				</div>
				<form name="attributeForm" id="attributeForm" action="javascript:void(0)">
					<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
				      <thead>
				          <tr>
				              <th width="5%"><input type="checkbox" name="chekall" id="chekall" class="ckAll" /></th>
				              <th width="15%"><fmt:message key="name" bundle="${messagesBundle}"/></th>
				              <th width="30%"><fmt:message key="type" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="input" bundle="${messagesBundle}"/><fmt:message key="theway" bundle="${messagesBundle}"/></th>
				              <th width="20%"><fmt:message key="sentences22" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="order" bundle="${messagesBundle}"/></th>
				              <th width="10%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
				          </tr>
				      </thead>
				      <tbody id="list_chckbox">
				      <c:forEach items="${attributeList}" var="attribute" varStatus="status">
					      <tr>
					      	<td><input type="checkbox" name="attribute_id" value="${attribute.id}"/></td>
					        <td>${attribute.name}</td>
					        <td>${type.name}</td>
					        <td>${attribute.input_type}</td>
					        <td>${attribute.values}</td>
					        <td>${attribute.order}</td>
					        <td>
					        	<div class="tablehand">
					        		<a href="javascript:void(0)" onclick="javascript:updateGoodsAttribute(${type.id},${attribute.id})" class="red"><fmt:message key="modify" bundle="${messagesBundle}"/></a>
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
				<jsp:setProperty property="url" value="/admin/goGoodsTypeAttribute?type_id=${type_id}" name="paging"/>
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
	  	function addGoodsAttribute(type_id){
	  		layer.open({
				  type: 2,
				  title: '添加属性',
				  shadeClose: true,
				  shade: 0.8,
				  area: ['750px', '400px'],
				  content: '<%=basePath%>admin/goAddGoodsAttribute?type_id='+type_id,
				end:function(index){
					window.location.reload();
				}
			});
	  	}
	  	
	  	function updateGoodsAttribute(type_id,attribute_id){
	  		layer.open({
				  type: 2,
				  title: '修改属性',
				  shadeClose: true,
				  shade: 0.8,
				  area: ['750px', '400px'],
				  content: '<%=basePath%>admin/goAddGoodsAttribute?type_id='+type_id+'&attribute_id='+attribute_id+'',
				end:function(index){
					window.location.reload();
				}
			});
			
	  	}
	  	
		
		function deleteGoodsAttribute(){
			var params= $('#attributeForm').serialize()
			if(confirm("请确认是否删除此数据")){
				 $.ajax({
					url:"<%=basePath%>admin/deleteGoodsAttribute", //后台处理程序
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