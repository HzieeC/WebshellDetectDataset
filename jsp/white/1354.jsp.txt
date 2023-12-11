<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<html>
  <head>
    <title>ti_admin Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>Add ti_admin</h1>
	
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
				passwd:
			</td>
			<td><input name="passwd" id="passwd" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				real_name:
			</td>
			<td><input name="real_name" id="real_name" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				phone:
			</td>
			<td><input name="phone" id="phone" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				email:
			</td>
			<td><input name="email" id="email" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cellphone:
			</td>
			<td><input name="cellphone" id="cellphone" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				fax:
			</td>
			<td><input name="fax" id="fax" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				qq:
			</td>
			<td><input name="qq" id="qq" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				msn:
			</td>
			<td><input name="msn" id="msn" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_state:
			</td>
			<td><input name="user_state" id="user_state" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_type:
			</td>
			<td><input name="user_type" id="user_type" type="text" /></td>
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
				oper_date:
			</td>
			<td><input name="oper_date" id="oper_date" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				staff_id:
			</td>
			<td><input name="staff_id" id="staff_id" type="text" /></td>
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
				<input type="hidden" name="bpm_id" value="4114" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
