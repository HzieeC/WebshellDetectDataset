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
 	<jsp:useBean id="promot" scope="page" class="com.weishang.my.bean.MyBean"/><!-- 精彩活动-->
 	
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
				<div class="sales_tab sales_tab_sort">热门专题</div>
				<div class="sales_tab sales_tab_full"></div>
				<div class="clear page_news_list">
					<ul>
						<c:forEach items="${activityList}" var="activity" varStatus="status">
							<!--无图片的li增加一个class样式 <li class="no_img_list">-->
							<c:if test="${activity.img==null || activity.img==''}">
								<li class="no_img_list">
									<div class="list_f">
										<div class="title"><a href="<%=basePath%>huodong/${activity.id}.html">${activity.name}</a></div>
										<div class="na_cont">${activity.desc}</div>
									</div>
								</li>
							</c:if>
							<c:if test="${activity.img!=null&&activity.img!=''}">
								<li>
									<div class="list_img">
										<a href="<%=basePath%>huodong/${activity.id}.html" class="left activities_img"><img src="<%=basePath%>${activity.img}" /></a>
									</div>
									<div class="list_f">
										<div class="title"><a target="_blank" href="<%=basePath%>huodong/${activity.id}.html">${activity.name}</a></div>
										<div class="na_cont">${activity.desc}</div>
									</div>
								</li>
							</c:if>
						</c:forEach>
					</ul>
					<div class="clear"></div>		
				</div>
				<div class="clear"></div>
				<div class="paginationBox blue">
                    <jsp:useBean id="paging" scope="page" class="com.weishang.bean.Page"/>
					<jsp:setProperty property="user" value="user" name="paging"/>
					<jsp:setProperty property="crrent" value="${pageNo}" name="paging"/>
					<jsp:setProperty property="suffix" value="" name="paging"/>
					<jsp:setProperty property="sumPage" value="${sum}" name="paging"/>
					<jsp:setProperty property="color" value="red" name="paging"/>
					<jsp:setProperty property="url" value="/jingcaihuodong?menuId=${menuId}" name="paging"/>
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