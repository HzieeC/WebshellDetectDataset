<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="com.bizoss.trade.ti_internews.*" %>
<%@page import="java.util.*" %>
<%@ page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	
	Map ti_internews = new Hashtable();
	
	String newstitle = "";
	if(request.getParameter("newstitle")!=null && !request.getParameter("newstitle").equals("")){
		newstitle = request.getParameter("newstitle").trim();
		ti_internews.put("title",newstitle);
	}
	
	String end_date = "";
	if(request.getParameter("txtEndDate")!=null && !request.getParameter("txtEndDate").equals("")){
		end_date = request.getParameter("txtEndDate");
		ti_internews.put("end_date",end_date);
	}	
	String start_date = "";
	if(request.getParameter("txtStartDate")!=null && !request.getParameter("txtStartDate").equals("")){
		start_date = request.getParameter("txtStartDate");
		ti_internews.put("start_date",start_date);
	}
	
	String cust_id = "";
	if( session.getAttribute("session_cust_id") != null )
	{
		cust_id = session.getAttribute("session_cust_id").toString();
		ti_internews.put("cust_id",cust_id);
	}
	Ti_internewsInfo ti_internewsInfo = new Ti_internewsInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_internewsInfo.getListByPage(ti_internews,Integer.parseInt(iStart),limit);
	int counter = ti_internewsInfo.getCountByObj(ti_internews);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?newstitle="+newstitle+"&txtEndDate="+end_date+"&txtStartDate="+start_date+"&cust_id="+cust_id+"&iStart=",Integer.parseInt(iStart),limit);
	
	
	String para = "newstitle="+newstitle+"&txtEndDate="+end_date+"&txtStartDate="+start_date+"&cust_id="+cust_id+"&iStart="+Integer.parseInt(iStart); 

%>
<html>
  <head>

    <title>通知公告</title>
		<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css"/>
		<script type="text/javascript" src="/js/commen.js"></script>
		<script type="text/javascript" src="internews.js"></script>
		<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>通知公告</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
		<!--<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
	    <tr>
	      <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
	      <td width="91%" align="left">
	  <h4>您可以按“标题”或“时间段”查询</h4>
	  <span>1、填写关键字，您将及时了解与该产品相关的所有商机。</span><br/>
	  </td>
	    </tr>
	  </table>-->
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left">
				标题:<input name="newstitle" type="text" maxlength="100"/>&nbsp;&nbsp;
				按发布时间段选择:
			 <input name="txtStartDate" type="text" id="txtStartDate" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'txtEndDate\',{d:-1})}',readOnly:true})" size="15" />
					- 
			<input name="txtEndDate" id="txtEndDate" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'txtStartDate\',{d:1})}',readOnly:true})" size="15"/>
				<input name="searchInfo" type="button" onclick="return search()" value="查询"/>	
			</td>
		</tr>
	</table>

	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td align="center" ><%=pageString %></td></tr>
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
			
		  	<th>标题</th>
		  	
		  	<th width="20%">发布日期</th>
		  	
				<th width="10%">修改</th>
				
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String title="",publish_date="",news_id="";
		  			if(map.get("news_id")!=null) news_id = map.get("news_id").toString();
				  	if(map.get("title")!=null) title = map.get("title").toString();  if(title.length()>30) title=title.substring(0,30)+"..."; 
					//if lenght>35 intercept string fixed by Zhouxq
				  	if(map.get("publish_date")!=null) publish_date = map.get("publish_date").toString();
						if(publish_date.length()>10) publish_date=publish_date.substring(0,10);
						
						title = "<a href=updateInfo.jsp?news_id="+news_id+">"+ title + "</a>";
		  %>
		
		<tr>
				<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=news_id %>" /></td>

		  	<td><%=title%></td>

		  	<td  width="20%"><%=publish_date%></td>

				<td width="10%"><a href="updateInfo.jsp?news_id=<%=news_id%>&<%=para%>"><img src="/program/admin/images/edit.gif" title="修改" border="0"/></a></td>
	  		<td width="10%"><a href="javascript:delOneNews('<%=news_id%>');"><img src="/program/admin/images/delete.gif" title="删除" border="0"/></a></td>
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
		<tr><td align="center" ><%=pageString %></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="5023" />
	  </form>
</body>

</html>
