<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page" /> 
<%
String news_id = bean.GenTradeId ();

String cust_id = "", publish_user_id = "";
if( session.getAttribute("session_cust_id") != null )
	{
		cust_id = session.getAttribute("session_cust_id").toString();
	}
if( session.getAttribute("session_user_id") != null )
	{
		publish_user_id = session.getAttribute("session_user_id").toString();
	}

%>
<html>
  <head>
    <title>通知公告</title>
		<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css"/>
		<script type="text/javascript" src="internews.js"></script>
</head>

<body>
	<h1>新增通知公告</h1>
	
	<form action="/doTradeReg.do" method="post" name="addForm">
	
	<input name="news_id" id="news_id" type="hidden" value=<%=news_id%> />
	<input name="cust_id" id="cust_id" type="hidden" value=<%=cust_id%> />
	<input name="publish_user_id" id="publish_user_id" type="hidden" value=<%=publish_user_id%> />
	
	<table width="100%" cellpadding="1" cellspacing="1" border="0" class="listtab">
		<tr>
			<td align="right" width="15%">
				标题<font color="red">*</font>
			</td>
			<td><input name="title" id="title" type="text" maxlength="100" size="30"/></td>
		</tr>
		
		<tr>
			<td align="right" width="15%">
				内容<font color="red">*</font>
			</td>
			<td><textarea name="content" id="content"></textarea>
				<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
				<script type="text/javascript">
					CKEDITOR.replace('content');
				</script>
			</td>
		</tr>

		<tr>
			<td align="right" width="10%">
				备注:
			</td>
			<td><input name="remark" id="remark" type="text" maxlength="100" size="30"/></td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="6923" />
				<input class="buttoncss" type="button" name="tradeSub" value="提交" onclick="return checkForm()"/>&nbsp;&nbsp;
				<input class="buttoncss" type="button" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
