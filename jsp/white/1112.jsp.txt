<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 

<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page" />
<%
String vote_id = bean. GenTradeId ();
 
String user_id="",cust_id="";	
	if( session.getAttribute("session_user_id") != null )
	{
		user_id = session.getAttribute("session_user_id").toString();
	}
	
	if( session.getAttribute("session_cust_id") != null )
	{
		cust_id = session.getAttribute("session_cust_id").toString();
	}
%>

<html>
  <head>
    <title>新增在线调查</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type="text/javascript" src="vote.js"></script>
</head>

<body>
	<h1>新增在线调查</h1>
	
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
	
	<form action="/doTradeReg.do" method="post" name="addForm" id="addForm" target="_self">
	<input name="vote_id" id="vote_id" type="hidden" value="<%=vote_id%>"/>
	<input name="cust_id" id="cust_id" type="hidden" value="<%=cust_id%>"/>
	<input name="user_id" id="user_id" type="hidden" value="<%=user_id%>"/>
	<input name="vote_count" id="vote_count" type="hidden" value="0" /> 
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		
		<tr>
			<td align="right" width="15%">
				调查主题<font color="red">*</font>
			</td>
			<td colspan="3">
			<input name="vote_title" id="vote_title"  type="text" style="width:150px;" maxlength="50"/></td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				开始时间<font color="red">*</font>
			</td>
			<td>
			<input name="start_date" id="txtStartDate" type="text" class="Wdate" value=""
		onclick="WdatePicker({maxDate:'#F{$dp.$D(\'txtEndDate\',{d:-1})}',readOnly:true})" style="width:150px;" /></td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				结束时间<font color="red">*</font>
			</td>
			<td>
			<input name="end_date" id="txtEndDate" type="text" class="Wdate" value=""
			onclick="WdatePicker({minDate:'#F{$dp.$D(\'txtStartDate\',{d:1})}',readOnly:true})" style="width:150px;"/></td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				是否支持多选:
			</td>
			<td><input name="is_multi" id="is_multi" type="radio"  value="0"   checked />是&nbsp;
             <input name="is_multi" id="is_multi" type="radio"  value="1"  />否&nbsp;&nbsp;</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="9970" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onClick="return chekedform()" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
