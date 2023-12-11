<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<html>
  <head>
    <title>ti_relanguage Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>Add ti_relanguage</h1>
	
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
				lang_id:
			</td>
			<td><input name="lang_id" id="lang_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				resume_id:
			</td>
			<td><input name="resume_id" id="resume_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				lang_type:
			</td>
			<td><input name="lang_type" id="lang_type" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rw_type:
			</td>
			<td><input name="rw_type" id="rw_type" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				ls_type:
			</td>
			<td><input name="ls_type" id="ls_type" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="2757" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
