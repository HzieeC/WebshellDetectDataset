<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_payment.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_payment = new Hashtable();

	
	String req_pay_code="";
	
	String req_pay_name="";
	
	String req_pay_desc="";
	

	
	if(request.getParameter("req_pay_code")!=null && !request.getParameter("req_pay_code").equals("")){
		req_pay_code = request.getParameter("req_pay_code");
		ti_payment.put("pay_code",req_pay_code);
	}
	
	if(request.getParameter("req_pay_name")!=null && !request.getParameter("req_pay_name").equals("")){
		req_pay_name = request.getParameter("req_pay_name");
		ti_payment.put("pay_name",req_pay_name);
	}
	
	if(request.getParameter("req_pay_desc")!=null && !request.getParameter("req_pay_desc").equals("")){
		req_pay_desc = request.getParameter("req_pay_desc");
		ti_payment.put("pay_desc",req_pay_desc);
	}
	

	Ti_paymentInfo ti_paymentInfo = new Ti_paymentInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_paymentInfo.getListByPage(ti_payment,Integer.parseInt(iStart),limit);
	int counter = ti_paymentInfo.getCountByObj(ti_payment);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>支付方式管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>支付方式管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
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
	  
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				
				
					支付代码:<input name="req_pay_code" type="text" />&nbsp;
		  		
					支付名称:<input name="req_pay_name" type="text" />&nbsp;
		  		
		  		

				<input name="searchInfo" type="button" value="搜索" onclick="return search();"/>	
			</td>
		</tr>
	</table>

	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString %></td></tr>
	</table>
	
	<% 
		int listsize = 0;
		if(list!=null && list.size()>0){
			listsize = list.size();
	%>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>支付代码</th>
		  	
		  	<th>支付名称</th>
		  	
		  	<th>详细描述</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String pay_id="",pay_code="",pay_name="",pay_desc="",pay_account="",pay_email="",passwd="",hand_fare="",enabled="",user_id="",in_date="";
		  			  	if(map.get("pay_id")!=null) pay_id = map.get("pay_id").toString();
  	if(map.get("pay_code")!=null) pay_code = map.get("pay_code").toString();
  	if(map.get("pay_name")!=null) pay_name = map.get("pay_name").toString();
  	if(map.get("pay_desc")!=null) pay_desc = map.get("pay_desc").toString();
	if(pay_desc.length() > 10){
		pay_desc = pay_desc.substring(0,10);
	}
  	if(map.get("pay_account")!=null) pay_account = map.get("pay_account").toString();
  	if(map.get("pay_email")!=null) pay_email = map.get("pay_email").toString();
  	if(map.get("passwd")!=null) passwd = map.get("passwd").toString();
  	if(map.get("hand_fare")!=null) hand_fare = map.get("hand_fare").toString();
  	if(map.get("enabled")!=null) enabled = map.get("enabled").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=pay_id %>" /></td>
			
		  	<td><%=pay_code%></td>
		  	
		  	<td><%=pay_name%></td>
		  	
		  	<td><%=pay_desc%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?pay_id=<%=pay_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=pay_id%>','4141');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString %></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="4141" />
	  </form>
</body>

</html>
