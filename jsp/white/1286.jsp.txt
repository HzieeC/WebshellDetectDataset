<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page" /> 

<%
 
	
	String trade_id = bean.GenTradeId ();
	String re_trade_id = bean.GenTradeId ();
	String info_id = bean.GenTradeId ();
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
    <title>ti_message Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>
	<h1>Add ti_message</h1>
	
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
				trade_id:
			</td>
			<td><input name="trade_id" id="trade_id" type="text" value="<%=trade_id%>" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				re_trade_id:
			</td>
			<td><input name="re_trade_id" id="re_trade_id" type="text"  value="<%=re_trade_id%>" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				info_id:
			</td>
			<td><input name="info_id" id="info_id" type="text" value="<%=info_id%>" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				title:
			</td>
			<td><input name="title" id="title" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				content:
			</td>
			<td><input name="content" id="content" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				m_name:
			</td>
			<td><input name="m_name" id="m_name" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				m_email:
			</td>
			<td><input name="m_email" id="m_email" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				m_phone:
			</td>
			<td><input name="m_phone" id="m_phone" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				ip_addr:
			</td>
			<td><input name="ip_addr" id="ip_addr" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str1:
			</td>
			<td><input name="rsrv_str1" id="rsrv_str1" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str2:
			</td>
			<td><input name="rsrv_str2" id="rsrv_str2" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str3:
			</td>
			<td><input name="rsrv_str3" id="rsrv_str3" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				re_level:
			</td>
			<td><input name="re_level" id="re_level" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" type="text"  value="<%=cust_id%>"/></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date:
			</td>
			<td><input name="in_date" id="in_date" type="text" /></td>
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
			<td><input name="remark" id="remark" type="text"  /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="0807" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
