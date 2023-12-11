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

    <title>${goods.goodsName}</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="${goods.keywords}">
	<meta http-equiv="description" content="${goods.goodsBrief}">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
  		<div class="header_diver_bg">
			<div class="header_driver_form">
				<div class="left driver_info_img">
					<ul class="bigdriver">
						<li>
							<div class="div_i">
								<div class="div_img">
									<img src="<%=basePath%>${aunt.head}" />
								</div>
							</div>
							<ul class="left driver_info">
								<li class="driver_font">${aunt.name}</br>
										<label>${aunt.desc}</label>
								</li>
								<li class="driver_font2">客户说：</li>
								<c:forEach items="${aunt.comList}" var="comment" varStatus="status">
										<li>${comment.user}：${comment.content}</li>
								</c:forEach>
							</ul>
						</li>
					</ul>

					<div class=" clear smallScroll">
						<a class="sPrev" href="javascript:void(0)">&lt;</a>
						<div class="smallImg">
							<ul>
								<c:forEach items="${auntList}" var="aunt" varStatus="status">
									<li><a href="<%=basePath%>aunt/${aunt.id}.html"><img src="<%=basePath%>${aunt.head}" /></a></li>
								</c:forEach>
							</ul>
						</div>
						<a class="sNext" href="javascript:void(0)">&gt;</a>					</div>
				</div>
			</div>
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
			$(".driver_info_img").slide({ titCell:".smallImg li", mainCell:".bigdriver", effect:"fold", autoPlay:true,delayTime:200	,
				startFun:function(i,p){
					//控制小图自动翻页
					if(i==0){ jQuery(".driver_info_img .sPrev").click() } else if( i%4==0 ){ jQuery(".driver_info_img .sNext").click()}
				}
			});

			//小图左滚动切换
			$(".driver_info_img .smallScroll").slide({ mainCell:"ul",delayTime:100,vis:5,scroll:5,effect:"left",autoPage:true,prevCell:".sPrev",nextCell:".sNext",pnLoop:false });
		});
	</script>
  </body>
</html>