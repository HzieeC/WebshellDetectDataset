<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_interview_clip.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_interview_clip = new Hashtable();
	String seach_resume_name = "",search_clip_state="";
	if(request.getParameter("seach_resume_name")!=null && !request.getParameter("seach_resume_name").equals("")){
		seach_resume_name = request.getParameter("seach_resume_name");
		ti_interview_clip.put("resume_name",seach_resume_name);
	}
	if(request.getParameter("search_clip_state")!=null && !request.getParameter("search_clip_state").equals("")){
		search_clip_state = request.getParameter("search_clip_state");
		ti_interview_clip.put("clip_state",search_clip_state);
	}
	Ti_interview_clipInfo ti_interview_clipInfo = new Ti_interview_clipInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_interview_clipInfo.getListByPage(ti_interview_clip,Integer.parseInt(iStart),limit);
	int counter = ti_interview_clipInfo.getCountByObj(ti_interview_clip);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>面试夹管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>面试夹管理</h1>
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
				按简历名称：<input name="seach_resume_name" type="text" />
				按面试状态：<select name="search_clip_state">
				                <option value="">请选择</option>
								<option value="0">未通知面试</option>
								<option value="1">已通知面试</option>
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
			<td width="90%">
			    <input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
			</td>
			<td>
				总计：<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>简历名称</th>
		  	
		  	<th>简历来源</th>
		  	
			<th>应聘职位</th>
			
		  	<th>优先级</th>
		  	
		  	<th>面试状态</th>
		  	
		  	<th>时间</th>
		  	
			<th width="10%">查看</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String trade_id="",resumename="",reume_src="",jobname ="",job_id="",in_name="",in_reviews="",level="",clip_state="",cust_id="",in_date="",user_id="",remark="";
		  			  	if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
						if(map.get("resumename")!=null) resumename = map.get("resumename").toString();
						if(map.get("jobname")!=null) jobname = map.get("jobname").toString();		
						if(map.get("reume_src")!=null) reume_src = map.get("reume_src").toString();
						if(map.get("job_id")!=null) job_id = map.get("job_id").toString();
						if(map.get("in_name")!=null) in_name = map.get("in_name").toString();
						if(map.get("in_reviews")!=null) in_reviews = map.get("in_reviews").toString();
						if(map.get("level")!=null) level = map.get("level").toString();
						if(map.get("clip_state")!=null) clip_state = map.get("clip_state").toString();
						if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>19)in_date=in_date.substring(0,19);
						String cilp_name="";
						if(clip_state.equals("0")){
						   cilp_name="未通知面试";
						}
						if(clip_state.equals("1")){
						   cilp_name="已通知面试";
						}
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=trade_id %>" /></td>
			
		  	<td><%=resumename%></td>
		  	
		  	<td><%=reume_src%></td>
		  	
			<td><%=jobname%></td>
			
		  	<td><%=level%></td>
		  	
		  	<td><a href="index.jsp?search_clip_state=<%=clip_state%>"><%=cilp_name%></a></td>
		  	
		  	<td><%=in_date%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?trade_id=<%=trade_id %>"><img src="/program/admin/images/view.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=trade_id%>','1196');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="1196" />
	  </form>
</body>

</html>
