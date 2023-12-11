<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_payment.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>支付方式管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>

  <% 
  	String pay_id="";
  	if(request.getParameter("pay_id")!=null) pay_id = request.getParameter("pay_id");
  	Ti_paymentInfo ti_paymentInfo = new Ti_paymentInfo();
  	List list = ti_paymentInfo.getListByPk(pay_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String pay_code="",pay_name="",pay_desc="",pay_account="",pay_email="",passwd="",hand_fare="",enabled="",user_id="",in_date="";
  	if(map.get("pay_code")!=null) pay_code = map.get("pay_code").toString();
  	if(map.get("pay_name")!=null) pay_name = map.get("pay_name").toString();
  	if(map.get("pay_desc")!=null) pay_desc = map.get("pay_desc").toString();
  	if(map.get("pay_account")!=null) pay_account = map.get("pay_account").toString();
  	if(map.get("pay_email")!=null) pay_email = map.get("pay_email").toString();
  	if(map.get("passwd")!=null) passwd = map.get("passwd").toString();
  	if(map.get("hand_fare")!=null) hand_fare = map.get("hand_fare").toString();
  	if(map.get("enabled")!=null) enabled = map.get("enabled").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();

  %>
	
	<h1>修改支付方式</h1>
	
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
				pay_code<font color="red">*</font>
			</td>
			<td><input name="pay_code" id="pay_code" size="20" maxlength="20" value="<%=pay_code %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pay_name<font color="red">*</font>
			</td>
			<td><input name="pay_name" id="pay_name" size="20" maxlength="20" value="<%=pay_name %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pay_desc:
			</td>
			<td><input name="pay_desc" id="pay_desc" size="20" maxlength="20" value="<%=pay_desc %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pay_account:
			</td>
			<td><input name="pay_account" id="pay_account" size="20" maxlength="20" value="<%=pay_account %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pay_email:
			</td>
			<td><input name="pay_email" id="pay_email" size="20" maxlength="20" value="<%=pay_email %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				passwd:
			</td>
			<td><input name="passwd" id="passwd" size="20" maxlength="20" value="<%=passwd %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				hand_fare:
			</td>
			<td><input name="hand_fare" id="hand_fare" size="20" maxlength="20" value="<%=hand_fare %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				enabled:
			</td>
			<td><input name="enabled" id="enabled" size="20" maxlength="20" value="<%=enabled %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id:
			</td>
			<td><input name="user_id" id="user_id" size="20" maxlength="20" value="<%=user_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date:
			</td>
			<td><input name="in_date" id="in_date" size="20" maxlength="20" value="<%=in_date %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="7504" />
	  			<input type="hidden" name="pay_id" value="<%=pay_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
