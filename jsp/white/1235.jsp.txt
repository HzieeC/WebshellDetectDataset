<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_comment.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>评论管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>

  <% 
  	String trade_id="";
  	if(request.getParameter("trade_id")!=null) trade_id = request.getParameter("trade_id");
  	Ti_commentInfo ti_commentInfo = new Ti_commentInfo();
  	List list = ti_commentInfo.getListByPk(trade_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String info_id="",content="",cust_id="",in_date="",b_content="",user_id="";
  	if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("b_content")!=null) b_content = map.get("b_content").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();

  %>
	
	<h1>修改评论</h1>
	
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
				<font color="red">*</font>
			</td>
			<td><input name="info_id" id="info_id" size="20" maxlength="20" value="<%=info_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				<font color="red">*</font>
			</td>
			<td><input name="content" id="content" size="20" maxlength="20" value="<%=content %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				<font color="red">*</font>
			</td>
			<td><input name="cust_id" id="cust_id" size="20" maxlength="20" value="<%=cust_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				<font color="red">*</font>
			</td>
			<td><input name="in_date" id="in_date" size="20" maxlength="20" value="<%=in_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				<font color="red">*</font>
			</td>
			<td><input name="b_content" id="b_content" size="20" maxlength="20" value="<%=b_content %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				<font color="red">*</font>
			</td>
			<td><input name="user_id" id="user_id" size="20" maxlength="20" value="<%=user_id %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="8517" />
	  			<input type="hidden" name="trade_id" value="<%=trade_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
