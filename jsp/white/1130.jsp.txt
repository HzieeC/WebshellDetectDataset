<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_interview_note.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>面试通知信息管理</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>

  <% 
  	String trade_id="";
  	if(request.getParameter("trade_id")!=null) trade_id = request.getParameter("trade_id");
  	Ti_interview_noteInfo ti_interview_noteInfo = new Ti_interview_noteInfo();
  	List list = ti_interview_noteInfo.getListByPk(trade_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String reume_user_id="",job_id="",do_date="",note_title="",note_desc="",note_addr="",note_result="",rsrv_str1="",rsrv_str2="",rsrv_str3="",cust_id="",in_date="",user_id="",remark="";
					String user_name="",job_title="",cust_name="";
	
	if(map.get("user_name")!=null) user_name = map.get("user_name").toString();
	if(map.get("job_title")!=null) job_title = map.get("job_title").toString();
	if(map.get("cust_name")!=null) cust_name = map.get("cust_name").toString();
	
	if(map.get("reume_user_id")!=null) reume_user_id = map.get("reume_user_id").toString();
  	if(map.get("job_id")!=null) job_id = map.get("job_id").toString();
  	if(map.get("do_date")!=null) do_date = map.get("do_date").toString();
  	if(map.get("note_title")!=null) note_title = map.get("note_title").toString();
  	if(map.get("note_desc")!=null) note_desc = map.get("note_desc").toString();
  	if(map.get("note_addr")!=null) note_addr = map.get("note_addr").toString();
  	if(map.get("note_result")!=null) note_result = map.get("note_result").toString();
  	if(map.get("rsrv_str1")!=null) rsrv_str1 = map.get("rsrv_str1").toString();
  	if(map.get("rsrv_str2")!=null) rsrv_str2 = map.get("rsrv_str2").toString();
  	if(map.get("rsrv_str3")!=null) rsrv_str3 = map.get("rsrv_str3").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

  %>
	
	<h1>修改面试通知信息</h1>
	
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
				通知标题:
			</td>
			<td><%=note_title%></td>
		</tr>	
		
		<tr>
			<td align="right" width="10%">
				应聘人:
			</td>
			<td><%=user_name%></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				应聘职位:
			</td>
			<td><%=job_title%></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				公司名称:
			</td>
			<td><%=cust_name%></td>
		</tr>		
		
		<tr>
			<td align="right" width="10%">
				面试时间:
			</td>
			<td><%=do_date%></td>
		</tr>

		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="7776" />
	  			<input type="hidden" name="trade_id" value="<%=trade_id %>" />
				
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
