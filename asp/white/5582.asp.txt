<%@ CODEPAGE=65001 %>
<%
'///////////////////////////////////////////////////////////////////////////////
'// 插件应用:    Z-Blog(http://www.rainbowsoft.org)
'// 插件制作:    
'// 备    注:    PING中心通知程序
'// 最后修改：   2005-9-16
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
If BlogUser.Level>3 Then Call ShowError(6) 

If CheckPluginState("PingTool")=False Then Call ShowError(48)


Dim EditArticle

Set EditArticle=New TArticle

If Not IsEmpty(Request.QueryString("id")) Then
	If EditArticle.LoadInfobyID(Request.QueryString("id")) Then

	Else
		Call ShowError(9)
	End If
Else
	Call ShowError(9)
End If


Dim PingContent

Dim TBContent

PingContent=LoadFromFile(BlogPath & "Plugin/PingTool/data/ping.html","utf-8")

TBContent=Request.QueryString("tbs")

%><!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="<%=ZC_BLOG_LANGUAGE%>" lang="<%=ZC_BLOG_LANGUAGE%>">
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta http-equiv="Content-Language" content="<%=ZC_BLOG_LANGUAGE%>" />
	<link rel="stylesheet" rev="stylesheet" href="../../CSS/admin.css" type="text/css" media="screen" />
	<title><%=BlogTitle%></title>
</head>
<body onload="window.location='../../cmd.asp?act=ArticleMng'">
			<div id="divMain">
			<div class="Header">Ping中心和引用通告发送器</div>
			<div class="form">
			<form id="edit" method="post" action="">
			<%

If Request.QueryString("ping")="True" Then
	Call SendPing
End If

If Replace(Replace(Replace(TBContent,vbCr,""),vbLf,"")," ","")<>"" Then
	Call SendTB
End If



Function SendPing

	Dim Url,Urls
	Urls=Split(Replace(PingContent,vbCr,""),vbLf)

	For Each Url In Urls
		If Trim(Url)<>"" Then
			Call SendPing_Single(url)
		End If
	Next

End Function


Function SendTB

	Dim Url,Urls
	Urls=Split(Replace(TBContent,vbCr,""),vbLf)

	For Each Url In Urls
		If Trim(Url)<>"" Then
			Call SendTB_Single(url)
		End If
	Next

End Function

Function SendPing_Single(url)

	On Error Resume Next

	Dim s
	s = "<?xml version=""1.0""?><methodCall><methodName>weblogUpdates.ping</methodName><params><param><value>"&TransferHTML(ZC_BLOG_NAME,"[<][>][&][""]")&"</value></param><param><value>"&EditArticle.HtmlUrl&"</value></param></params></methodCall>"

	Response.Write "<p>发送Ping到:" & Url & "</p>"
	Response.Flush

	Dim objPing
	Set objPing = Server.CreateObject("MSXML2.ServerXMLHTTP")
	objPing.SetTimeOuts 10000, 10000, 10000, 10000 
	'第一个数值：解析DNS名字的超时时间10秒 
	'第二个数值：建立Winsock连接的超时时间10秒 
	'第三个数值：发送数据的超时时间10秒 
	'第四个数值：接收response的超时时间10秒 

	objPing.open "POST",url,False

	objPing.setRequestHeader "Content-Type", "text/xml"
	objPing.send s

	Set objPing = Nothing

	Err.Clear

End Function


Function SendTB_Single(url)

	On Error Resume Next

	Dim objTrackBack

	Set objTrackBack=New TTrackBack

	objTrackBack.URL=EditArticle.Url
	objTrackBack.Title=EditArticle.Title
	objTrackBack.Blog=ZC_BLOG_NAME
	objTrackBack.Excerpt=Left(EditArticle.HtmlContent,250)

	Response.Write "<p>发送引用通告到:" & Url & "</p>"
	Response.Flush

	If objTrackBack.Send(url) Then SendTrackBack=True
	Set objTrackBack=Nothing

	Err.Clear

End Function



Call System_Terminate()

If Err.Number<>0 then
  Call ShowError(0)
End If
%>
</div></form></div>
</body>
</html>
