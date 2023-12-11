<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="randomId" class="com.bizoss.frame.util.RandomID" scope="page" /> 
<html>
  <head>
    <title>新增简历投递</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<%
	String trade_id = randomId.GenTradeId();

	String cust_id="",publish_user_id="";
	if(session.getAttribute("session_cust_id")!=null){
		cust_id  = session.getAttribute("session_cust_id").toString();
	}
	if(session.getAttribute("session_user_id")!=null){
		publish_user_id  = session.getAttribute("session_user_id").toString();
	}
%>


<body>
	<h1>新增简历投递</h1>
	
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

	<input name="trade_id" id="trade_id" type="hidden" value="<%=trade_id%>" />
	<input name="user_id" id="user_id" type="hidden" value="<%=publish_user_id%>"/>
	<input name="cust_id" id="cust_id" type="hidden" value="<%=cust_id%>"/>
	
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				所属招聘信息:
			</td>
			<td><input name="info_id" id="info_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				所属简历信息:
			</td>
			<td><input name="reume_id" id="reume_id" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				留言标题:
			</td>
			<td><input name="reume_title" id="reume_title" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				留言内容:
			</td>
			<td><input name="reume_desc" id="reume_desc" type="text" /></td>
		</tr>

		
		<tr>
			<td align="right" width="10%">
				求职人姓名:
			</td>
			<td><input name="reume_name" id="reume_name" type="text" /></td>
		</tr>
		

		<tr>
			<td align="right" width="10%">
				联系电话:
			</td>
			<td><input name="contact_phone" id="contact_phone" type="text" /></td>
		</tr>
		
		
		<tr>
			<td align="right" width="10%">
				联系地址:
			</td>
			<td><input name="contact_addr" id="contact_addr" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				邮箱:
			</td>
			<td><input name="reume_email" id="reume_email" type="text" /></td>
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
				<input type="hidden" name="bpm_id" value="8822" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
