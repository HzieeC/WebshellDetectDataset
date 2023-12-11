<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_subscribeinfo.*,com.bizoss.trade.ti_personal.*,com.bizoss.trade.ti_customer.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%






		Ti_personalInfo ti_personalInfo = new Ti_personalInfo();
		Ti_customerInfo ti_customerInfo=new Ti_customerInfo();

	request.setCharacterEncoding("UTF-8");
	Ti_subscribeinfo ti_subscribeinfo = new Ti_subscribeinfo();
	//String s_title = "";
	//if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
	//	s_title = request.getParameter("s_title");
	//	news.setTitle(s_title);
	//}
	Ti_subscribeinfoInfo ti_subscribeinfoInfo = new Ti_subscribeinfoInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_subscribeinfoInfo.getListByPage(ti_subscribeinfo,Integer.parseInt(iStart),limit);
	int counter = ti_subscribeinfoInfo.getCountByObj(ti_subscribeinfo);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>商机订阅管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>商机订阅管理</h1>
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
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>会员名称</th>
		  	
		  	<th>信息类型</th>
		  	
		  	<th>信息标题</th>
		  	
		  	<th>信息缩略图</th>
		  	
		  	<th>信息发布日期</th>
		  	
		  	<th>卖家名称</th>
		  	
		  	<th>发送日期</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String sub_id="",user_id="",info_type="",info_id="",info_title="",info_desc="",info_img="",pub_date="",cust_name="",cust_id="",in_date="";
		  			  	String user_name="";
		  			  	if(map.get("sub_id")!=null) sub_id = map.get("sub_id").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	
  	 user_name=ti_personalInfo.getPersonalNameByPersonalId(user_id);
  	
  	if(map.get("info_type")!=null) info_type = map.get("info_type").toString();
  	
  	if(info_type.equals("0")) info_type="商品";
  		if(info_type.equals("1")) info_type="卖家资讯";
  			if(info_type.equals("0")) info_type="平台资讯";
  	
  	
  	if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
  	if(map.get("info_title")!=null) info_title = map.get("info_title").toString();
  	if(map.get("info_desc")!=null) info_desc = map.get("info_desc").toString();
  	if(map.get("info_img")!=null) info_img = map.get("info_img").toString();
  	if(map.get("pub_date")!=null) pub_date = map.get("pub_date").toString();
if(pub_date.length()>19)pub_date=pub_date.substring(0,19);
  	if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	
  		 cust_name =	ti_customerInfo.getCustNameByCustId(cust_id);
  	
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=sub_id %>" /></td>
			
		  	<td><%=user_name%></td>
		  	
		  	<td><%=info_type%></td>
		  	
		  	<td><%=info_title%></td>
		  	
		  	<td><%=info_img%></td>
		  	
		  	<td><%=pub_date%></td>
		  	
		  	<td><%=cust_name%></td>
		  	
		  	<td><%=in_date%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?sub_id=<%=sub_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=sub_id%>','9197');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="9197" />
	  </form>
</body>

</html>
