<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.trade.ts_custclass.*" %>

<%
	String cust_id="",user_id="";	
	if( session.getAttribute("session_cust_id") != null )
	{
		cust_id = session.getAttribute("session_cust_id").toString();
	}
	if( session.getAttribute("session_user_id") != null )
	{
		user_id = session.getAttribute("session_user_id").toString();
	}
		
%>


<html>
  <head>
    <title>ts_custclass Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="/program/admin/sendemail/plugins/thickbox/jquery.js"></script>
	<script type="text/javascript" src="js_custclass.js"></script>
</head>

<body>
	<h1>新增企业级别</h1>
	
	
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>企业级别信息管理</h4>
		  <span> 级别编号必须为数字且唯一。</span><br/>
		  </td>
        </tr>
      </table>
      <br/>

	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				级别编号<font color="red">*</font>
			</td>
			<td><input name="cust_class" id="cust_class" type="text" size="3" maxlength="3" value=""  onKeyUp="javascript:	if(!/^[0-9][0-9]*$/.test(this.value)){this.value='';}else{checkCustClass(this.value);}"/>
			<span id="alertCheckMessage" style="display:none">级别编码已经存在！</span></td>
		</tr>
		
		<div id="hiddenCustData" style="display:none">
			
		</div>
		
		<tr>
			<td align="right" width="10%">
				级别名称<font color="red">*</font>
			</td>
			<td><input name="cust_class_name" id="cust_class_name" type="text" /></td>
		</tr>
		
		<input name="cust_oper_person" id="cust_oper_person" value="<%=user_id%>" type="hidden" />	
		
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
				<input type="hidden" name="bpm_id" value="3983" />
				<input type="button" class="buttoncss" name="tradeSub" onclick="submitForm();" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
