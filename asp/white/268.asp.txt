<!--#include file = "private.asp"-->

<%
'######################################
' eWebEditor v5.5 - Advanced online web based WYSIWYG HTML editor.
' Copyright (c) 2003-2008 eWebSoft.com
'
' For further information go to http://www.ewebsoft.com/
' This copyright notice MUST stay intact for use.
'######################################
%>

<%

sPosition = sPosition & "后台管理首页"

Call Header()
Call Content()
Call Footer()




Sub Content()
	%>

	<table border=0 cellspacing=1 align=center class=navi>
	<tr><th><%=sPosition%></th></tr>
	</table>

	<br>

	<table border=0 cellspacing=1 align=center class=list>
	<tr><th colspan=2>eWebEditor 5.5 版权、常用联系方法、技术支持</th></tr>
	<tr>
		<td width="15%">软件版本：</td>
		<td width="85%">eWebEditor Version 5.5 for ASP 简体中文商业版</td>
	</tr>
	<tr>
		<td width="15%">版权所有：</td>
		<td width="85%"><a href="http://www.ewebsoft.com" target="_blank">eWebSoft.com</a>&nbsp;&nbsp;已获得国家版权局颁发的《计算机软件著作权登记证书》,登记号:2004SR06549</td>
	</tr>
	<tr>
		<td width="15%">主页地址：</td>
		<td width="85%"><a href="http://www.ewebeditor.net/" target="_blank">eWebEditor中文站(www.eWebEditor.net)</a>&nbsp;&nbsp;&nbsp;<a href="http://service.ewebeditor.net/" target="_blank">客服中心(service.eWebEditor.net)</a></td>
	</tr>
	<tr>
		<td width="15%">联系方式：</td>
		<td width="85%">电话：0591-83633592&nbsp;&nbsp;&nbsp;&nbsp;传真：0591-83633593&nbsp;&nbsp;&nbsp;&nbsp;QQ：589808&nbsp;&nbsp;&nbsp;&nbsp;邮箱：<a href="mailto:service@ewebsoft.com">service@ewebsoft.com</a></td>
	</tr>
	</table>

	<br>

	<table border=0 cellspacing=1 align=center class=list>
	<tr><th colspan=2>服务器的有关参数</th><th colspan=2>组件支持有关参数</th></tr>
	<tr>
		<td width="15%">服务器名：</td>
		<td width="45%"><%=Request.ServerVariables("SERVER_NAME")%></td>
		<td width="20%">ADO 数据对象：</td>
		<td width="20%"><%=Get_ObjInfo("adodb.connection", 1)%></td>
	</tr>
	<tr>
		<td width="15%">服务器IP：</td>
		<td width="45%"><%=Request.ServerVariables("LOCAL_ADDR")%></td>
		<td width="20%">FSO 文本文件读写：</td>
		<td width="20%"><%=Get_ObjInfo("scripting.filesystemobject", 0)%></td>
	</tr>
	<tr>
		<td width="15%">服务器端口：</td>
		<td width="45%"><%=Request.ServerVariables("SERVER_PORT")%></td>
		<td width="20%">Stream 文件流：</td>
		<td width="20%"><%=Get_ObjInfo("Adodb."&"Stream", 0)%></td>
	</tr>
	<tr>
		<td width="15%">服务器时间：</td>
		<td width="45%"><%=Now()%></td>
		<td width="20%">Microsoft.XMLHTTP：</td>
		<td width="20%"><%=Get_ObjInfo("Microsoft.XMLHTTP", 0)%></td>
	</tr>
	<tr>
		<td width="15%">IIS版本：</td>
		<td width="45%"><%=Request.ServerVariables("SERVER_SOFTWARE")%></td>
		<td width="20%">Microsoft.XMLDOM：</td>
		<td width="20%"><%=Get_ObjInfo("Microsoft.XMLDOM", 0)%></td>
	</tr>
	<tr>
		<td width="15%">服务器操作系统：</td>
		<td width="45%"><%=Request.ServerVariables("OS")%></td>
		<td width="20%">CDONTS 虚拟SMTP发信：</td>
		<td width="20%"><%=Get_ObjInfo("CDONTS.NewMail", 1)%></td>
	</tr>
	<tr>
		<td width="15%">脚本超时时间：</td>
		<td width="45%"><%=Server.ScriptTimeout%> 秒</td>
		<td width="20%">LyfUpload 上传组件：</td>
		<td width="20%"><%=Get_ObjInfo("LyfUpload.UploadFile", 1)%></td>
	</tr>
	<tr>
		<td width="15%">站点物理路径：</td>
		<td width="45%"><%=request.ServerVariables("APPL_PHYSICAL_PATH")%></td>
		<td width="20%">AspUpload 上传组件：</td>
		<td width="20%"><%=Get_ObjInfo("Persits.Upload.1", 1)%></td>
	</tr>
	<tr>
		<td width="15%">服务器CPU数量：</td>
		<td width="45%"><%=Request.ServerVariables("NUMBER_OF_PROCESSORS")%> 个</td>
		<td width="20%">SA-FileUp 上传组件：</td>
		<td width="20%"><%=Get_ObjInfo("SoftArtisans.FileUp", 1)%></td>
	</tr>
	<tr>
		<td width="15%">服务器解译引擎：</td>
		<td width="45%"><%=ScriptEngine & "/" & ScriptEngineMajorVersion & "." & ScriptEngineMinorVersion & "." & ScriptEngineBuildVersion %></td>
		<td width="20%">AspJpeg 图像处理组件：</td>
		<td width="20%"><%=Get_ObjInfo("Persits.Jpeg",1)%></td>
	</tr>
	</table>

	
	<%
End Sub

Function Get_ObjInfo(obj, ver)
	On Error Resume Next
	Dim objTest, sTemp
	Set objTest = Server.CreateObject(obj)
	If Err.Number <> 0 Then
		Err.Clear
		Get_ObjInfo = "<font class=red><b>×</b></font>&nbsp;<font class=gray>不支持</font>"
	Else
		sTemp = ""
		If ver = 1 Then
			sTemp = objTest.version
			If IsNull(sTemp) Then sTemp = objTest.about
			sTemp = Replace(sTemp, "Version", "")
			sTemp = "&nbsp;<font class=tims><font class=blue>" & sTemp & "</font></font>"
		End If
		Get_ObjInfo = "<b>√</b>&nbsp;<font class=gray>支持</font>" & sTemp
	End If
	Set objTest = Nothing
	If Err.Number <> 0 Then Err.Clear
End Function

%>