<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="com.bizoss.trade.ti_internews.*" %>
<%@ page import="java.util.*" %>
<%
	String news_id="";
	if(request.getParameter("news_id")!=null) news_id = request.getParameter("news_id");
	Ti_internewsInfo ti_internewsInfo = new Ti_internewsInfo();
	List list = ti_internewsInfo.getListByPk(news_id);
	Hashtable map = new Hashtable();
	if(list!=null && list.size()>0) map = (Hashtable)list.get(0);
	
	String title="",content="",remark="";
	if(map.get("title")!=null) title = map.get("title").toString();
	if(map.get("content")!=null) content = map.get("content").toString();
	if(map.get("remark")!=null) remark = map.get("remark").toString();
	
	String newstitle = "";
	if(request.getParameter("newstitle")!=null && !request.getParameter("newstitle").equals("")){
		newstitle = request.getParameter("newstitle").trim();
	}
	String cust_id = "";
	if( session.getAttribute("session_cust_id") != null )
	{
		cust_id = session.getAttribute("session_cust_id").toString();
	}
	String end_date = "";
	if(request.getParameter("txtEndDate")!=null && !request.getParameter("txtEndDate").equals("")){
		end_date = request.getParameter("txtEndDate");
	}	
	String start_date = "";
	if(request.getParameter("txtStartDate")!=null && !request.getParameter("txtStartDate").equals("")){
		start_date = request.getParameter("txtStartDate");
	}
	String iStart = "0";
	if(request.getParameter("iStart")!=null) iStart = request.getParameter("iStart");
	
	String para = "/program/admin/internews/index.jsp?newstitle="+newstitle+"&txtEndDate="+end_date+"&txtStartDate="+start_date+"&cust_id="+cust_id+"&iStart="+Integer.parseInt(iStart); 
%>
<html>
  <head>
    
    <title>通知公告信息修改</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="internews.js"></script>
</head>

<body>
	
	<h1>修改通知公告</h1>
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<table width="100%" cellpadding="1" cellspacing="1" border="0" class="listtab">
		<tr>
			<td align="right" width="15%">
				标题<font color="red">*</font>
			</td>
			<td><input name="title" id="title" value="<%=title %>" type="text" maxlength="100" size="30"/></td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				内容<font color="red">*</font>
			</td>
			<td>
				<textarea name="content" id="content"><%=content %></textarea>
				<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
				<script type="text/javascript">
					CKEDITOR.replace('content');
				</script>
			</td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				备注:
			</td>
			<td><input name="remark" id="remark" value="<%=remark %>" type="text" maxlength="100" size="30"/></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="2000" />
				<input type="hidden" name="news_id" value="<%=news_id%>" />
				<input type="hidden" name="jumpurl" value="<%=para%>" />
				<input class="buttoncss" type="submit" name="tradeSub" value="提交" onclick="return checkForm()"/>&nbsp;&nbsp;
				<input class="buttoncss" type="button" name="tradeRut" value="返回" onclick="window.location.href='<%=para%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
