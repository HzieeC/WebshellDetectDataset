<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_onlineservice.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>在线客服2管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>

  <% 
  	String info_id="";
  	if(request.getParameter("info_id")!=null) info_id = request.getParameter("info_id");
  	Ti_onlineserviceInfo ti_onlineserviceInfo = new Ti_onlineserviceInfo();
  	List list = ti_onlineserviceInfo.getListByPk(info_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String contact_name="",ser_type="",account="",sort_no="",is_display="",remark="";
  	if(map.get("contact_name")!=null) contact_name = map.get("contact_name").toString();
  	if(map.get("ser_type")!=null) ser_type = map.get("ser_type").toString();
  	if(map.get("account")!=null) account = map.get("account").toString();
  	if(map.get("sort_no")!=null) sort_no = map.get("sort_no").toString();
  	if(map.get("is_display")!=null) is_display = map.get("is_display").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

  %>
	
	<h1>修改在线客服2</h1>
	
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
				contact_name<font color="red">*</font>
			</td>
			<td><input name="contact_name" id="contact_name" size="20" maxlength="20" value="<%=contact_name %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				ser_type:
			</td>
			<td><input name="ser_type" id="ser_type" size="20" maxlength="20" value="<%=ser_type %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				account<font color="red">*</font>
			</td>
			<td><input name="account" id="account" size="20" maxlength="20" value="<%=account %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				sort_no<font color="red">*</font>
			</td>
			<td><input name="sort_no" id="sort_no" size="20" maxlength="20" value="<%=sort_no %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				is_display<font color="red">*</font>
			</td>
			<td><input name="is_display" id="is_display" size="20" maxlength="20" value="<%=is_display %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				remark<font color="red">*</font>
			</td>
			<td><input name="remark" id="remark" size="20" maxlength="20" value="<%=remark %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="1420" />
	  			<input type="hidden" name="info_id" value="<%=info_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
