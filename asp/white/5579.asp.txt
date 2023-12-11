<%@ CODEPAGE=65001 %>
<%
'///////////////////////////////////////////////////////////////////////////////
'// 插件应用:    Z-Blog(http://www.rainbowsoft.org)
'// 插件制作:    zx.asd
'// 备    注:    Ping中心通知程序
'// 最后修改：   2005-12-8
'// 最后版本:    
'///////////////////////////////////////////////////////////////////////////////
%>
<% Option Explicit %>
<% On Error Resume Next %>
<% Response.Charset="UTF-8" %>
<% Response.Buffer=True %>
<!-- #include file="../../c_option.asp" -->
<!-- #include file="../../function/c_function.asp" -->
<!-- #include file="../../function/c_system_lib.asp" -->
<!-- #include file="../../function/c_system_base.asp" -->
<!-- #include file="../../function/c_system_event.asp" -->
<!-- #include file="../../function/c_system_plugin.asp" -->
<%

Call System_Initialize()

'检查非法链接
Call CheckReference("")

'检查权限
If BlogUser.Level>1 Then Call ShowError(6) 

If CheckPluginState("PingTool")=False Then Call ShowError(48)

BlogTitle="Ping中心和引用通告发送器"

If Not IsEmpty(Request.QueryString("ok")) Then
	Call SaveToFile(BlogPath & "Plugin/PingTool/data/ping.html",Request.Form("txaContent"),"utf-8",False)
	Call SetBlogHint(True,Empty,Empty)
End If


%><!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="<%=ZC_BLOG_LANGUAGE%>" lang="<%=ZC_BLOG_LANGUAGE%>">
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta http-equiv="Content-Language" content="<%=ZC_BLOG_LANGUAGE%>" />
	<link rel="stylesheet" rev="stylesheet" href="../../CSS/admin.css" type="text/css" media="screen" />
	<script language="JavaScript" src="../../script/common.js" type="text/javascript"></script>
	<title><%=BlogTitle%></title>
</head>
<body>

			<div id="divMain">
<div class="Header">Ping中心和引用通告发送器</div>
<div id="divMain2">
<%
Call GetBlogHint()
%>
<form border="1" name="edit" id="edit" method="post" action="tool.asp?ok">
<p><b>设置Ping中心</b></p>
<p><textarea style="height:300px;width:100%" name="txaContent" id="txaContent"><%=TransferHTML(LoadFromFile(BlogPath & "Plugin/PingTool/data/ping.html","utf-8"),"[textarea]")%></textarea></p>
<p>&nbsp;</p>
<p><input type="submit" class="button" value="提交" name="B1"/></p>
</form>
</div>
<script>


</script>
</body>
</html>
<%
Call System_Terminate()

If Err.Number<>0 then
  Call ShowError(0)
End If
%>

