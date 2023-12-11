<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page"/>

<%
	String pri_key = bean.GenTradeId();
	
	String session_user_id="";
	if( session.getAttribute("session_user_id") != null )
	{
		session_user_id = session.getAttribute("session_user_id").toString();
	}	
%>

<html>
  <head>
    <title>支付方式管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<h1>新增支付方式</h1>
	
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
	
	<input name="pay_id" id="pay_id" type="hidden" value="<%=pri_key%>"/>

	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				pay_code<font color="red">*</font>
			</td>
			<td><input name="pay_code" id="pay_code" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pay_name<font color="red">*</font>
			</td>
			<td><input name="pay_name" id="pay_name" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pay_desc:
			</td>
			<td>
			
			<textarea name="pay_desc" id="pay_desc" rows="4" cols="30"></textarea>
			
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pay_account:
			</td>
			<td><input name="pay_account" id="pay_account" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pay_email:
			</td>
			<td><input name="pay_email" id="pay_email" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				passwd:
			</td>
			<td><input name="passwd" id="passwd" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				hand_fare:
			</td>
			<td><input name="hand_fare" id="hand_fare" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				enabled:
			</td>
			<td><input name="enabled" id="enabled" size="20" maxlength="20" type="text" /></td>
		</tr>
		

		<input name="user_id" id="user_id" value="<%=session_user_id%>" type="hidden" />
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="4911" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
