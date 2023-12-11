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
 	
 	
 	
    <title>${recept.title}</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="${recept.keywords}">
	<meta http-equiv="description" content="${recept.descript}">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
  		<div class="clear page_tips">
				当前位置：<a href="<%=basePath%>">首页</a>><a href="<%=basePath%>auto/19.html">帮助中心</a>><span>${cms.name}</span>
		</div>
		<div class="header_news_box">
			<ul class="left news_menu">
				<jsp:useBean id="myReceptList" scope="page" class="com.weishang.my.bean.MyBean"/><!--帮助中心的菜单 -->
				<jsp:setProperty property="host" value="<%=basePath%>" name="myReceptList"/>
				<c:forEach items="${myReceptList.receptAndCms}" var="recept" varStatus="status">
					<li class="news_menu_t  news_menu_til">${recept.recept.name}</li>
					<li style="border-bottom:1px solid #dcdcdc">
						<ul>
							<c:forEach items="${recept.receptList}" var="childrenRecept" varStatus="status">
								<li class="news_menu_t"><a href="${childrenRecept.modUrl}">${childrenRecept.name}</a></li>
							</c:forEach>
						</ul>
					</li>
				</c:forEach>
			</ul>
			<div class="right bottom_list">
				${cms.content}
			</div>
		</div>	
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
    <script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/My97DatePicker/calendar.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/My97DatePicker/WdatePicker.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.SuperSlide.2.1.1.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/main_js.js"></script>
	<script type="text/javascript">
		function form_smt(){
			var store_id=document.getElementById("store_id").value;
			var type_id=document.getElementById("type_id").value;
			var time=document.getElementById("time").value;
			if(store_id==""||store_id==null){
	         	alert("请选择门店！");
	        }else if(type_id==""||type_id==null){
	         	alert("请选择服务类型！");
	        }else if(time==""||time==null){
	         	alert("请选择时间！");
	        }else{
	        	document.forms["fast_form"].submit();
	        }
		}
	</script>
  </body>
</html>
