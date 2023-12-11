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
				当前位置：<a href="<%=basePath%>">首页</a>><a href="<%=basePath%>promot/11.html">精彩活动</a>><span>${cms.name}</span>
			</div>
    	<div class="clear sales">
			<div class="left news_contain">
				<!--标题 -->
				<div class="news_title">${cms.name}</div>
				<!--来源 -->
				<div class="news_tip">
					<span>作者：${cms.person}</span>
					<span>发布于：${cms.time}</span>
					<div class="bdsharebuttonbox" style="float:right">
						<a href="#" class="bds_more" data-cmd="more"></a>
						<a href="#" class="bds_qzone" data-cmd="qzone"></a>
						<a href="#" class="bds_tsina" data-cmd="tsina"></a>
						<a href="#" class="bds_tqq" data-cmd="tqq"></a>
						<a href="#" class="bds_renren" data-cmd="renren">
						</a><a href="#" class="bds_weixin" data-cmd="weixin"></a>
					</div>
					<span style="float:right" class="z1">
						分享到：
					</span>
<script>window._bd_share_config={"common":{"bdSnsKey":{},"bdText":"","bdMini":"2","bdPic":"","bdStyle":"0","bdSize":"16"},"share":{},"image":{"viewList":["qzone","tsina","tqq","renren","weixin"],"viewText":"分享到：","viewSize":"16"},"selectShare":{"bdContainerClass":null,"bdSelectMiniList":["qzone","tsina","tqq","renren","weixin"]}};with(document)0[(getElementsByTagName('head')[0]||body).appendChild(createElement('script')).src='http://bdimg.share.baidu.com/static/api/js/share.js?v=89860593.js?cdnversion='+~(-new Date()/36e5)];</script>
				</div>
				<!--来源结束 -->
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