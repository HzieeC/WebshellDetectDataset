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
			
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
			      <thead>
			          <tr>
			              <th width="65%"><fmt:message key="name" bundle="${messagesBundle}"/></th>
			              <th width="35%">商品货号</th>
			          </tr>
			      </thead>
			      <c:forEach items="${goodsList}" var="goods" varStatus="status">
				      <tr>
				        <td>${goods.goodsName}</td>
				        <td>${goods.goodsSn}</td>
				      </tr>
			      </c:forEach>
			    </table>
			</div>
		</div>
		
		
	</div>
	<!--本页主内容结束 -->
	  </fmt:bundle>
	  <script language="javascript">
		$(function(){
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-32);
			$(window).resize(function () {          //当浏览器大小变化时
			    var height = $(window).height();
				$(".middle_cnt_c2").height(height-32);
			});
		})

		function deleteGoodsAndOrder(){
			var params= $('#typeForm').serialize()
			if(confirm("请确认是否删除此数据")){
				 $.ajax({
					url:"<%=basePath%>admin/deleteGoodsType", //后台处理程序
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