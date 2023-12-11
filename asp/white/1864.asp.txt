<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<%
If Dvbbs.UserID>0 Then
	Dim TruePassWord,usercookies,i
	usercookies=Request.Cookies(Dvbbs.Forum_sn)("usercookies")
	TruePassWord=Dvbbs.Createpass
	If (Isnull(usercookies) or usercookies="") And Not Isnumeric(usercookies) Then usercookies=0

	Call updateCookiesInfo(usercookies,TruePassWord)
	'检查写入是否成功如果成功则更新数据
	i=0
	Do While i<3
		If Dvbbs.checkStr(Trim(Request.Cookies(Dvbbs.Forum_sn)("password")))=TruePassWord Then
			Dvbbs.Execute("Update [Dv_user] Set TruePassWord='"&TruePassWord&"' where UserID="&Dvbbs.UserID)
			Dvbbs.MemberWord = TruePassWord
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@truepassword").text= TruePassWord
			Response.write "Cookies更新成功！"
			Exit Do
		Else
			Response.Write "第"&(i+1)&"次更新失败，重新发送Cookies信息"
			Call updateCookiesInfo(usercookies,TruePassWord)
		End If
		i=i+1
	Loop
End If

'更新用户Cookies信息
Sub updateCookiesInfo(usercookies,TruePassWord)
	Select Case Cint(usercookies)
		Case 0
			Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
		Case 1
			Response.Cookies(Dvbbs.Forum_sn).Expires=Date+1
			Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
		Case 2
			Response.Cookies(Dvbbs.Forum_sn).Expires=Date+31
			Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
		Case 3
			Response.Cookies(Dvbbs.Forum_sn).Expires=Date+365
			Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
	End Select
	Response.Cookies(Dvbbs.Forum_sn).path=Dvbbs.cookiepath
	Response.Cookies(Dvbbs.Forum_sn)("username") = Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@username").text
	Response.Cookies(Dvbbs.Forum_sn)("UserID") = Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userid").text
	Response.Cookies(Dvbbs.Forum_sn)("userclass") = Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userclass").text 
	Response.Cookies(Dvbbs.Forum_sn)("userhidden") = Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userhidden").text
	Response.Cookies(Dvbbs.Forum_sn)("password") = TruePassWord
	Response.Flush
End Sub
Dvbbs.PageEnd()
%>