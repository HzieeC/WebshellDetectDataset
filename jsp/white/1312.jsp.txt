<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page"/>

<%
	String pri_key = bean.GenTradeId();
%>

<html>
  <head>
    <title>案源线索管理绠＄悊</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<h1>鏂板案源线索管理</h1>
	
	<!--
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>-----------------</h4>
		  <span>1----------------銆?/span><br/>
		  <span>2----------------銆?/span>
		  </td>
        </tr>
      </table>
      <br/>
	  -->
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<input name="id" id="id" type="hidden" value="<%=pri_key%>"/>

	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				tittle<font color="red">*</font>
			</td>
			<td><input name="tittle" id="tittle" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				content<font color="red">*</font>
			</td>
			<td><input name="content" id="content" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				area_name<font color="red">*</font>
			</td>
			<td><input name="area_name" id="area_name" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date<font color="red">*</font>
			</td>
			<td><input name="in_date" id="in_date" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				state<font color="red">*</font>
			</td>
			<td><input name="state" id="state" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				src_url<font color="red">*</font>
			</td>
			<td><input name="src_url" id="src_url" size="20" maxlength="20" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="4128" />
				<input type="submit" class="buttoncss" name="tradeSub" value="鎻愪氦" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="杩斿洖" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
