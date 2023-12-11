<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%> 
<%@page import="com.bizoss.trade.ts_category.*" %>
<%@page import="com.bizoss.trade.ti_brand.Ti_brandInfo" %>
<%@page import="com.bizoss.trade.ti_channel.Ti_channelInfo" %>
<%@page import="com.bizoss.trade.ti_news.Ti_newsInfo" %>
<%@page import="com.bizoss.trade.ti_admin.Ti_adminInfo" %>
<%@page import="com.bizoss.trade.ti_zhuanti.Ti_zhuantiInfo" %>
<jsp:useBean id="bean" class="com.bizoss.frame.util.RandomID" scope="page" /> 
<%

String commen_id = bean. GenTradeId ();
String user_id="",cust_id="";	
	if( session.getAttribute("session_user_id") != null )
	{
		user_id = session.getAttribute("session_user_id").toString();
	}
	if(session.getAttribute("session_cust_id")!=null){
	     cust_id  =session.getAttribute("session_cust_id").toString();
	}
	Ti_zhuantiInfo zhuantiinfo = new Ti_zhuantiInfo();
	String select= zhuantiinfo.getSelzhuanti();
%>
<html>
  <head>
    <title>新增专题评论</title>
	<link href="/program/admin/index/css/style.css" rel="stylesheet" type="text/css">
	<script type="text/javascript" src="zhuanticomment.js"></script>
</head>

<body>
	<h1>新增专题评论</h1>
	<form action="/doTradeReg.do" method="post" name="addForm">
	<input name="commen_id" id="commen_id" type="hidden" value="<%=commen_id%>" />
	<input name="user_id" id="user_id" type="hidden" value="<%=user_id%>"  />
	<input name="up_num" id="up_num" type="hidden" value="0" />
	<input name="down_num" id="down_num" type="hidden" value="0" />
	<table width="100%" cellpadding="0" cellspacing="1" border="0" class="listtab">
		
		<tr>
			<td align="right" width="10%">
				专题名称<font color="red">*</font>
			</td>
			<td>
			<select name="zhuanti_id" id="zhuanti_id" size="1" style="width:130px">
				<option value="">请选择</option>
				<%=select%>
		  </select></td>
		</tr>
		
	<!--	<tr>
			<td align="right" width="10%">
				内容:
			</td>
			<td><input name="content" id="content" type="text" /></td>
		</tr>-->
		<tr>
			<td align="right" width="10%">
				评论内容<font color="red">*</font>
			</td>
			<td colspan="3">
			<textarea name="content" id="content"></textarea>
			<script type="text/javascript" src="/program/plugins/ckeditor/ckeditor.js"></script>
			<script type="text/javascript">
				CKEDITOR.replace('content');
			</script></td>
		</tr>			
		
	</table>
	
	<table width="100%" cellpadding="0" cellspacing="0" border="0">
		<tr>
			<td align="center">
				<input type="hidden" name="bpm_id" value="6304" />
				<input type="button" class="buttoncss" name="tradeSub" value="提交" onclick="return subForm();"/>&nbsp;&nbsp;
				<input type="button" class="buttoncss" name="tradeRut" value="返回" onclick="window.location.href='index.jsp';"/>
			</td>
		</tr>
	</table>
	</form>
</body>

</html>
