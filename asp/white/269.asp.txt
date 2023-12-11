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

sPosition = sPosition & "修改用户名及密码"

Call Header()
Call Content()
Call Footer()


Sub Content()
	Select Case sAction
	Case "MODI"
		Call DoModi()
	Case Else
		Call ShowForm()
	End Select
End Sub


Sub ShowForm()
	%>
	<table border=0 cellspacing=1 align=center class=navi>
	<tr><th><%=sPosition%></th></tr>
	</table>

	<br>

	<table border=0 cellspacing=1 align=center class=form>
	<form action='?action=modi' method=post name=myform onsubmit="return checkModipwdForm()">
	<tr>
		<th>设置名称</th>
		<th>基本参数设置</th>
		<th>设置说明</th>
	</tr>
	<tr>
		<td width="15%">新用户名：</td>
		<td width="55%"><input type=text class=input size=20 name=newusr value="<%=inHTML(Session("eWebEditor_User"))%>"></td>
		<td width="30%"><span class=red>*</span>&nbsp;&nbsp;旧用户名：<span class=blue><%=outHTML(Session("eWebEditor_User"))%></span></td>
	</tr>
	<tr>
		<td width="15%">新 密 码：</td>
		<td width="55%"><input type=password class=input size=20 name=newpwd1 maxlength=30></td>
		<td width="30%"><span class=red>*</span></td>
	</tr>
	<tr>
		<td width="15%">确认密码：</td>
		<td width="55%"><input type=password class=input size=20 name=newpwd2 maxlength=30></td>
		<td width="30%"><span class=red>*</span></td>
	</tr>
	<tr><td align=center colspan=3><input type=submit name=bSubmit value="  提交  "></a>&nbsp;<input type=reset name=bReset value="  重填  "></td></tr>
	</form>
	</table>


	<%
End Sub

Sub DoModi()

	Dim sNewUsr, sNewPwd1, sNewPwd2
	sNewUsr = Trim(Request("newusr"))
	sNewPwd1 = Trim(Request("newpwd1"))
	sNewPwd2 = Trim(Request("newpwd2"))

	If sNewUsr = "" Then
		GoError "新用户名不能为空！"
	End If
	If sNewPwd1 = "" then
		GoError "新密码不能为空！"
	End If
	If sNewPwd1 <> sNewPwd2 Then
		GoError "新密码和确认密码不相同！"
	End If

	sUsername = sNewUsr
	sPassword = sNewPwd1

	Call WriteConfig()

	%>
	<table border=0 cellspacing=1 align=center class=navi>
	<tr><th><%=sPosition%></th></tr>
	</table>

	<br>

	<table border=0 cellspacing=1 align=center class=list>
	<tr>
		<td>登录用户名及密码修改成功！</td>
	</tr>
	</table>
	<%
End Sub

%>