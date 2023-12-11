<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<html>
  <head>
    <title>ti_workexper Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>Add ti_workexper</h1>
	
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
				work_id:
			</td>
			<td><input name="work_id" id="work_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				resume_id:
			</td>
			<td><input name="resume_id" id="resume_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				company:
			</td>
			<td><input name="company" id="company" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_type:
			</td>
			<td><input name="cust_type" id="cust_type" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_size:
			</td>
			<td><input name="cust_size" id="cust_size" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				bisiness:
			</td>
			<td><input name="bisiness" id="bisiness" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				depart:
			</td>
			<td><input name="depart" id="depart" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				career_type:
			</td>
			<td><input name="career_type" id="career_type" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				career:
			</td>
			<td><input name="career" id="career" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				work_date:
			</td>
			<td><input name="work_date" id="work_date" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				salary:
			</td>
			<td><input name="salary" id="salary" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				work_desc:
			</td>
			<td><input name="work_desc" id="work_desc" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="5649" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
