<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_vote.*" %>
<%@page import="com.bizoss.trade.ti_voteoption.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_vote = new Hashtable();
	String g_title = "";
	if(request.getParameter("seach_vote_title")!=null && !request.getParameter("seach_vote_title").equals("")){
		g_title = request.getParameter("seach_vote_title");
		ti_vote.put("vote_title",g_title);
	}
	String g_start_date = "";
   if(request.getParameter("start_date")!=null && !request.getParameter("start_date").equals("")){
	g_start_date = request.getParameter("start_date");
	ti_vote.put("start_date",g_start_date);
	}
	String g_end_date = "";
	if(request.getParameter("end_date")!=null && !request.getParameter("end_date").equals("")){
	g_end_date = request.getParameter("end_date");
	ti_vote.put("end_date",g_end_date);
	}
	Ti_voteInfo ti_voteInfo = new Ti_voteInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_voteInfo.getListByPage(ti_vote,Integer.parseInt(iStart),limit);
	int counter = ti_voteInfo.getCountByObj(ti_vote);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?seach_vote_title="+g_title+"&end_date="+g_end_date+"&start_date="+g_start_date+"&iStart=",Integer.parseInt(iStart),limit);
  Ti_voteoptionInfo optioninfo = new Ti_voteoptionInfo();
  
  String para ="seach_vote_title="+g_title+"&end_date="+g_end_date+"&start_date="+g_start_date+"&iStart="+Integer.parseInt(iStart);
%>
<html>
  <head>
    
    <title>在线调查</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="vote.js"></script>
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>在线调查</h1>
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
				调查主题:<input name="seach_vote_title" id="seach_vote_title" type="text" />
				&nbsp;
						按有效时间段:
						<input name="start_date" id="txtStartDate" type="text" class="Wdate" value="" 
						
						onclick="WdatePicker({maxDate:'#F{$dp.$D(\'txtEndDate\',{d:-1})}',readOnly:true})" size="15" />
						 - 
						<input name="end_date" id="txtEndDate" type="text" class="Wdate" value="" 
						
						onclick="WdatePicker({minDate:'#F{$dp.$D(\'txtStartDate\',{d:1})}',readOnly:true})" size="15"/>						
				<input name="searchInfo" type="button" value="搜索" onclick="return seacher()"/>	
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
			
		  	<th width="35%">调查主题</th>
		  	
		  	<th>开始时间</th>
		  	
		  	<th>结束时间</th>
		  	
		  	<th>投票数</th>
		  	
			<th width="15%">操作</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String vote_id="",cust_id="",vote_title="",start_date="",end_date="",is_multi="",vote_count="",user_id="",in_date="";
		  			  	if(map.get("vote_id")!=null) vote_id = map.get("vote_id").toString();
						if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
						if(map.get("vote_title")!=null) vote_title = map.get("vote_title").toString();
							if(vote_title.length()>19){
								  vote_title=vote_title.substring(0,19);
								}
						if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
						if(start_date.length()>10)start_date=start_date.substring(0,10);
						if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
						if(end_date.length()>10)end_date=end_date.substring(0,10);
						if(map.get("is_multi")!=null) is_multi = map.get("is_multi").toString();
						if(map.get("vote_count")!=null) vote_count = map.get("vote_count").toString();
						if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>10)in_date=in_date.substring(0,10);
						
                          int bool=  optioninfo.checkVoteoption(vote_id);
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=vote_id %>" /></td>
			<input type="hidden" name="bool<%=i%>" id="bool<%=i%>" value="<%=bool%>">
		  	<td><a href="updateInfo.jsp?vote_id=<%=vote_id %>&<%=para%>"><%=vote_title%></a></td>
		  	
		  	<td><%=start_date%></td>
		  	
		  	<td><%=end_date%></td>
		  	
		  	<td><%=vote_count%></td>
		  	
			<td width="15%"><a href="voteoptionindex.jsp?vote_id=<%=vote_id%>&<%=para%>">调查选项</a>|
			<a href="updateInfo.jsp?vote_id=<%=vote_id %>&<%=para%>">修改</a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=vote_id%>','5228','<%=i%>');"><img src="/program/admin/images/delete.gif" title="删除" /></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="5228" />
	  </form>
</body>

</html>
