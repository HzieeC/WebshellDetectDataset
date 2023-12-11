<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE HTML>
<html>
  <head>
    <base href="<%=basePath%>">
    <jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
 	<jsp:useBean id="active" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 精彩活动-->
 	<jsp:useBean id="promt" scope="page" class="com.weishang.my.bean.MyBean"/><!--活动-->
    <title>${recept.title}</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="${recept.keywords}">
	<meta http-equiv="description" content="${recept.descript}">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
  	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/page.css">
  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
		<div class="clear contain">			
		<ul class="clear rent_flow">
			<li>
				<span class="left num_icon">1</span>
				<p class="left mien_tit">预订车辆</br><font>提前为您预留</font></p>					
			</li>
			<li style="border-left:0px solid red">
				<span class="left num_icon">2</span>
				<p class="left mien_tit">签订合同</br><font>双方共同验车</font></p>
			</li>
			<li style="border-left:0px solid red">
				<span class="left num_icon">3</span>
				<p class="left mien_tit">开心旅途</br><font>一路为您保驾护航</font></p>
			</li>
			<li style="border-left:0px solid red">
				<span class="left num_icon">4</span>
				<p class="left mien_tit">退还车辆</br><font>完成租车使用</font></p>
			</li>
		</ul>
		<div class="clear sales">
			<div class="left sales_contain">
				<div class="sales_tab sales_tab_sort">低价促销</div>
				<div class="sales_tab sales_tab_full"></div>
				
				<!--循环-->
				<c:forEach items="${marketList}" var="market" varStatus="status">
					<div class="clear sales_list">
						<a href="<%=basePath%>front/promotDetail/${market.id}.html">
							<img class="left car_img" src="<%=basePath%>${market.img}" />
						</a>
						<div class="left car_right_c">
							<div class="left car_name"><a href="<%=basePath%>front/promotDetail/${market.id}.html">${market.title}</a></div>
							<div class="clear"></div>
							<ul class="left sales_info">
								<li class=sales_info_c5>
									${market.desc}
								</li>
								<li class="sales_info_c4">
									<div><span class="orange">￥<span class="money_big">${market.price}</span>/套餐价</span></div>						
								</li>
								<li class="rig_b">
									<a class="order_btn" href="<%=basePath%>front/activetiOrderOne/${market.id}.html">预 定</a>
								</li>
							</ul>
							<div class="clear"></div>
						</div>					
					</div>
				</c:forEach>
				<!--循环结束-->
				<div class="paginationBox blue">
                    <jsp:useBean id="paging" scope="page" class="com.weishang.bean.Page"/>
					<jsp:setProperty property="user" value="user" name="paging"/>
					<jsp:setProperty property="crrent" value="${pageNo}" name="paging"/>
					<jsp:setProperty property="suffix" value="" name="paging"/>
					<jsp:setProperty property="sumPage" value="${sum}" name="paging"/>
					<jsp:setProperty property="color" value="red" name="paging"/>
					<jsp:setProperty property="url" value="promot?menuId=${menuId}" name="paging"/>
					${paging.pageString}
                </div>
			</div>
			<!--右侧模块-->
			<jsp:include  page="/template/${folder.tpl.folder}/page/web_right.jsp"/>
		</div>
	</div>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
    <script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
  </body>
</html>