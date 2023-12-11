<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_credit.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>ti_credit Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script language="javascript" type="text/javascript" src="/program/plugins/calendar/WdatePicker.js"></script>
	<script type="text/javascript" src="credit.js"></script>
</head>

<body>

  <% 
  	String credit_id="";
	
	String user_id="";
	if( session.getAttribute("session_user_id") != null )
	{
		user_id = session.getAttribute("session_user_id").toString();
	}
	
  	if(request.getParameter("credit_id")!=null) credit_id = request.getParameter("credit_id");
  	Ti_creditInfo ti_creditInfo = new Ti_creditInfo();
  	List list = ti_creditInfo.getListByPk(credit_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String cust_id="",credit_title="",credit_desc="",start_date="",end_date="",department="",in_date="";
  	if(map.get("cust_id")!=null) cust_id = map.get("cust_id").toString();
  	if(map.get("credit_title")!=null) credit_title = map.get("credit_title").toString();
  	if(map.get("credit_desc")!=null) credit_desc = map.get("credit_desc").toString();
  	if(map.get("start_date")!=null) start_date = map.get("start_date").toString();
  	if(map.get("end_date")!=null) end_date = map.get("end_date").toString();
  	if(map.get("department")!=null) department = map.get("department").toString();
  	if(map.get("in_date")!=null) in_date = map.get("in_date").toString();

	
	
	String s_cust_name = "";
	if(request.getParameter("search_cust_name")!=null && !request.getParameter("search_cust_name").equals("")){
		s_cust_name = request.getParameter("search_cust_name");
	}
	String s_credit_title = "";
	if(request.getParameter("search_credit_title")!=null && !request.getParameter("search_credit_title").equals("")){
		s_credit_title = request.getParameter("search_credit_title");
	}
	String s_department = "";
	if(request.getParameter("search_department")!=null && !request.getParameter("search_department").equals("")){
		s_department = request.getParameter("search_department");
	}	
	String s_EndDate1 = "";
	if(request.getParameter("search_EndDate1")!=null && !request.getParameter("search_EndDate1").equals("")){
		s_EndDate1 = request.getParameter("search_EndDate1");
	}
	String s_EndDate2 = "";
	if(request.getParameter("search_EndDate2")!=null && !request.getParameter("search_EndDate2").equals("")){
		s_EndDate2 = request.getParameter("search_EndDate2");
	}
	String s_StartDate2 = "";
	if(request.getParameter("search_StartDate2")!=null && !request.getParameter("search_StartDate2").equals("")){
		s_StartDate2 = request.getParameter("search_StartDate2");
	}
	String s_StartDate1 = "";
	if(request.getParameter("search_StartDate1")!=null && !request.getParameter("search_StartDate1").equals("")){
		s_StartDate1 = request.getParameter("search_StartDate1");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	String para ="/program/admin/custcredit/index.jsp?search_cust_name="+s_cust_name+"&search_credit_title="+s_credit_title+"&search_department="+s_department+"&search_EndDate1="+s_EndDate1+"&search_EndDate2="+s_EndDate2+"&search_StartDate2="+s_StartDate2+"&search_StartDate1="+s_StartDate1+"&iStart="+Integer.parseInt(iStart);
  %>
	
	<h1>修改会员证书信息</h1>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		

		
		<tr>
			<td align="right" width="20%">
				证书名称<font color="red">*</font>
			</td>
			<td><input name="credit_title" id="credit_title" value="<%=credit_title %>" type="text"  maxlength="50"/></td>
		</tr>
		
				<tr>
			<td align="right" width="10%">
				证书图片:
			</td>
			<td>
				<jsp:include page="/program/inc/uploadImgInc.jsp">
					<jsp:param name="attach_root_id"
				value="<%=credit_id%>" />
				</jsp:include>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				证书详情:
			</td>
			<td>
			<textarea name="credit_desc" ><%=credit_desc%></textarea>
			<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
			<script type="text/javascript">
				CKEDITOR.replace('credit_desc');
			</script>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				证书有效起始时间<font color="red">*</font>
			</td>
			<td>
			<input value="<%=start_date %>" name="start_date" type="text" id="s_date" class="Wdate" value="" onclick="WdatePicker({maxDate:'#F{$dp.$D(\'e_date\',{d:-1})}',readOnly:true})" size="15"  width="150px"/>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				证书有效截止时间<font color="red">*</font>
			</td>
			<td>
			<input value="<%=end_date %>" name="end_date" id="e_date" type="text" class="Wdate" value="" onclick="WdatePicker({minDate:'#F{$dp.$D(\'s_date\',{d:1})}',readOnly:true})" size="15" width="150px"/>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				发证机关:
			</td>
			<td><input name="department" id="department" value="<%=department %>" type="text" maxlength="50"/></td>
		</tr>
		
<input name="user_id" id="user_id" value="<%=user_id %>" type="hidden" />
	<input name="cust_id" id="cust_id" value="<%=cust_id %>" type="hidden"  />		
<input name="in_date" id="in_date" value="" type="hidden" />
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="1955" />
	  			<input type="hidden" name="credit_id" value="<%=credit_id %>" />
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onclick="return checkInfo();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
