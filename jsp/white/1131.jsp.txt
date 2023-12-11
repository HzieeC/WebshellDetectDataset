<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page"/>

<%
	String pri_key = bean.GenTradeId();
%>

<html>
  <head>
    <title>面试通知信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<h1>新增面试通知信息</h1>
	

	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<input name="trade_id" id="trade_id" type="hidden" value="<%=pri_key%>"/>

	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				应聘人<font color="red">*</font>
			</td>
			<td><input name="reume_user_id" id="reume_user_id" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				应聘职位<font color="red">*</font>
			</td>
			<td><input name="job_id" id="job_id" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				面试时间<font color="red">*</font>
			</td>
			<td><input name="do_date" id="do_date" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				通知标题<font color="red">*</font>
			</td>
			<td><input name="note_title" id="note_title" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				通知描述:
			</td>
			<td><input name="note_desc" id="note_desc" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				通知地址:
			</td>
			<td><input name="note_addr" id="note_addr" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				通知结果:
			</td>
			<td><input name="note_result" id="note_result" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str1:
			</td>
			<td><input name="rsrv_str1" id="rsrv_str1" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str2:
			</td>
			<td><input name="rsrv_str2" id="rsrv_str2" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str3:
			</td>
			<td><input name="rsrv_str3" id="rsrv_str3" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				公司名称:
			</td>
			<td><input name="cust_id" id="cust_id" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				发布时间:
			</td>
			<td><input name="in_date" id="in_date" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id:
			</td>
			<td><input name="user_id" id="user_id" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				remark:
			</td>
			<td><input name="remark" id="remark" size="20" maxlength="20" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="3396" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
