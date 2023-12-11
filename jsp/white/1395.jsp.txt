<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_bidinfo.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_bidinfo = new Hashtable();
	String s_bidding_cust_name = "";
	if(request.getParameter("s_bidding_cust_name")!=null && !request.getParameter("s_bidding_cust_name").equals("")){
		s_bidding_cust_name = request.getParameter("s_bidding_cust_name");
		ti_bidinfo.put("bidding_cust_name",s_bidding_cust_name);
	}
	String s_bidding_title = "";
	if(request.getParameter("s_bidding_title")!=null && !request.getParameter("s_bidding_title").equals("")){
		s_bidding_title = request.getParameter("s_bidding_title");
		ti_bidinfo.put("bidding_title",s_bidding_title);
	}	
	Ti_bidinfoInfo ti_bidinfoInfo = new Ti_bidinfoInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_bidinfoInfo.getListByPage(ti_bidinfo,Integer.parseInt(iStart),limit);
	int counter = ti_bidinfoInfo.getCountByObj(ti_bidinfo);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>竞标管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="js_commen.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>竞标管理</h1>
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
				招标公司:<input name="s_bidding_cust_name" type="text" />
				招标标题:<input name="s_bidding_title" type="text" />
				<input name="searchInfo" type="button" value="查询" onclick="searcher();"/>	
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

			<th>招标公司</th>	
			
		  	<th>招标标题</th>
			
		  	<th>竞标人名称</th>			

		  	<th>竞标状态</th>			

		  	<th>竞标时间</th>


			<th width="10%">操作</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String trade_id="",info_id="",title="",price="",bid_date="",content="",bid_state="",bid_name="",bid_compname="",bid_email="",bid_phone="",rsrv_str1="",rsrv_str2="",rsrv_str3="",cust_id="",in_date="",user_id="",remark="";
	String bidding_cust_name = "",bidding_title="",max_state="";
	if(map.get("max_state")!=null) max_state = map.get("max_state").toString();
	if(map.get("bidding_title")!=null) bidding_title = map.get("bidding_title").toString();
	if(map.get("bidding_cust_name")!=null) bidding_cust_name = map.get("bidding_cust_name").toString();
	if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
  	if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("price")!=null) price = map.get("price").toString();
  	if(map.get("bid_date")!=null) bid_date = map.get("bid_date").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("bid_state")!=null) bid_state = map.get("bid_state").toString();
  	if(map.get("bid_name")!=null) bid_name = map.get("bid_name").toString();
  	if(map.get("bid_compname")!=null) bid_compname = map.get("bid_compname").toString();
  	if(map.get("bid_email")!=null) bid_email = map.get("bid_email").toString();
  	if(map.get("bid_phone")!=null) bid_phone = map.get("bid_phone").toString();
  	if(map.get("rsrv_str1")!=null) rsrv_str1 = map.get("rsrv_str1").toString();
  	if(map.get("rsrv_str2")!=null) rsrv_str2 = map.get("rsrv_str2").toString();
  	if(map.get("rsrv_str3")!=null) rsrv_str3 = map.get("rsrv_str3").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>10)in_date=in_date.substring(0,10);
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
	String bid_state_string = "未中标";
	if(!bid_state.equals("0") && bid_state.equals(max_state)){
		bid_state_string = "第"+bid_state+"轮中标";
	}

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=trade_id %>" /></td>
			
		  	<td><%=bidding_cust_name%></td>
		  	
		  	<td><%=bidding_title%></td>

		  	<td><%=bid_name%></td>		  	

		  	<td><%=bid_state_string%></td>			
		  	
		  	<td><%=in_date%></td>
		  	
		  	
		  	
			<td width="10%"><a href="updateInfo.jsp?trade_id=<%=trade_id %>"><img src="/program/admin/images/edit.gif" title="操作" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=trade_id%>','4787');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="4787" />
	  </form>
</body>

</html>
