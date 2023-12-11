<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_personal.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%
	request.setCharacterEncoding("UTF-8");
	Map ti_personal = new Hashtable();
	Ti_personalInfo ti_personalInfo = new Ti_personalInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String userInfo_s = "";
	if(request.getParameter("userInfo")!=null) userInfo_s = request.getParameter("userInfo");
	List list = ti_personalInfo.getUserListSMS(ti_personal,Integer.parseInt(iStart),limit);
	int counter = ti_personalInfo.getUserListSMSCount(ti_personal);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>
<html>
  <head>
    
    <title>短信群发</title>
		<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	
</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>短信群发</h1>
			</td>

		</tr>
	</table>

		<form action="/doTradeReg.do" method="post" name="sendForm">
		
		<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">

        <tr>

          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>

          <td width="91%" align="left">

		  <h4>关于短信群发</h4>

		  <span>1、 选择相应的用户发送信息</span><br/>
		  
		  <span>2、 初始显示8个用户，可在右侧输入框输入字段自动根据用户名查询！</span><br/>

		  <span>3、$User_Name$为短信模板用户名称，用于替换用户名！</span>

		  </td>

        </tr>

      </table>

      <br/>

	
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
				
			</td>
			<td>
				总计:<%=counter %>条
			</td>
		</tr>
	</table>
	<table>
		<tr>
			<td style="padding-left:10px">选择用户:</td>
			<td>
      <select id="fromBox" multiple="multiple" name="fromBox">		
		<% 
		  		for(int i=0;i<list.size();i++){
		  			Hashtable map = (Hashtable)list.get(i);
		  			String user_id="",user_name="",passwd="",state_code="",user_class="",question="",answer="",inter_num="",real_name="",phone="",email="",cellphone="",fax="",qq="",msn="",post_code="",address="",sex="",birth="",area_attr="",marry="",blood_type="",career="",job="",hobby="",income="",last_login="",rsrv_str1="",rsrv_str2="",rsrv_str3="",rsrv_str4="",rsrv_str5="",rsrv_str6="",post_state="",in_date="",pub_user_id="";
		  			  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("user_name")!=null) user_name = map.get("user_name").toString();
  	if(map.get("passwd")!=null) passwd = map.get("passwd").toString();
  	if(map.get("state_code")!=null) state_code = map.get("state_code").toString();
  	if(map.get("user_class")!=null) user_class = map.get("user_class").toString();
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
  	if(map.get("rsrv_str1")!=null) rsrv_str1 = map.get("rsrv_str1").toString();
  	if(map.get("rsrv_str2")!=null) rsrv_str2 = map.get("rsrv_str2").toString();
  	if(map.get("rsrv_str3")!=null) rsrv_str3 = map.get("rsrv_str3").toString();
  	if(map.get("rsrv_str4")!=null) rsrv_str4 = map.get("rsrv_str4").toString();
  	if(map.get("rsrv_str5")!=null) rsrv_str5 = map.get("rsrv_str5").toString();
  	if(map.get("rsrv_str6")!=null) rsrv_str6 = map.get("rsrv_str6").toString();
  	if(map.get("post_state")!=null) post_state = map.get("post_state").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
if(in_date.length()>19)in_date=in_date.substring(0,19);
  	if(map.get("pub_user_id")!=null) pub_user_id = map.get("pub_user_id").toString();
		
		String userInfo = user_name + "|" + phone;
		  %>
		  <option value= "<%=userInfo%>"><%=user_name%></option>
		  <%
		  }
		  %>

      </td>

	  <tr>
	  <td style="padding-left:10px">信息模板:</td>
	  <td><textarea name = "SMS_Content" id="SMS_Content" style="width:400px;height:100px">您好$User_Name$,时间绸都网在搞活动！</textarea></td>
	  </tr>
	  </table>
	        <br/>
			      <br/>
<table width="100%" cellspacing="0" cellpadding="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" value="6605" name="bpm_id">
				<input type="hidden" value="" name="all_trage_user" id="all_trage_user">
				<input type="submit" onclick="submitForm();" value="提交" name="tradeSub" class="buttoncss">&nbsp;&nbsp;
				<input type="button" onclick="window.location.href='index.jsp';" value="返回" name="tradeRut" class="buttoncss">
			</td>
		</tr>
	</table>
    </form>	

	
	<%
		 }
	%>

</body>

</html>
