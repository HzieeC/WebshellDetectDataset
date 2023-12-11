<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_resume_delivery.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_resume_delivery = new Hashtable();
	String job_name_para = "",job_id="",resume_name_para = "",s_start_date = "",s_end_date = "";
	if(request.getParameter("job_name_para")!=null && !request.getParameter("job_name_para").equals("")){
		job_name_para = request.getParameter("job_name_para");
		ti_resume_delivery.put("job_name",job_name_para);
	}
	if(request.getParameter("job_id")!=null && !request.getParameter("job_id").equals("")){
		job_id = request.getParameter("job_id");
		ti_resume_delivery.put("info_id",job_id);
	}
	if(request.getParameter("resume_name_para")!=null && !request.getParameter("resume_name_para").equals("")){
		resume_name_para = request.getParameter("resume_name_para");
		ti_resume_delivery.put("resume_name",resume_name_para);
	}
	if(request.getParameter("s_start_date")!=null && !request.getParameter("s_start_date").equals("")){
		s_start_date = request.getParameter("s_start_date");
		ti_resume_delivery.put("start_date",s_start_date);
	}
	if(request.getParameter("s_end_date")!=null && !request.getParameter("s_end_date").equals("")){
		s_end_date = request.getParameter("s_end_date");
		ti_resume_delivery.put("end_date",s_end_date);
	}
	Ti_resume_deliveryInfo ti_resume_deliveryInfo = new Ti_resume_deliveryInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_resume_deliveryInfo.getListByPage(ti_resume_delivery,Integer.parseInt(iStart),limit);
	int counter = ti_resume_deliveryInfo.getCountByObj(ti_resume_delivery);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?job_name_para="+job_name_para+"&resume_name_para="+resume_name_para+"&s_start_date="+s_start_date+"&s_end_date="+s_end_date+"&iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>简历投递管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>简历投递管理</h1>
			</td>
			<td>
				<!--<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>-->
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
	  
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				招聘职位:<input name="job_name_para" type="text" />
				简历姓名:<input name="resume_name_para" type="text" />
				投递日期:
<input name="s_start_date" type="text" id="s_start_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'s_end_date\',{d:-1})}',readOnly:true})" size="15" width="150px"/>-
<input name="s_end_date" id="s_end_date" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'s_start_date\',{d:1})}',readOnly:true})" size="15" width="150px"/>
				<input name="searchInfo" type="submit" value="搜索"/>
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
			
		  	<th>所属招聘信息</th>

			<th>所属简历信息</th>
		  	
		  	<th>求职人姓名</th>
		  	
		  	<th>投递日期</th>
		  	
			<th width="10%">查看</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String trade_id="",info_id="",reume_id="",reume_name="",in_date="";
					String job_name="",resume_title="";

					if(map.get("job_name")!=null) job_name = map.get("job_name").toString();
					if(map.get("resume_title")!=null) resume_title = map.get("resume_title").toString();

		  			  	if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
						if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
						if(map.get("reume_id")!=null) reume_id = map.get("reume_id").toString();
						if(map.get("reume_name")!=null) reume_name = map.get("reume_name").toString();
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>19)in_date=in_date.substring(0,19);

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=trade_id %>" /></td>
			
		  	<td><a href="/program/admin/recruit/updateInfo.jsp?job_id=<%=info_id%>" target="_blank"><%=job_name%></a></td>

			<td>
				<a href="/program/admin/resume/updateInfo.jsp?resume_id=<%=reume_id%>" target="_blank"><%=resume_title%></a>
			</td>
		  	
		  	<td><a href="updateInfo.jsp?trade_id=<%=trade_id %>"><%=reume_name%></a></td>
		  	
		  	<td><%=in_date%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?trade_id=<%=trade_id %>"><img src="/program/admin/images/view.gif" title="查看" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=trade_id%>','0589');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="0589" />
	  </form>
</body>

</html>
