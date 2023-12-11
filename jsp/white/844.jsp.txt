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
 	<jsp:useBean id="slide" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 幻灯片 -->
 	<jsp:useBean id="active" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 精彩活动-->
 	<jsp:useBean id="news" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 新闻-->
 	<jsp:useBean id="promt" scope="page" class="com.weishang.my.bean.MyBean"/><!--活动-->

    <title>${market.title}</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="${market.title}">
	<meta http-equiv="description" content="${market.title}">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
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
			<div class="clear page_tips">
				当前位置：<a href="<%=basePath%>">首页</a>><a href="<%=basePath%>promot/11.html">打包促销</a>><span>${market.title}</span>
			</div>
			<div class="clear sales">
				<div class="left sales_contain">
					<ul class="car_info_box">
						<li class="car_info">
							<div class="left car_info_img">
								<ul class="bigImg">
									<c:forEach items="${market.imgList}" var="img" varStatus="status">
										<li>
											<a href="javascript:void(0)" target="_blank"><img src="<%=basePath%>${img.img_path}" /></a>
										</li>
									</c:forEach>
								</ul>
							
								<div class="smallScroll">
									<a class="sPrev" href="javascript:void(0)">←</a>
									<div class="smallImg">
											 <ul>
											 	<c:forEach items="${market.imgList}" var="img" varStatus="status">
													<li><a><img src="<%=basePath%>${img.small_path}" /></a></li>
												</c:forEach>
											</ul>
									</div>
									<a class="sNext" href="javascript:void(0)">→</a>
								</div>						
							</div>
							<div class="left car_info_cs">
								<div class="b_name car_info_til">${market.title}</div>
								<div class="car_info_cs_li">
									<font color="red">本活动包含的产品：</font><br>
									<c:forEach items="${market.goodsList}" var="goods" varStatus="status">
										${goods.goodsName}<br>
									</c:forEach>
								</div>
								<div class="clear"></div>
								<a class="order_car" href="<%=basePath%>front/activetiOrderOne/${market.id}.html">立即预定</a>
							</div>
						</li>
						<li class="clear car_discribe">
							<div class="car_discribe_til">活动简介<span>ABOUT MODELS</span></div>
							<div class="car_discribe_cont">
								${market.longText}
							</div>
						</li>
					</ul>
				</div>
				<!--右侧模块-->
				<jsp:include  page="/template/${folder.tpl.folder}/page/web_right.jsp"/>
			</div>
		</div>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
    <script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/My97DatePicker/calendar.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/My97DatePicker/WdatePicker.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.SuperSlide.2.1.1.js"></script>
	<script>
	$(function(){
		//大图切换
		$(".car_info_img").slide({ titCell:".smallImg li", mainCell:".bigImg", effect:"fold", autoPlay:false,delayTime:200	});

		//小图左滚动切换
		$(".car_info_img .smallScroll").slide({ mainCell:"ul",delayTime:100,vis:4,scroll:4,effect:"left",autoPage:true,prevCell:".sPrev",nextCell:".sNext",pnLoop:false });
	});
	</script>
  </body>
</html>