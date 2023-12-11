<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_personal.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@page import=" com.bizoss.trade.ts_memlevel.*" %>

<%
	request.setCharacterEncoding("UTF-8");
	Map ti_personal = new Hashtable();
	
String _user_name = "",_start_date="",_end_date="",_state="",_user_class="";
	if(request.getParameter("seach_user_name")!=null && !request.getParameter("seach_user_name").equals("")){
		_user_name = request.getParameter("seach_user_name");
		ti_personal.put("user_name",_user_name);
	}
	if(request.getParameter("start_date")!=null && !request.getParameter("start_date").equals("")){
		_start_date = request.getParameter("start_date");
		ti_personal.put("start_date",_start_date);
	}
	if(request.getParameter("end_date")!=null && !request.getParameter("end_date").equals("")){
		_end_date = request.getParameter("end_date");
	ti_personal.put("end_date",_end_date);
	}
		if(request.getParameter("seach_state_code")!=null && !request.getParameter("seach_state_code").equals("")){
		_state = request.getParameter("seach_state_code");
		ti_personal.put("state_code",_state);
	}
	
	if(request.getParameter("seach_user_class")!=null && !request.getParameter("seach_user_class").equals("")){
		_user_class = request.getParameter("seach_user_class");
		ti_personal.put("user_class",_user_class);
	}
	String level="",levelSearch="";
	Ts_memlevelInfo ts_memlevelInfo= new Ts_memlevelInfo();
	
	Ti_personalInfo ti_personalInfo = new Ti_personalInfo();
	
	Map memLevel=ts_memlevelInfo.getAll(); 
	
	levelSearch = ts_memlevelInfo.getAllLevel();
	
	String _user_id="";
	if(request.getParameter("user_id")!=null){	_user_id=request.getParameter("user_id");}
	
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	List list = ti_personalInfo.getListByPage(ti_personal,Integer.parseInt(iStart),limit);
	int counter = ti_personalInfo.getCountByObj(ti_personal);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?seach_user_name="+_user_name+"&seach_state_code="+_state+"&seach_user_class="+_user_class+"&iStart=",Integer.parseInt(iStart),limit);
	
	String para ="seach_user_name="+_user_name+"&seach_state_code="+_state+"&seach_user_class="+_user_class+"&iStart="+Integer.parseInt(iStart);
