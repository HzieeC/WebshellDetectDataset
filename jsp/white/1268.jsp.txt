<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_re_quote.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>报价回复管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>

  <% 
  	String re_trade_id="";
  	if(request.getParameter("re_trade_id")!=null) re_trade_id = request.getParameter("re_trade_id");
  	Ti_re_quoteInfo ti_re_quoteInfo = new Ti_re_quoteInfo();
  	List list = ti_re_quoteInfo.getListByPk(re_trade_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String trade_id="",title="",content="",rsrv_st1="",rsrv_str2="",rsrv_str3="",cust_id="",in_date="",user_id="",remark="";
  	if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("rsrv_st1")!=null) rsrv_st1 = map.get("rsrv_st1").toString();
  	if(map.get("rsrv_str2")!=null) rsrv_str2 = map.get("rsrv_str2").toString();
  	if(map.get("rsrv_str3")!=null) rsrv_str3 = map.get("rsrv_str3").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

  %>
	
	<h1>修改报价回复</h1>
	
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
				trade_id<font color="red">*</font>
			</td>
			<td><input name="trade_id" id="trade_id" size="20" maxlength="20" value="<%=trade_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				title<font color="red">*</font>
			</td>
			<td><input name="title" id="title" size="20" maxlength="20" value="<%=title %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				content<font color="red">*</font>
			</td>
			<td><input name="content" id="content" size="20" maxlength="20" value="<%=content %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_st1<font color="red">*</font>
			</td>
			<td><input name="rsrv_st1" id="rsrv_st1" size="20" maxlength="20" value="<%=rsrv_st1 %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str2<font color="red">*</font>
			</td>
			<td><input name="rsrv_str2" id="rsrv_str2" size="20" maxlength="20" value="<%=rsrv_str2 %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str3<font color="red">*</font>
			</td>
			<td><input name="rsrv_str3" id="rsrv_str3" size="20" maxlength="20" value="<%=rsrv_str3 %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id<font color="red">*</font>
			</td>
			<td><input name="cust_id" id="cust_id" size="20" maxlength="20" value="<%=cust_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date<font color="red">*</font>
			</td>
			<td><input name="in_date" id="in_date" size="20" maxlength="20" value="<%=in_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_id<font color="red">*</font>
			</td>
			<td><input name="user_id" id="user_id" size="20" maxlength="20" value="<%=user_id %>" type="text" /></td>
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
				<input type="hidden" name="bpm_id" value="0739" />
	  			<input type="hidden" name="re_trade_id" value="<%=re_trade_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkSub();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
