<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_user.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Ti_user ti_user = new Ti_user();
	//String s_title = "";
	//if(request.getParameter("s_title")!=null && !request.getParameter("s_title").equals("")){
	//	s_title = request.getParameter("s_title");
	//	news.setTitle(s_title);
	//}
	Ti_userInfo ti_userInfo = new Ti_userInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_userInfo.getListByPage(ti_user,Integer.parseInt(iStart),limit);
	int counter = ti_userInfo.getCountByObj(ti_user);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>ti_user Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>ti_user Manager</h1>
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
			
		  	<th>user_name</th>
		  	
		  	<th>cust_id</th>
		  	
		  	<th>passwd</th>
		  	
		  	<th>user_state</th>
		  	
		  	<th>passwd_ques</th>
		  	
		  	<th>passwd_answer</th>
		  	
		  	<th>real_name</th>
		  	
		  	<th>org_id</th>
		  	
		  	<th>role_code</th>
		  	
		  	<th>last_login</th>
		  	
		  	<th>in_date</th>
		  	
		  	<th>pub_user_id</th>
		  	
		  	<th>remark</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String user_id="",user_name="",cust_id="",passwd="",user_state="",passwd_ques="",passwd_answer="",real_name="",org_id="",role_code="",last_login="",in_date="",pub_user_id="",remark="";
		  			  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("user_name")!=null) user_name = map.get("user_name").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("passwd")!=null) passwd = map.get("passwd").toString();
  	if(map.get("user_state")!=null) user_state = map.get("user_state").toString();
  	if(map.get("passwd_ques")!=null) passwd_ques = map.get("passwd_ques").toString();
  	if(map.get("passwd_answer")!=null) passwd_answer = map.get("passwd_answer").toString();
  	if(map.get("real_name")!=null) real_name = map.get("real_name").toString();
  	if(map.get("org_id")!=null) org_id = map.get("org_id").toString();
  	if(map.get("role_code")!=null) role_code = map.get("role_code").toString();
  	if(map.get("last_login")!=null) last_login = map.get("last_login").toString();
if(last_login.length()>19)last_login=last_login.substring(0,19);
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);
  	if(map.get("pub_user_id")!=null) pub_user_id = map.get("pub_user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=user_id %>" /></td>
			
		  	<td><%=user_name%></td>
		  	
		  	<td><%=cust_id%></td>
		  	
		  	<td><%=passwd%></td>
		  	
		  	<td><%=user_state%></td>
		  	
		  	<td><%=passwd_ques%></td>
		  	
		  	<td><%=passwd_answer%></td>
		  	
		  	<td><%=real_name%></td>
		  	
		  	<td><%=org_id%></td>
		  	
		  	<td><%=role_code%></td>
		  	
		  	<td><%=last_login%></td>
		  	
		  	<td><%=in_date%></td>
		  	
		  	<td><%=pub_user_id%></td>
		  	
		  	<td><%=remark%></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?user_id=<%=user_id %>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=user_id%>','5543');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
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
	  <input type="hidden" name="bpm_id" id="bpm_id" value="5543" />
	  </form>
</body>

</html>
