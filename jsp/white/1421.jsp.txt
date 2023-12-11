<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_re_inquiry.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>ti_re_inquiry Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String re_trade_id="";
  	if(request.getParameter("re_trade_id")!=null) re_trade_id = request.getParameter("re_trade_id");
  	Ti_re_inquiryInfo ti_re_inquiryInfo = new Ti_re_inquiryInfo();
  	List list = ti_re_inquiryInfo.getListByPk(re_trade_id);
  	Hashremap remap = new Hashremap();
  	if(list!=null && list.size()>0) remap = (Hashremap)list.get(0);
  	
  	String trade_id="",title="",content="",rsrv_str1="",rsrv_str2="",rsrv_str3="",cust_id="",in_date="",user_id="",remark="";
  	if(remap.get("trade_id")!=null) trade_id = remap.get("trade_id").toString();
  	if(remap.get("title")!=null) title = remap.get("title").toString();
  	if(remap.get("content")!=null) content = remap.get("content").toString();
  	if(remap.get("rsrv_str1")!=null) rsrv_str1 = remap.get("rsrv_str1").toString();
  	if(remap.get("rsrv_str2")!=null) rsrv_str2 = remap.get("rsrv_str2").toString();
  	if(remap.get("rsrv_str3")!=null) rsrv_str3 = remap.get("rsrv_str3").toString();
  	if(remap.get("cust_id")!=null) cust_id = remap.get("cust_id").toString();
  	if(remap.get("in_date")!=null) in_date = remap.get("in_date").toString();
  	if(remap.get("user_id")!=null) user_id = remap.get("user_id").toString();
  	if(remap.get("remark")!=null) remark = remap.get("remark").toString();

  %>
	
	<h1>Update ti_re_inquiry</h1>
	
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
			<td><input name="trade_id" id="trade_id" value="<%=trade_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				title:
			</td>
			<td><input name="title" id="title" value="<%=title %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				content:
			</td>
			<td><input name="content" id="content" value="<%=content %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str1:
			</td>
			<td><input name="rsrv_str1" id="rsrv_str1" value="<%=rsrv_str1 %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str2:
			</td>
			<td><input name="rsrv_str2" id="rsrv_str2" value="<%=rsrv_str2 %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str3:
			</td>
			<td><input name="rsrv_str3" id="rsrv_str3" value="<%=rsrv_str3 %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" value="<%=cust_id %>" type="text" /></td>
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
		
		<tr>
			<td align="right" width="10%">
				remark:
			</td>
			<td><input name="remark" id="remark" value="<%=remark %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="6793" />
	  			<input type="hidden" name="re_trade_id" value="<%=re_trade_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
