<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_member.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ts_custclass.*" %>
<%@ page import="com.bizoss.trade.tb_commpara.Tb_commparaInfo" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_member = new Hashtable();
	String s_cust_name = "";
	if(request.getParameter("s_cust_name")!=null && !request.getParameter("s_cust_name").equals("")){
		s_cust_name = request.getParameter("s_cust_name");
		ti_member.put("cust_name",s_cust_name);
	}
	String s_user_class = "";
	if(request.getParameter("s_user_class")!=null && !request.getParameter("s_user_class").equals("")){
		s_user_class = request.getParameter("s_user_class");
		ti_member.put("user_class",s_user_class);
	}	
	String s_cust_type = "";
	if(request.getParameter("s_cust_type")!=null && !request.getParameter("s_cust_type").equals("")){
		s_cust_type = request.getParameter("s_cust_type");
		ti_member.put("cust_type",s_cust_type);
	}		
	String s_credit_start = "";
	if(request.getParameter("s_credit_start")!=null && !request.getParameter("s_credit_start").equals("")){
		s_credit_start = request.getParameter("s_credit_start");
		ti_member.put("credit_start",s_credit_start);
	}		
	String s_credit_end = "";
	if(request.getParameter("s_credit_end")!=null && !request.getParameter("s_credit_end").equals("")){
		s_credit_end = request.getParameter("s_credit_end");
		ti_member.put("credit_end",s_credit_end);
	}			
	Ti_memberInfo ti_memberInfo = new Ti_memberInfo();
	Map custclassinfoMap = new Hashtable();
	Ts_custclassInfo custclassinfo = new Ts_custclassInfo();
	String custclass_select =  custclassinfo.getSelectString(custclassinfoMap,"");
	
	String iStart = "0";
	int limit = 20;
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	String state = tb_commparaInfo.getSelectItem("39","");     
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_memberInfo.getList_credit_ByPage(ti_member,Integer.parseInt(iStart),limit);
	int counter = ti_memberInfo.getCount_credit_ByObj(ti_member);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>会员信誉度管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="js_commen.js"></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>会员信誉度管理</h1>
			</td>
			<td>
		
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
				会员名称:<input name="s_cust_name" type="text" />
				会员类型:<select name="s_cust_type" >
									<option value="">请选择</option>
									<option value="0">企业</option>
									<option value="1">个人</option>
									</select>	
				信誉度:<input name="s_credit_start" type="text" />-<input name="s_credit_end" type="text" />					
				<input name="searchInfo" type="button" value="查询" onclick="searcher();"/>	
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

			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>会员名称</th>
		  	
		  	<th>会员类型</th>
		  	
		  	
		  	<th>当前信誉度</th>
			
			<th>注册时间</th>
			
	  		<th width="10%">查看</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String cust_id="",cust_name="",cust_type="",state_code="",user_class="",reg_date="";
					String cust_class_name="",e="",f="",g="",total_level="0";
						if(map.get("total_level")!=null) total_level = map.get("total_level").toString();
		  			  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
						if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
						if(map.get("cust_type")!=null) cust_type = map.get("cust_type").toString();
						if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
						if(map.get("user_class")!=null) user_class = map.get("user_class").toString();
						if(map.get("reg_date")!=null) reg_date = map.get("reg_date").toString();
						if(map.get("cust_class_name")!=null) cust_class_name = map.get("cust_class_name").toString();
						if(reg_date.length()>19)reg_date=reg_date.substring(0,19);
						
						if(map.get("e")!=null) e = map.get("e").toString();
						if(map.get("f")!=null) f = map.get("f").toString();
						if(map.get("g")!=null) g = map.get("g").toString();
						
					//a：未审核 b：审核未通过 c：正常/审核通过 d：禁用
			
					
					String cust_type_string = "企业";
					if(cust_type.equals("1")){
						cust_type_string="个人";
					}
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=cust_id %>" /></td>
			
		  	<td><a href="creditIndex.jsp?cust_id=<%=cust_id %>"><%=cust_name%></a></td>
		  	
		  	<td><%=cust_type_string%></td>

		  	<td><%=total_level%></td>
		  	
		  	<td><%=reg_date%></td>
			<td width="10%">
			<a href="creditIndex.jsp?cust_id=<%=cust_id %>"><img src="/program/admin/images/view.gif" title="查看" /></a> &nbsp;
			</td>
			
			
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">

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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="5779" />
	  </form>
</body>

</html>
