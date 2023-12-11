<%@ CODEPAGE=65001 %>
<%
'///////////////////////////////////////////////////////////////////////////////
'// 插件应用:    Z-Blog 1.7
'// 插件制作:    
'// 备    注:    
'// 最后修改：   
'// 最后版本:    
'///////////////////////////////////////////////////////////////////////////////
%>
<% Option Explicit %>
<% On Error Resume Next %>
<% Response.Charset="UTF-8" %>
<% Response.Buffer=True %>
<!-- #include file="../../c_option.asp" -->
<!-- #include file="../../function/c_function.asp" -->
<!-- #include file="../../function/c_function_md5.asp" -->
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

If CheckPluginState("BackupDB")=False Then Call ShowError(48)

BlogTitle="BackupDB（Z-Blog的数据库备份及升级程序）"


	If Request.QueryString("act")="BackupDB" Then
		randomize
		Dim bkdbname,fso
		bkdbname="backup"&year(now) & right("0" & month(now),2) & right("0" & day(now),2) & right("0" & hour(now),2) & right("0" & minute(now),2) & right("0" & second(now),2) &"_"& MD5(Now)  & ".mdb"
		Set fso=server.createobject("scripting.filesystemobject")
		if fso.fileexists(BlogPath&ZC_DATABASE_PATH) then
		fso.copyfile BlogPath&ZC_DATABASE_PATH,BlogPath&"DATA\"&bkdbname
		End if
		Set fso = Nothing
		
		Call SetBlogHint_Custom("‼ 提示:当前数据库已备份,可点击链接下载至本地保存.")

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
<div class="Header"><%=BlogTitle%></div>
<div class="SubMenu">
<span class="m-left m-now"><a href="backupdb.asp">数据库备份</a></span>
<span class="m-left"><a href="updatedb.asp">数据库结构升级</a></span>
</div>
<div id="divMain2">
<% Call GetBlogHint() %>
<form id="edit" name="edit" method="post" action="?">
<%

	If Request.QueryString("act")="BackupDB" Then
		Response.Write "<p><b>数据库备份</b></p>"
		Response.Write "<p>备份成功:<a href="""&ZC_BLOG_HOST&"DATA/"&bkdbname&""" target=""_blank"">"&ZC_BLOG_HOST&"DATA/"&bkdbname&"</a></p>"
		Response.Write "<p></p><hr/><p></p>"
	End If

	Response.Write "<p><b>数据库备份</b>:备份当前的数据库到DATA目录</p>"
	Response.Write "<p><input class=""button"" style=""width:100px"" type=""submit"" value=""提交"" id=""btnPost"" onclick=""this.form.action+='act=BackupDB';return window.confirm('"& ZC_MSG058 &"');""/></p>" & vbCrlf
	Response.Write "<p></p>"

%>

</form>

</div>
<script language="javascript">
function ChangeValue(obj){

	if (obj.value=="True")
	{
	obj.value="False";
	return true;
	}

	if (obj.value=="False")
	{
	obj.value="True";
	return true;
	}
}
</script>
</body>
</html>
<%
Call System_Terminate()

If Err.Number<>0 then
  Call ShowError(0)
End If
%>
