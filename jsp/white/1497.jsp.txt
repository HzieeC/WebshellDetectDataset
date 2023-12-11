<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.ti_customer.*" %>
<%@page import="java.util.*" %>
<%@page import="com.lll.sys.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Ti_customer ti_customer = new Ti_customer();
	//String s_title = "";
	//if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
	//	s_title = request.getParameter("s_title");
	//	news.setTitle(s_title);
	//}
	Ti_customerInfo ti_customerInfo = new Ti_customerInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_customerInfo.getListByPage(ti_customer,Integer.parseInt(iStart),limit);
	int counter = ti_customerInfo.getCountByObj(ti_customer);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>ti_customer Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>ti_customer Manager</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.png" /></a>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
	<table width="100%" cellpadding="0" cellspacing="0" style="border:1px solid #B9BBD4;">
		<tr>
			<td align="left" bgcolor="#E7E7F0">
				<input name="xxx" type="text" /><input name="searchInfo" type="button" value="Search"/>	
			</td>
		</tr>
	</table>

	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr><td align="center" bgcolor="#ECE6E6"><%=pageString %></td></tr>
	</table>
	
	<% 
		int listsize = 0;
		if(list!=null && list.size()>0){
			listsize = list.size();
	%>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td bgcolor="#cecece" width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td bgcolor="#cecece">
				Total:<%=counter %>
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>cust_name</th>
		  	
		  	<th>class_attr</th>
		  	
		  	<th>area_attr</th>
		  	
		  	<th>phone</th>
		  	
		  	<th>contact_name</th>
		  	
		  	<th>publish_date</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String cust_id="",cust_name="",shop_name="",dev_user_id="",cust_rage="",cust_supply="",cust_stock="",cust_type="",client_status="",recommend="",cust_state="",cust_key="",main_product="",cust_desc="",class_attr="",area_attr="",phone="",fax="",company_addr="",business_addr="",post_code="",email="",website="",contact_name="",contact_depart="",contact_job="",contact_sex="",contact_cellphone="",contact_msn="",contact_qq="",publish_date="",reg_money_type="",reg_money="",reg_date="",reg_addr="",corporate="",reg_no="",tax_no="",company_code="",bank_code="",bank_no="",make_sum="",staff_num="",kwo_num="",brand_name="",month_sum="",month_unit="",year_in="",year_out="",year_sum="",man_auth="",control_type="",main_market="",main_cust="",isoem="";
		  			  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
  	if(map.get("shop_name")!=null) shop_name = map.get("shop_name").toString();
  	if(map.get("dev_user_id")!=null) dev_user_id = map.get("dev_user_id").toString();
  	if(map.get("cust_rage")!=null) cust_rage = map.get("cust_rage").toString();
  	if(map.get("cust_supply")!=null) cust_supply = map.get("cust_supply").toString();
  	if(map.get("cust_stock")!=null) cust_stock = map.get("cust_stock").toString();
  	if(map.get("cust_type")!=null) cust_type = map.get("cust_type").toString();
  	if(map.get("client_status")!=null) client_status = map.get("client_status").toString();
  	if(map.get("recommend")!=null) recommend = map.get("recommend").toString();
  	if(map.get("cust_state")!=null) cust_state = map.get("cust_state").toString();
  	if(map.get("cust_key")!=null) cust_key = map.get("cust_key").toString();
  	if(map.get("main_product")!=null) main_product = map.get("main_product").toString();
  	if(map.get("cust_desc")!=null) cust_desc = map.get("cust_desc").toString();
  	if(map.get("class_attr")!=null) class_attr = map.get("class_attr").toString();
  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
  	if(map.get("phone")!=null) phone = map.get("phone").toString();
  	if(map.get("fax")!=null) fax = map.get("fax").toString();
  	if(map.get("company_addr")!=null) company_addr = map.get("company_addr").toString();
  	if(map.get("business_addr")!=null) business_addr = map.get("business_addr").toString();
  	if(map.get("post_code")!=null) post_code = map.get("post_code").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("website")!=null) website = map.get("website").toString();
  	if(map.get("contact_name")!=null) contact_name = map.get("contact_name").toString();
  	if(map.get("contact_depart")!=null) contact_depart = map.get("contact_depart").toString();
  	if(map.get("contact_job")!=null) contact_job = map.get("contact_job").toString();
  	if(map.get("contact_sex")!=null) contact_sex = map.get("contact_sex").toString();
  	if(map.get("contact_cellphone")!=null) contact_cellphone = map.get("contact_cellphone").toString();
  	if(map.get("contact_msn")!=null) contact_msn = map.get("contact_msn").toString();
  	if(map.get("contact_qq")!=null) contact_qq = map.get("contact_qq").toString();
  	if(map.get("publish_date")!=null) publish_date = map.get("publish_date").toString();
