<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page" /> 
<%
	 String option_id = bean. GenTradeId ();
	 String vote_id="";
	 if(request.getParameter("vote_id")!=null) 
	 vote_id = request.getParameter("vote_id");

%>
<html>
  <head>
    <title>新增在线调查选项</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="voteoption.js"></script>
</head>

<body>
	<h1>新增在线调查选项</h1>
	<form action="/doTradeReg.do" method="post" name="addForm">
	<input name="option_id" id="option_id" type="hidden" value="<%=option_id%>"/>
	<input name="vote_id" id="vote_id" type="hidden" value="<%=vote_id%>"/>
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		
		<tr>
			<td align="right" width="10%">
				选项名称<font color="red">*</font>
			</td>
			<td><input name="option_name" id="option_name" type="text" /></td>
		</tr>
		
		<tr>
			<td align="right" width="10%">
				投票数:
			</td>
			<td><input name="option_count" id="option_count" type="text" value="0" size="5" onKeyUp="if(!/^[0-9][0-9]*$/.test(this.value))this.value=''"/>（只能为数字）</td>
		</tr>
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="9402" />
				<input type="submit" class="buttoncss" name="tradeSub" value="提交" onClick="return chekedform()"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='voteoptionindex.jsp?vote_id=<%=vote_id%>';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
