<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<html>
  <head>
    <title>ti_custlevel Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>Add ti_custlevel</h1>
	
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
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_class:
			</td>
			<td><input name="cust_class" id="cust_class" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_start_date:
			</td>
			<td><input name="cust_start_date" id="cust_start_date" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_end_date:
			</td>
			<td><input name="cust_end_date" id="cust_end_date" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_oper_person:
			</td>
			<td><input name="cust_oper_person" id="cust_oper_person" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_oper_date:
			</td>
			<td><input name="cust_oper_date" id="cust_oper_date" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="1481" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
