<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_zhuanti.*" %>
<%@page import="com.bizoss.trade.ti_zhuanticomment.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ts_category.Ts_categoryInfo" %>
<%@page import="com.bizoss.trade.ti_admin.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>

<%
	request.setCharacterEncoding("UTF-8");
	Ts_categoryInfo  ts_categoryInfo  = new Ts_categoryInfo();
	String session_cust_id = "";	
	if( session.getAttribute("session_cust_id") != null ){
		session_cust_id = session.getAttribute("session_cust_id").toString();
	}
	Map ti_zhuanti = new Hashtable();
	String g_title = "";
	if(request.getParameter("seach_title")!=null && !request.getParameter("seach_title").equals("")){
		g_title = request.getParameter("seach_title");
		ti_zhuanti.put("title",g_title);
	}
	String g_ch_attr = "";
	if(request.getParameter("seach_cat_id_group")!=null && !request.getParameter("seach_cat_id_group").equals("")){
		g_ch_attr = request.getParameter("seach_cat_id_group");
		ti_zhuanti.put("cat_id_group",g_ch_attr);
	}
	String start_date = "";
   if(request.getParameter("start_date")!=null && !request.getParameter("start_date").equals("")){
	start_date = request.getParameter("start_date");
	ti_zhuanti.put("start_date",start_date);
	}
	String end_date = "";
	if(request.getParameter("end_date")!=null && !request.getParameter("end_date").equals("")){
	end_date = request.getParameter("end_date");
	ti_zhuanti.put("end_date",end_date);
	}
	Ti_zhuantiInfo ti_zhuantiInfo = new Ti_zhuantiInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_zhuantiInfo.getListByPage(ti_zhuanti,Integer.parseInt(iStart),limit);
	int counter = ti_zhuantiInfo.getCountByObj(ti_zhuanti);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?seach_cat_id_group="+g_ch_attr+"&seach_title="+g_title+"&end_date="+end_date+"&start_date="+start_date+"&iStart=",Integer.parseInt(iStart),limit);
  Ti_adminInfo admininfo = new Ti_adminInfo();
  Ti_zhuanticommentInfo zhcomInfo = new Ti_zhuanticommentInfo();
  Map catMap = ts_categoryInfo.getCatClassMap("3");
  
  String para ="seach_cat_id_group="+g_ch_attr+"&seach_title="+g_title+"&end_date="+end_date+"&start_date="+start_date+"&iStart="+Integer.parseInt(iStart);
  
%>
<html>
  <head>
    
    <title>专题管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="zhuanti.js"></script>
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/engine.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/util.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ti_channelInfo.js'></script>
	<script type='text/javascript' src='<%=request.getContextPath()%>/dwr/interface/Ts_categoryInfo.js'></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>专题管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post"method="post" id="indexForm" target="_self">
	
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				专题名称:<input name="seach_title" id="seach_title" type="text" />
				&nbsp;
						按发布时间段:
						<input name="start_date" id="txtStartDate" type="text" class="Wdate" value="" 
						
						onclick="WdatePicker({maxDate:'#F{$dp.$D(\'txtEndDate\',{d:-1})}',readOnly:true})" size="15" />
						 - 
						<input name="end_date" id="txtEndDate" type="text" class="Wdate" value="" 
						
						onclick="WdatePicker({minDate:'#F{$dp.$D(\'txtStartDate\',{d:1})}',readOnly:true})" size="15"/>
						<br/>
					
					<div class="select1">
						按&nbsp;分&nbsp;类:
						<select name="sort1" id="sort1" onclick="setSecondClass(this.value);" >
							<option value="">请选择</option>
					  </select>	
						<select name="sort2" id="sort2" onclick="setTherdClass(this.value);">
							  <option value="">请选择</option>
						</select>		
						<select name="sort3" id="sort3">
							  <option value="">请选择</option>
						</select>		
				<input type="hidden" name="seach_cat_id_group" id="seach_cat_id_group" value=""/>			
				
				 <script type="text/javascript" src="classify.js"></script>
				<input name="searchInfo" type="button" value="搜索" onclick="return searchForm()"/>
          </div>				
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
			
		  	<th>标题</th>
			
			<th>分类</th>		  			  	
		
		  	<th>发布时间</th>
		  	
		  	<th>发布人</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String zhuanti_id="",cat_id_group="",title="",in_date="",user_id="",user_name= "";
		  			  	if(map.get("zhuanti_id")!=null) zhuanti_id = map.get("zhuanti_id").toString();
						if(map.get("cat_id_group")!=null) cat_id_group = map.get("cat_id_group").toString();
						if(map.get("title")!=null) title = map.get("title").toString();
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>16)in_date=in_date.substring(0,16);
						if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
						if(map.get("user_name")!=null) user_name = map.get("user_name").toString();
						 user_name=admininfo.getUserNameByPK(user_id);
						 int  ckeckcomment =0;
						 ckeckcomment = zhcomInfo.checkZhuantiComment(zhuanti_id);					
									
						 StringBuffer output =new StringBuffer();
							if(!cat_id_group.equals("")){
							  String catIds[] =	cat_id_group.split("\\|");	
							  for(String catId:catIds)
							  {
								 if(catMap!=null)
								 {
									 if(catMap.get(catId)!=null)
										 {
										  output.append("<a href='index.jsp?cat_id_group="+catId+"'>"+catMap.get(catId).toString()+"</a> ");                 
										 }                  
								 
								  }                 
							   }		    
									}
						
		  %>
		
		 <tr>
			<td width="5%" align="center">
			<input type="checkbox" name="checkone<%=i%>" id="checkone<%=i %>" value="<%=zhuanti_id %>" />
			<input type="hidden" name="ckeckcomment<%=i%>" id="ckeckcomment<%=i%>" value="<%=ckeckcomment%>">
			</td>
					  	
		  	<td>
			<a href="updateInfo.jsp?zhuanti_id=<%=zhuanti_id %>"><%=title%></a></td>
			
			<td><%=output%></td>
		  	
		  	<td><%=in_date%></td>
		  	
		  	<td><%=user_name%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?zhuanti_id=<%=zhuanti_id %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=zhuanti_id%>','8689','<%=i%>');"><img src="/program/admin/images/delete.gif" title="删除" /></a></td>
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
	  <input type="hidden" name="cust_id" value="<%=session_cust_id%>" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="8689" />
	  </form>
</body>

</html>
