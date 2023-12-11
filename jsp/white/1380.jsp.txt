<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="com.bizoss.trade.ti_workexper.*" %>
<%@page import="java.util.*" %>
<html>
  <head>
    
    <title>ti_workexper Manager</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
</head>

<body>

  <% 
  	String work_id="";
  	if(request.getParameter("work_id")!=null) work_id = request.getParameter("work_id");
  	Ti_workexperInfo ti_workexperInfo = new Ti_workexperInfo();
  	List list = ti_workexperInfo.getListByPk(work_id);
  	Hashtable map = new Hashtable();
  	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
  	
  	String resume_id="",company="",cust_type="",cust_size="",bisiness="",depart="",career_type="",career="",work_date="",salary="",work_desc="";
  	if(map.get("resume_id")!=null) resume_id = map.get("resume_id").toString();
  	if(map.get("company")!=null) company = map.get("company").toString();
  	if(map.get("cust_type")!=null) cust_type = map.get("cust_type").toString();
  	if(map.get("cust_size")!=null) cust_size = map.get("cust_size").toString();
  	if(map.get("bisiness")!=null) bisiness = map.get("bisiness").toString();
  	if(map.get("depart")!=null) depart = map.get("depart").toString();
  	if(map.get("career_type")!=null) career_type = map.get("career_type").toString();
  	if(map.get("career")!=null) career = map.get("career").toString();
  	if(map.get("work_date")!=null) work_date = map.get("work_date").toString();
  	if(map.get("salary")!=null) salary = map.get("salary").toString();
  	if(map.get("work_desc")!=null) work_desc = map.get("work_desc").toString();

  %>
	
	<h1>Update ti_workexper</h1>
	
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
				resume_id:
			</td>
			<td><input name="resume_id" id="resume_id" value="<%=resume_id %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				company:
			</td>
			<td><input name="company" id="company" value="<%=company %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_type:
			</td>
			<td><input name="cust_type" id="cust_type" value="<%=cust_type %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				cust_size:
			</td>
			<td><input name="cust_size" id="cust_size" value="<%=cust_size %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				bisiness:
			</td>
			<td><input name="bisiness" id="bisiness" value="<%=bisiness %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				depart:
			</td>
			<td><input name="depart" id="depart" value="<%=depart %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				career_type:
			</td>
			<td><input name="career_type" id="career_type" value="<%=career_type %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				career:
			</td>
			<td><input name="career" id="career" value="<%=career %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				work_date:
			</td>
			<td><input name="work_date" id="work_date" value="<%=work_date %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				salary:
			</td>
			<td><input name="salary" id="salary" value="<%=salary %>" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				work_desc:
			</td>
			<td><input name="work_desc" id="work_desc" value="<%=work_desc %>" type="text" /></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="1414" />
	  			<input type="hidden" name="work_id" value="<%=work_id %>" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" />&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
