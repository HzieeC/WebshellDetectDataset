<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_quote.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_quote = new Hashtable();

	
	String req_info_id="";
	
	String req_title="";
	
	String req_quote_date="";
	
	String req_quote_name="";
	

	
	if(request.getParameter("req_info_id")!=null && !request.getParameter("req_info_id").equals("")){
		req_info_id = request.getParameter("req_info_id");
		ti_quote.put("info_id",req_info_id);
	}
	
	if(request.getParameter("req_title")!=null && !request.getParameter("req_title").equals("")){
		req_title = request.getParameter("req_title");
		ti_quote.put("title",req_title);
	}
	
	if(request.getParameter("req_quote_date")!=null && !request.getParameter("req_quote_date").equals("")){
		req_quote_date = request.getParameter("req_quote_date");
		ti_quote.put("quote_date",req_quote_date);
	}
	
	if(request.getParameter("req_quote_name")!=null && !request.getParameter("req_quote_name").equals("")){
		req_quote_name = request.getParameter("req_quote_name");
		ti_quote.put("quote_name",req_quote_name);
	}
	

	Ti_quoteInfo ti_quoteInfo = new Ti_quoteInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_quoteInfo.getListByPage(ti_quote,Integer.parseInt(iStart),limit);
	int counter = ti_quoteInfo.getCountByObj(ti_quote);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>求购报价管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>求购报价管理</h1>
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
				
				
					产品:<input name="req_info_id" type="text" />&nbsp;
		  		
					报价标题:<input name="req_title" type="text" />&nbsp;
		  		
					期望回复时间:<input name="req_quote_date" type="text" />&nbsp;
		  		
					报价人名称:<input name="req_quote_name" type="text" />&nbsp;
		  		

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
			
		  	<th>产品</th>
		  	
		  	<th>报价标题</th>
		  	
		  	<th>期望回复时间</th>
		  	
		  	<th>报价人名称</th>
		  	
			<th width="10%">查看</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String trade_id="",info_id="",title="",content="",quote_date="",quote_name="",quote_compname="",quote_email="",quote_phone="",quote_file="",rsrv_str1="",rsrv_str2="",rsrv_str3="",cust_id="",in_date="",user_id="",remark="";
						String biz_title="";
					  if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
  	if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
	if(map.get("biz_title")!=null) biz_title = map.get("biz_title").toString();
	if(biz_title.length() > 15) biz_title = biz_title.substring(0,15) + "...";	
  	if(map.get("title")!=null) title = map.get("title").toString();
	if(title.length() > 15) title = title.substring(0,15) + "...";
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("quote_date")!=null) quote_date = map.get("quote_date").toString();
if(quote_date.length()>19)quote_date=quote_date.substring(0,19);
  	if(map.get("quote_name")!=null) quote_name = map.get("quote_name").toString();
  	if(map.get("quote_compname")!=null) quote_compname = map.get("quote_compname").toString();
  	if(map.get("quote_email")!=null) quote_email = map.get("quote_email").toString();
  	if(map.get("quote_phone")!=null) quote_phone = map.get("quote_phone").toString();
  	if(map.get("quote_file")!=null) quote_file = map.get("quote_file").toString();
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
			
		  	<td><%=biz_title%></td>
		  	
		  	<td><%=title%></td>
		  	
		  	<td><%=quote_date%></td>
		  	
		  	<td><%=quote_name%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?trade_id=<%=trade_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=trade_id%>','2122');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="2122" />
	  </form>
</body>

</html>
