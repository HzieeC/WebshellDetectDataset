<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<html>
  <head>
    <title>ti_finance_history Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>Add ti_finance_history</h1>
	
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
				trade_id:
			</td>
			<td><input name="trade_id" id="trade_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				num:
			</td>
			<td><input name="num" id="num" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				vmoney:
			</td>
			<td><input name="vmoney" id="vmoney" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				reason:
			</td>
			<td><input name="reason" id="reason" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date:
			</td>
			<td><input name="in_date" id="in_date" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id:
			</td>
			<td><input name="user_id" id="user_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				remark:
			</td>
			<td><input name="remark" id="remark" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="1133" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
