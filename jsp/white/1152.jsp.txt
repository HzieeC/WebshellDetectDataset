<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_job_attention.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>求职关注信息</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String trade_id="";
  	if(request.getParameter("trade_id")!=null) trade_id = request.getParameter("trade_id");
  	Ti_job_attentionInfo ti_job_attentionInfo = new Ti_job_attentionInfo();
  	List list = ti_job_attentionInfo.getListByPk(trade_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String jobname="",in_name="",in_reviews="",level="",clip_state="",cust_id="",in_date="",user_id="",remark="";
  	if(map.get("jobname")!=null) jobname = map.get("jobname").toString();
  	if(map.get("in_name")!=null) in_name = map.get("in_name").toString();
  	if(map.get("in_reviews")!=null) in_reviews = map.get("in_reviews").toString();
  	if(map.get("level")!=null) level = map.get("level").toString();
  	if(map.get("clip_state")!=null) clip_state = map.get("clip_state").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

  %>
	
	<h1>查看求职关注信息</h1>
	
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
				招聘职位：
			</td>
			<td><%=jobname %></td>
			<td align="right" width="10%">
				招聘描述：
			</td>
			<td><%=in_reviews %></td>
		</tr>
		
	   <tr>
			<td align="right" width="10%">
				优先级：
			</td>
			<td><%=level %></td>
			<td align="right" width="10%">
				面试状态;
			</td>
			<td><%if(clip_state.equals("0")) out.print("未通知面试"); if(clip_state.equals("1")) out.print("已通知面试"); %></td>
		
		</tr>
		
		<tr>
			<td align="right" width="10%">
				时间：
			</td>
			<td><%=in_date %></td>
			<td align="right" width="10%">
				备注：
			</td>
			<td><%=remark %></td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="6270" />
	  			<input type="hidden" name="trade_id" value="<%=trade_id %>" />
				<!--<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;-->
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
