<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_memcomplaint.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_memcomplaint = new Hashtable();
	String session_cust_id="";
	if(session.getAttribute("session_cust_id")!=null){
	  session_cust_id=session.getAttribute("session_cust_id").toString();   
	}
	String s_send_cust_name = "",s_get_cust_name = "",s_deal_state ="",defense_type="";
	if(request.getParameter("s_send_cust_name")!=null && !request.getParameter("s_send_cust_name").equals("")){
		s_send_cust_name = request.getParameter("s_send_cust_name");
	   ti_memcomplaint.put("send_cust_name",s_send_cust_name);
	}
	if(request.getParameter("s_get_cust_name")!=null && !request.getParameter("s_get_cust_name").equals("")){
		s_get_cust_name = request.getParameter("s_get_cust_name");
	   ti_memcomplaint.put("get_cust_name",s_get_cust_name);
	}
	if(request.getParameter("s_deal_state")!=null && !request.getParameter("s_deal_state").equals("")){
		s_deal_state = request.getParameter("s_deal_state");
	   ti_memcomplaint.put("deal_state",s_deal_state);
	}
	if(request.getParameter("defense_type")!=null && !request.getParameter("defense_type").equals("")){
		defense_type = request.getParameter("defense_type");
	   if(defense_type.equals("0")){
	   ti_memcomplaint.put("out",defense_type);  
	   }
	   if(defense_type.equals("1")){
	    ti_memcomplaint.put("in",defense_type);  
	   }
	}
	Ti_memcomplaintInfo ti_memcomplaintInfo = new Ti_memcomplaintInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_memcomplaintInfo.getListByPage(ti_memcomplaint,Integer.parseInt(iStart),limit);
	int counter = ti_memcomplaintInfo.getCountByObj(ti_memcomplaint);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>投诉举报</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>投诉举报</h1>
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
				投诉人：<input name="s_send_cust_name" id="s_send_cust_name" type="text" />
				被投诉人：<input name="s_get_cust_name" id="s_get_cust_name" type="text" />
				是否已申辩：<select name="defense_type">
				         <option value="">请选择</option> 
				        <option value="1">是</option>
						<option value="0">否</option>						
						</select>
				处理结果：<select name="s_deal_state">
				        <option value="">请选择</option> 
						<option value="0">未处理</option>
						<option value="1">已处理</option>
						</select>
				<input name="searchInfo" type="button" value="搜索" onclick="javascript:document.indexForm.submit();"/>	
			
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
			
			<td align="right">
				总计：<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			
		  	<th>投诉人</th>
		  	
		  	<th>被投诉人</th>
		  
		    <th>是否申辩</th>

			<th>处理状态</th>
		  
		  	<th>时间</th>
		  	
			<th width="10%">查看</th>
	  		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String com_id="",comid="",send_cust_name="",get_cust_name="",content="",in_date="",deal_state="";
		  			  	if(map.get("com_id")!=null) com_id = map.get("com_id").toString();
						if(map.get("comid")!=null) comid = map.get("comid").toString();{
						if(comid.equals("1"))content="是"; else {content="否";}}
						if(map.get("send_cust_name")!=null) send_cust_name = map.get("send_cust_name").toString();
						if(map.get("get_cust_name")!=null) get_cust_name = map.get("get_cust_name").toString();
						//if(map.get("content")!=null) content = map.get("content").toString();
						if(content.length()>19)content=content.substring(0,16)+"...";
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>19)in_date=in_date.substring(0,19);
						if(map.get("deal_state")!=null) deal_state = map.get("deal_state").toString();
						
		  %>
		
		<tr>
			
		  	<td><a href="javascript:searchByName('s_send_cust_name','<%=send_cust_name%>');"><%=send_cust_name%></a></td>
		  	
		  	<td><a href="javascript:searchByName('s_get_cust_name','<%=get_cust_name%>');"><%=get_cust_name%></a></td>
		  	
		  	<td><a href="index.jsp?defense_type=<%=comid%>"><%=content%></a></td>
			
			
			<td><%
			if(deal_state.equals("0")){
			%>
			<a href="index.jsp?s_deal_state=<%=deal_state%>">未处理</a>
			<%	
			}if(deal_state.equals("1")){
			%>
			<a href="index.jsp?s_deal_state=<%=deal_state%>">已处理</a>
			<%}%></td>

		  	<td><%=in_date%></td>
		  	
		  	
		  	
			<td width="10%"><a href="updateInfo.jsp?com_id=<%=com_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
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
			<td align="right">
				总计：<%=counter %>条
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="5309" />
	  </form>
</body>

</html>
