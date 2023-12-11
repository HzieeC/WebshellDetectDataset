<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_zhuanti.*" %>
<%@page import="com.bizoss.trade.ti_zhuanticomment.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ti_admin.*" %>
<%@page import="com.bizoss.trade.ts_category.Ts_categoryInfo" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Ti_adminInfo admininfo = new Ti_adminInfo();
	Ti_zhuantiInfo zhtinfo = new Ti_zhuantiInfo();
	Map ti_zhuanticomment = new Hashtable();
	String g_title = "";
	if(request.getParameter("title")!=null && !request.getParameter("title").equals("")){
		g_title = request.getParameter("title");
		ti_zhuanticomment.put("title",g_title);
	}
	String g_ch_attr = "";
	if(request.getParameter("zhuanti_id")!=null && !request.getParameter("zhuanti_id").equals("")){
		g_ch_attr = request.getParameter("zhuanti_id");
		//ti_zhuanticomment.put("zhuanti_id",g_ch_attr);
	}
	String start_date = "";
   if(request.getParameter("start_date")!=null && !request.getParameter("start_date").equals("")){
	start_date = request.getParameter("start_date");
	ti_zhuanticomment.put("start_date",start_date);
	}
	String end_date = "";
	if(request.getParameter("end_date")!=null && !request.getParameter("end_date").equals("")){
	end_date = request.getParameter("end_date");
	ti_zhuanticomment.put("end_date",end_date);
	}
	Ti_zhuanticommentInfo ti_zhuanticommentInfo = new Ti_zhuanticommentInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_zhuanticommentInfo.getListByPage(ti_zhuanticomment,Integer.parseInt(iStart),limit);
	int counter = ti_zhuanticommentInfo.getCountByObj(ti_zhuanticomment);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?title="+g_title+"&end_date="+end_date+"&start_date="+start_date+"&iStart=",Integer.parseInt(iStart),limit);
	 

	String para = "title="+g_title+"&end_date="+end_date+"&start_date="+start_date+"&iStart="+Integer.parseInt(iStart);
	
%>
<html>
  <head>
    
    <title>专题评论</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="zhuanticomment.js"></script>
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	
 </head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>专题评论</h1>
			</td>			
		</tr>
	</table>
	
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>点击专题名称可修改专题评论</h4>
		  <span></span><br/>
		  
		  </td>
        </tr>
      </table>
      <br/>
	  
	
	<form action="index.jsp" name="indexForm" method="post">
	  
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				专题名称:<input name="title" id="title" type="text" />
				&nbsp;
						按发布时间段:
						<input name="start_date" id="txtStartDate" type="text" class="Wdate" value="" 
						
						onclick="WdatePicker({maxDate:'#F{$dp.$D(\'txtEndDate\',{d:-1})}',readOnly:true})" size="15" />
						 - 
						<input name="end_date" id="txtEndDate" type="text" class="Wdate" value="" 
						
						onclick="WdatePicker({minDate:'#F{$dp.$D(\'txtStartDate\',{d:1})}',readOnly:true})" size="15"/>						
						<input name="searchInfo" type="button" value="搜索" onclick="return searchForm()"/>	
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
			
		  	<th>专题名称</th>
		  	
		  	<th>评论时间</th>
		  	
		  	<th>评论人</th>
		  	
		  	<th>他人同意数</th>
		  	
		  	<th>他人反对数</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
	
		<% 
		      String commen_id="",zhuanti_id="",zh_name="",user_name="",in_date="",user_id="",up_num="",down_num="";
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);		  			
		  	    if(map.get("commen_id")!=null) commen_id = map.get("commen_id").toString();
				if(map.get("zhuanti_id")!=null) zhuanti_id = map.get("zhuanti_id").toString();
				if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
				if(in_date.length()>19)in_date=in_date.substring(0,19);
				if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
				if(map.get("up_num")!=null) up_num = map.get("up_num").toString();
				if(map.get("down_num")!=null) down_num = map.get("down_num").toString();
				
				 user_name= admininfo.getUserNameByPK(user_id);
				
				zh_name= zhtinfo.getZhTitleById(zhuanti_id);			
						              
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=commen_id %>" /></td>
			
		  	<td><a href="updateInfo.jsp?commen_id=<%=commen_id %>&<%=para%>"><%=zh_name%></a></td>
		  	
		  	<td><%=in_date%></td>
		  	
		  	<td><%=user_name%></td>
		  	
		  	<td><%=up_num%></td>
		  	
		  	<td><%=down_num%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?commen_id=<%=commen_id %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=commen_id%>','3916');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="3916" />
	  </form>
</body>

</html>
