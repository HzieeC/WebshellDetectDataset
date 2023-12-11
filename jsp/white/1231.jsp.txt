<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_auction.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_auction = new Hashtable();

	
	String req_auction_name="";
	
	String req_auction_desc="";
	
	String req_good_id="";
	
	String req_start_date="";
	
	String req_end_date="";
	
	String req_start_price="";
	
	String req_a_price="";
	
	String req_rate_price="";
	
	String req_margin="";
	
	String req_user_id="";
	
	String req_in_date="";
	

	
	if(request.getParameter("req_auction_name")!=null && !request.getParameter("req_auction_name").equals("")){
		req_auction_name = request.getParameter("req_auction_name");
		ti_auction.put("auction_name",req_auction_name);
	}
	
	if(request.getParameter("req_auction_desc")!=null && !request.getParameter("req_auction_desc").equals("")){
		req_auction_desc = request.getParameter("req_auction_desc");
		ti_auction.put("auction_desc",req_auction_desc);
	}
	
	if(request.getParameter("req_good_id")!=null && !request.getParameter("req_good_id").equals("")){
		req_good_id = request.getParameter("req_good_id");
		ti_auction.put("good_id",req_good_id);
	}
	
	if(request.getParameter("req_start_date")!=null && !request.getParameter("req_start_date").equals("")){
		req_start_date = request.getParameter("req_start_date");
		ti_auction.put("start_date",req_start_date);
	}
	
	if(request.getParameter("req_end_date")!=null && !request.getParameter("req_end_date").equals("")){
		req_end_date = request.getParameter("req_end_date");
		ti_auction.put("end_date",req_end_date);
	}
	
	if(request.getParameter("req_start_price")!=null && !request.getParameter("req_start_price").equals("")){
		req_start_price = request.getParameter("req_start_price");
		ti_auction.put("start_price",req_start_price);
	}
	
	if(request.getParameter("req_a_price")!=null && !request.getParameter("req_a_price").equals("")){
		req_a_price = request.getParameter("req_a_price");
		ti_auction.put("a_price",req_a_price);
	}
	
	if(request.getParameter("req_rate_price")!=null && !request.getParameter("req_rate_price").equals("")){
		req_rate_price = request.getParameter("req_rate_price");
		ti_auction.put("rate_price",req_rate_price);
	}
	
	if(request.getParameter("req_margin")!=null && !request.getParameter("req_margin").equals("")){
		req_margin = request.getParameter("req_margin");
		ti_auction.put("margin",req_margin);
	}
	
	if(request.getParameter("req_user_id")!=null && !request.getParameter("req_user_id").equals("")){
		req_user_id = request.getParameter("req_user_id");
		ti_auction.put("user_id",req_user_id);
	}
	
	if(request.getParameter("req_in_date")!=null && !request.getParameter("req_in_date").equals("")){
		req_in_date = request.getParameter("req_in_date");
		ti_auction.put("in_date",req_in_date);
	}
	

	Ti_auctionInfo ti_auctionInfo = new Ti_auctionInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_auctionInfo.getListByPage(ti_auction,Integer.parseInt(iStart),limit);
	int counter = ti_auctionInfo.getCountByObj(ti_auction);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>拍卖信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>拍卖信息管理</h1>
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
				
				
					活动名称:<input name="req_auction_name" type="text" />&nbsp;
		  		
				<!--活动描述:<input name="req_auction_desc" type="text" />&nbsp; -->
		  		
					拍卖商品名称:<input name="req_good_id" type="text" />&nbsp; <!-- 可以换成名称 -->
		  		
					拍卖开始时间:<input name="req_start_date" type="text" />&nbsp;
		  		
					拍卖结束时间:<input name="req_end_date" type="text" />&nbsp;
		  		
					<!--	起拍价:<input name="req_start_price" type="text" />&nbsp;
		  		
					一口价:<input name="req_a_price" type="text" />&nbsp;
		  		
					加价幅度:<input name="req_rate_price" type="text" />&nbsp;
		  		
					保证金:<input name="req_margin" type="text" />&nbsp;-->
		  		<!-- 操作人: -->
					<input name="req_user_id" type="hidden"  />&nbsp;
		  		
				<!-- 操作时间:<input name="req_in_date" type="text" />&nbsp; -->
		  		

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
			
		  	<th>活动名称</th>
		  	
		  	<th>拍卖商品名称</th>
		  	
		  	<th>拍卖开始时间</th>
		  	
		  	<th>拍卖结束时间</th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String auction_id="",auction_name="",auction_desc="",good_id="",start_date="",end_date="",start_price="",a_price="",rate_price="",margin="",user_id="",in_date="";
		  			  	if(map.get("auction_id")!=null) auction_id = map.get("auction_id").toString();
  	if(map.get("auction_name")!=null) auction_name = map.get("auction_name").toString();
  	if(map.get("auction_desc")!=null) auction_desc = map.get("auction_desc").toString();
  	if(map.get("good_id")!=null) good_id = map.get("good_id").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
if(start_date.length()>19)start_date=start_date.substring(0,19);
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
if(end_date.length()>19)end_date=end_date.substring(0,19);
  	if(map.get("start_price")!=null) start_price = map.get("start_price").toString();
  	if(map.get("a_price")!=null) a_price = map.get("a_price").toString();
  	if(map.get("rate_price")!=null) rate_price = map.get("rate_price").toString();
  	if(map.get("margin")!=null) margin = map.get("margin").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=auction_id %>" /></td>
			
		  	<td><%=auction_name%></td>
		  	
		  	<td><%=auction_desc%></td>
		  	
		  	<td><%=good_id%></td>
		  	
		  	<td><%=start_date%></td>
		  	
		  	<td><%=end_date%></td>
		  	
		  	<td><%=start_price%></td>
		  	
		  	<td><%=a_price%></td>
		  	
		  	<td><%=rate_price%></td>
		  	
		  	<td><%=margin%></td>
		  	
		  	<td><%=user_id%></td>
		  	
		  	<td><%=in_date%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?auction_id=<%=auction_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=auction_id%>','2836');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="2836" />
	  </form>
</body>

</html>
