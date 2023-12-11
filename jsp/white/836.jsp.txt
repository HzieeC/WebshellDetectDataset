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

    <title>${recept.title}</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="${recept.keywords}">
	<meta http-equiv="description" content="${recept.descript}">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/reste.css"/>
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/swyc.css"/>
  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
 		<div class="banner"></div>
	<div class="service">
	  <div class="service-cont">
	    <div class="service-1">
	      <h2>服务项目<br/>	
	        <span>SERVICE PROJECT</span></h2>
	      <P>可满足各种企业用车需求，包括月租、年租、公车改革、班车接送、机场接送等服务</P>
	    </div>
	    <div class="service-2">
	      <div class="list-item">
	        <dl class="item1">
	          <dd class="list1"></dd>
	          <dt>机车接送</dt>
	        </dl>
	        <dl>
	          <dd class="list2"></dd>
	          <dt>企业班车</dt>
	        </dl>
	        <dl>
	          <dd class="list3"></dd>
	          <dt>会议用车</dt>
	        </dl>
	        <dl>
	          <dd class="list4"></dd>
	          <dt>商务包车</dt>
	        </dl>
	        <dl>
	          <dd class="list5"></dd>
	          <dt>企业长租</dt>
	        </dl>
	        <dl>
	          <dd class="list6"></dd>
	          <dt>订制服务</dt>
	        </dl>
	      </div>
	    </div>
	    <a href="tencent://message/?Menu=yes&amp;amp;uin=1919594905&amp;amp;Service=58&amp;amp;SigT=A7F6FEA02730C98899FC429646EE5F0F8B07D60CDEB7B92BC282BDA1F06D5554ADE2BFC92CBCCF96E35A5EC498283351E06E0662B61D2E8F34CD8139398DB25706D282E2CA08CEE42FE40B0791F2EAA671DBA2D8F063EFAD25EA303F640944093AC3E91262B95AE48749D900E5C1D2A57CE9ACE6F9F3D62E&amp;amp;SigU=30E5D5233A443AB21AEEA6590C43E3B9C77B3FB3245197F712955C23FC4D59A8BF31C300FAE02ED67D65C2E4DF70D9127DE32DEA99E9A6210EC70BC32CAA0A8D0508817C65939307" class="servise-3">咨询详情</a>
	  </div>
	</div>
	
	<div class="recommend clear">
	  <div class="contain_news_ico">
	    <p class="icon_name_c">车型推荐</p>
	    <p class="icon_name_e">MODELS  RECOMMENDED</p>
	    <P class="describ">车型丰富，品种齐全，300多辆汽车可供用户选择，为客户提供合理化的用方案</P>
	  </div>
	  <div class="tab clear">
	    <div class="tab-top"> 
	    <!--<span class="one">5座轿车</span> <span class="two">7-18座商务</span> <span class="three">19座以上大巴</span> </div>-->
	    <div id="slideBox" class="slideBox">
				<!--
					<div class="hd">
						<ul><li>1</li><li>2</li><li>3</li></ul>
					</div>
				-->
				<div class="bd">
					<ul>
						<li><a href="javascript:void(0)" target="_blank"><img src="<%=basePath%>template/${folder.tpl.folder}/images/car_03.jpg"/></a></li>
						<li><a href="javascript:void(0)" target="_blank"><img src="<%=basePath%>template/${folder.tpl.folder}/images/car_03.jpg"/></a></li>
						<li><a href="javascript:void(0)" target="_blank"><img src="<%=basePath%>template/${folder.tpl.folder}/images/car_03.jpg"/></a></li>
					</ul>
				</div>
				
				<!-- 下面是前/后按钮代码，如果不需要删除即可 -->
				<a class="prev" href="javascript:void(0)"></a>
				<a class="next" href="javascript:void(0)"></a>
	
			</div>
	      <p class="info">车型：大众迈腾       日租：450元/天       月租：9000元/月</p>
	      <span class="btn"><a href="<%=basePath%>goods/7.html">更多车型</a></span> </div>
	</div>
	     <script id="jsID" type="text/javascript">
			jQuery(".slideBox").slide({mainCell:".bd ul",autoPlay:true});
			</script>
	<div class="clear"></div> 
	<div class="partner clear">
	  <div class="contain_news_ico">
	    <p class="icon_name_c">合作伙伴</p>
	    <p class="icon_name_e">PARTNERS</p>
	    <P class="describ">24小时服务热线，为客户提供合理化的方案</P>
	  </div>
	  <ul class="partner-list">
	  	<jsp:useBean id="coo" scope="page" class="com.weishang.bean.ReceptBean"/><!--友情链接-->
		<jsp:setProperty property="flag" value="1" name="coo"/>
		<jsp:setProperty property="pageNo" value="1" name="coo"/>
		<jsp:setProperty property="pageSize" value="100" name="coo"/>
		<c:forEach items="${coo.cooList}" var="coo" varStatus="status">
		    <li> <img src="<%=basePath%>${coo.logo}" />
		      <a href="${coo.url}" class="describ" target="_blank">
		        <p class="describ-1">${coo.name}</p>
		      </a>
		    </li>
	 	</c:forEach>
	  </ul>
	</div>
	<ul class="clear rent_flow">
	  <li> <span class="left num_icon">1</span>
	    <p class="left mien_tit">预订车辆</br>
	      <font>提前为您预留</font></p>
	  </li>
	  <li style="border-left:0px solid red"> <span class="left num_icon">2</span>
	    <p class="left mien_tit">签订合同</br>
	      <font>双方共同验车</font></p>
	  </li>
	  <li style="border-left:0px solid red"> <span class="left num_icon">3</span>
	    <p class="left mien_tit">开心旅途</br>
	      <font>一路为您保驾护航</font></p>
	  </li>
	  <li style="border-left:0px solid red"> <span class="left num_icon">4</span>
	    <p class="left mien_tit">退还车辆</br>
	      <font>完成租车使用</font></p>
	  </li>
	</ul>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
    <script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.SuperSlide.2.1.1.js"></script>
	<script>
		$(function(){
	
		$('.partner-list li').mouseenter(function(){
			$(this).find('.describ').show();
			}).mouseleave(function(e) {
	           $(this).find('.describ').hide(); 
	        });	
		})
	</script>
  </body>
</html>