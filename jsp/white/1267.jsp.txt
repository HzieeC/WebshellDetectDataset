<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_re_quote.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Hashtable ti_re_quote = new Hashtable();

	
	String req_trade_id="";
	
	String req_title="";
	
	String req_content="";
	
	String req_rsrv_st1="";
	
	String req_rsrv_str2="";
	
	String req_rsrv_str3="";
	
	String req_cust_id="";
	
	String req_in_date="";
	
	String req_user_id="";
	
	String req_remark="";
	

	
	if(request.getParameter("req_trade_id")!=null && !request.getParameter("req_trade_id").equals("")){
		req_trade_id = request.getParameter("req_trade_id");
		ti_re_quote.put("trade_id",req_trade_id);
	}
	
	if(request.getParameter("req_title")!=null && !request.getParameter("req_title").equals("")){
		req_title = request.getParameter("req_title");
		ti_re_quote.put("title",req_title);
	}
	
	if(request.getParameter("req_content")!=null && !request.getParameter("req_content").equals("")){
		req_content = request.getParameter("req_content");
		ti_re_quote.put("content",req_content);
	}
	
	if(request.getParameter("req_rsrv_st1")!=null && !request.getParameter("req_rsrv_st1").equals("")){
		req_rsrv_st1 = request.getParameter("req_rsrv_st1");
		ti_re_quote.put("rsrv_st1",req_rsrv_st1);
	}
	
	if(request.getParameter("req_rsrv_str2")!=null && !request.getParameter("req_rsrv_str2").equals("")){
		req_rsrv_str2 = request.getParameter("req_rsrv_str2");
		ti_re_quote.put("rsrv_str2",req_rsrv_str2);
	}
	
	if(request.getParameter("req_rsrv_str3")!=null && !request.getParameter("req_rsrv_str3").equals("")){
		req_rsrv_str3 = request.getParameter("req_rsrv_str3");
		ti_re_quote.put("rsrv_str3",req_rsrv_str3);
	}
	
	if(request.getParameter("req_cust_id")!=null && !request.getParameter("req_cust_id").equals("")){
		req_cust_id = request.getParameter("req_cust_id");
		ti_re_quote.put("cust_id",req_cust_id);
	}
	
	if(request.getParameter("req_in_date")!=null && !request.getParameter("req_in_date").equals("")){
		req_in_date = request.getParameter("req_in_date");
		ti_re_quote.put("in_date",req_in_date);
	}
	
	if(request.getParameter("req_user_id")!=null && !request.getParameter("req_user_id").equals("")){
		req_user_id = request.getParameter("req_user_id");
		ti_re_quote.put("user_id",req_user_id);
	}
	
	if(request.getParameter("req_remark")!=null && !request.getParameter("req_remark").equals("")){
		req_remark = request.getParameter("req_remark");
		ti_re_quote.put("remark",req_remark);
	}
	

	Ti_re_quoteInfo ti_re_quoteInfo = new Ti_re_quoteInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_re_quoteInfo.getListByPage(ti_re_quote,Integer.parseInt(iStart),limit);
	int counter = ti_re_quoteInfo.getCountByObj(ti_re_quote);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>报价回复管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="index.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>报价回复管理</h1>
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
				
				
					:<input name="req_trade_id" type="text" />&nbsp;
		  		
					:<input name="req_title" type="text" />&nbsp;
		  		
					:<input name="req_content" type="text" />&nbsp;
		  		
					:<input name="req_rsrv_st1" type="text" />&nbsp;
		  		
					:<input name="req_rsrv_str2" type="text" />&nbsp;
		  		
					:<input name="req_rsrv_str3" type="text" />&nbsp;
		  		
					:<input name="req_cust_id" type="text" />&nbsp;
		  		
					:<input name="req_in_date" type="text" />&nbsp;
		  		
					:<input name="req_user_id" type="text" />&nbsp;
		  		
					:<input name="req_remark" type="text" />&nbsp;
		  		

				<input name="searchInfo" type="button" value="搜索" onclick="return search();"/>	
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
			
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
		  	<th></th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String re_trade_id="",trade_id="",title="",content="",rsrv_st1="",rsrv_str2="",rsrv_str3="",cust_id="",in_date="",user_id="",remark="";
		  			  	if(map.get("re_trade_id")!=null) re_trade_id = map.get("re_trade_id").toString();
  	if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("rsrv_st1")!=null) rsrv_st1 = map.get("rsrv_st1").toString();
  	if(map.get("rsrv_str2")!=null) rsrv_str2 = map.get("rsrv_str2").toString();
  	if(map.get("rsrv_str3")!=null) rsrv_str3 = map.get("rsrv_str3").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=re_trade_id %>" /></td>
			
		  	<td><%=trade_id%></td>
		  	
		  	<td><%=title%></td>
		  	
		  	<td><%=content%></td>
		  	
		  	<td><%=rsrv_st1%></td>
		  	
		  	<td><%=rsrv_str2%></td>
		  	
		  	<td><%=rsrv_str3%></td>
		  	
		  	<td><%=cust_id%></td>
		  	
		  	<td><%=in_date%></td>
		  	
		  	<td><%=user_id%></td>
		  	
		  	<td><%=remark%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?re_trade_id=<%=re_trade_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=re_trade_id%>','1871');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="1871" />
	  </form>
</body>

</html>
