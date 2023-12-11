<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.util.ParseXmlUtil" %>
<%@page import="com.bizoss.frame.util.Config" %>
<%@ page import="com.bizoss.trade.tb_commpara.*" %>
<%

	String node_name="";
	
    if(request.getParameter("node_name")!=null)
    {
		 node_name = request.getParameter("node_name");
 
	}
	
	Config configs = new Config();
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo();
	Map nodeNameMap = tb_commparaInfo.getOneCompara("77",node_name);
	String xmlPath = configs.getString("rootpath")+"WEB-INF/classes/quartz_job.xml";
	ParseXmlUtil parseXmlUtil = ParseXmlUtil.getInstance().init(xmlPath);
	
   
	%>
<html>
  <head>
    <title>自动更新设置</title>
		<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
		<script language="javascript" type="text/javascript" src="/program/admin/category/js_category.js"></script>
</head>

<body>

  <% 
  	String job_name="";
  	if(nodeNameMap.get("para_code1")!=null) job_name = nodeNameMap.get("para_code1").toString();
	

			
	Map updateMap = parseXmlUtil.getAttribute(node_name);

	String cron_expression = "";
	if(updateMap.get("cron-expression") != null){
		cron_expression = updateMap.get("cron-expression").toString();
	}	
				

  %>
	
	<h1>修改更新设置</h1>
	
	<table width="100%" align="center" cellpadding="0" cellspacing="0" class="dl_so">
        <tr>
          <td width="9%" align="center"><img src="/program/admin/index/images/ban_01.gif" /></td>
          <td width="91%" align="left">
		  <h4>更新规则说明</h4>
		  <span>1、秒   0-59   , - * /</span><br/>
		  <span>2、分   0-59   , - * /</span><br/>
		  <span>3、小时   0-23   , - * /</span><br/>
		  <span>4、日期   1-31   , - * ? / L W C</span><br/>
		  <span>5、月份   1-12 或者 JAN-DEC   , - * /</span><br/>
		  <span>6、星期   1-7 或者 SUN-SAT   , - * ? / L C #</span><br/>
		  <span>6、年（可选）   留空, 1970-2099   , - * / </span><br/>
		  <span>7、例如 "0 0 12 * * ?" 每天中午12点触发 </span><br/>
		  
		  
		  
		  </td>
        </tr>
      </table>
      <br/>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	<table width="100%" cellpadding="1" cellspacing="1" border="0"  class="listtab" >
		
		<tr>
			<td align="right" width="20%">
				计划名称<font color="#FF0000">*</font>
			</td>
			<td><%=job_name %></td>
		</tr>
		
		<tr>
			<td align="right" width="20%">
				计划规则<font color="#FF0000">*</font>
			</td>
			<td><input name="cron_expression" id="cron_expression" value="<%=cron_expression %>" type="text" maxlength="100"/></td>
		</tr>		
		
	 

	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="3543" />
				<input type="hidden" name="node_name" value="<%=node_name%>" />
				<input type="button"  class="buttoncss" name="tradeSub" value="提交"  onclick="submitForm();" />&nbsp;&nbsp;
				<input type="button"  class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>
<script>
	function submitForm(){
		var cron = document.getElementById("cron_expression").value;
		
		if(cron == ''){
			alert("计划规则不能为空！");
			return false;
		}
		
		document.addForm.submit();
		
	}
</script>
</html>
