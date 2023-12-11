<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_listadvinfo.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_listadvinfo = new Hashtable();
	String _key_words = "",_adv_title="";
	if(request.getParameter("n_key_words")!=null && !request.getParameter("n_key_words").equals("")){
	_key_words = request.getParameter("n_key_words");
	ti_listadvinfo.put("key_words",_key_words);
	
  }
  if(request.getParameter("n_adv_title")!=null && !request.getParameter("n_adv_title").equals("")){
	_adv_title = request.getParameter("n_adv_title");
	ti_listadvinfo.put("adv_title",_adv_title);
	
  }
	ti_listadvinfo.put("adv_rang",9);
  
  String start_start_date ="";
  if(request.getParameter("start_start_date")!=null && !request.getParameter("start_start_date").equals("")){
  start_start_date = request.getParameter("start_start_date");
	ti_listadvinfo.put("start_start_date",start_start_date);
	}
	String start_end_date = "";
	if(request.getParameter("start_end_date")!=null && !request.getParameter("start_end_date").equals("")){
  start_end_date = request.getParameter("start_end_date");
	ti_listadvinfo.put("start_end_date",start_end_date);
	}
	
	String end_start_date = "";
	if(request.getParameter("end_start_date")!=null && !request.getParameter("end_start_date").equals("")){
  end_start_date = request.getParameter("end_start_date");
	ti_listadvinfo.put("end_start_date",end_start_date);
	}
	String end_end_date = "";
	if(request.getParameter("end_end_date")!=null && !request.getParameter("end_end_date").equals("")){
  end_end_date = request.getParameter("end_end_date");
	ti_listadvinfo.put("end_end_date",end_end_date);
	}
	
  
	Ti_listadvinfoInfo ti_listadvinfoInfo = new Ti_listadvinfoInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_listadvinfoInfo.getListByPage(ti_listadvinfo,Integer.parseInt(iStart),limit);
	int counter = ti_listadvinfoInfo.getCountByObj(ti_listadvinfo);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?key_words="+_key_words+"&adv_title="+_adv_title+"&start_start_date="+start_start_date+"&start_end_date="+start_end_date+"&end_start_date="+end_start_date+"&end_end_date="+end_end_date+"&iStart=",Integer.parseInt(iStart),limit);
	String para =	"n_key_words="+_key_words+"&n_adv_title="+_adv_title+"&start_start_date="+start_start_date+"&start_end_date="+start_end_date+"&end_start_date="+end_start_date+"&end_end_date="+end_end_date+"&iStart="+Integer.parseInt(iStart)+"&limit="+limit;
	
	
%>
<html>
  <head>
    
    <title>关键字广告管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type="text/javascript" src="/js/commen.js"></script>
	<script language="javascript" type="text/javascript" src="js_listAdv.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>关键字广告管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left">
				<div>
					&nbsp;关&nbsp;键&nbsp;字:<input name="n_key_words" type="text" maxLength="50" />
					&nbsp;展示开始时间:
						
					<input name="start_start_date" type="text" id="txtStartDate" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'txtEndDate\',{d:-1})}',readOnly:true})" size="15" />
					- 
					<input name="start_end_date" id="txtEndDate" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'txtStartDate\',{d:1})}',readOnly:true})" size="15"/>
			</div>
			<div>
				&nbsp;广告名称:<input name="n_adv_title" type="text" maxLength="50"/>
				&nbsp;展示结束时间:
				<input name="end_start_date" type="text" id="txtStartDate1" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'txtEndDate1\',{d:-1})}',readOnly:true})" size="15" />
				- 
				<input name="end_end_date" id="txtEndDate1" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'txtStartDate1\',{d:1})}',readOnly:true})" size="15"/>
					
				<input name="searchInfo" type="button" value="搜索" onclick="searchForm()"/>	
				
			</div>
				

		</tr>
	</table>

	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe" >
		<tr><td align="center" bgcolor="#ECE6E6"><%=pageString %></td></tr>
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
			<td >
				共计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center">
				<input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>关键字</th>
		  	
		  	<th>广告名称</th>
		  	
		  	<th width="8%">显示顺序</th>
		  	
		  	<th width="15%">价格</th>
		  	
				<th width="15%">开始时间</th>
		  	
		  	<th width="15%">结束时间</th>
		  	
				<th width="5%">修改</th>
	  		<th width="5%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String adv_id="",key_type="",adv_rang="",key_words="",adv_title="",adv_text="",start_date="",end_date="",adv_post="",price="",adv_url="",contact="",contact_info="",user_id="",in_date="",remark="";
		if(map.get("adv_id")!=null) adv_id = map.get("adv_id").toString();
  	if(map.get("key_type")!=null) key_type = map.get("key_type").toString();
  	if(map.get("adv_rang")!=null) adv_rang = map.get("adv_rang").toString();
  	if(map.get("key_words")!=null) key_words = map.get("key_words").toString();
  	if(map.get("adv_title")!=null) adv_title = map.get("adv_title").toString(); if(adv_title.length()>18)adv_title=adv_title.substring(0,18)+"...";
  	if(adv_title.length()>25) adv_title=adv_title.substring(0,25);
  	if(map.get("adv_text")!=null) adv_text = map.get("adv_text").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
		if(start_date.length()>19)start_date=start_date.substring(0,19);
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
		if(end_date.length()>19)end_date=end_date.substring(0,19);
  	if(map.get("adv_post")!=null) adv_post = map.get("adv_post").toString();
  	if(map.get("price")!=null) price = map.get("price").toString();
  	if(map.get("adv_url")!=null) adv_url = map.get("adv_url").toString();
  	if(map.get("contact")!=null) contact = map.get("contact").toString();
  	if(map.get("contact_info")!=null) contact_info = map.get("contact_info").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
		if(in_date.length()>19)in_date=in_date.substring(0,19);
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=adv_id %>" /></td>
			
		  	<td><%=key_words%></td>
		  	
		  	<td><a href="updateInfo.jsp?adv_id=<%=adv_id %>&<%=para%>"><%=adv_title%></a></td>
		  	
		  	<td width="8%"><%=adv_post%></td>
		  	
		  	<td width="15%"><%=price%>元/天</td>
		  	
		   	<td width="15%"><%=start_date%></td>
		  	
		  	<td width="15%"><%=end_date%></td>
		  	
			<td width="5%"><a href="updateInfo.jsp?adv_id=<%=adv_id %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  	<td width="5%"><a href="javascript:delOneNews('<%=adv_id%>');"><img border="0" src="/program/admin/images/delete.gif" title="删除" /></a></td>
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
			<td >
				共计:<%=counter %>条
			</td>
		</tr>
	</table>
	<table width="100%" cellpadding="0" class="tablehe" cellspacing="0" border="0">
		<tr><td align="center" bgcolor="#ECE6E6"><%=pageString %></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="7073" />
	  <input name="adv_rang" id="adv_rang" value="9" type="HIDDEN" />
	  </form>
</body>

</html>
