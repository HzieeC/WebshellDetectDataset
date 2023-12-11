<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_vote.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>修改在线调查</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type="text/javascript" src="vote.js"></script>
</head>

<body>

  <% 
  	String vote_id="";
  	if(request.getParameter("vote_id")!=null) vote_id = request.getParameter("vote_id");
  	Ti_voteInfo ti_voteInfo = new Ti_voteInfo();
  	List list = ti_voteInfo.getListByPk(vote_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String cust_id="",vote_title="",start_date="",end_date="",is_multi="",vote_count="",user_id="",in_date="";
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("vote_title")!=null) vote_title = map.get("vote_title").toString();
  	if(map.get("start_date")!=null){
	start_date = map.get("start_date").toString();
	if(start_date.length()>10){
	  start_date=start_date.substring(0,10);
	}
	}
  	if(map.get("end_date")!=null) {
	end_date = map.get("end_date").toString();
	if(end_date.length()>10){
	  end_date=end_date.substring(0,10);
	}
	}
  	if(map.get("is_multi")!=null) is_multi = map.get("is_multi").toString();
  	if(map.get("vote_count")!=null) vote_count = map.get("vote_count").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();

	String g_title = "";
	if(request.getParameter("seach_vote_title")!=null && !request.getParameter("seach_vote_title").equals("")){
		g_title = request.getParameter("seach_vote_title");
	}
	String g_start_date = "";
   if(request.getParameter("start_date")!=null && !request.getParameter("start_date").equals("")){
	g_start_date = request.getParameter("start_date");
	}
	String g_end_date = "";
	if(request.getParameter("end_date")!=null && !request.getParameter("end_date").equals("")){
	g_end_date = request.getParameter("end_date");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	
	String para ="/program/admin/vote/index.jsp?seach_vote_title="+g_title+"&end_date="+g_end_date+"&start_date="+g_start_date+"&iStart="+Integer.parseInt(iStart);
  %>
	
	<h1>修改在线调查</h1>
	
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
			<td align="right" width="15%">
				调查主题<font color="red">*</font>
			</td>
			<td><input name="vote_title" id="vote_title" value="<%=vote_title %>" style="width:150px;" maxlength="50" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				开始时间<font color="red">*</font>
			</td>
			<td><input name="start_date" id="start_date" type="text" class="Wdate" value="<%=start_date%>" 
						
		onclick="WdatePicker({maxDate:'#F{$dp.$D(\'end_date\',{d:-1})}',readOnly:true})" style="width:150px;"/></td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				结束时间<font color="red">*</font>
			</td>
			<td><input name="end_date" id="end_date" type="text" class="Wdate" value="<%=end_date%>" 
						
			onclick="WdatePicker({minDate:'#F{$dp.$D(\'start_date\',{d:1})}',readOnly:true})" style="width:150px;"/></td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				是否支持多选:
			</td>
			<td><input name="is_multi" id="is_multi" type="radio"  value="0"  <%if(is_multi.equals("0")) out.print("checked");%> />是&nbsp;
             <input name="is_multi" id="is_multi" type="radio"  value="1" <%if(is_multi.equals("1")) out.print("checked");%> />否&nbsp;&nbsp;</td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				投票数:
			</td>
			<td><input name="vote_count" id="vote_count" value="<%=vote_count %>" type="text" size="5" onkeyup="if(isNaN(value))execCommand('undo')"/>（只能为数字）</td>
		</tr>
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="7721" />
	  			<input type="hidden" name="vote_id" value="<%=vote_id %>" />
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onClick="return chekedform2()"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
