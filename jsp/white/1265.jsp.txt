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

	ti_member.put("cust_type","1");
	ti_member.put("m_state","1");	
	Ti_memberInfo ti_memberInfo = new Ti_memberInfo();
	Map custclassinfoMap = new Hashtable();
	Ts_custclassInfo custclassinfo = new Ts_custclassInfo();
	
	//custclassinfoMap.put("cust_type","0");
	
	String custclass_select =  custclassinfo.getSelectString(custclassinfoMap,"");
	
	String iStart = "0";
	int limit = 20;
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo(); 
	String state = tb_commparaInfo.getSelectItem("39","");     
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_memberInfo.getListByPage(ti_member,Integer.parseInt(iStart),limit);
	int counter = ti_memberInfo.getCountByObj(ti_member);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>会员管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="js_commen.js"></script>
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="80%">
				<h1>会员管理</h1>
			</td>
			<td>
				<a href="addCompanyInfo.jsp">新增企业会员>></a>&nbsp;&nbsp;
				<a href="addPersonalInfo.jsp">新增个人会员>></a>
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
				按会员名称:<input name="s_cust_name" type="text" />
				按会员类型:<select name="s_cust_type" >
									<option value="">请选择</option>
									<option value="0">企业</option>
									<option value="1">个人</option>
									</select>
				按会员等级:<select name="s_user_class" >
									<option value="">请选择</option>
									<%=custclass_select%>
									</select>				
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
				<select name="up_operating" id="up_operating" onchange="changeTime()">
				  	<option value="">请选择...</option>	
					<option value="-">删除</option>	
					<option value="c">启用</option>
					<option value="d">禁用</option>
				  	<%=state%>
				</select>	
				<div id="b_g" style="display:none">
				开始时间:<input type="text" name="s_start_date" id="s_start_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'s_end_date\',{d:-1})}',readOnly:true})" size="15"  width="150px" />
				结束时间:<input type="text" name="s_end_date" id="s_end_date" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'s_start_date\',{d:1})}',readOnly:true})" size="15" width="150px" />
				</div>
				<input type="button" name="delInfo" onclick="operateInfo('up')" value="确定"  class="buttab" />
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
		  	
		  	<th>会员状态</th>
		  	
		  	<th>会员等级</th>
		  	
		  	<th>注册时间</th>
			
	  		<th width="20%">操作</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String cust_id="",cust_name="",cust_type="",state_code="",user_class="",reg_date="";
					String cust_class_name="",e="",f="",g="";
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
					String updatePage = "updateCompanyInfo.jsp";
					if(cust_type.equals("1")){
						cust_type_string="个人";
						updatePage = "updatePersonalInfo.jsp";
					}

					
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=cust_id %>" /></td>
			
		  	<td><a href="<%=updatePage%>?cust_id=<%=cust_id %>"><%=cust_name%></a></td>
		  	
		  	<td><a href="index.jsp?s_cust_type=<%=cust_type%>"><%=cust_type_string%></a></td>
		  	
		  	<td>
				<div style="margin-top:8px;"></div>
	            <span class="<%if(state_code.indexOf("c")>-1)out.print("blueon"); else out.print("blueoff");%>">启用</span> 
	            <span class="<%if(state_code.indexOf("d")>-1)out.print("blueon"); else out.print("blueoff");%>">禁用</span> 
	            <span class="<%if(!e.equals(""))out.print("blueon"); else out.print("blueoff");%>">推荐</span>
	            <span class="<%if(!f.equals(""))out.print("blueon"); else out.print("blueoff");%>">置顶</span>
	            <span class="<%if(!g.equals(""))out.print("blueon"); else out.print("blueoff");%>">头条</span> 
	           						
			</td>
		  	
		  	<td><%=cust_class_name%></td>
		  	
		  	<td><%=reg_date%></td>
			<td width="10%">

			<a href="<%=updatePage%>?cust_id=<%=cust_id %>"><img src="/program/admin/images/edit.gif" title="修改信息" /></a> &nbsp;
			
			<a href="updatePassWord.jsp?cust_id=<%=cust_id %>"><img src="/program/admin/images/hy_xg.jpg" title="修改密码" /></a> &nbsp;
			
	  		<a href="javascript:deleteOneInfo('<%=cust_id%>','5779');"><img src="/program/admin/images/delete.gif" title="删除" /></a> &nbsp;</td>
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
