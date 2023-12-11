<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_defense.*" %>
<%@page import="com.bizoss.trade.ti_memcomplaint.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>投诉举报</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  
  String session_cust_id="",session_user_id="";
	if(session.getAttribute("session_cust_id")!=null){
	  session_cust_id=session.getAttribute("session_cust_id").toString();   
	}
	if(session.getAttribute("session_user_id")!=null){
	  session_user_id=session.getAttribute("session_user_id").toString();   
	}
  	String com_id="";
  	if(request.getParameter("com_id")!=null) com_id = request.getParameter("com_id");
	Ti_defenseInfo ti_defenseInfo = new Ti_defenseInfo();
  	List defenselist = ti_defenseInfo.getdefenseListByPk(com_id);
	Ti_memcomplaintInfo ti_memcomplaintInfo = new Ti_memcomplaintInfo();
  	List list = ti_memcomplaintInfo.getListByPk(com_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String send_cust_id="",get_cust_id="",send_cust_name="",get_cust_name="",info_id="",content="",in_date="",deal_state="",deal_user_id="",deal_date="",deal_result="",user_id="",remark="";
  	if(map.get("send_cust_name")!=null) send_cust_name = map.get("send_cust_name").toString();
	if(map.get("get_cust_name")!=null) get_cust_name = map.get("get_cust_name").toString();
	if(map.get("send_cust_id")!=null) send_cust_id = map.get("send_cust_id").toString();
  	if(map.get("get_cust_id")!=null) get_cust_id = map.get("get_cust_id").toString();
  	if(map.get("info_id")!=null) info_id = map.get("info_id").toString();
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
	if(in_date.length()>19) in_date = in_date.substring(0,19);
  	if(map.get("deal_state")!=null) deal_state = map.get("deal_state").toString();
  	if(map.get("deal_user_id")!=null) deal_user_id = map.get("deal_user_id").toString();
  	if(map.get("deal_date")!=null) deal_date = map.get("deal_date").toString();
	if(deal_date.length()>19) deal_date = deal_date.substring(0,19);
  	if(map.get("deal_result")!=null) deal_result = map.get("deal_result").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

  %>
	
	<h1>查看投诉举报</h1>
	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		<tr>
		<td colspan="4">
	   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
	   <span style="font-size:14px;font-weight:bold;">投诉信息</span></td>
		    </tr>
		<tr>
			<td align="right" width="10%">
				投诉人：
			</td>
			<td><%=send_cust_name%></td>
			<td align="right" width="10%">
				被投诉人：
			</td>
			<td><%=get_cust_name%> </td>
		</tr>	
		<tr>
			<td align="right" width="10%">
				投诉时间：
			</td>
			<td><%=in_date %></td>
			<td align="right" width="10%">
				投诉内容：
			</td>
			<td><%=content%></td>
		</tr>
		<tr>
			<td align="right" width="10%">
				投诉人备注：
			</td>
			<td colspan="3"><%=remark%></td>
		</tr>
		
		
		<% 
		if(defenselist!=null && defenselist.size()>0){%>
		<tr>
		<td colspan="4">
	   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
	   <span style="font-size:14px;font-weight:bold;">申辩信息</span></td>
		    </tr>
		<tr>
		<td align="right" width="10%">申辩历史：</td>
		<td colspan="3" >
		  		<%for(int i=0;i<defenselist.size();i++){
		  			Hashtable defmap = (Hashtable)defenselist.get(i);
		  			String def_id="",defcontent="",defin_date="";
		  			  	if(defmap.get("content")!=null) defcontent = defmap.get("content").toString();
						if(content.length()>19)defcontent=defcontent.substring(0,19);
						if(defmap.get("in_date")!=null) defin_date = defmap.get("in_date").toString();
						if(defin_date.length()>19)defin_date=defin_date.substring(0,19);
						
		  %>
		
		  内容：<%=defcontent%></br>
		  时间：<%=defin_date%> </br></br>
		  	
		<%}%></td></tr><%}%>
		<%if(deal_state.equals("1")) { %>
		<tr>
		<td colspan="4">
	   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
	   <span style="font-size:14px;font-weight:bold;">处理信息</span></td>
		    </tr>
		<tr>
			<td align="right" width="10%">
				处理状态：
			</td>
			<td><%out.print("已处理");%></td>
			<td align="right" width="10%">
				处理时间：
			</td>
			<td><%=deal_date %></td>
		</tr>
		<tr>			
			<td align="right" width="10%">
				处理结果：
			</td>
			<td colspan="3"><%=deal_result %></td>
		</tr>
		<%}%>
		<%if(deal_state.equals("0")) { %>
		<tr>
		<td colspan="4">
	   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;
	   <span style="font-size:14px;font-weight:bold;">处理信息</span></td>
		    </tr>
		<tr><input type="hidden" name="deal_state" id="deal_state" value="1">
			
			<td align="right" width="10%">
				处理结果<font color="red">*</font>
			</td>
			<td colspan="3"><textarea name="deal_result" id="deal_result" rows="8" cols="60"></textarea></td>
		</tr><%}%>
	     
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="5754" />
	  		    <input type="hidden" name="com_id" value="<%=com_id %>" />
				<input type="hidden" name="deal_user_id" value="<%=session_user_id %>" />
				<%if(deal_state.equals("0")) { %>
				<input type="button" class="buttoncss" name="tradeSub" onClick="return checkInfo()"  value="提交" />&nbsp;&nbsp;				
				<%}%>
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
<script>
function checkInfo(){
 if(document.getElementById("deal_result").value==""){
   alert("请输入处理结果！");
   return false;
 }
 document.addForm.submit();
}
</script>