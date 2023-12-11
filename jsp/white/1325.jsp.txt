<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_admin.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ts_role.*" %>
<%@page import="com.bizoss.trade.ti_organize.*" %>
<%@page import="com.bizoss.trade.ti_member.*" %> 
<%@page import="com.bizoss.trade.ti_customer.Ti_customerInfo" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");

	Ti_admin ti_admin = new Ti_admin();
	
	String u_name = "";
	if(request.getParameter("u_name")!=null && !request.getParameter("u_name").equals("")){
		u_name = request.getParameter("u_name");
		ti_admin.setUser_name(u_name);
	}
	String r_name = "";
	if(request.getParameter("r_name")!=null && !request.getParameter("r_name").equals("")){
		r_name = request.getParameter("r_name");
		ti_admin.setReal_name(r_name);
	}
	
	/*
	String org = "";
	if(request.getParameter("org")!=null && !request.getParameter("org").equals("")){
		org = request.getParameter("org");
		ti_admin.setOrg_id(org);
	}
 */
	ti_admin.setUser_state("0");

	ti_admin.setUser_type("1");
	String cust_id = "";	
	if( session.getAttribute("session_cust_id") != null ){
		cust_id = session.getAttribute("session_cust_id").toString();
	}
	ti_admin.setCust_id(cust_id);
	
	Ts_roleInfo ts_roleInfo = new Ts_roleInfo();

	Ti_organizeInfo ti_organizeinfo = new Ti_organizeInfo();
	
	Ti_adminInfo ti_adminInfo = new Ti_adminInfo();
 
	Ti_memberInfo  ti_customerInfo  = new Ti_memberInfo();
	
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_adminInfo.getListByPage(ti_admin,Integer.parseInt(iStart),limit);
	int counter = ti_adminInfo.getCountByObj(ti_admin);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?r_name="+r_name+"&u_name="+u_name+"&iStart=",Integer.parseInt(iStart),limit);
	

	
	//String roleSelect = ts_roleInfo.getRoleBySelect(cust_id);
	//String orgSelect  = ti_organizeinfo.getOrganizeByUpIdSelect(cust_id,"000000000000000"); 
	
	
%>
<html>
  <head>
    
    <title>订单分配</title>
		<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
		<!--
		<script type='text/javascript' src='/dwr/interface/Ti_organizeInfo.js'></script>
		<script type='text/javascript' src='/dwr/interface/Ti_adminInfo.js'></script>
		<script type='text/javascript' src='/dwr/engine.js'></script>
		<script type='text/javascript' src='/dwr/util.js'></script>
		-->
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>订单分配</h1>
			</td>
		
		</tr>
	</table>
	 
	 <table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
				  <h4>提示信息:</h4>
				  <span>订单分配是将对应的企业分配给操作人员，则企业的订单归操作人员管理。</span>
          </td>
        </tr>
      </table>
<br/>

	<form action="index.jsp" name="indexForm" method="post">
	
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left">
				用户名：	<input name="u_name" type="text" value="" size="10" />
				真实姓名：<input name="r_name" type="text" value="" size="10" />
			
			 
			 <!--
							
				部&nbsp;&nbsp;门：
					<select name="sort1" id="sort1" onChange="setSecondClass(this.value);" >
						<option value="">请选择</option>
						<%//=orgSelect%>
					</select>
					<select name="sort2" id="sort2"  onChange="setTherdClass(this.value);" style="display:none;">
						  <option value="">请选择</option>
					</select> 
					<select name="sort3" id="sort3" style="display:none;" >
						  <option value="">请选择</option>
					</select> 
					<input name="org" id="org" type="hidden" />
		-->
					
					<input name="searchInfo" type="button" value="查询" onclick="javascript:document.indexForm.submit();" />	
			</td>
		</tr>
	</table>

	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td align="center"><%=pageString %></td></tr>
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
				总计:<%=counter%>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0" class="dl_bg">
		<tr>
			
			
		  	<th>用户名</th>
		  	
		  	<th>真实姓名</th>
		  	
		  	<th width="25%">所在部门</th>
		  
		  	<th>已分配企业管理</th>
		  	
		  	
				<th width="10%">分配企业</th>
	  	
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String user_id="",user_name="",real_name="",user_state="",user_type="",
		  			org_id="",role_code="",oper_date="",org_id_value="",role_code_value="";
		  			  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
						  	if(map.get("user_name")!=null) user_name = map.get("user_name").toString();
						  	if(map.get("real_name")!=null) real_name = map.get("real_name").toString();
						  	if(map.get("user_type")!=null) user_type = map.get("user_type").toString();
						  	if(map.get("user_state")!=null) user_state = map.get("user_state").toString();
 
						  	if(map.get("org_id") != null) {
									org_id = map.get("org_id").toString();
									String org_id_str[] = org_id.split("\\|");
									if(!org_id.equals("")){
										for(int k = 0;k < org_id_str.length;k++){
											org_id_value += ti_organizeinfo.getOrgNameById(org_id_str[k]) + "&nbsp;";
										}
									}
								}
						  	
						  	//if(map.get("oper_date")!=null) oper_date = map.get("oper_date").toString();
								//if(oper_date.length()>19)oper_date=oper_date.substring(0,19);
 
								int dev_num=0;
								if(!user_id.trim().equals(""))
								{
								  dev_num  =ti_customerInfo.getAssignCount(user_id);								
								
								}
  		 
  		  %>
		
		<tr>
		
		  	<td><%=user_name%></td>
		  	
		  	<td><%=real_name%></td>
		  	
		  	<td><%=org_id_value%></td>
		  	
		  	<td>
		  		<a href="oassigned.jsp?o_dev_user_id=<%=user_id%>&user_name=<%=user_name%>"><img src="/program/admin/images/page_paintbrush.png" border="0" title="已分配企业管理" /></a>
		  		 &nbsp;<font color="#666666">(已分配企业个数:<%=dev_num%>)</font>
		  		</td>
	
	  		<td width="5%"><a href="orderassign.jsp?dev_user_id=<%=user_id%>&user_name=<%=user_name%>"><img src="/program/admin/images/xinzeng.png" border="0" title="分配企业" /></a></td>
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
				总计:<%=counter%>条
			</td>
		</tr>
	</table>
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td align="center"><%=pageString %></td></tr>
	</table>
	
	<%
		 }
	%>
	
	  <input type="hidden" name="listsize" id="listsize" value="<%=listsize%>" />
	  <input type="hidden" name="pkid" id="pkid" value="" />
	  <input type="hidden" name="user_state" id="user_state" value="" />
	  <input name="cust_id" id="cust_id" type="hidden" value="<%=cust_id%>" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="" />
	  </form>
</body>

</html>
