<!-- #include file="conn.asp" -->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/Dv_ClsOther.asp" -->
<%
Dim Str
Dvbbs.Stats="查看文件"
Dim Downid,Rs
If CInt(Dvbbs.GroupSetting(49))=0 Or Dvbbs.GroupSetting(61)=0 Then Dvbbs.AddErrCode(54)
If request("id")="" Then
	Dvbbs.AddErrCode(35)
ElseIf Not IsNumeric(request("id")) Then
	Dvbbs.AddErrCode(35)
Else
	DownID=Clng(request("id"))
End If
Dvbbs.ShowErr()

'论坛下载限制(包括文章、积分、金钱、魅力、威望、精华、被删数、注册时间)
Dim BoardUserLimited,i,UploadUserQStr,DownUserQStr,UserInfo
BoardUserLimited = Split(Dvbbs.Board_Setting(55),"|")
'下载扣除负分修改
Dim Sql,UserSession
		Sql="Select UserID,UserName,UserPassword,UserEmail,UserPost,UserTopic,userWealth,userEP,userCP,UserPower,UserMoney,UserTicket"
		Sql=Sql & " From [Dv_User] Where UserID = " & Dvbbs.UserID
		Set Rs = Dvbbs.Execute(Sql)
		If Rs.EOF Then
			Dvbbs.UserID = 0:Dvbbs.LetGuestSession()
		Else
		Set UserSession = Dvbbs.RecordsetToxml(rs,"userinfo","xml")
		End If 
		Rs.close()
		Set Rs=Nothing
