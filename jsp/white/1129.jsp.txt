<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_interview_note.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_interview_note = new Hashtable();

	
	String req_user_name="";
	
	String req_job_title="";
	
	String req_do_date="";
	
	String req_note_title="";
	
	String req_cust_name="";
	
	String req_in_date="";
	

	
	if(request.getParameter("req_user_name")!=null && !request.getParameter("req_user_name").equals("")){
		req_user_name = request.getParameter("req_user_name");
		ti_interview_note.put("user_name",req_user_name);
	}
	
	if(request.getParameter("req_job_title")!=null && !request.getParameter("req_job_title").equals("")){
		req_job_title = request.getParameter("req_job_title");
		ti_interview_note.put("job_title",req_job_title);
	}
	
	if(request.getParameter("req_do_date")!=null && !request.getParameter("req_do_date").equals("")){
		req_do_date = request.getParameter("req_do_date");
		ti_interview_note.put("do_date",req_do_date);
	}
	
	if(request.getParameter("req_note_title")!=null && !request.getParameter("req_note_title").equals("")){
		req_note_title = request.getParameter("req_note_title");
		ti_interview_note.put("note_title",req_note_title);
	}
	
	if(request.getParameter("req_cust_name")!=null && !request.getParameter("req_cust_name").equals("")){
		req_cust_name = request.getParameter("req_cust_name");
		ti_interview_note.put("cust_name",req_cust_name);
	}
	
	if(request.getParameter("req_in_date")!=null && !request.getParameter("req_in_date").equals("")){
		req_in_date = request.getParameter("req_in_date");
		ti_interview_note.put("in_date",req_in_date);
	}
	

	Ti_interview_noteInfo ti_interview_noteInfo = new Ti_interview_noteInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_interview_noteInfo.getListByPage(ti_interview_note,Integer.parseInt(iStart),limit);
	int counter = ti_interview_noteInfo.getCountByObj(ti_interview_note);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>面试通知信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>面试通知信息管理</h1>
			</td>
			<td>
				
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
	<!--
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>-----------------</h4>
		  <span>1----------------。</span><br/>
		  <span>2----------------。</span>
		  </td>
        </tr>
      </table>
      <br/>
	  -->
	  
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				
				
					应聘人:<input name="req_user_name" type="text" />&nbsp;
		  		
					应聘职位:<input name="req_job_title" type="text" />&nbsp;
		  		
					公司名称:<input name="req_cust_name" type="text" />&nbsp;
		  		

				<input name="searchInfo" type="button" value="搜索" onclick="return search();"/>	
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
			
		  	<th>应聘人</th>
		  	
		  	<th>应聘职位</th>
		  	
		  	
		  	<th>公司名称</th>
		  	
		  	<th>面试时间</th>
		  	
			<th width="10%">查看</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String trade_id="",reume_user_id="",job_id="",do_date="",note_title="",note_desc="",note_addr="",note_result="",rsrv_str1="",rsrv_str2="",rsrv_str3="",cust_id="",in_date="",user_id="",remark="";
					String user_name="",job_title="",cust_name="";
	if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
	if(map.get("user_name")!=null) user_name = map.get("trade_id").toString();
	if(map.get("job_title")!=null) job_title = map.get("job_title").toString();
	if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
	if(map.get("reume_user_id")!=null) reume_user_id = map.get("reume_user_id").toString();
  	if(map.get("job_id")!=null) job_id = map.get("job_id").toString();
  	if(map.get("do_date")!=null) do_date = map.get("do_date").toString();
  	if(map.get("note_title")!=null) note_title = map.get("note_title").toString();
  	if(map.get("note_desc")!=null) note_desc = map.get("note_desc").toString();
  	if(map.get("note_addr")!=null) note_addr = map.get("note_addr").toString();
  	if(map.get("note_result")!=null) note_result = map.get("note_result").toString();
  	if(map.get("rsrv_str1")!=null) rsrv_str1 = map.get("rsrv_str1").toString();
  	if(map.get("rsrv_str2")!=null) rsrv_str2 = map.get("rsrv_str2").toString();
  	if(map.get("rsrv_str3")!=null) rsrv_str3 = map.get("rsrv_str3").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=trade_id %>" /></td>
			
		  	<td><%=user_name%></td>
		  	
		  	<td><%=job_title%></td>
		  	
		  	
		  	<td><%=cust_name%></td>
		  	
		  	<td><%=do_date%></td>
			
			<td width="10%"><a href="updateInfo.jsp?trade_id=<%=trade_id %>"><img src="/program/admin/images/edit.gif" title="查看" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=trade_id%>','7372');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="7372" />
	  </form>
</body>

</html>
