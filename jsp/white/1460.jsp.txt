<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ts_memlevel.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Ts_memlevel ts_memlevel = new Ts_memlevel();
	
	String _class_name = "";
	if(request.getParameter("_class_name")!=null && !request.getParameter("_class_name").equals("")){
		_class_name = request.getParameter("_class_name");
		ts_memlevel.setClass_name(_class_name);
		
	}
	
	Ts_memlevelInfo ts_memlevelInfo = new Ts_memlevelInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ts_memlevelInfo.getListByPage(ts_memlevel,Integer.parseInt(iStart),limit);
	int counter = ts_memlevelInfo.getCountByObj(ts_memlevel);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?_class_name="+_class_name+"&iStart=",Integer.parseInt(iStart),limit);
	
	String para  ="_class_name="+_class_name+"&iStart="+Integer.parseInt(iStart);
%>
<html>
  <head>
    
    <title>级别管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
		<script language="javascript" src="js_personalLevel.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>级别管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">

	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				&nbsp;按级别名称:<input name="_class_name" type="text" />
				<input name="searchInfo" type="button" value="搜索" onclick="searchForm();" />	
			</td>
		</tr>
	</table>

	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString %></td></tr>
	</table>
	
	<% 
		int listsize = 0;
		if(list!=null && list.size()>0){
			listsize = list.size();
	%>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td>
				共计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>级别名称</th>
		  	
		  	<th>级别折扣率</th>
		  	
		  	<th>积分上限</th>
		  	
		  	<th>积分下限</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String user_class="",class_name="",discount="",up_num="",down_num="",rsrv_str1="",rsrv_str2="",rsrv_str3="";
		  			  	if(map.get("user_class")!=null) user_class = map.get("user_class").toString();
  	if(map.get("class_name")!=null) class_name = map.get("class_name").toString();
  	if(map.get("discount")!=null) discount = map.get("discount").toString();
  	if(map.get("up_num")!=null) up_num = map.get("up_num").toString();
  	if(map.get("down_num")!=null) down_num = map.get("down_num").toString();
  	if(map.get("rsrv_str1")!=null) rsrv_str1 = map.get("rsrv_str1").toString();
  	if(map.get("rsrv_str2")!=null) rsrv_str2 = map.get("rsrv_str2").toString();
  	if(map.get("rsrv_str3")!=null) rsrv_str3 = map.get("rsrv_str3").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=user_class %>" /></td>
			
		  	<td><a href="updateInfo.jsp?user_class=<%=user_class %>"><%=class_name%></a></td>
		  	
		  	<td><%=discount%></td>
		  	
		  	<td><%=up_num%></td>
		  	
		  	<td><%=down_num%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?user_class=<%=user_class %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=user_class%>','7017');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td>
				共计:<%=counter %>条
			</td>
		</tr>
	</table>
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString %></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="7017" />
	  </form>
</body>

</html>
