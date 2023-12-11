<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_workexper.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Ti_workexper ti_workexper = new Ti_workexper();
	//String s_title = "";
	//if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
	//	s_title = request.getParameter("s_title");
	//	news.setTitle(s_title);
	//}
	Ti_workexperInfo ti_workexperInfo = new Ti_workexperInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_workexperInfo.getListByPage(ti_workexper,Integer.parseInt(iStart),limit);
	int counter = ti_workexperInfo.getCountByObj(ti_workexper);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>ti_workexper Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>ti_workexper Manager</h1>
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
				<input name="xxx" type="text" /><input name="searchInfo" type="button" value="Search"/>	
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
				Total:<%=counter %>
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>resume_id</th>
		  	
		  	<th>company</th>
		  	
		  	<th>bisiness</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String work_id="",resume_id="",company="",cust_type="",cust_size="",bisiness="",depart="",career_type="",career="",work_date="",salary="",work_desc="";
		  			  	if(map.get("work_id")!=null) work_id = map.get("work_id").toString();
  	if(map.get("resume_id")!=null) resume_id = map.get("resume_id").toString();
  	if(map.get("company")!=null) company = map.get("company").toString();
  	if(map.get("cust_type")!=null) cust_type = map.get("cust_type").toString();
  	if(map.get("cust_size")!=null) cust_size = map.get("cust_size").toString();
  	if(map.get("bisiness")!=null) bisiness = map.get("bisiness").toString();
  	if(map.get("depart")!=null) depart = map.get("depart").toString();
  	if(map.get("career_type")!=null) career_type = map.get("career_type").toString();
  	if(map.get("career")!=null) career = map.get("career").toString();
  	if(map.get("work_date")!=null) work_date = map.get("work_date").toString();
  	if(map.get("salary")!=null) salary = map.get("salary").toString();
  	if(map.get("work_desc")!=null) work_desc = map.get("work_desc").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=work_id %>" /></td>
			
		  	<td><%=resume_id%></td>
		  	
		  	<td><%=company%></td>
		  	
		  	<td><%=bisiness%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?work_id=<%=work_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=work_id%>','9884');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
				Total:<%=counter %>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="9884" />
	  </form>
</body>

</html>
