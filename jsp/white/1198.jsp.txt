<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page" /> 

<%
 
	
	String trade_id = bean.GenTradeId ();
	//String re_trade_id = bean.GenTradeId ();
	//String info_id = bean.GenTradeId ();
	String cust_id = "",user_id="";
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
    <title>供应询价</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>新增供应询价</h1>
	
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
				唯一标示
			</td>
			<td><input name="trade_id" id="trade_id" type="text" value="<%=trade_id%>" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				产品id
			</td>
			<td><input name="info_id" id="info_id" type="text" value="24735rWtG0CFwd6" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				询价标题:
			</td>
			<td><input name="title" id="title" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				订货总量:
			</td>
			<td><input name="order_num" id="order_num" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				期望价格:
			</td>
			<td><input name="req_price" id="req_price" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				详细内容:
			</td>
			<td><input name="content" id="content" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				期望回复时间:
			</td>
			<td><input name="req_date" id="req_date" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				寻价单:
			</td>
			<td><input name="req_file" id="req_file" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				re_name名称:
			</td>
			<td><input name="re_name" id="re_name" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				req_compname企业:
			</td>
			<td><input name="req_compname" id="req_compname" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				req_email:
			</td>
			<td><input name="req_email" id="req_email" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				req_phone:
			</td>
			<td><input name="req_phone" id="req_phone" type="text" /></td>
		</tr>
		
		
		<tr>
			<td align="right" width="10%">
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" type="text" value="<%=cust_id%>" /></td>
		</tr>
	
		
		<tr>
			<td align="right" width="10%">
				user_id:
			</td>
			<td><input name="user_id" id="user_id" type="text" value="<%=user_id%>" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				remark:
			</td>
			<td><input name="remark" id="remark" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="6666" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
