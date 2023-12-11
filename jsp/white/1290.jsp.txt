<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<html>
  <head>
    <title>ti_user Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>Add ti_user</h1>
	
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
				user_id:
			</td>
			<td><input name="user_id" id="user_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_name:
			</td>
			<td><input name="user_name" id="user_name" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				passwd:
			</td>
			<td><input name="passwd" id="passwd" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_state:
			</td>
			<td><input name="user_state" id="user_state" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				passwd_ques:
			</td>
			<td><input name="passwd_ques" id="passwd_ques" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				passwd_answer:
			</td>
			<td><input name="passwd_answer" id="passwd_answer" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				real_name:
			</td>
			<td><input name="real_name" id="real_name" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				org_id:
			</td>
			<td><input name="org_id" id="org_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				role_code:
			</td>
			<td><input name="role_code" id="role_code" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				last_login:
			</td>
			<td><input name="last_login" id="last_login" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date:
			</td>
			<td><input name="in_date" id="in_date" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pub_user_id:
			</td>
			<td><input name="pub_user_id" id="pub_user_id" type="text" /></td>
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
				<input type="hidden" name="bpm_id" value="2522" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
