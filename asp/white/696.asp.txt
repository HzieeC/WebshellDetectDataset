<%@language=vbscript codepage=936 %>
<!--#include file="Conn.asp"-->
<!--#include file="../Inc/Config.asp"-->
<!--#include file="inc/md5.asp"-->

<%
dim sql,rs
dim username,password,CheckCode
username=replace(trim(request("username")),"'","")
password=replace(trim(Request("password")),"'","")
CheckCode=replace(trim(Request("CheckCode")),"'","")
if UserName="" then
	FoundErr=True
	ErrMsg=ErrMsg & "<br><li>用户名不能为空！</li>"
end if
if Password="" then
	FoundErr=True
	ErrMsg=ErrMsg & "<br><li>密码不能为空！</li>"
end if
if CheckCode="" then
	FoundErr=True
	ErrMsg=ErrMsg & "<br><li>验证码不能为空！</li>"
end if
if session("CheckCode")="" then
	FoundErr=True
	ErrMsg=ErrMsg & "<br><li>你登录时间过长，请重新返回登录页面进行登录。</li>"
end if
if CheckCode<>CStr(session("CheckCode")) then
	FoundErr=True
	ErrMsg=ErrMsg & "<br><li>您输入的确认码和系统产生的不一致，请重新输入。</li>"
end if
if FoundErr<>True then
	password=md5(password)
	set rs=server.createobject("adodb.recordset")
	sql="select * from Admin where password='"&password&"' and username='"&username&"'"
	rs.open sql,conn,1,3
	if rs.bof and rs.eof then
		FoundErr=True
		ErrMsg=ErrMsg & "<br><li>用户名或密码错误！！！</li>"
	else
		if password<>rs("password") then
			FoundErr=True
			ErrMsg=ErrMsg & "<br><li>用户名或密码错误！！！</li>"
		else
		    RndPassword=GetRndPassword(16)
			rs("LastLoginIP")=Request.ServerVariables("REMOTE_ADDR")
			rs("LastLoginTime")=now()
			rs("LoginTimes")=rs("LoginTimes")+1
			rs("RndPassword")=RndPassword
			rs.update
			session.Timeout=SessionTimeout
			session("AdminName")=rs("username")
			session("AdminPassword")=rs("Password")		
			session("RndPassword")=RndPassword
			rs.close
			set rs=nothing
			call CloseConn()
			Response.Redirect "default.asp"
		end if
	end if
	rs.close
	set rs=nothing
end if
if FoundErr=True then
	call WriteErrMsg()
end if
call CloseConn()

'****************************************************
'过程名：WriteErrMsg
'作  用：显示错误提示信息
'参  数：无
'****************************************************
sub WriteErrMsg()
	dim strErr
	strErr=strErr & "<html><head><title>错误信息</title><meta http-equiv='Content-Type' content='text/html; charset=gb2312'>" & vbcrlf
	strErr=strErr & "<link href='style.css' rel='stylesheet' type='text/css'></head><body>" & vbcrlf
	strErr=strErr & "<table cellpadding=2 cellspacing=1 border=0 width=400 class='border' align=center>" & vbcrlf
	strErr=strErr & "  <tr align='center'><td height='22' class='title'><strong>错误信息</strong></td></tr>" & vbcrlf
	strErr=strErr & "  <tr><td height='100' class='tdbg' valign='top'><b>产生错误的可能原因：</b><br>" & errmsg &"</td></tr>" & vbcrlf
	strErr=strErr & "  <tr align='center'><td class='tdbg'><a href='Login.asp'>&lt;&lt; 返回登录页面</a></td></tr>" & vbcrlf
	strErr=strErr & "</table>" & vbcrlf
	strErr=strErr & "</body></html>" & vbcrlf
	response.write strErr
end sub

Function GetRndPassword(PasswordLen)
	Dim Ran,i,strPassword
	strPassword=""
	For i=1 To PasswordLen
		Randomize
		Ran = CInt(Rnd * 2)
		Randomize
		If Ran = 0 Then
			Ran = CInt(Rnd * 25) + 97
			strPassword =strPassword & UCase(Chr(Ran))
		ElseIf Ran = 1 Then
			Ran = CInt(Rnd * 9)
			strPassword = strPassword & Ran
		ElseIf Ran = 2 Then
			Ran = CInt(Rnd * 25) + 97
			strPassword =strPassword & Chr(Ran)
		End If
	Next
	GetRndPassword=strPassword
End Function
%>