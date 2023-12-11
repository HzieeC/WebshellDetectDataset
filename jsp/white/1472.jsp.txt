<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_memcredit.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>会员信誉度管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String trade_id="";
  	if(request.getParameter("trade_id")!=null) trade_id = request.getParameter("trade_id");
  	Ti_memcreditInfo ti_memcreditInfo = new Ti_memcreditInfo();
  	List list = ti_memcreditInfo.getListByPk(trade_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String get_cust_id="",level="",in_date="",content="",user_id="",cust_id="",remark="";
	String get_cust_name="",send_cust_name="";
	if(map.get("get_cust_name")!=null) get_cust_name = map.get("get_cust_name").toString();
	if(map.get("send_cust_name")!=null) send_cust_name = map.get("send_cust_name").toString();
  	if(map.get("get_cust_id")!=null) get_cust_id = map.get("get_cust_id").toString();
  	if(map.get("level")!=null) level = map.get("level").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();
	String level_string ="好评";
		if(level.equals("0")){
		level_string ="中评";
		} else if(level.equals("-1")) {
		level_string ="差评";
		}
  %>
	
	<h1>会员信誉度查看</h1>
	
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
				会员名称:
			</td>
			<td><%=get_cust_name %></td>

			<td align="right" width="10%">
				评价等级:
			</td>
			<td><%=level_string %></td>
		</tr>
		

		<tr>
			<td align="right" width="10%">
				评价内容:
			</td>
			<td><%=content %></td>

			<td align="right" width="10%">
				评价人:
			</td>
			<td><%=send_cust_name %></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				评价时间:
			</td>
			<td colspan="3"><%=in_date %></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="6015" />
	  			<input type="hidden" name="trade_id" value="<%=trade_id %>" />
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='creditIndex.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
