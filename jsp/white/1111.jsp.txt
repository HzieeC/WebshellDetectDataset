<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_voteoption.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
    request.setCharacterEncoding("UTF-8");
	
    String g_vote_id="",vote_title="";
	 if(request.getParameter("vote_id")!=null) 
	 g_vote_id = request.getParameter("vote_id");
	Ti_voteoption ti_voteoption = new Ti_voteoption();
	ti_voteoption.setVote_id(g_vote_id);
	Ti_voteoptionInfo ti_voteoptionInfo = new Ti_voteoptionInfo();
	if(g_vote_id!=null){
	     vote_title=ti_voteoptionInfo.getvote_titleById(g_vote_id);
	}
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_voteoptionInfo.getListByPage(ti_voteoption,Integer.parseInt(iStart),limit);
	int counter = ti_voteoptionInfo.getCountByObj(ti_voteoption);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
	String g_title = "";
	if(request.getParameter("seach_vote_title")!=null && !request.getParameter("seach_vote_title").equals("")){
		g_title = request.getParameter("seach_vote_title");
	}
	String g_start_date = "";
   if(request.getParameter("start_date")!=null && !request.getParameter("start_date").equals("")){
	g_start_date = request.getParameter("start_date");
	}
	String g_end_date = "";
	if(request.getParameter("end_date")!=null && !request.getParameter("end_date").equals("")){
	g_end_date = request.getParameter("end_date");
	}
	
	String para ="/program/admin/vote/index.jsp?seach_vote_title="+g_title+"&end_date="+g_end_date+"&start_date="+g_start_date+"&iStart="+Integer.parseInt(iStart);
%>
<html>
  <head>
    
    <title>在线调查选项管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>在线调查选项管理</h1>
			</td>
			<td>
				<a href="voteoptionaddInfo.jsp?vote_id=<%=g_vote_id%>"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4><%=vote_title%>的调查选项 &nbsp;&nbsp;
		  <a href="<%=para%>"/>返回在线调查</h4>
		  <span></span><br/>
		  </td>
        </tr>
      </table>
      <br/>
	  
	  
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
				总计:<%=counter%>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>选项名称</th>
		  	
		  	<th>投票数</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String option_id="",vote_id="",option_name="",option_count="";
		  			  	if(map.get("option_id")!=null) option_id = map.get("option_id").toString();
						if(map.get("vote_id")!=null) vote_id = map.get("vote_id").toString();
						if(map.get("option_name")!=null) option_name = map.get("option_name").toString();
						if(map.get("option_count")!=null) option_count = map.get("option_count").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=option_id %>" /></td>
			
		  	<td><%=option_name%></td>
		  	
		  	<td><%=option_count%></td>
		  	
			<td width="10%"><a href="voteoptionupdateInfo.jsp?option_id=<%=option_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=option_id%>','1350');"><img src="/program/admin/images/delete.gif" title="删除" /></a></td>
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
				总计:<%=counter%>条
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="1350" />
	  </form>
</body>

</html>
