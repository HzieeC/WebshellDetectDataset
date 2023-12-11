<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<html>
  <head>
    <title>ti_cust_relation Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>Add ti_cust_relation</h1>
	
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
				relation_id:
			</td>
			<td><input name="relation_id" id="relation_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id_a:
			</td>
			<td><input name="cust_id_a" id="cust_id_a" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id_b:
			</td>
			<td><input name="cust_id_b" id="cust_id_b" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				type_a:
			</td>
			<td><input name="type_a" id="type_a" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				type_b:
			</td>
			<td><input name="type_b" id="type_b" type="text" /></td>
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
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="8200" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
