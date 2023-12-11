<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_cust_relation.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>ti_cust_relation Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String relation_id="";
  	if(request.getParameter("relation_id")!=null) relation_id = request.getParameter("relation_id");
  	Ti_cust_relationInfo ti_cust_relationInfo = new Ti_cust_relationInfo();
  	List list = ti_cust_relationInfo.getListByPk(relation_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String cust_id_a="",cust_id_b="",type_a="",type_b="",in_date="",user_id="";
  	if(map.get("cust_id_a")!=null) cust_id_a = map.get("cust_id_a").toString();
  	if(map.get("cust_id_b")!=null) cust_id_b = map.get("cust_id_b").toString();
  	if(map.get("type_a")!=null) type_a = map.get("type_a").toString();
  	if(map.get("type_b")!=null) type_b = map.get("type_b").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();

  %>
	
	<h1>Update ti_cust_relation</h1>
	
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
				cust_id_a:
			</td>
			<td><input name="cust_id_a" id="cust_id_a" value="<%=cust_id_a %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id_b:
			</td>
			<td><input name="cust_id_b" id="cust_id_b" value="<%=cust_id_b %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				type_a:
			</td>
			<td><input name="type_a" id="type_a" value="<%=type_a %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				type_b:
			</td>
			<td><input name="type_b" id="type_b" value="<%=type_b %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date:
			</td>
			<td><input name="in_date" id="in_date" value="<%=in_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id:
			</td>
			<td><input name="user_id" id="user_id" value="<%=user_id %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="8844" />
	  			<input type="hidden" name="relation_id" value="<%=relation_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
