<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>  
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page" />

<html>
  <head>
    <title>渠道分销产品管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css"> 
	<script type="text/javascript" src="param.js"></script>
</head>

<body> 
	 
<%
		String param_id = bean.GenTradeId();
%>
	 
	 

	<h1>新增产品</h1>
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<input name="param_id" id="param_id" type="hidden" value="<%=param_id%>"/>
	<input name="param_attr" id="param_attr" type="hidden" value="114" />
	<input name="param_code" id="param_code" type="hidden" value="product_code" />
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		 

		<tr>
			<td align="right" width="10%">
				产品名称<font color="red">*</font>
			</td>
			<td width="30%"><input name="param_name" id="param_name" type="text" size="30" maxlength="30"/></td> 
			<td align="right" width="10%">
				收益比例<font color="red">*</font>
			</td>
			<td><input name="para_code1" id="para_code1" type="text" size="30" maxlength="30"/></td>

		</tr>
	
		<tr> 
			<td align="right" width="10%">
				结算方式<font color="red">*</font>
			</td>
			<td width="30%"><input name="para_code2" id="para_code2" type="text" size="30" maxlength="50" /></td> 
			 
			<td align="right" width="10%">
				产品说明:
			</td>
			<td><input name="para_code3" id="para_code3" type="text" maxlength="50" size="30" /></td>
		</tr> 

		<tr>
 
			<td align="right" width="10%">
				产品代码:
			</td>
			<td width="30%" colspan="3"><input name="para_code4" id="para_code4" type="text" size="30" maxlength="50"/></td> 
			 
			 
		</tr>

		 
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="8622" />
				<input type="submit" class="buttoncss" name="tradeSub" onclick="return checkAdd()" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
