<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_auction.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>拍卖信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>

  <% 
  	String auction_id="";
  	if(request.getParameter("auction_id")!=null) auction_id = request.getParameter("auction_id");
  	Ti_auctionInfo ti_auctionInfo = new Ti_auctionInfo();
  	List list = ti_auctionInfo.getListByPk(auction_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String auction_name="",auction_desc="",good_id="",start_date="",end_date="",start_price="",a_price="",rate_price="",margin="",user_id="",in_date="";
  	if(map.get("auction_name")!=null) auction_name = map.get("auction_name").toString();
  	if(map.get("auction_desc")!=null) auction_desc = map.get("auction_desc").toString();
  	if(map.get("good_id")!=null) good_id = map.get("good_id").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
  	if(map.get("start_price")!=null) start_price = map.get("start_price").toString();
  	if(map.get("a_price")!=null) a_price = map.get("a_price").toString();
  	if(map.get("rate_price")!=null) rate_price = map.get("rate_price").toString();
  	if(map.get("margin")!=null) margin = map.get("margin").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();

  %>
	
	<h1>修改拍卖信息</h1>
	
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
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				auction_name<font color="red">*</font>
			</td>
			<td><input name="auction_name" id="auction_name" size="20" maxlength="20" value="<%=auction_name %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				auction_desc<font color="red">*</font>
			</td>
			<td><input name="auction_desc" id="auction_desc" size="20" maxlength="20" value="<%=auction_desc %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				good_id<font color="red">*</font>
			</td>
			<td><input name="good_id" id="good_id" size="20" maxlength="20" value="<%=good_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				start_date<font color="red">*</font>
			</td>
			<td><input name="start_date" id="start_date" size="20" maxlength="20" value="<%=start_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				end_date<font color="red">*</font>
			</td>
			<td><input name="end_date" id="end_date" size="20" maxlength="20" value="<%=end_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				start_price<font color="red">*</font>
			</td>
			<td><input name="start_price" id="start_price" size="20" maxlength="20" value="<%=start_price %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				a_price<font color="red">*</font>
			</td>
			<td><input name="a_price" id="a_price" size="20" maxlength="20" value="<%=a_price %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rate_price<font color="red">*</font>
			</td>
			<td><input name="rate_price" id="rate_price" size="20" maxlength="20" value="<%=rate_price %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				margin<font color="red">*</font>
			</td>
			<td><input name="margin" id="margin" size="20" maxlength="20" value="<%=margin %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id<font color="red">*</font>
			</td>
			<td><input name="user_id" id="user_id" size="20" maxlength="20" value="<%=user_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date<font color="red">*</font>
			</td>
			<td><input name="in_date" id="in_date" size="20" maxlength="20" value="<%=in_date %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="1169" />
	  			<input type="hidden" name="auction_id" value="<%=auction_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