if(publish_date.length()>19)publish_date=publish_date.substring(0,19);
  	if(map.get("reg_money_type")!=null) reg_money_type = map.get("reg_money_type").toString();
  	if(map.get("reg_money")!=null) reg_money = map.get("reg_money").toString();
  	if(map.get("reg_date")!=null) reg_date = map.get("reg_date").toString();
if(reg_date.length()>19)reg_date=reg_date.substring(0,19);
  	if(map.get("reg_addr")!=null) reg_addr = map.get("reg_addr").toString();
  	if(map.get("corporate")!=null) corporate = map.get("corporate").toString();
  	if(map.get("reg_no")!=null) reg_no = map.get("reg_no").toString();
  	if(map.get("tax_no")!=null) tax_no = map.get("tax_no").toString();
  	if(map.get("company_code")!=null) company_code = map.get("company_code").toString();
  	if(map.get("bank_code")!=null) bank_code = map.get("bank_code").toString();
  	if(map.get("bank_no")!=null) bank_no = map.get("bank_no").toString();
  	if(map.get("make_sum")!=null) make_sum = map.get("make_sum").toString();
  	if(map.get("staff_num")!=null) staff_num = map.get("staff_num").toString();
  	if(map.get("kwo_num")!=null) kwo_num = map.get("kwo_num").toString();
  	if(map.get("brand_name")!=null) brand_name = map.get("brand_name").toString();
  	if(map.get("month_sum")!=null) month_sum = map.get("month_sum").toString();
  	if(map.get("month_unit")!=null) month_unit = map.get("month_unit").toString();
  	if(map.get("year_in")!=null) year_in = map.get("year_in").toString();
  	if(map.get("year_out")!=null) year_out = map.get("year_out").toString();
  	if(map.get("year_sum")!=null) year_sum = map.get("year_sum").toString();
  	if(map.get("man_auth")!=null) man_auth = map.get("man_auth").toString();
  	if(map.get("control_type")!=null) control_type = map.get("control_type").toString();
  	if(map.get("main_market")!=null) main_market = map.get("main_market").toString();
  	if(map.get("main_cust")!=null) main_cust = map.get("main_cust").toString();
  	if(map.get("isoem")!=null) isoem = map.get("isoem").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=cust_id %>" /></td>
			
		  	<td><%=cust_name%></td>
		  	
		  	<td><%=class_attr%></td>
		  	
		  	<td><%=area_attr%></td>
		  	
		  	<td><%=phone%></td>
		  	
		  	<td><%=contact_name%></td>
		  	
		  	<td><%=publish_date%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?cust_id=<%=cust_id %>">修改</a></td>
	  		<td width="10%"><a href="/doTradeReg.do?pkid=<%=cust_id%>&bpm_id=1975">删除</a></td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td bgcolor="#cecece" width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td bgcolor="#cecece">
				Total:<%=counter %>
			</td>
		</tr>
	</table>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr><td align="center" bgcolor="#ECE6E6"><%=pageString %></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="1975" />
	  </form>
</body>

</html>
