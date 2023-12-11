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
	<link rel="stylesheet" href="/program/admin/sms/plugins/thickbox/thickbox.css" type="text/css" media="screen" />
	<script type="text/javascript" src="/program/admin/sms/js/jquery-1.4.2.min.js"></script>
	<script type="text/javascript" src="/program/admin/sms/sms.js"></script>
	<script type="text/javascript" src="/program/admin/sms/plugins/thickbox/thickbox.js"></script>
	
<script>
	$(document).ready(function() {
		$('#userInfo').dblclick(function() {
			var nums = $('#user_num').text();
			if($(this).find(':selected')) {
			$('#user_num').text(""+(nums-1));	
			$('#userInfo option:selected').remove();		  
			}

		});
	});
</script>

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
		  <span>2、 选择相应用户名双击后可删除用户</span><br/>
		  <span>3、 点击查询会员按钮，在弹出的页面中可选择用户，删除用户！</span><br/>
		  <span>4、$User_Name$为短信模板用户名称，用于替换用户名！</span>

		  </td>

        </tr>

      </table>

      <br/>


	<% 
		int listsize = 0;
		if(list!=null && list.size()>0){
			listsize = list.size();
	%>
	


		


	<table width="100%" cellpadding="1" cellspacing="1" border="0" class="listtab" >
		<tr>
			<td align="right" width="15%">选择用户:</td>
			<td  style="padding-left:15px;">
      <select id="userInfo" multiple="multiple" name="userInfo" style="width:400px;height:200px">		
		
	      </select>
		  
      </td>
	</tr>
	<tr>
		<td>
		</td>
		<td>
			  已选中<span id="user_num">0</span>个用户
			<a href="searchPersonal.jsp?TB_iframe=true&height=400&width=700&KeepThis=true" title="会员列表" class="thickbox">
			<button class="buttoncss">查询会员</button>
			</a>
	</td>
	</tr>
	  <tr>
	  <td align="right" width="15%">短信内容:</td>
	  <td colspan="2"  style="padding-left:15px;"><textarea name = "SMS_Content" id="SMS_Content" style="width:400px;height:100px">您好$User_Name$,世界绸都网在搞活动！</textarea></td>
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
