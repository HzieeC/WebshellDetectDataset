<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_collect.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_collect = new Hashtable();

	
	String req_cust_id="";
	
	String req_title="";
	
	String req_info_url="";
	
	String req_info_type="";
	
	String req_in_date="";
	
	String req_user_id="";
	

	
	if(request.getParameter("req_cust_id")!=null && !request.getParameter("req_cust_id").equals("")){
		req_cust_id = request.getParameter("req_cust_id");
		ti_collect.put("cust_id",req_cust_id);
	}
	
	if(request.getParameter("req_title")!=null && !request.getParameter("req_title").equals("")){
		req_title = request.getParameter("req_title");
		ti_collect.put("title",req_title);
	}
	
	if(request.getParameter("req_info_url")!=null && !request.getParameter("req_info_url").equals("")){
		req_info_url = request.getParameter("req_info_url");
		ti_collect.put("info_url",req_info_url);
	}
	
	if(request.getParameter("req_info_type")!=null && !request.getParameter("req_info_type").equals("")){
		req_info_type = request.getParameter("req_info_type");
		ti_collect.put("info_type",req_info_type);
	}
	
	if(request.getParameter("req_in_date")!=null && !request.getParameter("req_in_date").equals("")){
		req_in_date = request.getParameter("req_in_date");
		ti_collect.put("in_date",req_in_date);
	}
	
	if(request.getParameter("req_user_id")!=null && !request.getParameter("req_user_id").equals("")){
		req_user_id = request.getParameter("req_user_id");
		ti_collect.put("user_id",req_user_id);
	}
	

	Ti_collectInfo ti_collectInfo = new Ti_collectInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_collectInfo.getListByPage(ti_collect,Integer.parseInt(iStart),limit);
	int counter = ti_collectInfo.getCountByObj(ti_collect);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>在线客服111管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>在线客服111管理</h1>
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
				
				
					cust_id:<input name="req_cust_id" type="text" />&nbsp;
		  		
					标题:<input name="req_title" type="text" />&nbsp;
		  		
					info_url:<input name="req_info_url" type="text" />&nbsp;
		  		
					info_type:<input name="req_info_type" type="text" />&nbsp;
		  		
					in_date:<input name="req_in_date" type="text" />&nbsp;
		  		
					user_id:<input name="req_user_id" type="text" />&nbsp;
		  		

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
			
		  	<th>cust_id</th>
		  	
		  	<th>标题</th>
		  	
		  	<th>info_url</th>
		  	
		  	<th>info_type</th>
		  	
		  	<th>in_date</th>
		  	
		  	<th>user_id</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String trade_id="",cust_id="",title="",info_url="",info_type="",in_date="",user_id="";
		  			  	if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("info_url")!=null) info_url = map.get("info_url").toString();
  	if(map.get("info_type")!=null) info_type = map.get("info_type").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=trade_id %>" /></td>
			
		  	<td><%=cust_id%></td>
		  	
		  	<td><%=title%></td>
		  	
		  	<td><%=info_url%></td>
		  	
		  	<td><%=info_type%></td>
		  	
		  	<td><%=in_date%></td>
		  	
		  	<td><%=user_id%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?trade_id=<%=trade_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=trade_id%>','2249');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="2249" />
	  </form>
</body>

</html>
