<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
	 <jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
  <head>
    <base href="<%=basePath%>">
    
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
    <div class="clear contain">
    	<div class="clear page_tips">
				当前位置：<a href="<%=basePath%>">首页</a>><span>联系我们</span>
			</div>
    	<div class="clear sales">
			<div class="left news_contain">
		
				<!--详情内容-->
				<div class="news_cont">
          			${cms.content}
          		</div>
          		<!--详情内容结束-->
          	</div>
			<!--右侧模块-->
			<jsp:include  page="/template/${folder.tpl.folder}/page/web_right.jsp"/>
			
		</div>
    </div>
    <jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
  </body>
</html>