<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_reply.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>审核回答</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="index.js"></script>
</head>

<body>

  <% 
  	String trade_id="";
  	if(request.getParameter("trade_id")!=null) trade_id = request.getParameter("trade_id");
  	Ti_replyInfo ti_replyInfo = new Ti_replyInfo();
  	List list = ti_replyInfo.getListByPk(trade_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String ask_id="",state="",contents="",good="",user_id="",cust_id="",in_date="",title="";
  	if(map.get("ask_id")!=null) ask_id = map.get("ask_id").toString();
  	if(map.get("state")!=null) state = map.get("state").toString();
  	if(map.get("contents")!=null) contents = map.get("contents").toString();
  	if(map.get("good")!=null) good = map.get("good").toString();
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("title")!=null) title = map.get("title").toString();

  %>
	
	<h1>审核回答</h1>
	
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
				问题:
			</td>
			<td colspan="3"><%=title%></td>
			
		</tr>
		
		<tr>
			<td align="right" width="10%">
				审批状态<font color="red">*</font>
			</td>
			<td>
			<%if(state.equals("0")) out.print("未审批");%>
            <%if(state.equals("2")) out.print("审批未通过");%>			
			</td>
			<td align="right" width="10%">
				发布日期:
			</td>
			<td><%=in_date%></td>
		</tr>
		
		<tr>
			
		</tr>
		
		<tr>
			<td align="right" width="10%">
				回答内容<font color="red">*</font>
			</td>
			<td colspan="3">
			<%=contents%>
			</td>
		</tr>
		
	</table>
	<script language="javascript">
		function failure(){
			document.getElementById("state").value="2";
			document.addForm.submit();
		}
	</script>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
			<input type="hidden" name="pkid" value="<%=trade_id %>" />
			   <input type="hidden" name="jumpurl" value="/program/admin/replyaudit/index.jsp" />				
				<input type="hidden" name="bpm_id" id="bpm_id" value="1566" />	  
				<input type="hidden" name="state" id="state" value="1">										
				<input type="submit" class="buttoncss" name="tradeSub" value="审批通过" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeSub" value="审批不通过" onClick="failure();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
