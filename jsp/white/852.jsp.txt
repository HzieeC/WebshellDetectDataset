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
		<div class="title_c"><span>商品管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<a href="javascript:void(0)" onclick="javascript:deleteGoods()" class="button-1">-<fmt:message key="delete" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="<%=basePath%>/admin/goAddGoods" class="button-1">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
					</ul> 
				</div>
				<form name="goodsForm" id="goodsForm" action="javascript:void(0)">
					<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
				      <thead>
				          <tr>
				              <th width="3%"><input type="checkbox" name="chekall" id="chekall" class="ckAll" value="${type.id}"/></th>
				              <th width="5%"><fmt:message key="no" bundle="${messagesBundle}"/></th>
				              <th width="15%"><fmt:message key="name" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="goodssn" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="price" bundle="${messagesBundle}"/></th>
				              <th width="8%"><fmt:message key="is_shelves" bundle="${messagesBundle}"/></th>
				              <th width="8%"><fmt:message key="is_best" bundle="${messagesBundle}"/></th>
				              <th width="8%"><fmt:message key="is_new" bundle="${messagesBundle}"/></th>
				              <th width="8%"><fmt:message key="is_hot" bundle="${messagesBundle}"/></th>
				              <th width="8%"><fmt:message key="recommend" bundle="${messagesBundle}"/><fmt:message key="order" bundle="${messagesBundle}"/></th>
				              <th width="8%"><fmt:message key="stock" bundle="${messagesBundle}"/></th>
				              <th width="9%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
				          </tr>
				      </thead>
				      <tbody id="list_chckbox">
				      <c:forEach items="${goodsList}" var="goods" varStatus="status">
					      <tr>
					      	<td><input autocomplete="off" type="checkbox" name="goods_id" value="${goods.goodsId}"/></td>
					        <td>${goods.goodsSn}</td>
					        <td>${goods.goodsName}</td>
					        <td>${goods.goodsSn}</td>
					        <td>${goods.shopPrice}（元）</td>
					        <c:if test="${goods.isShelves==1}">
					        	<td><img onclick="updateShelves(${goods.goodsId},0)" src="<%=basePath%>images/yes.gif"/></td>
					        </c:if>
					        <c:if test="${goods.isShelves!=1}">
					        	<td><img onclick="updateShelves(${goods.goodsId},1)" src="<%=basePath%>images/no.gif"/></td>
					        </c:if>
					        <c:if test="${goods.isBest==1}">
					        	<td><img onclick="updateBest(${goods.goodsId},0)" src="<%=basePath%>images/yes.gif"/></td>
					        </c:if>
					        <c:if test="${goods.isBest!=1}">
					        	<td><img onclick="updateBest(${goods.goodsId},1)" src="<%=basePath%>images/no.gif"/></td>
					        </c:if>
					        <c:if test="${goods.isNew==1}">
					        	<td><img onclick="updateNew(${goods.goodsId},0)" src="<%=basePath%>images/yes.gif"/></td>
					        </c:if>
					        <c:if test="${goods.isNew!=1}">
					        	<td><img onclick="updateNew(${goods.goodsId},1)" src="<%=basePath%>images/no.gif"/></td>
					        </c:if>
					        <c:if test="${goods.isHot==1}">
					        	<td><img onclick="updateHot(${goods.goodsId},0)" src="<%=basePath%>images/yes.gif"/></td>
					        </c:if>
					        <c:if test="${goods.isHot!=1}">
					        	<td><img onclick="updateHot(${goods.goodsId},1)" src="<%=basePath%>images/no.gif"/></td>
					        </c:if>
					        <td>${goods.sortOrder}</td>
					        <td>${goods.goodsNumber}（${goods.category.measure_unit}）</td>
					        <td>
					        	<div class="tablehand">
					        		<a href="<%=basePath%>/admin/myAdmin?tag=goUpdateGoods&id=${goods.goodsId}" class="red"><fmt:message key="modify" bundle="${messagesBundle}"/></a>
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
				<jsp:setProperty property="url" value="/admin/goGoodsList" name="paging"/>
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
		
	  	function addGoodsType(){
	  		document.getElementById("modfy").src='<%=basePath%>admin/goAddGoodsType';
			$('.theme-popover-mask').fadeIn(100);
			$('.theme-popover').slideDown(200);
	  	}
	  	
	  	function updateGoodsType(type_id){
	  		document.getElementById("modfy").src='<%=basePath%>admin/goAddGoodsType?type_id='+type_id;
			$('.theme-popover-mask').fadeIn(100);
			$('.theme-popover').slideDown(200);
	  	}
	  	
	  	$('.theme-poptit .close').click(function(){
			$('.theme-popover-mask').fadeOut(100);
			$('.theme-popover').slideUp(200);
			window.location.reload();
		})
		
		function deleteGoods(){
			var params= $('#goodsForm').serialize()
			if(confirm("请确认是否删除此数据")){
				layer.msg('加载中', {icon: 16,time: 1000},function(){
					 $.ajax({
						url:"<%=basePath%>admin/myAdmin?tag=deleteGoods", //后台处理程序
						type:'post',         //数据发送方式
						dataType:'json',
						data:params,         //要传递的数据
						success:function(data){
							if(data.status==200){
								window.location.reload();
							}else{
								layer.alert(data.tip);
							}
						}
					});
				}); 
			}
		}
		
		function updateShelves(id,flag){
    		if(confirm("请确认是否执行此操作")){
    			layer.msg('加载中', {icon: 16,time: 1000},function(){
					$.ajax({
						url:"<%=basePath%>admin/myAdmin?tag=updateShelves", //后台处理程序
						type:'post',         //数据发送方式
						dataType:'json',
						data:{id:id,flag:flag},         //要传递的数据
						success:function(data){
							if(data.status=='200'){
								window.location.reload();
							}else{
								layer.alert(data.tip);
							}
						}
					});
				}); 
    		}
    	}
    	
    	function updateBest(id,flag){
    		if(confirm("请确认是否执行此操作")){
    			layer.msg('加载中', {icon: 16,time: 1000},function(){
					$.ajax({
						url:"<%=basePath%>admin/myAdmin?tag=updateBest", //后台处理程序
						type:'post',         //数据发送方式
						dataType:'json',
						data:{id:id,flag:flag},         //要传递的数据
						success:function(data){
							if(data.status=='200'){
								window.location.reload();
							}else{
								layer.alert(data.tip);
							}
						}
					});
				}); 
    		}
    	}
    	
    	function updateNew(id,flag){
    		if(confirm("请确认是否执行此操作")){
    			layer.msg('加载中', {icon: 16,time: 1000},function(){
					$.ajax({
						url:"<%=basePath%>admin/myAdmin?tag=updateNew", //后台处理程序
						type:'post',         //数据发送方式
						dataType:'json',
						data:{id:id,flag:flag},         //要传递的数据
						success:function(data){
							if(data.status=='200'){
								window.location.reload();
							}else{
								layer.alert(data.tip);
							}
						}
					});
				}); 
    		}
    	}
    	
    	function updateHot(id,flag){
    		if(confirm("请确认是否执行此操作")){
    			layer.msg('加载中', {icon: 16,time: 1000},function(){
					$.ajax({
						url:"<%=basePath%>admin/myAdmin?tag=updateHot", //后台处理程序
						type:'post',         //数据发送方式
						dataType:'json',
						data:{id:id,flag:flag},         //要传递的数据
						success:function(data){
							if(data.status=='200'){
								window.location.reload();
							}else{
								layer.alert(data.tip);
							}
						}
					});
				}); 
    		}
    	}
	  </script>