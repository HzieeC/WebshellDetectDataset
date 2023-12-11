<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_shoptem.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ts_custclass.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Ti_shoptem ti_shoptem = new Ti_shoptem();
	String s_title = "";
	if(request.getParameter("search_tem_name")!=null && !request.getParameter("search_tem_name").equals("")){
		s_title = request.getParameter("search_tem_name");
		ti_shoptem.setTem_name(s_title);
	}
	Ti_shoptemInfo ti_shoptemInfo = new Ti_shoptemInfo();
	
	Ts_custclassInfo custClassInfo =  new Ts_custclassInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_shoptemInfo.getListByPage(ti_shoptem,Integer.parseInt(iStart),limit);
	int counter = ti_shoptemInfo.getCountByObj(ti_shoptem);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?search_tem_name="+s_title+"&iStart=",Integer.parseInt(iStart),limit);
	String para ="search_tem_name="+s_title+"&iStart="+Integer.parseInt(iStart);
%>
<html>
  <head>
    
    <title>会员模板管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script language="javascript" type="text/javascript" src="shoptem.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>会员模板管理</h1>
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
				模板名称:<input name="search_tem_name" id="search_tem_name" type="text" />
				
				<input name="searchInfo" type="button" value="查询" onclick="return search();"/>	
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
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>模板名称	</th>
		  	
		  	<th>会员级别</th>
		  	
		  	<th>模板代码</th>
		  	
		  	<th>是否有效</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String tem_id="",tem_name="",cust_class="",enabledStr="有效",cust_class_name="",cust_id="",tem_code="",tem_image="",tem_path="",enabled="",remark="";
		  			  	if(map.get("tem_id")!=null) tem_id = map.get("tem_id").toString();
  	if(map.get("tem_name")!=null) tem_name = map.get("tem_name").toString();
	
	
  	if(map.get("cust_class")!=null) {
		cust_class = map.get("cust_class").toString();
		cust_class_name = custClassInfo.getcust_class_name(cust_class);
	}
	
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("tem_code")!=null) tem_code = map.get("tem_code").toString();
  	if(map.get("tem_image")!=null) tem_image = map.get("tem_image").toString();
  	if(map.get("tem_path")!=null) tem_path = map.get("tem_path").toString();
  	if(map.get("enabled")!=null) {
	
	enabled = map.get("enabled").toString();
	
	if(enabled.equals("1")){
	enabledStr = "无效";
	}
	
	}
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=tem_id %>" /></td>
			
		  	<td><%=tem_name%></td>
		  	
		  	<td><%=cust_class_name%></td>
		  	
		  	<td><%=tem_code%></td>
		  	
		  	<td><%=enabledStr%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?tem_id=<%=tem_id %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=tem_id%>','0089');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
				总计:<%=counter %>条
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="0089" />
	  </form>
</body>

</html>
