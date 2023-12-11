<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_memcomplaint.*,com.bizoss.trade.ti_personal.*,com.bizoss.trade.ti_customer.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");

	Map ti_memcomplaint = new Hashtable();
	
	String _deal_state = "",_start_date="",_end_date="";

	if(request.getParameter("seach_deal_state")!=null && !request.getParameter("seach_deal_state").equals("")){
		_deal_state = request.getParameter("seach_deal_state");
		ti_memcomplaint.put("deal_state",_deal_state);
	}
	if(request.getParameter("s_start_date")!=null && !request.getParameter("s_start_date").equals("")){
		_start_date = request.getParameter("s_start_date");
		ti_memcomplaint.put("start_date",_start_date);
	}	
	if(request.getParameter("s_end_date")!=null && !request.getParameter("s_end_date").equals("")){
		_end_date = request.getParameter("s_end_date");
		ti_memcomplaint.put("end_date",_end_date);
	}

	Ti_customerInfo ti_customerInfo=new Ti_customerInfo();
	Ti_personalInfo ti_personalInfo = new Ti_personalInfo();
	Ti_memcomplaintInfo ti_memcomplaintInfo = new Ti_memcomplaintInfo();
	
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_memcomplaintInfo.getListByPage(ti_memcomplaint,Integer.parseInt(iStart),limit);
	int counter = ti_memcomplaintInfo.getCountByObj(ti_memcomplaint);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?seach_deal_state="+_deal_state+"&start_date="+_start_date+"&s_end_date="+_end_date+"&iStart=",Integer.parseInt(iStart),limit);
	
	String para ="seach_deal_state="+_deal_state+"&start_date="+_start_date+"&s_end_date="+_end_date+"&iStart="+Integer.parseInt(iStart);
%>
<html>
  <head>
    
    <title>投诉信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="js_complaint.js"></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>投诉信息管理</h1>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">

	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				&nbsp;按处理状态:
				<select name="seach_deal_state">
					<option  value="">请选择</option>	
					<option  value="1">已处理</option>	
					<option  value="0">未处理</option>	
				</select>	
				&nbsp;按时间段:
				<input name="s_start_date" type="text" id="s_start_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'s_end_date\',{d:-1})}',readOnly:true})" size="15"  width="150px"/>
				-
				<input name="s_end_date" id="s_end_date" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'s_start_date\',{d:1})}',readOnly:true})" size="15" width="150px"/>
				<input name="searchInfo" type="button" value="搜索" onclick="searchForm();"/>	
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
			<th>投诉人</th>  
		  	<th>被投诉企业</th>  	
		  	<th>处理状态</th> 	
		  	<th>投诉时间</th> 	
			<th width="10%">查看</th>
	  		<th width="10%">删除</th>
		</tr>
		<% 
				Hashtable map = new Hashtable();
		  		for(int i=0;i<list.size();i++){
		  			map= (Hashtable)list.get(i);
		  			String info_id="",user_id="",cust_id="",content="",in_date="",deal_state="",deal_user_id="",deal_date="",deal_result="";
		  			  	String cust_name="",user_name="",__deal_state="";
		  			  	
					if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
					if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
					if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
						cust_name = ti_customerInfo.getCustNameByCustId(cust_id);
						user_name=ti_personalInfo.getPersonalNameByPersonalId(user_id);
					
					if(map.get("content")!=null) content = map.get("content").toString();
					if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>19)in_date=in_date.substring(0,19);
					if(map.get("deal_state")!=null) deal_state = map.get("deal_state").toString();
						__deal_state=deal_state;
					if(deal_state.equals("0")) deal_state="未处理";
						if(deal_state.equals("1")) deal_state="已处理";
					
					if(map.get("deal_user_id")!=null) deal_user_id = map.get("deal_user_id").toString();
					if(map.get("deal_date")!=null) deal_date = map.get("deal_date").toString();
						if(deal_date.length()>19)deal_date=deal_date.substring(0,19);
					if(map.get("deal_result")!=null) deal_result = map.get("deal_result").toString();

		  %>
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i%>" id="checkone<%=i%>" value="<%=info_id%>" /></td>
			<td><%=user_name%></td>		
		  	<td><%=cust_name%></td>	  	
		  	<td><a href="index.jsp?deal_state=<%=__deal_state%>"><%=deal_state%></a></td>	  	
			<td><%=in_date%></td>
			<td width="10%"><a href="updateInfo.jsp?info_id=<%=info_id %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="<%if(__deal_state.equals("0"))out.print("修改");else out.print("查看");%>" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=info_id%>','5742');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	
		  <input type="hidden" name="listsize" id="listsize" value="<%=listsize%>" />
		  <input type="hidden" name="pkid" id="pkid" value="" />
		  <input type="hidden" name="bpm_id" id="bpm_id" value="5742" />
	  </form>
</body>

</html>