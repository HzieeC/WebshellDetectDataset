<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_tradeinfo.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_tradeinfo = new Hashtable();

	
	String req_info_id="";
	
	String req_trade_state="";
	
	String req_cust_id="";
	
	String req_user_id="";
	
	String req_content="";
	
	String req_in_date="";
	

	
	if(request.getParameter("info_id")!=null && !request.getParameter("info_id").equals("")){
		req_info_id = request.getParameter("info_id");
		ti_tradeinfo.put("info_id",req_info_id);
	}
	
	if(request.getParameter("req_trade_state")!=null && !request.getParameter("req_trade_state").equals("")){
		req_trade_state = request.getParameter("req_trade_state");
		ti_tradeinfo.put("trade_state",req_trade_state);
	}
	
	if(request.getParameter("req_cust_id")!=null && !request.getParameter("req_cust_id").equals("")){
		req_cust_id = request.getParameter("req_cust_id");
		ti_tradeinfo.put("cust_id",req_cust_id);
	}
	
	if(request.getParameter("req_user_id")!=null && !request.getParameter("req_user_id").equals("")){
		req_user_id = request.getParameter("req_user_id");
		ti_tradeinfo.put("user_id",req_user_id);
	}
	
	if(request.getParameter("req_content")!=null && !request.getParameter("req_content").equals("")){
		req_content = request.getParameter("req_content");
		ti_tradeinfo.put("content",req_content);
	}
	
	if(request.getParameter("req_in_date")!=null && !request.getParameter("req_in_date").equals("")){
		req_in_date = request.getParameter("req_in_date");
		ti_tradeinfo.put("in_date",req_in_date);
	}
	

	Ti_tradeinfoInfo ti_tradeinfoInfo = new Ti_tradeinfoInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_tradeinfoInfo.getListByPage(ti_tradeinfo,Integer.parseInt(iStart),limit);
	int counter = ti_tradeinfoInfo.getCountByObj(ti_tradeinfo);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>交易信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>交易信息管理</h1>
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
				
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			
			
		  	<th>客户名称</th>
		  	
		  	<th>交易状态</th>
		  	
		  	<th width="50%">留言内容</th>

		  	<th>发布时间</th>
		  	
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String trade_id="",info_id="",trade_state="",cust_id="",user_id="",content="",in_date="";
		  			  	if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
  	if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
  	if(map.get("trade_state")!=null) trade_state = map.get("trade_state").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);

		  %>
		
		<tr>
			
			
		  	<td><%=cust_id%></td>
		  	
		  	<td><%=trade_state%></td>
			
		  	<td width="50%"><%=content%></td>
		  	
		  	<td><%=in_date%></td>
		
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				
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
		<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="0079" />
	  </form>
</body>

</html>
