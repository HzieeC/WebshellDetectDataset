<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_credit.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_credit = new Hashtable();

	String s_cust_name = "";
	if(request.getParameter("search_cust_name")!=null && !request.getParameter("search_cust_name").equals("")){
		s_cust_name = request.getParameter("search_cust_name");
		ti_credit.put("cust_name",s_cust_name);
	}
	String s_cust_type = "";
	if(request.getParameter("search_cust_type")!=null && !request.getParameter("search_cust_type").equals("")){
		s_cust_type = request.getParameter("search_cust_type");
		ti_credit.put("cust_type",s_cust_type);
	}	
	
	
	String s_credit_title = "";
	if(request.getParameter("search_credit_title")!=null && !request.getParameter("search_credit_title").equals("")){
		s_credit_title = request.getParameter("search_credit_title");
		ti_credit.put("credit_title",s_credit_title);
	}
	String s_department = "";
	if(request.getParameter("search_department")!=null && !request.getParameter("search_department").equals("")){
		s_department = request.getParameter("search_department");
		ti_credit.put("department",s_department);
	}	
	String s_EndDate1 = "";
	if(request.getParameter("search_EndDate1")!=null && !request.getParameter("search_EndDate1").equals("")){
		s_EndDate1 = request.getParameter("search_EndDate1");
		ti_credit.put("end_date_1",s_EndDate1);
	}
	String s_EndDate2 = "";
	if(request.getParameter("search_EndDate2")!=null && !request.getParameter("search_EndDate2").equals("")){
		s_EndDate2 = request.getParameter("search_EndDate2");
		ti_credit.put("end_date_2",s_EndDate2);
	}
	String s_StartDate2 = "";
	if(request.getParameter("search_StartDate2")!=null && !request.getParameter("search_StartDate2").equals("")){
		s_StartDate2 = request.getParameter("search_StartDate2");
		ti_credit.put("start_date_2",s_StartDate2);
	}
	String s_StartDate1 = "";
	if(request.getParameter("search_StartDate1")!=null && !request.getParameter("search_StartDate1").equals("")){
		s_StartDate1 = request.getParameter("search_StartDate1");
		ti_credit.put("start_date_1",s_StartDate1);
	}
	
	Ti_creditInfo ti_creditInfo = new Ti_creditInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_creditInfo.getListByPage(ti_credit,Integer.parseInt(iStart),limit);
	int counter = ti_creditInfo.getCountByObj(ti_credit);

	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?search_cust_name="+s_cust_name+"&search_credit_title="+s_credit_title+"&search_department="+s_department+"&search_EndDate1="+s_EndDate1+"&search_EndDate2="+s_EndDate2+"&search_StartDate2="+s_StartDate2+"&search_StartDate1="+s_StartDate1+"&iStart=",Integer.parseInt(iStart),limit);
	
	String para ="search_cust_name="+s_cust_name+"&search_credit_title="+s_credit_title+"&search_department="+s_department+"&search_EndDate1="+s_EndDate1+"&search_EndDate2="+s_EndDate2+"&search_StartDate2="+s_StartDate2+"&search_StartDate1="+s_StartDate1+"&iStart="+Integer.parseInt(iStart);
%>
<html>
  <head>
    
    <title>会员资质证书管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="credit.js"></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>会员证书管理</h1>
			</td>

		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	

	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				
				按会员名称:
				<input  type="text"  maxlength="25" name="search_cust_name" id="search_cust_name" />
				
				按证书名称:
				<input  type="text"  maxlength="25" name="search_credit_title" id="search_credit_title" />
				
				按发证机关:
				<input  type="text"  maxlength="25" name="search_department" id="search_department" />
				
				按会员名称:
				<select name="search_cust_type" id="search_cust_type" >
				<option value="">请选择</option>
				<option value="0">企业</option>
				<option value="1">会员</option>
				</select>
								
		</tr>
		<tr>
			<td align="left" >		
				按有效期起始时间:
				  <input name="search_StartDate1" type="text" id="search_StartDate1" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'search_StartDate2\',{d:-1})}',readOnly:true})" size="15"  width="150px"/>
				  -
				  <input name="search_StartDate2" id="search_StartDate2" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'search_StartDate1\',{d:1})}',readOnly:true})" size="15" width="150px"/>
	  
				按有效期截止时间:
				  <input name="search_EndDate1" type="text" id="search_EndDate1" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'search_EndDate2\',{d:-1})}',readOnly:true})" size="15"  width="150px"/>
				  -
				  <input name="search_EndDate2" id="search_EndDate2" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'search_EndDate1\',{d:1})}',readOnly:true})" size="15" width="150px"/>
	  
	  	  
				<input name="searchInfo" type="button" onclick="search()" value="查询"/>	
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
			
		  	<th>会员名称</th>
			
			<th>会员类型</th>
		  	
		  	<th>证书名称</th>
		  	
		  	<th>发证机关</th>
		  	
		  	<th>证书有效起始日期</th>
			
		  	<th>证书有效截止日期</th>
			
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String credit_id="",cust_id="",cust_name="",credit_title="",credit_desc="",start_date="",end_date="",department="",user_id="",in_date="";
					String cust_type="";
					if(map.get("cust_type")!=null) cust_type = map.get("cust_type").toString();
					if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
					if(map.get("credit_id")!=null) credit_id = map.get("credit_id").toString();
					if(map.get("cust_id")!=null)  cust_id = map.get("cust_id").toString();
					if(map.get("cust_id")!=null)  cust_id = map.get("cust_id").toString();
					if(map.get("credit_title")!=null) credit_title = map.get("credit_title").toString(); if(credit_title.length()>23) credit_title=credit_title.substring(0,23)+"...";
					if(map.get("credit_desc")!=null) credit_desc = map.get("credit_desc").toString();
					if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
					if(start_date.length()>19)start_date=start_date.substring(0,19);
					if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
					if(end_date.length()>19)end_date=end_date.substring(0,19);
					if(map.get("department")!=null) department = map.get("department").toString();
					if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
					if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
					if(in_date.length()>19)in_date=in_date.substring(0,19);

					String cust_type_string = "企业";
					if(cust_type.equals("1")){
						cust_type_string = "个人";
					}
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=credit_id %>" /></td>
			
		  	<td><a href="javascript:searchByName('search_cust_name','<%=cust_name%>');"><%=cust_name%></a></td>
			
			<td><a href="index.jsp?search_cust_type=<%=cust_type%>"><%=cust_type_string%></a></td>
		  	
		  	<td><a href="updateInfo.jsp?credit_id=<%=credit_id %>&<%=para%>"><%=credit_title%></a></td>
		  	
		  	<td><a href="javascript:searchByName('search_department','<%=department%>');"><%=department%></a></td>
		  	
		  	<td><%=start_date%></td>
			
			<td><%=end_date%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?credit_id=<%=credit_id %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=credit_id%>','8859');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	<script>
		function searchByName(key,val){
			document.getElementById(key).value=val;
			document.indexForm.submit();
		}
	</script>

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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="8859" />
	  </form>
</body>

</html>