'下载扣除负分修改
Set UserInfo=UserSession.documentElement.selectSingleNode("userinfo")
If Ubound(BoardUserLimited)=12 Then
	For i=0 To 12
		BoardUserLimited(i)=Dvbbs.CheckNumeric(BoardUserLimited(i))
	Next
	'文章
	If BoardUserLimited(0)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户发贴最少为 <B>"&BoardUserLimited(0)&"</B> 才能下载&action=OtherErr"
		If Clng(UserInfo.getAttribute("userpost"))<=Clng(BoardUserLimited(0)) Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户发贴最少为 <B>"&BoardUserLimited(0)&"</B> 才能下载&action=OtherErr"
	End If
	'积分
	If BoardUserLimited(1)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户积分最少为 <B>"&BoardUserLimited(1)&"</B> 才能下载&action=OtherErr"
		If Clng(UserInfo.getAttribute("userep"))<=Clng(BoardUserLimited(1)) Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户积分最少为 <B>"&BoardUserLimited(1)&"</B> 才能下载&action=OtherErr"
	End If
	'金钱
	If BoardUserLimited(2)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户金钱最少为 <B>"&BoardUserLimited(2)&"</B> 才能下载&action=OtherErr"
		If Clng(UserInfo.getAttribute("userwealth"))<=Clng(BoardUserLimited(2)) Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户金钱最少为 <B>"&BoardUserLimited(2)&"</B> 才能下载&action=OtherErr"
	End If
	'魅力
	If BoardUserLimited(3)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户魅力最少为 <B>"&BoardUserLimited(3)&"</B> 才能下载&action=OtherErr"
		If Clng(UserInfo.getAttribute("usercp"))<=Clng(BoardUserLimited(3)) Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户魅力最少为 <B>"&BoardUserLimited(3)&"</B> 才能下载&action=OtherErr"
	End If
	'威望
	If BoardUserLimited(4)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户威望最少为 <B>"&BoardUserLimited(4)&"</B> 才能下载&action=OtherErr"
		If Clng(UserInfo.getAttribute("userpower"))<=Clng(BoardUserLimited(4)) Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户威望最少为 <B>"&BoardUserLimited(4)&"</B> 才能下载&action=OtherErr"
	End If
	'精华
	If BoardUserLimited(5)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户精华最少为 <B>"&BoardUserLimited(5)&"</B> 才能下载&action=OtherErr"
		If Clng(UserInfo.getAttribute("userisbest"))<=Clng(BoardUserLimited(5)) Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户精华最少为 <B>"&BoardUserLimited(5)&"</B> 才能下载&action=OtherErr"
	End If
	'删贴
	If BoardUserLimited(6)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户被删贴少于 <B>"&BoardUserLimited(6)&"</B> 才能下载&action=OtherErr"
		If Clng(UserInfo.getAttribute("userdel"))>=Clng(BoardUserLimited(6)) Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户被删贴少于 <B>"&BoardUserLimited(6)&"</B> 才能下载&action=OtherErr"
	End If
	'注册时间
	If BoardUserLimited(7)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户注册时间大于 <B>"&BoardUserLimited(7)&"</B> 分钟才能下载&action=OtherErr"
		If DateDiff("s",UserInfo.getAttribute("joindate"),Now)<Clng(BoardUserLimited(7))*60 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户注册时间大于 <B>"&BoardUserLimited(7)&"</B> 分钟才能下载&action=OtherErr"
	End If
	Dim DownUserMoney,DownUserWealth,DownUserEp
	If Dvbbs.UserID >0 Then
		DownUserMoney = Dvbbs.CheckNumeric(UserInfo.getAttribute("usermoney"))
		DownUserWealth = Dvbbs.CheckNumeric(UserInfo.getAttribute("userwealth"))
		DownUserEp = Dvbbs.CheckNumeric(UserInfo.getAttribute("userep"))
	Else
		DownUserMoney = 0
		DownUserWealth = 0
		DownUserEp = 0
	End If
	If BoardUserLimited(9)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户下载扣除金币 <B>"&BoardUserLimited(9)&"</B> 个，你未登陆，不能下载。&action=OtherErr"
		If DownUserMoney >= Clng(BoardUserLimited(9)) Then
			DownUserQStr = DownUserQStr & ",UserMoney=UserMoney-"&BoardUserLimited(9)
			UploadUserQStr = UploadUserQStr & ",UserMoney=UserMoney+"&(BoardUserLimited(9)*BoardUserLimited(12))\100
		Else
			Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户下载扣除金币 <B>"&BoardUserLimited(9)&"</B> 个，你金币数不够，不能下载。&action=OtherErr"
		End If
	End If
	If BoardUserLimited(10)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户下载扣除金钱 <B>"&BoardUserLimited(10)&"</B> 个，你未登陆，不能下载。&action=OtherErr"
		If DownUserWealth >= Clng(BoardUserLimited(10)) Then
			DownUserQStr = DownUserQStr & ",UserWealth=UserWealth-"&BoardUserLimited(10)
			UploadUserQStr = UploadUserQStr & ",UserWealth=UserWealth+"&(BoardUserLimited(10)*BoardUserLimited(12))\100
		Else
			Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户下载扣除金钱 <B>"&BoardUserLimited(10)&"</B> 个，你金钱数不够，不能下载。&action=OtherErr"
		End If
	End If
	If BoardUserLimited(11)>0 Then
		If Dvbbs.UserID = 0 Then Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户下载扣除积分 <B>"&BoardUserLimited(11)&"</B> 个，你未登陆，不能下载。&action=OtherErr"
		If DownUserEp >= Clng(BoardUserLimited(11)) Then
			DownUserQStr = DownUserQStr & ",UserEp=UserEp-"&BoardUserLimited(11)
			UploadUserQStr = UploadUserQStr & ",UserEp=UserEp+"&(BoardUserLimited(11)*BoardUserLimited(12))\100
		Else
			Response.redirect "showerr.asp?ErrCodes=<li>本版面设置了用户下载扣除积分 <B>"&BoardUserLimited(11)&"</B> 个，你积分不够，不能下载。&action=OtherErr"
		End If
	End If
