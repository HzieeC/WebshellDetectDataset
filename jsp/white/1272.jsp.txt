<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ts_custclass.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>修改级别信息</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="js_search.js"></script>
</head>

<body>

  <% 
  
  	String cust_id="",user_id="";	
	if( session.getAttribute("session_cust_id") != null )
	{
		cust_id = session.getAttribute("session_cust_id").toString();
	}
	if( session.getAttribute("session_user_id") != null )
	{
		user_id = session.getAttribute("session_user_id").toString();
	}
	
	
  	String cust_class="";
  	if(request.getParameter("cust_class")!=null) cust_class = request.getParameter("cust_class");
  	Ts_custclassInfo ts_custclassInfo = new Ts_custclassInfo();
  	List list = ts_custclassInfo.getListByPk(cust_class);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String cust_class_name="",remark="";
  	if(map.get("cust_class_name")!=null) cust_class_name = map.get("cust_class_name").toString();
  	if(map.get("remark")!=null) remark = map.get("remark").toString();

	
	String search_cust_class = "",search_cust_class_name="";
	if(request.getParameter("s_cust_class")!=null && !request.getParameter("s_cust_class").equals("")){
		search_cust_class = request.getParameter("s_cust_class");
	}
	if(request.getParameter("s_cust_class_name")!=null && !request.getParameter("s_cust_class_name").equals("")){
		search_cust_class_name = request.getParameter("s_cust_class_name");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String para ="/program/admin/custclass/index.jsp?s_cust_class_name="+search_cust_class_name+"&s_cust_class="+search_cust_class+"&iStart="+Integer.parseInt(iStart);
  %>
	
	<h1>修改级别信息</h1>

	
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				级别名称<font color="red">*</font>
			</td>
			<td><input name="cust_class_name" id="cust_class_name" value="<%=cust_class_name %>" type="text" /></td>
		</tr>
		
		
		
		<tr>
			<td align="right" width="10%">
				备注:
			</td>
			<td><input name="remark" id="remark" value="<%=remark %>" type="text" /></td>
		</tr>
		
		
		<input name="cust_oper_person" id="cust_oper_person" value="<%=user_id %>" type="hidden" />
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="5104" />
	  			<input type="hidden" name="cust_class" value="<%=cust_class %>" />
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkInfo();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
