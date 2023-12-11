<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page"/>

<%
	String pri_key = bean.GenTradeId();
%>

<html>
  <head>
    <title>拍卖信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<h1>新增拍卖信息</h1>
	
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
	
	<input name="auction_id" id="auction_id" type="hidden" value="<%=pri_key%>"/>

	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				auction_name<font color="red">*</font>
			</td>
			<td><input name="auction_name" id="auction_name" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				auction_desc<font color="red">*</font>
			</td>
			<td><input name="auction_desc" id="auction_desc" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				good_id<font color="red">*</font>
			</td>
			<td><input name="good_id" id="good_id" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				start_date<font color="red">*</font>
			</td>
			<td><input name="start_date" id="start_date" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				end_date<font color="red">*</font>
			</td>
			<td><input name="end_date" id="end_date" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				start_price<font color="red">*</font>
			</td>
			<td><input name="start_price" id="start_price" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				a_price<font color="red">*</font>
			</td>
			<td><input name="a_price" id="a_price" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rate_price<font color="red">*</font>
			</td>
			<td><input name="rate_price" id="rate_price" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				margin<font color="red">*</font>
			</td>
			<td><input name="margin" id="margin" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id<font color="red">*</font>
			</td>
			<td><input name="user_id" id="user_id" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date<font color="red">*</font>
			</td>
			<td><input name="in_date" id="in_date" size="20" maxlength="20" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="4635" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
