<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_message.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.ti_news.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	 Ti_newsInfo newsinfo = new Ti_newsInfo();
	Map ti_message = new Hashtable();
	String s_title = "",s_name="";
	String get_trade_id="";
  	if(request.getParameter("trade_id")!=null) {
	get_trade_id = request.getParameter("trade_id");
	ti_message.put("trade_id",get_trade_id);
	}
	if(request.getParameter("search_title")!=null && !request.getParameter("search_title").equals("")){
		s_title = request.getParameter("search_title");
		ti_message.put("title",s_title);
	}
	if(request.getParameter("search_name")!=null && !request.getParameter("search_name").equals("")){
		s_name = request.getParameter("search_name");
		ti_message.put("m_name",s_name);
	}
	Ti_messageInfo ti_messageInfo = new Ti_messageInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_messageInfo.getChildListByPage(ti_message,Integer.parseInt(iStart),limit);
	int counter = ti_messageInfo.getChildCountByObj(ti_message);
	String pageString = new PageTools().getGoogleToolsBar(counter,"Chlidindex.jsp?search_title="+s_title+"&search_name="+s_name+"&iStart=",Integer.parseInt(iStart),limit);
	String para = "search_title="+s_title+"&search_name="+s_name+"&iStart="+Integer.parseInt(iStart);
	
	//String output = ti_messageInfo.getmessagetreeList(get_trade_id);
%>
<html>
  <head>
    
    <title>资讯留言管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="message.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>资讯留言管理</h1>
			</td>
			
		</tr>
	</table>
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>点击带有“<img src="/program/admin/images/1.gif" border="0">”图标的评论标题可查看对该评论的评论</h4><br/>
		</td>
        </tr>
      </table>
      <br/>
	<form action="childindex.jsp" name="indexForm" method="post">
	
	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
				按标题：<input name="search_title" id="search_title" type="text" />
				<input name="searchInfo" type="button"   value="搜索" onclick="ChlidsearchForm()"/>
                <input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp?<%=para%>';"/>				
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
				<input type="button" name="delInfo" onclick="delNewsInfo()" value="删除" class="buttab"/>
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
		
			<th>评论标题</th>
		  	
		  	<th>内容</th>
		  	
			<th>评论人</th>
			
			<th width="12%">查看详细</th>
			
	  		<th width="10%">删除</th>
		</tr>
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String trade_id="",re_trade_id="",info_id="",title="",m_name="",cust_id="",in_date="",content="";
		  			  	if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
						if(map.get("re_trade_id")!=null) re_trade_id = map.get("re_trade_id").toString();
						if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
						if(map.get("content")!=null) content = map.get("content").toString();
						if(map.get("title")!=null) title = map.get("title").toString();
						if(map.get("m_name")!=null) m_name = map.get("m_name").toString();
						if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>19)in_date=in_date.substring(0,19);
					   Boolean hasDown =  ti_messageInfo.checkSonById(trade_id);	
  	
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=trade_id %>" /></td>
			<td>
			<%
	       if(hasDown){%>
 	
		  	<input type="hidden" name="hasdown<%=i%>" value="1" id="hasdown<%=i%>" />
		  	<a href="childindex.jsp?trade_id=<%=trade_id %>" title="点击查看对该评论的评论"><img src="/program/admin/images/1.gif" border="0" style="cursor:pointer;"/><%=title%></a>
		  	<%}else{%>
		  		
		  		<%=title%>
		  		<%}%></td>
			<td><%=content%></td>
			
		  	<td><%=m_name%></td>
			
			
		  	
			<td width="12%"><a href="childupdateInfo.jsp?trade_id=<%=trade_id %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="查看" /></a></td>
	  		<td width="10%"><a href="javascript:delOneNews('<%=i%>','<%=trade_id%>');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
		</tr>
		
		  <%
		  		}
		  %>
		<!---->
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delNewsInfo()" value="删除" class="buttab"/>
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
	  <input type="hidden" name="trade_id" id="trade_id" value="<%=get_trade_id%>" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="4162" />
	  </form>
</body>

</html>
