<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_finance.*" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.frame.util.PageTools" %>
<%@page import="com.bizoss.trade.ti_finance_history.*" %>

<%
	request.setCharacterEncoding("UTF-8");
	Map ti_finance = new Hashtable();
	String session_cust_id="";
	if(session.getAttribute("session_cust_id")!=null){
	  session_cust_id=session.getAttribute("session_cust_id").toString(); 
	}
	Ti_financeInfo ti_financeInfo = new Ti_financeInfo();
	String iStart = "0";
	int limit = 20;
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	Hashtable mapf = new Hashtable();
	mapf.put("cust_id",session_cust_id);
	mapf.put("finance_type","0");
	mapf.put("account_type","0");
	List list = ti_financeInfo.getListByPk2(mapf);
	Hashtable mapp = new Hashtable();
  if(list!=null && list.size()>0) mapp = (Hashtable)list.get(0);
	String  vmoney="0",use_vmoney="",frz_vmoney="",remarkk="";
	String cust_name = "";		 
	if(mapp.get("vmoney")!=null) vmoney = mapp.get("vmoney").toString();	
		
		
	Map ti_finance_history = new Hashtable();
	ti_finance_history.put("cust_id",session_cust_id);
	ti_finance_history.put("type","0");
	
	//0：积分 1：虚拟币
	Ti_finance_historyInfo ti_finance_historyInfo = new Ti_finance_historyInfo();
	List listt = ti_finance_historyInfo.getListByPage(ti_finance_history,Integer.parseInt(iStart),limit);
	int counter = ti_finance_historyInfo.getCountByObj(ti_finance_history);
	String pageString = new PageTools().getGoogleToolsBar(counter,"index.jsp?iStart=",Integer.parseInt(iStart),limit);
%>

<html>
  <head>
    
    <title>我的积分管理</title>
	<link rel="stylesheet" rev="stylesheet" href="/templets/html/8diansc/css/main_right.css" type="text/css" />
		<link href="/program/company/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="/js/commen.js"></script>
	<script type="text/javascript" src="inter.js"></script>
</head>

<body>
	<tr>
        <td><table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_main">
            <tr>
              <th width="5%" height="40" align="center"><img src="/program/member/index/images/icon1.gif" ></th>
              <th width="65%"><h3>我的积分管理</h3></th>
			  <th width="30%"></th>
            </tr>
          </table>
	
	<form action="index.jsp" name="indexForm" method="post">
	
 
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left"><br>
		  		<h2 style="color:#FF4524">您当前的总积分为：<%=vmoney%>分</h2>
		  		 
		   
		  </td>
        </tr>
      </table>
      <br/>
	  
	<% 
		int listsize = 0;
		if(listt!=null && listt.size()>0){
			listsize = listt.size();
	%>  
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString %></td></tr>
	</table>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="table_main" border="0">
		<tr>
		  <th>异动数额</th>
			<th>异动原因</th>			
		  <th>异动时间</th>		  	
		</tr>		
		<% 
		  		for(int i=0;i<listt.size();i++){
		  			Hashtable map = (Hashtable)listt.get(i);
		  			String trade_id="",cust_id="",num="",vtype="",reason="",in_date="",user_id="",remark="";
		  			  	if(map.get("trade_id")!=null) trade_id = map.get("trade_id").toString();
						if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
						if(map.get("num")!=null) num = map.get("num").toString();
						if(map.get("type")!=null) vtype = map.get("type").toString();
						if(map.get("reason")!=null) reason = map.get("reason").toString();
						if(map.get("in_date")!=null) in_date = map.get("in_date").toString();
						if(in_date.length()>19)in_date=in_date.substring(0,19);
						if(map.get("user_id")!=null) user_id = map.get("user_id").toString();
						if(map.get("remark")!=null) remark = map.get("remark").toString();

		  %>
		
		<tr>
		  <td><%=num%></td>
			<td><%=reason%></td>
		  <td><%=in_date%></td>
		</tr>
		
		  <%
		  		}
		  %>
		
	</table>
	
	 
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td><%=pageString %></td></tr>
	</table>
	
	<%
		}else{
	%>
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="tablehe">
		<tr><td>当前无历史异动记录！</td></tr>
	</table>
	<%}%>
	
	<input type="hidden" name="listsize" id="listsize" value="<%=listsize %>" />
	<input type="hidden" name="pkid" id="pkid" value="" />
	<input type="hidden" name="bpm_id" id="bpm_id" value="0099" />
	</form>
</body>
</html>
