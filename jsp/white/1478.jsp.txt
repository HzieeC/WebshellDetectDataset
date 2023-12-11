<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_wholesale.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>批发管理管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>

  <% 
  	String act_id="";
  	if(request.getParameter("act_id")!=null) act_id = request.getParameter("act_id");
  	Ti_wholesaleInfo ti_wholesaleInfo = new Ti_wholesaleInfo();
  	List list = ti_wholesaleInfo.getListByPk(act_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String good_id="",good_name="",prices="",enabled="",user_id="",in_date="";
  	if(map.get("good_id")!=null) good_id = map.get("good_id").toString();
  	if(map.get("good_name")!=null) good_name = map.get("good_name").toString();
  	if(map.get("prices")!=null) prices = map.get("prices").toString();
  	if(map.get("enabled")!=null) enabled = map.get("enabled").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();

  %>
	
	<h1>修改批发管理</h1>
	
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
				0:
			</td>
			<td><input name="good_id" id="good_id" size="20" maxlength="" value="<%=good_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				0:
			</td>
			<td><input name="good_name" id="good_name" size="20" maxlength="" value="<%=good_name %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				0:
			</td>
			<td><input name="prices" id="prices" size="20" maxlength="" value="<%=prices %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				0:
			</td>
			<td><input name="enabled" id="enabled" size="20" maxlength="" value="<%=enabled %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id:
			</td>
			<td><input name="user_id" id="user_id" size="20" maxlength="20" value="<%=user_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				0:
			</td>
			<td><input name="in_date" id="in_date" size="20" maxlength="" value="<%=in_date %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="8304" />
	  			<input type="hidden" name="act_id" value="<%=act_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