%>
<html>
  <head>
    
    <title>个人会员管理</title>
		<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
		<script type='text/javascript' src='/dwr/interface/Ts_areaInfo.js'></script>
		<script type='text/javascript' src='/dwr/engine.js'></script>
		<script type='text/javascript' src='/dwr/util.js'></script>
		
		<script type="text/javascript" src="/js/commen.js"></script>
		<script type="text/javascript" src="js_personal.js"></script>
		
		<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>个人会员管理</h1>
			</td>
			<td>
				<a href="addInfo.jsp"><img src="/program/admin/index/images/post.gif" /></a>
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">

	<table width="100%" cellpadding="0" cellspacing="0" class="dl_su">
		<tr>
			<td align="left" >
			&nbsp;按会员名称:<input name="seach_user_name" id="seach_user_name" type="text" />
			
			&nbsp;按会员状态:
								<select name="seach_state_code" id="seach_state_code">
									<option value="">
										请选择
									</option>
									<option value="0">
										正常
									</option>
									<option value="1">
										禁用
									</option>
										<option value="2">
										未激活
									</option>
								</select>
								
				&nbsp;按会员级别:
								<select name="seach_user_class" id="seach_user_class">
										<option value="">
										请选择
										</option>
										<%=levelSearch%>
								</select>
			
			&nbsp; 按注册时间段:
					<input name="start_date" type="text" id="start_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'end_date\',{d:-1})}',readOnly:true})" size="15" />
					- 
				  <input name="end_date" id="end_date" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'start_date\',{d:1})}',readOnly:true})" size="15"/>
		
			<input name="searchInfo" type="button" value="搜索" onclick="searchForm();"/>	
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
	
				<input type="button" name="delInfo" onclick="oper('1')" value="禁用" class="buttab"/>
	
				<input type="button" name="delInfo" onclick="oper('0')" value="启用" class="buttab"/>
			</td>
			
			<td>
				共计:<%=counter %>条
			</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			<th width="5%" align="center"><input type="checkbox" name="checkall" id="checkall" onclick="selectAll()"></th>
			
		  	<th>会员名称</th>
		  	
		  	<th>状态</th>
		  	
		  	<th>用户级别</th>
		  	
		  	<th>注册时间</th>
		  	
		  	<th>用户密码</th>
		  	
			<th width="10%">修改</th>
	  		<th width="10%">删除</th>
		</tr>
		
		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String user_id="",user_name="",passwd="",state_code="",user_class="",question="",answer="",inter_num="",real_name="",phone="",email="",cellphone="",fax="",qq="",msn="",post_code="",address="",sex="",birth="",area_attr="",marry="",blood_type="",career="",job="",hobby="",income="",last_login="",rsrv_str1="",rsrv_str2="",rsrv_str3="",rsrv_str4="",rsrv_str5="",rsrv_str6="",post_state="",in_date="",pub_user_id="";
		String _state_code="";
				 	String class_name="",__user_class="";
		if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("user_name")!=null) user_name = map.get("user_name").toString();
  	if(map.get("passwd")!=null) passwd = map.get("passwd").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();	
  	_state_code=state_code;
  	if(state_code.equals("0")){state_code="正常";}
  		if(state_code.equals("1")){state_code="禁用";}
  			if(state_code.equals("2")){state_code="未激活";}
  	
  	if(map.get("user_class")!=null) user_class = map.get("user_class").toString();
  	__user_class=user_class;
  	if(memLevel.get(user_class)!=null)   class_name = memLevel.get(user_class).toString();
  	
  	if(map.get("question")!=null) question = map.get("question").toString();
  	if(map.get("answer")!=null) answer = map.get("answer").toString();
  	if(map.get("inter_num")!=null) inter_num = map.get("inter_num").toString();
  	if(map.get("real_name")!=null) real_name = map.get("real_name").toString();
  	if(map.get("phone")!=null) phone = map.get("phone").toString();
  	if(map.get("email")!=null) email = map.get("email").toString();
  	if(map.get("cellphone")!=null) cellphone = map.get("cellphone").toString();
  	if(map.get("fax")!=null) fax = map.get("fax").toString();
  	if(map.get("qq")!=null) qq = map.get("qq").toString();
  	if(map.get("msn")!=null) msn = map.get("msn").toString();
  	if(map.get("post_code")!=null) post_code = map.get("post_code").toString();
  	if(map.get("address")!=null) address = map.get("address").toString();
  	if(map.get("sex")!=null) sex = map.get("sex").toString();
  	if(map.get("birth")!=null) birth = map.get("birth").toString();
  	if(map.get("area_attr")!=null) area_attr = map.get("area_attr").toString();
  	if(map.get("marry")!=null) marry = map.get("marry").toString();
  	if(map.get("blood_type")!=null) blood_type = map.get("blood_type").toString();
  	if(map.get("career")!=null) career = map.get("career").toString();
  	if(map.get("job")!=null) job = map.get("job").toString();
  	if(map.get("hobby")!=null) hobby = map.get("hobby").toString();
  	if(map.get("income")!=null) income = map.get("income").toString();
  	if(map.get("last_login")!=null) last_login = map.get("last_login").toString();
		if(last_login.length()>19)last_login=last_login.substring(0,19);
  	if(map.get("post_state")!=null) post_state = map.get("post_state").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
		if(in_date.length()>19)in_date=in_date.substring(0,19);
  	if(map.get("pub_user_id")!=null) pub_user_id = map.get("pub_user_id").toString();

		  %>
		
		<tr>
			<td width="5%" align="center"><input type="checkbox" name="checkone<%=i %>" id="checkone<%=i %>" value="<%=user_id %>" /></td>
			
		  	<td><a href="updateInfo.jsp?user_id=<%=user_id %>"><%=user_name%></a></td>
		  	
		  	<td><a href="index.jsp?state_code=<%=_state_code%>"><%=state_code%></a></td>
		  	
		  	<td><a href="index.jsp?user_class=<%=__user_class%>"><%=class_name%></a></td>
		  
		  	<td><%=in_date%></td>
		  	
		  	<td><a href="ChangerPassw.jsp?user_id=<%=user_id%>&<%=para%>">密码修改</a></td>
		  	
			<td width="10%"><a href="updateInfo.jsp?user_id=<%=user_id %>&<%=para%>"><img src="/program/admin/images/edit.gif" title="修改" /></a></td>
	  		<td width="10%"><a href="javascript:deleteOneInfo('<%=user_id%>','7794');"><img src="/program/admin/images/delete.gif" title="删除" /></a></a></td>
		</tr>
		
		 <input type="hidden" name="user_id" id="user_id" value="<%=user_id%>" />
		 <input type="hidden" name="user_class" id="user_class" value="<%=_user_class%>" />
	 
		  <%
		  		}
		  %>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
				<input type="button" name="delInfo" onclick="delIndexInfo()" value="删除" class="buttab"/>
	
			<input type="button" name="delInfo" onclick="oper('1')" value="禁用" class="buttab"/>
	
				<input type="button" name="delInfo" onclick="oper('0')" value="启用" class="buttab"/>
			</td>
			<td>
			共计:<%=counter %>条
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
	  <input type="hidden" name="state_code" id="state_code" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="7794" />
	  <input type="hidden" name="trade_type_code" id="trade_type_code" value="">
	  </form>
</body>

</html>
