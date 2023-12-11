<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@page import="java.util.*" %>
<%@page import="com.bizoss.trade.util.ParseXmlUtil" %>
<%@page import="com.bizoss.frame.util.Config" %>
<%@ page import="com.bizoss.trade.tb_commpara.*" %>

<%
	request.setCharacterEncoding("UTF-8");
	
	Config configs = new Config();
	Tb_commparaInfo tb_commparaInfo = new Tb_commparaInfo();
	List updateList = tb_commparaInfo.getListByParamAttr("77");
	String xmlPath = configs.getString("rootpath")+"WEB-INF/classes/quartz_job.xml";
	ParseXmlUtil parseXmlUtil = ParseXmlUtil.getInstance().init(xmlPath);
	int size = updateList.size();
	
	%>
	
<html>
  <head>
    
    <title>自动更新设置</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">

</head>

<body>
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td width="90%">
				<h1>自动更新设置</h1>
			</td>
			<td>
				
			</td>
		</tr>
	</table>
	
	<form action="index.jsp" name="indexForm" method="post">
	

	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
			
			</td>
			<td>
				总计:<%=size%>条
			</td>
		</tr>
	</table>
	
	<%
		if(updateList != null && size > 0){
	%>
	
	<table width="100%" cellpadding="1" cellspacing="1" class="listtab" border="0">
		<tr>
			
			<th>更新项目</th>
			
		  	<th>更新时间规则</th>
		  	
			<th width="10%">修改</th>
		</tr>
		
		
		  <% 	
			
			for(int i = 0;i < size; i++){
			
			Map map = (Hashtable)updateList.get(i);
			String node_name = "",job_name="";
			if(map.get("para_code2") != null && !map.get("para_code2").toString().equals("")){
				node_name = map.get("para_code2").toString();
			}
			if(map.get("para_code1") != null && !map.get("para_code1").toString().equals("")){
				job_name = map.get("para_code1").toString();
			}			
			
			if(!node_name.equals("")){
			
				Map updateMap = parseXmlUtil.getAttribute(node_name);

				String cron_expression = "";
				if(updateMap.get("cron-expression") != null){
					cron_expression = updateMap.get("cron-expression").toString();
				}	

		  %>
		
		<tr>
		  	<td>
			
			     <%=job_name%>
			
			</td>			
			
		  	<td>
			
			     <%=cron_expression%>
			
			</td>
			
		  	<td width="10%">
			
			    <a href="updateInfo.jsp?node_name=<%=node_name%>"><img src="/program/admin/images/edit.gif" title="修改" border="0"/></a>
			
			</td>			
		  	

		</tr>
		
		<%
				}
			}
		%>

	</table>

	<%
		}
	%>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0" class="dl_bg">
		<tr>
			<td width="90%">
			
			</td>
			<td>
				总计:<%=size%>条
			</td>
		</tr>
	</table>
	

	
	  
	   <input type="hidden" name="pkid" id="pkid" value="" />
	   <input type="hidden" name="state_code" id="state_code" value="" />
	   <input type="hidden" name="up_operating" id="up_operating" value="" />
	   <input type="hidden" name="s_start_date" id="s_start_date" value="" />
	   <input type="hidden" name="s_end_date" id="s_end_date" value="" />
	  <input type="hidden" name="bpm_id" id="bpm_id" value="1452" />
	  </form>
</body>

</html>
