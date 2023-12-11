<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_cust_relation.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_cust_relation = new Hashtable();
	String s_cust_name = "";
	if(request.getParameter("s_cust_name")!=null && !request.getParameter("s_cust_name").equals("")){
		s_cust_name = request.getParameter("s_cust_name");
		ti_cust_relation.put("cust_name",s_cust_name);
	}
	Ti_cust_relationInfo ti_cust_relationInfo = new Ti_cust_relationInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_cust_relationInfo.getListByPage(ti_cust_relation,Integer.parseInt(iStart),limit);
	int counter = ti_cust_relationInfo.getCountByObj(ti_cust_relation);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>商友关系查看</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="relation.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>商友关系查看</h1>
			</td>
			<td>
			
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
				按会员名称:<input name="s_cust_name" type="text" /><input name="searchInfo" type="button" value="查询" onclick="searcher();"/>	
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
			
		  	<th>请求人</th>
		  	
		  	<th>接受人</th>
		  	
		  	<th>关系状态</th>		  	
		  	
		  	<th>建立时间</th>
		  	
	  		<th width="10%">删除</th>
		</tr>
		 
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String relation_id="",cust_id_a="",cust_id_b="",type_a="",type_b="",in_date="",user_id="";
					String cust_name_a="",cust_name_b="";
		  			  	if(map.get("relation_id")!=null) relation_id = map.get("relation_id").toString();
  	if(map.get("cust_id_a")!=null) cust_id_a = map.get("cust_id_a").toString();
  	if(map.get("cust_id_b")!=null) cust_id_b = map.get("cust_id_b").toString();
  	if(map.get("type_a")!=null) type_a = map.get("type_a").toString();
  	if(map.get("type_b")!=null) type_b = map.get("type_b").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
	if(map.get("cust_name_a")!=null) cust_name_a = map.get("cust_name_a").toString();
	if(map.get("cust_name_b")!=null) cust_name_b = map.get("cust_name_b").toString();
	
	String relation_type = "";
	if(type_a.equals("0")&&type_b.equals("0")){
		relation_type = "<font color='green'>互为好友</font>";
	}
	if(type_a.equals("1")&&type_b.equals("0")){
		relation_type = cust_name_a + "是" + cust_name_b +"的好友 <br/> " + cust_name_b + "不是" + cust_name_a +"的好友";
	}
	if(type_a.equals("0")&&type_b.equals("1")){
		relation_type = cust_name_b + "是" + cust_name_a +"的好友 <br/> " + cust_name_a + "不是" + cust_name_b +"的好友";
	}	
	
	
		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=relation_id %>" /></td>
			
		  	<td><%=cust_name_a%></td>
		  	
		  	<td><%=cust_name_b%></td>
		  	
		  	<td><%=relation_type%></td>
		  	
		  	<td><%=in_date%></td>

	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=relation_id%>','8611');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="8611" />
	  </form>
</body>

</html>
