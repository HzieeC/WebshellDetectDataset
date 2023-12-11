<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_admin.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>ti_admin Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type='text/javascript' src='js_pwd.js'></script>
</head>

<body>

  <% 
  	String cust_id="";
  	if(request.getParameter("cust_id")!=null) cust_id = request.getParameter("cust_id");
  	Ti_adminInfo ti_adminInfo = new Ti_adminInfo();
  	String allUsers = ti_adminInfo.getAll_User(cust_id);
  	
  
String s_cust_name = "",s_area_attr="",s_class_attr="",s_email="",s_recommend="",s_StartDate="",s_EndDate="",s_cust_state="",s_cust_class="",s_user_name="";
	if(request.getParameter("search_cust_name")!=null && !request.getParameter("search_cust_name").equals("")){
		s_cust_name = request.getParameter("search_cust_name");
	}
	if(request.getParameter("search_area_attr")!=null && !request.getParameter("search_area_attr").equals("")){
		s_area_attr = request.getParameter("search_area_attr");
	}
	if(request.getParameter("search_class_attr")!=null && !request.getParameter("search_class_attr").equals("")){
		s_class_attr = request.getParameter("search_class_attr");
	}

	if(request.getParameter("search_email")!=null && !request.getParameter("search_email").equals("")){
		s_email = request.getParameter("search_email");
	}

	if(request.getParameter("search_cust_state")!=null && !request.getParameter("search_cust_state").equals("")){
		s_cust_state = request.getParameter("search_cust_state");
	}
						
	if(request.getParameter("search_cust_class")!=null && !request.getParameter("search_cust_class").equals("")){
		s_cust_class = request.getParameter("search_cust_class");
	}

	if(request.getParameter("search_recommend")!=null && !request.getParameter("search_recommend").equals("")){
		s_recommend = request.getParameter("search_recommend");
	}
	
	
	if(request.getParameter("search_StartDate")!=null && !request.getParameter("search_StartDate").trim().equals("")){
		s_StartDate = request.getParameter("search_StartDate");
	}
	
	if(request.getParameter("search_EndDate")!=null && !request.getParameter("search_EndDate").trim().equals("")){
		s_EndDate = request.getParameter("search_EndDate");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String para ="/program/admin/custpwdreset/index.jsp?search_cust_name="+s_cust_name+"&search_area_attr="+s_area_attr+"&search_class_attr="+s_class_attr+"&search_email="+s_email+"&search_cust_state="+s_cust_state+"&search_cust_class="+s_cust_class+"&search_cust_state="+s_cust_state+"&search_recommend="+s_recommend+"&search_StartDate="+s_StartDate+"&search_EndDate="+s_EndDate+"&iStart="+Integer.parseInt(iStart);
  %>
	
	<h1>重置企业用户密码</h1>
	

	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
<tr>
	<td align="right" width="20%">
	用户名<font color="red">*</font>
	</td>
	<td>
		<select name="user_id" id="user_id" style="width:200px;">
		<option value="">
		请选择相应用户
		</option>
		<%=allUsers%>
		</select>
	</td>
</tr>		
<tr>
	<td align="right" width="20%">
	新密码<font color="red">*</font>
	</td>
	<td><input type="password" name="new_passwd" id="new_passwd" maxlength="32" size="32" style="width:200px;"/>&nbsp;6-20 个字符，只允许数字和英文字母，有大小写区分</td>
</tr>
<tr>
	<td align="right" width="20%">
	重复密码<font color="red">*</font>
	</td>
	<td><input type="password" name="passwd" id="confirm_password" maxlength="32" size="32" style="width:200px;"/>&nbsp;6-20 个字符，只允许数字和英文字母，有大小写区分</td>
</tr>
	
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="1243" />
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="button" class="buttoncss" name="tradeSub" onclick="submitForm()" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
