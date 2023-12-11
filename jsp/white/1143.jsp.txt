<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_memcomplaint.*,com.bizoss.trade.ti_personal.*,com.bizoss.trade.ti_customer.*,com.bizoss.trade.ti_admin.*" %>
<%@page import="java.util.*" %>

<%
String _user_id="";
if( session.getAttribute("session_user_id") != null )
	{
		_user_id = session.getAttribute("session_user_id").toString();
	}
	
	Ti_customerInfo ti_customerInfo=new Ti_customerInfo();
	
	Ti_personalInfo ti_personalInfo = new Ti_personalInfo();

  Ti_adminInfo ti_adminInfo = new Ti_adminInfo();
%>


<html>
  <head>
    
    <title>查看/修改投诉信息</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String info_id="";
  	if(request.getParameter("info_id")!=null) info_id = request.getParameter("info_id");
  	Ti_memcomplaintInfo ti_memcomplaintInfo = new Ti_memcomplaintInfo();
  	List list = ti_memcomplaintInfo.getListByPk(info_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String user_id="",cust_id="",content="",in_date="",deal_state="",deal_user_id="",deal_date="",deal_result="";
  	String cust_name="",user_name="",deal_user_name="";
  	if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("deal_user_id")!=null) deal_user_id = map.get("deal_user_id").toString();
  	
  		 cust_name =	ti_customerInfo.getCustNameByCustId(cust_id);
  		 user_name=ti_personalInfo.getPersonalNameByPersonalId(user_id);
			deal_user_name = ti_adminInfo.getUserNameByPK(deal_user_id);
			
  	if(map.get("content")!=null) content = map.get("content").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
  	if(map.get("deal_state")!=null) deal_state = map.get("deal_state").toString();
  	
  	if(map.get("deal_date")!=null) deal_date = map.get("deal_date").toString();
  	if(map.get("deal_result")!=null) deal_result = map.get("deal_result").toString();
	
	
	String _deal_state = "",_start_date="",_end_date="";

	if(request.getParameter("seach_deal_state")!=null && !request.getParameter("seach_deal_state").equals("")){
		_deal_state = request.getParameter("seach_deal_state");
	}
	if(request.getParameter("s_start_date")!=null && !request.getParameter("s_start_date").equals("")){
		_start_date = request.getParameter("s_start_date");
	}	
	if(request.getParameter("s_end_date")!=null && !request.getParameter("s_end_date").equals("")){
		_end_date = request.getParameter("s_end_date");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String para ="/program/admin/memcomplaint/index.jsp?seach_deal_state="+_deal_state+"&start_date="+_start_date+"&s_end_date="+_end_date+"&iStart="+Integer.parseInt(iStart);
  %>
	
	<h1>处理投诉信息</h1>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td  colspan="6">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0" />&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">投诉信息</span>			
			</td>
	    </tr>
		
		<tr>
			<td align="right">
				投诉人:
			</td>
			<td ><%=user_name%>
			<td align="right" width="12%">
				被投诉企业:
			</td>
			<td colspan="3"><%=cust_name %></td>
		</tr>
		
		
		<tr>
			<td align="right" width="10%">
				投诉内容:
			</td>
			<td colspan="6"><%=content %></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				投诉时间:
			</td>
			<td colspan="6"><%=in_date %></td>
		</tr>
		<textarea name="content" style="display:none;"><%=content%></textarea>
					
		<%
		if(deal_state.equals("1")){//已处理
		%>
				<tr>
			<td  colspan="6">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0" />&nbsp;&nbsp;
			   <span style="font-size:14px;font-weight:bold;">投诉信息处理结果</span>			
			</td>
	    </tr>
	    
			<tr>
			<td align="right" width="10%">
				处理结果:
			</td>
				<td colspan="6"><%=deal_result%></td>
			</tr>
				
				<tr>
					<td align="right" width="10%">
						处理人:
					</td>
					<td><%=deal_user_name%>
				
					<td align="right" width="10%">
					处理时间:
					</td>
					<td colspan="3"><%=deal_date%></td>
			</tr>
				
			
				</table>
		<input name="deal_result" value="<%=deal_result%>" type="hidden" />

	
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="6300" />
	  			<input type="hidden" name="info_id" value="<%=info_id%>" />
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
			
			
			
			
		<%}else{//未处理 %>
		
		<tr>
			<td  colspan="6">
			   &nbsp;&nbsp;<img src="/program/admin/images/infotip.gif" border="0">&nbsp;&nbsp;<span style="font-size:14px;font-weight:bold;">处理投诉信息</span>			</td>
    </tr>

		<tr>
			<td align="right" width="10%">
				处理结果:
			</td>
			<td colspan="6"> <textarea name="deal_result" id="deal_result" cols="50" rows="5" value="" type="text" ></textarea></td>
		</tr>
		
		<input name="deal_date" id="deal_date" value="" type="hidden" />
			<input name="deal_user_id" id="deal_user_id" value="<%=_user_id %>" type="hidden" />
			<input name="deal_state" id="deal_state" value="1" type="hidden" />
			<input name="cust_id" id="cust_id" value="<%=cust_id%>" type="hidden" />
			<input name="user_id" id="user_id" value="<%=user_id%>" type="hidden" />
			<input name="in_date" id="in_date" value="<%=in_date %>" type="hidden" />
			
		
	</table>
	

	
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="6300" />
	  			<input type="hidden" name="info_id" value="<%=info_id %>" />
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	
			
		<%}%>
		
		
	</form>
</body>

</html>
