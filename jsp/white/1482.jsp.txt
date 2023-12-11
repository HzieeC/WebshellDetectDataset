<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_emailtem.*" %>
<%@page import="com.bizoss.trade.ti_admin.*" %>
<%@page import="com.bizoss.trade.ti_customer.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable<String,String> ti_emailtem = new Hashtable<String,String>();
	String s_tem_name = "";
	if(request.getParameter("search_tem_name")!=null && !request.getParameter("search_tem_name").equals("")){
		s_tem_name = request.getParameter("search_tem_name");
		ti_emailtem.put("tem_name",s_tem_name);
	}
	String s_tem_type = "";
	if(request.getParameter("search_tem_type")!=null && !request.getParameter("search_tem_type").equals("")){
		s_tem_type = request.getParameter("search_tem_type");
		ti_emailtem.put("tem_type",s_tem_type);
	}	
	String end_date = "";
	if(request.getParameter("txtEndDate")!=null && !request.getParameter("txtEndDate").equals("")){
		end_date = request.getParameter("txtEndDate");
		ti_emailtem.put("end_date",end_date);
	}	
	String start_date = "";
	if(request.getParameter("txtStartDate")!=null && !request.getParameter("txtStartDate").equals("")){
		start_date = request.getParameter("txtStartDate");
		ti_emailtem.put("start_date",start_date);
	}	
	
	String s_cust_name = "";
	if(request.getParameter("search_cust_name")!=null && !request.getParameter("search_cust_name").equals("")){
		s_cust_name = request.getParameter("search_cust_name");
		ti_emailtem.put("cust_name",s_cust_name);
	}
			
	Ti_adminInfo userInfo = new Ti_adminInfo();
	Ti_customerInfo custInfo = new Ti_customerInfo();
	Ti_emailtemInfo ti_emailtemInfo = new Ti_emailtemInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_emailtemInfo.getListByPage(ti_emailtem,Integer.parseInt(iStart),limit);
	//out.print("=============list========"+list);
	int counter = ti_emailtemInfo.getCountByObj(ti_emailtem);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?search_tem_name="+s_tem_name+"&search_tem_type="+s_tem_type+"&txtEndDate="+end_date+"&txtStartDate="+start_date+"&search_cust_name="+s_cust_name+"&iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>邮件内容管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="emailtem.js"></script>
	<script type="text/javascript" src="js_commen.js"></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>企业邮件模板管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	

	<form action="index.jsp" name="indexForm" method="post">
	
	<table width="100%" class="dl_su" cellpadding="0" cellspacing="0" >
		<tr>
		
			<td align="left" >
			模板名称:<input name="search_tem_name" id="search_tem_name" type="text" />

			模板类型:
				<select class="input" name="search_tem_type" id="search_tem_type" >
					<option value="">请选择</option>
					<option value="0">系统模板</option>
					<option value="1">业务模板</option>
			   </select>
				&nbsp;按发布时间段选择: 
			 <input name="txtStartDate" type="text" id="txtStartDate" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'txtEndDate\',{d:-1})}',readOnly:true})" size="15" />
					- 
			<input name="txtEndDate" id="txtEndDate" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'txtStartDate\',{d:1})}',readOnly:true})" size="15"/>
				
				<input name="searchInfo" type="button" onclick="return search()" value="查询"/>	
				
			</td>
		</tr>
		
	</table>

	<table width="100%" cellpadding="0"  class="tablehe"  cellspacing="0" border="0">
		<tr><td align="center" ><%=pageString %></td></tr>
	</table>
	
	<% 
		int listsize = 0;
		if(list!=null && list.size()>0){
			listsize = list.size();
	%>
	
	<table width="100%" class="dl_bg" cellpadding="0"  cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td >
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>

		  	<th>模板类型</th>
		  	
		  	<th>邮件标题</th>
		  	
		  	<th>发布时间</th>
		  	
		  	<th>发布人</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
				Hashtable map = new Hashtable();
		  		for(int i=0;i<list.size();i++){
		  			map = (Hashtable)list.get(i);
		  			String tem_id="",cust_id="",tem_type="",tem_name="",content="",in_date="",user_id="",user_name="",cust_name="";

		  			  	if(map.get("tem_id")!=null) tem_id = map.get("tem_id").toString();
						if(map.get("cust_id")!=null)  {
							cust_id = map.get("cust_id").toString();
							if(!user_id.equals("") && user_id != null) {	
								cust_name = custInfo.getCustNameByCustId(cust_id);
							}
						}
						//cust_name = map.get("cust_name").toString();
						if(map.get("tem_type")!=null) tem_type = map.get("tem_type").toString();
						if(map.get("tem_name")!=null) tem_name = map.get("tem_name").toString();
						if(tem_name.length()>19) tem_name=tem_name.substring(0,19)+"...";
						if(map.get("content")!=null) content = map.get("content").toString();
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>19)in_date=in_date.substring(0,19);
						if(map.get("user_id")!=null) {
							user_id = map.get("user_id").toString();
							if(!user_id.equals("") && user_id != null) {	
								user_name = userInfo.getUserNameByPK(user_id);
							}
						}

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i%>" id="checkone<%=i%>" value="<%=tem_id%>" /></td>
			<input type="hidden" name="tem_type<%=i%>" id="tem_type<%=i%>" value="<%=tem_type%>">
		  	<td><%
			if(  ("1").equals(tem_type) ){
			%>
			业务模板
			<%
			} else {
			%>
			系统模板
			<%
			}
			%></td>
		  	
		  	<td><%=tem_name%></td>
		  	
		  	<td><%=in_date%></td>
		  	
		  	<td><%=user_name%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?tem_id=<%=tem_id%>&search_tem_name=<%=s_tem_name%>&search_tem_type=<%=s_tem_type%>&txtEndDate=<%=end_date%>&txtStartDate=<%=start_date%>&search_cust_name=<%=s_cust_name%>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
			
			<%if( ("1").equals(tem_type) ){
			%>
			
	  		<td width="10%"><a href="javascript:delOption('<%=tem_id%>','6758')"><img src="/program/admin/images/delete.gif" title="删除" /></a></td>
			
			<%} else {
			
			%>
			
			<td width="10%">系统模板不可删除</td>
			
			<%
			}
			%>
			
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%"  class="dl_bg"  cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td >
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" class="tablehe" cellpadding="0" cellspacing="0" border="0">
		<tr><td align="center" ><%=pageString %></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="6758" />
	  </form>
</body>

</html>
