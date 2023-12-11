<!--#include file=conn.asp-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/Dv_ClsOther.asp" -->
<%
Dim action
action=Request("action")
Select Case action
	Case "hidden"
		Call hidden()
	Case "online"
		Call online()
	Case "stylemod"
		Call stylemod()
	Case "setlistmod"
		Call SetListmod
	Case "dispBoard"
		Call dispBoard()
	Case "ReGroup"
		Call ReGroup
	Case Else
End Select
Dvbbs.PageEnd()
If IsNull(Request.ServerVariables("HTTP_REFERER")) Or Request.ServerVariables("HTTP_REFERER")="" Then
	Response.Redirect "index.asp"
Else
	Response.Redirect Request.ServerVariables("HTTP_REFERER")
End If

Sub hidden()
	If Dvbbs.UserID=0 Then Exit Sub
	Dvbbs.execute("update [Dv_online] set userhidden=1 where userid="&Dvbbs.userid)
	Dvbbs.execute("update [Dv_user] set userhidden=1 where userid="&Dvbbs.userid)
	Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userhidden").text=1
	Dim usercookies
	usercookies=request.cookies(Dvbbs.Forum_sn)("usercookies")
	If IsNull(usercookies) or usercookies="" then usercookies="0"
	Select Case usercookies
		Case "0"
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
	Response.Cookies(Dvbbs.Forum_sn)("userhidden") = 1
	Response.Cookies(Dvbbs.Forum_sn).path=Dvbbs.cookiepath
End Sub

Sub online()
	If Dvbbs.UserID=0 Then Exit Sub
	Dvbbs.execute("update [dv_online] set userhidden=2 where userid="&Dvbbs.userid)
	Dvbbs.execute("update [Dv_user] set userhidden=2,lastlogin=" & SqlNowString & " where userid="&Dvbbs.userid)
	Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userhidden").text=2
	Dim  usercookies
	usercookies=request.cookies(Dvbbs.Forum_sn)("usercookies")
	If IsNull(usercookies) or usercookies="" Then usercookies="0"
	Select Case usercookies
		Case "0"
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
	End select
	Response.Cookies(Dvbbs.Forum_sn)("userhidden") = 2
	Response.Cookies(Dvbbs.Forum_sn).path=Dvbbs.cookiepath
End Sub

Sub Stylemod()
	Dim skinid
	skinid=Request("skinid")
	Response.Cookies("skin").expires= date+7
	Response.Cookies("skin")("skinid_"&Dvbbs.boardid)=skinid
	Response.Cookies("skin").path=Dvbbs.cookiepath
End Sub

Sub SetListmod()
	Dim mode
	If CInt(request("thisvalue"))=0 Then
		mode=1
	Else
		mode=0
	End If
	Response.Cookies("List").path=Dvbbs.cookiepath
	Response.Cookies("List").expires= date+7
	Response.Cookies("List")("list"&CInt(Request("id")))=mode
	'Response.Write "<script language=""javascript"" type=""text/javascript"">parent.location.reload();</script>"
	'Response.End 
End Sub

Sub dispBoard()
	Dim mode
	If request("do")="" Then
		mode="none"
	Else
		mode=""
	End If
	Response.Cookies("Disp").path=Dvbbs.cookiepath
	Response.Cookies("Disp").expires= date+7
	Response.Cookies("Disp")("list"&CInt(Request("id")))=mode
	'Response.Write "<script language=""javascript"" type=""text/javascript"">parent.location.reload();</script>"
	'Response.End 
End Sub

%>
