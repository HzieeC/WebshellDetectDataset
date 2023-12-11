<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_user.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>ti_user Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String user_id="";
  	if(request.getParameter("user_id")!=null) user_id = request.getParameter("user_id");
  	Ti_userInfo ti_userInfo = new Ti_userInfo();
  	List list = ti_userInfo.getListByPk(user_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String user_name="",cust_id="",passwd="",user_state="",passwd_ques="",passwd_answer="",real_name="",org_id="",role_code="",last_login="",in_date="",pub_user_id="",remark="";
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
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("pub_user_id")!=null) pub_user_id = map.get("pub_user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

  %>
	
	<h1>Update ti_user</h1>
	
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
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				user_name:
			</td>
			<td><input name="user_name" id="user_name" value="<%=user_name %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_id:
			</td>
			<td><input name="cust_id" id="cust_id" value="<%=cust_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				passwd:
			</td>
			<td><input name="passwd" id="passwd" value="<%=passwd %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				user_state:
			</td>
			<td><input name="user_state" id="user_state" value="<%=user_state %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				passwd_ques:
			</td>
			<td><input name="passwd_ques" id="passwd_ques" value="<%=passwd_ques %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				passwd_answer:
			</td>
			<td><input name="passwd_answer" id="passwd_answer" value="<%=passwd_answer %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				real_name:
			</td>
			<td><input name="real_name" id="real_name" value="<%=real_name %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				org_id:
			</td>
			<td><input name="org_id" id="org_id" value="<%=org_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				role_code:
			</td>
			<td><input name="role_code" id="role_code" value="<%=role_code %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				last_login:
			</td>
			<td><input name="last_login" id="last_login" value="<%=last_login %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				in_date:
			</td>
			<td><input name="in_date" id="in_date" value="<%=in_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				pub_user_id:
			</td>
			<td><input name="pub_user_id" id="pub_user_id" value="<%=pub_user_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				remark:
			</td>
			<td><input name="remark" id="remark" value="<%=remark %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="2578" />
	  			<input type="hidden" name="user_id" value="<%=user_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
