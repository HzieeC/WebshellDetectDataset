<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.trade.ts_custclass.*" %>
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" />  
<%

	  String tem_id = randomId.GenTradeId();
	  //String selectStr ="";
	  String selectStr = new Ts_custclassInfo().getSelectString(new Hashtable(),"");
%>

<html>
  <head>
    <title>ti_shoptem Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="shoptem.js"></script>

</head>

<body>
	<h1>新增会员模板</h1>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
<input name="tem_id" id="tem_id" value="<%=tem_id%>" type="hidden" />
		
		<tr>
			<td align="right" width="10%">
				模板名称<font color="red">*</font>
			</td>
			<td><input name="tem_name" id="tem_name" type="text" maxlength="25" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				企业级别<font color="red">*</font>
			</td>
			<td>
			<select  name="cust_class" id="cust_class" style="width:100px">
				<option value="">请选择</option>
				<%=selectStr %>
			</select>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				用户ID:
			</td>
			<td><input name="cust_id" id="cust_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				模板代码:
			</td>
			<td><input name="tem_code" id="tem_code" type="text"  maxlength="25" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				模板缩略图:
			</td> 
			<td>
				<input name="tem_image" id="tem_image" type="text"   maxlength="50" size="40"/>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				模板地址:
			</td>
			<td><input name="tem_path" id="tem_path" type="text"   maxlength="50" size="40"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				是否有效:
			</td>
			<td>
			<input type="radio" name="enabled" id="enabled_1" value="0" checked /> 是 &nbsp;
			<input type="radio" name="enabled" id="enabled_0" value="1" />   否&nbsp;
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				备注:
			</td>
			<td><input name="remark" id="remark" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="8819" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkInfo();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
