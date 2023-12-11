<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_wholesale.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_wholesale = new Hashtable();

	
	String req_good_id="";
	
	String req_good_name="";
	
	String req_prices="";
	
	String req_enabled="";
	
	String req_in_date="";
	

	
	if(request.getParameter("req_good_id")!=null && !request.getParameter("req_good_id").equals("")){
		req_good_id = request.getParameter("req_good_id");
		ti_wholesale.put("good_id",req_good_id);
	}
	
	if(request.getParameter("req_good_name")!=null && !request.getParameter("req_good_name").equals("")){
		req_good_name = request.getParameter("req_good_name");
		ti_wholesale.put("good_name",req_good_name);
	}
	
	if(request.getParameter("req_prices")!=null && !request.getParameter("req_prices").equals("")){
		req_prices = request.getParameter("req_prices");
		ti_wholesale.put("prices",req_prices);
	}
	
	if(request.getParameter("req_enabled")!=null && !request.getParameter("req_enabled").equals("")){
		req_enabled = request.getParameter("req_enabled");
		ti_wholesale.put("enabled",req_enabled);
	}
	
	if(request.getParameter("req_in_date")!=null && !request.getParameter("req_in_date").equals("")){
		req_in_date = request.getParameter("req_in_date");
		ti_wholesale.put("in_date",req_in_date);
	}
	

	Ti_wholesaleInfo ti_wholesaleInfo = new Ti_wholesaleInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_wholesaleInfo.getListByPage(ti_wholesale,Integer.parseInt(iStart),limit);
	int counter = ti_wholesaleInfo.getCountByObj(ti_wholesale);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>批发管理管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>批发管理管理</h1>
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
				
				
					0:<input name="req_good_id" type="text" />&nbsp;
		  		
					0:<input name="req_good_name" type="text" />&nbsp;
		  		
					0:<input name="req_prices" type="text" />&nbsp;
		  		
					0:<input name="req_enabled" type="text" />&nbsp;
		  		
					0:<input name="req_in_date" type="text" />&nbsp;
		  		

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
			
		  	<th>0</th>
		  	
		  	<th>0</th>
		  	
		  	<th>0</th>
		  	
		  	<th>0</th>
		  	
		  	<th>0</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String act_id="",good_id="",good_name="",prices="",enabled="",user_id="",in_date="";
		  			  	if(map.get("act_id")!=null) act_id = map.get("act_id").toString();
  	if(map.get("good_id")!=null) good_id = map.get("good_id").toString();
  	if(map.get("good_name")!=null) good_name = map.get("good_name").toString();
  	if(map.get("prices")!=null) prices = map.get("prices").toString();
  	if(map.get("enabled")!=null) enabled = map.get("enabled").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=act_id %>" /></td>
			
		  	<td><%=good_id%></td>
		  	
		  	<td><%=good_name%></td>
		  	
		  	<td><%=prices%></td>
		  	
		  	<td><%=enabled%></td>
		  	
		  	<td><%=in_date%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?act_id=<%=act_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=act_id%>','6201');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="6201" />
	  </form>
</body>

</html>
