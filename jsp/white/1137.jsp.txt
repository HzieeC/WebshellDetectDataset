<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_onlineservice.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_onlineservice = new Hashtable();

	
	String req_contact_name="";
	
	String req_account="";
	
	String req_sort_no="";
	
	String req_is_display="";
	
	String req_remark="";
	

	
	if(request.getParameter("req_contact_name")!=null && !request.getParameter("req_contact_name").equals("")){
		req_contact_name = request.getParameter("req_contact_name");
		ti_onlineservice.put("contact_name",req_contact_name);
	}
	
	if(request.getParameter("req_account")!=null && !request.getParameter("req_account").equals("")){
		req_account = request.getParameter("req_account");
		ti_onlineservice.put("account",req_account);
	}
	
	if(request.getParameter("req_sort_no")!=null && !request.getParameter("req_sort_no").equals("")){
		req_sort_no = request.getParameter("req_sort_no");
		ti_onlineservice.put("sort_no",req_sort_no);
	}
	
	if(request.getParameter("req_is_display")!=null && !request.getParameter("req_is_display").equals("")){
		req_is_display = request.getParameter("req_is_display");
		ti_onlineservice.put("is_display",req_is_display);
	}
	
	if(request.getParameter("req_remark")!=null && !request.getParameter("req_remark").equals("")){
		req_remark = request.getParameter("req_remark");
		ti_onlineservice.put("remark",req_remark);
	}
	

	Ti_onlineserviceInfo ti_onlineserviceInfo = new Ti_onlineserviceInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_onlineserviceInfo.getListByPage(ti_onlineservice,Integer.parseInt(iStart),limit);
	int counter = ti_onlineserviceInfo.getCountByObj(ti_onlineservice);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>在线客服2管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>在线客服2管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
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
				
				
					contact_name:<input name="req_contact_name" type="text" />&nbsp;
		  		
					account:<input name="req_account" type="text" />&nbsp;
		  		
					sort_no:<input name="req_sort_no" type="text" />&nbsp;
		  		
					is_display:<input name="req_is_display" type="text" />&nbsp;
		  		
					remark:<input name="req_remark" type="text" />&nbsp;
		  		

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
			
		  	<th>contact_name</th>
		  	
		  	<th>ser_type</th>
		  	
		  	<th>account</th>
		  	
		  	<th>sort_no</th>
		  	
		  	<th>is_display</th>
		  	
		  	<th>remark</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String info_id="",contact_name="",ser_type="",account="",sort_no="",is_display="",remark="";
		  			  	if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
  	if(map.get("contact_name")!=null) contact_name = map.get("contact_name").toString();
  	if(map.get("ser_type")!=null) ser_type = map.get("ser_type").toString();
  	if(map.get("account")!=null) account = map.get("account").toString();
  	if(map.get("sort_no")!=null) sort_no = map.get("sort_no").toString();
  	if(map.get("is_display")!=null) is_display = map.get("is_display").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=info_id %>" /></td>
			
		  	<td><%=contact_name%></td>
		  	
		  	<td><%=ser_type%></td>
		  	
		  	<td><%=account%></td>
		  	
		  	<td><%=sort_no%></td>
		  	
		  	<td><%=is_display%></td>
		  	
		  	<td><%=remark%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?info_id=<%=info_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=info_id%>','2422');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="2422" />
	  </form>
</body>

</html>
