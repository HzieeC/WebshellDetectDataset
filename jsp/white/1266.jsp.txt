<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page"/>

<%
	String pri_key = bean.GenTradeId();
%>

<html>
  <head>
    <title>报价回复管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<h1>新增报价回复</h1>
	
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
	
	<input name="re_trade_id" id="re_trade_id" type="hidden" value="<%=pri_key%>"/>

	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				trade_id<font color="red">*</font>
			</td>
			<td><input name="trade_id" id="trade_id" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				title<font color="red">*</font>
			</td>
			<td><input name="title" id="title" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				content<font color="red">*</font>
			</td>
			<td><input name="content" id="content" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_st1<font color="red">*</font>
			</td>
			<td><input name="rsrv_st1" id="rsrv_st1" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str2<font color="red">*</font>
			</td>
			<td><input name="rsrv_str2" id="rsrv_str2" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str3<font color="red">*</font>
			</td>
			<td><input name="rsrv_str3" id="rsrv_str3" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id<font color="red">*</font>
			</td>
			<td><input name="cust_id" id="cust_id" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date<font color="red">*</font>
			</td>
			<td><input name="in_date" id="in_date" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id<font color="red">*</font>
			</td>
			<td><input name="user_id" id="user_id" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				remark<font color="red">*</font>
			</td>
			<td><input name="remark" id="remark" size="20" maxlength="20" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="8612" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
