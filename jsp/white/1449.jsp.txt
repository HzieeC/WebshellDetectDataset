<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.tb_goodstock.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>tb_goodstock Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String trade_id="";
  	if(request.getParameter("trade_id")!=null) trade_id = request.getParameter("trade_id");
  	Tb_goodstockInfo tb_goodstockInfo = new Tb_goodstockInfo();
  	List list = tb_goodstockInfo.getListByPk(trade_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String goods_id="",cust_id="",one_price="",total_price="",before_num="",vary_num="",now_num="",vary_reason="",rsrv_str1="",rsrv_str2="",rsrv_str3="",rsrv_str4="",rsrv_str5="",rsrv_str6="",publish_date="",publish_user_id="";
  	if(map.get("goods_id")!=null) goods_id = map.get("goods_id").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("one_price")!=null) one_price = map.get("one_price").toString();
  	if(map.get("total_price")!=null) total_price = map.get("total_price").toString();
  	if(map.get("before_num")!=null) before_num = map.get("before_num").toString();
  	if(map.get("vary_num")!=null) vary_num = map.get("vary_num").toString();
  	if(map.get("now_num")!=null) now_num = map.get("now_num").toString();
  	if(map.get("vary_reason")!=null) vary_reason = map.get("vary_reason").toString();
  	if(map.get("rsrv_str1")!=null) rsrv_str1 = map.get("rsrv_str1").toString();
  	if(map.get("rsrv_str2")!=null) rsrv_str2 = map.get("rsrv_str2").toString();
  	if(map.get("rsrv_str3")!=null) rsrv_str3 = map.get("rsrv_str3").toString();
  	if(map.get("rsrv_str4")!=null) rsrv_str4 = map.get("rsrv_str4").toString();
  	if(map.get("rsrv_str5")!=null) rsrv_str5 = map.get("rsrv_str5").toString();
  	if(map.get("rsrv_str6")!=null) rsrv_str6 = map.get("rsrv_str6").toString();
  	if(map.get("publish_date")!=null) publish_date = map.get("publish_date").toString();
  	if(map.get("publish_user_id")!=null) publish_user_id = map.get("publish_user_id").toString();

  %>
	
	<h1>Update tb_goodstock</h1>
	
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
				goods_id:
			</td>
			<td><input name="goods_id" id="goods_id" value="<%=goods_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" value="<%=cust_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				one_price:
			</td>
			<td><input name="one_price" id="one_price" value="<%=one_price %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				total_price:
			</td>
			<td><input name="total_price" id="total_price" value="<%=total_price %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				before_num:
			</td>
			<td><input name="before_num" id="before_num" value="<%=before_num %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				vary_num:
			</td>
			<td><input name="vary_num" id="vary_num" value="<%=vary_num %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				now_num:
			</td>
			<td><input name="now_num" id="now_num" value="<%=now_num %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				vary_reason:
			</td>
			<td><input name="vary_reason" id="vary_reason" value="<%=vary_reason %>" type="text" /></td>
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
				rsrv_str4:
			</td>
			<td><input name="rsrv_str4" id="rsrv_str4" value="<%=rsrv_str4 %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str5:
			</td>
			<td><input name="rsrv_str5" id="rsrv_str5" value="<%=rsrv_str5 %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				rsrv_str6:
			</td>
			<td><input name="rsrv_str6" id="rsrv_str6" value="<%=rsrv_str6 %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				publish_date:
			</td>
			<td><input name="publish_date" id="publish_date" value="<%=publish_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				publish_user_id:
			</td>
			<td><input name="publish_user_id" id="publish_user_id" value="<%=publish_user_id %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="8768" />
	  			<input type="hidden" name="trade_id" value="<%=trade_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
