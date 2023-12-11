<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>

<%
	 request.setCharacterEncoding("UTF-8");

%>
<html>
  <head>
   	<title>栏目更新</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	
	<script type="text/javascript" src="/js/jquery-1.4.4.min.js"></script>
	<script type="text/javascript" src="sitemap.js"></script>	
	</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>生成网站地图</h1>
			</td>
			<td>
				<!--a href="addInfo.jsp"><img src="/program/admin/index/images/post.png" /></a-->
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">

	<table width="100%" cellpadding="2" cellspacing="2" border="0" class="dl_su">


		<tr>
			<td align="left">
				<input type="button" name="delInfo" onclick="generator();" value="生成" class="buttab"/> 		
			</td>
			<td align="left">
				<div id="backMessage"></div>
			</td>
		</tr>
	</table>

	<br/><br/>




	  </form>
</body>

</html>
