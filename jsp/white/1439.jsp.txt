<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<html>
  <head>
    <title>举报信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>Add ti_memreport</h1>
	<%
	
	  String _user_id="";
if( session.getAttribute("session_user_id") != null )
	{
		_user_id = session.getAttribute("session_user_id").toString();
	}
	
	
	%>

	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				info_id:
			</td>
			<td><input name="info_id" id="info_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id:
			</td>
			<td><input name="user_id" id="user_id" value="" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				goods_id:
			</td>
			<td><input name="goods_id" id="goods_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				content:
			</td>
			<td><input name="content" id="content" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date:
			</td>
			<td><input name="in_date" id="in_date" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				deal_state:
			</td>
			<td><input name="deal_state" id="deal_state" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				deal_user_id:
			</td>
			<td><input name="deal_user_id" id="deal_user_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				deal_date:
			</td>
			<td><input name="deal_date" id="deal_date" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				deal_result:
			</td>
			<td><input name="deal_result" id="deal_result" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="4294" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