End If
If Dvbbs.Forum_Setting(76)="" Or Dvbbs.Forum_Setting(76)="0" Then Dvbbs.Forum_Setting(76)="UploadFile/"
If right(Dvbbs.Forum_Setting(76),1)<>"/" Then Dvbbs.Forum_Setting(76)=Dvbbs.Forum_Setting(76)&"/"
Dim uploadpath,filename
uploadpath=Dvbbs.Forum_Setting(76)
Set Rs=Dvbbs.Execute("Select * From dv_upfile Where F_id="&downid)
If Rs.Eof And Rs.Bof Then
	Dvbbs.AddErrCode(32)
ElseIf Dvbbs.BoardID <> Int(Rs("F_BoardID")) Then
	Dvbbs.AddErrCode(32)	Rem 判断版面来源
Else
	If DownUserQStr <> "" Then
		UserInfo.setAttribute "usermoney", (DownUserMoney - BoardUserLimited(9))
		UserInfo.setAttribute "userwealth", (DownUserWealth - BoardUserLimited(10))
		UserInfo.setAttribute "userep", (DownUserEp - BoardUserLimited(11))
		Dvbbs.Execute("Update Dv_User Set "&Right(DownUserQStr,Len(DownUserQStr)-1)&" Where UserID="&Dvbbs.UserID)
	End If
	If DownUserQStr <> "" Then Dvbbs.Execute("Update Dv_User Set "&Right(UploadUserQStr,Len(UploadUserQStr)-1)&" Where UserID="&Rs("F_UserID")&"")
	Dvbbs.Execute("Update dv_upfile Set F_DownNum=F_DownNum+1 Where F_ID="&DownID)
    If Dvbbs.Forum_Setting(75)="0" Then
		If Rs("F_OldName") = "" Or IsNull(Rs("F_OldName")) Then
			Response.Redirect uploadpath&rs("F_filename")
		Else
			downloadFile Server.MapPath(uploadpath&rs("F_filename")),Rs("F_OldName")
		End If
	Else
		filename=Replace(rs("F_filename"),"..","")&""
		If Request.ServerVariables("HTTP_REFERER")="" Or InStr(Request.ServerVariables("HTTP_REFERER"),Request.ServerVariables("SERVER_NAME"))=0 Or filename="" Then
			Response.Redirect "index.asp"
		Else
			downloadFile Server.MapPath(Dvbbs.Forum_Setting(76)&filename),Rs("F_OldName")
		End If
	End If
End If
Rs.close
Set Rs=Nothing
Dvbbs.ShowErr()
Dvbbs.PageEnd()

Sub downloadFile(strFile,FileOldName)
	On error resume next
	Server.ScriptTimeOut=999999
	Dim S,fso,f,intFilelength,strFilename,DownFileName
	strFilename = strFile
	Response.Clear
	Set s = Dvbbs.iCreateObject("ADODB.Stream")
	s.Open
	s.Type = 1
	Set fso = Dvbbs.iCreateObject("Scripting.FileSystemObject")
	If Not fso.FileExists(strFilename) Then
		Response.Write("<h1>错误: </h1><br>系统找不到指定文件")
		Exit Sub
	End If
	Set f = fso.GetFile(strFilename)
		intFilelength = f.size
		s.LoadFromFile(strFilename)
		If err Then
			Response.Write("<h1>错误: </h1>" & err.Description & "<p>")
			Response.End
		End If
		Set fso=Nothing
		Dim Data
		Data=s.Read
		s.Close
		Set s=Nothing
		If FileOldName="" Or IsNull(FileOldName) Then DownFileName=f.name Else DownFileName=FileOldName
		If Response.IsClientConnected Then
			Response.AddHeader "Content-Disposition", "attachment; filename=" &  DownFileName
			Response.AddHeader "Content-Length", intFilelength
			Response.CharSet = "UTF-8"
			Response.ContentType = "application/octet-stream"
			Response.BinaryWrite Data
			Response.Flush
		End If
End Sub
%>