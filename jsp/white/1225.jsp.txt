<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page"/>

<%
	String pri_key = bean.GenTradeId();
%>

<html>
  <head>
    <title>求购报价管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<h1>新增求购报价</h1>
	
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
	
	<input name="trade_id" id="trade_id" type="hidden" value="<%=pri_key%>"/>

	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				产品<font color="red">*</font>
			</td>
			<td><input name="info_id" id="info_id" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				报价标题<font color="red">*</font>
			</td>
			<td><input name="title" id="title" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				详细内容<font color="red">*</font>
			</td>
			<td><input name="content" id="content" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				期望回复时间<font color="red">*</font>
			</td>
			<td><input name="quote_date" id="quote_date" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				报价人名称<font color="red">*</font>
			</td>
			<td><input name="quote_name" id="quote_name" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				报价人企业名称<font color="red">*</font>
			</td>
			<td><input name="quote_compname" id="quote_compname" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				报价人Email<font color="red">*</font>
			</td>
			<td><input name="quote_email" id="quote_email" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				报价人电话<font color="red">*</font>
			</td>
			<td><input name="quote_phone" id="quote_phone" size="20" maxlength="20" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				报价单<font color="red">*</font>
			</td>
			<td><input name="quote_file" id="quote_file" size="20" maxlength="20" type="text" /></td>
		</tr>

		<tr>
			<td align="right" width="10%">
				备注:
			</td>
			<td><textarea name="remark" id="remark" rows="8" cols="40"></textarea></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
			
				<input name="user_id" id="user_id"  type="hidden" />
				<input type="hidden" name="bpm_id" value="5809" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
