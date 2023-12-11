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
				<c:forEach items="${cmsList}" var="cms" varStatus="status">
					<div class="clear activities_list"> 
						<a href="<%=basePath%>page/${cms.id}.html" class="left activities_img"><img src="<%=basePath%>${cms.img}" /></a>
						<div class="right activitise_box">
							<div class="activities_name"><a href="<%=basePath%>page/${cms.id}.html">${cms.name}</a></div>
							<div class="activities_describe">${cms.description}</div>
							<div class="right activities_seeinfo"><a href="<%=basePath%>page/${cms.id}.html">查看详情</a></div>	
						</div>			
					</div>
				</c:forEach>
				<div class="clear"></div>
				<div class="paginationBox blue">
                    <jsp:useBean id="paging" scope="page" class="com.weishang.bean.Page"/>
					<jsp:setProperty property="user" value="user" name="paging"/>
					<jsp:setProperty property="crrent" value="${pageNo}" name="paging"/>
					<jsp:setProperty property="suffix" value="" name="paging"/>
					<jsp:setProperty property="sumPage" value="${sum}" name="paging"/>
					<jsp:setProperty property="color" value="red" name="paging"/>
					<jsp:setProperty property="url" value="goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}&order=${order}" name="paging"/>
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