<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/md5.asp"-->
<!--#include file="dv_dpo/cls_dvapi.asp"-->
<%	
	If Not IsObject(Conn) Then ConnectionDatabase
	Dim activeuser,TempNum
	If Not CLng(DVbbs.UserSession.documentElement.selectSingleNode("userinfo/@userid").text)=0 Then
		activeuser="delete from Dv_online where userid= "& CLng(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userid").text)
		Conn.Execute activeuser,TempNum
		'更新缓存总用户在线数据
		MyBoardOnline.Forum_UserOnline = MyBoardOnline.Forum_UserOnline - TempNum
		Dvbbs.Name="Forum_UserOnline"
		Dvbbs.value=MyBoardOnline.Forum_UserOnline
	Else
		If IsNumeric(DVbbs.UserSession.documentElement.selectSingleNode("userinfo/@statuserid").text) Then 
			activeuser="delete from Dv_online where id="& DVbbs.UserSession.documentElement.selectSingleNode("userinfo/@statuserid").text
			Conn.Execute activeuser,TempNum
			'更新缓存总用户在线数据
			MyBoardOnline.Forum_GuestOnline = MyBoardOnline.Forum_GuestOnline - TempNum
			Dvbbs.Name="Forum_GuestOnline"
			Dvbbs.value=MyBoardOnline.Forum_GuestOnline
		End If 
	End If
	MyBoardOnline.Forum_Online = MyBoardOnline.Forum_Online - TempNum
	Dvbbs.Name="Forum_Online"
	Dvbbs.value=MyBoardOnline.Forum_Online
	Response.Cookies(Dvbbs.Forum_sn).path=Dvbbs.cookiepath
	Response.Cookies(Dvbbs.Forum_sn)("username")=""
	Response.Cookies(Dvbbs.Forum_sn)("password")=""
	Response.Cookies(Dvbbs.Forum_sn)("userclass")=""
	Response.Cookies(Dvbbs.Forum_sn)("userid")=""
	Response.Cookies(Dvbbs.Forum_sn)("userhidden")=""
	Response.Cookies(Dvbbs.Forum_sn)("usercookies")=""
	If EnabledSession Then	
		Session(Dvbbs.CacheName & "UserID")=Empty
	End If
	Set Dvbbs.UserSession=Nothing 
	Session("flag")=Empty

	'-----------------------------------------------------------------
	'系统整合
	'-----------------------------------------------------------------
	Dim DvApi_Obj,DvApi_SaveCookie,SysKey
	If DvApi_Enable Then
		Md5OLD = 1
		SysKey = Md5(Dvbbs.MemberName&DvApi_SysKey,16)
		Md5OLD = 0
		Set DvApi_Obj = New DvApi
			DvApi_SaveCookie = DvApi_Obj.SetCookie(SysKey,Dvbbs.MemberName,"","")
		Set DvApi_Obj = Nothing
		Response.Write DvApi_SaveCookie
		Response.Flush
	End If
	'-----------------------------------------------------------------
	'Response.Redirect Dvbbs.Forum_Info(11)
	response.write"<script language=JavaScript>"
	response.write"setTimeout(""window.location='"&Dvbbs.Forum_Info(11)&"'"",1000);"
	response.write"</script>"
	Dvbbs.PageEnd()
%>
