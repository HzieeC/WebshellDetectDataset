<!--#include FILE="conn.asp"-->
<!--#include file="inc/const.asp" -->
<!--#include file="inc/dv_ubbcode.asp"-->
<!--#include file="inc/dv_clsother.asp" -->
<!--#include file="inc/ubblist.asp"-->
<%
Dvbbs.Loadtemplates("show")
Dim username
Dim abgcolor
Dim bbsurl,Sql
Dim MyIsBoard,MyDepth
Dim replyID
bbsurl=""
Dvbbs.stats=Template.Strings(22)
Dvbbs.Nav()
If Dvbbs.BoardID=0 then
	MyIsBoard=2
	MyDepth=0
Else
	MyIsBoard=1
	MyDepth=Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text
End If

Dim dv_ubb
Dim EmotPath
EmotPath=Split(Dvbbs.Forum_emot,"|||")(0)		'em心情路径
Set dv_ubb=new Dvbbs_UbbCode
dv_ubb.PostType=2
If Cint(Dvbbs.GroupSetting(49))=0 then Dvbbs.AddErrCode(54)
Dvbbs.ShowErr()

If Dvbbs.Forum_Setting(76)="" Or Dvbbs.Forum_Setting(76)="0" Then Dvbbs.Forum_Setting(76)="UploadFile/"
If right(Dvbbs.Forum_Setting(76),1)<>"/" Then Dvbbs.Forum_Setting(76)=Dvbbs.Forum_Setting(76)&"/"

If request("action")="send" Then
	card()
ElseIf request("action")="save" Then
	cardsave()
ElseIf request("action")="cards" Then
	showcard()
Else
	main()
End If
Dvbbs.ActiveOnline
Dvbbs.NewPassword()
Set dv_ubb=Nothing 
Dvbbs.Footer()
Dvbbs.PageEnd()
'=====================贺卡演示====================
Sub showcard()
	Dvbbs.stats=Template.Strings(49)
	Dvbbs.Head_var 0,0,template.Strings(0),"show.asp"
	Dim cid,msnid,Rs
	Dim sender,incept,body,title,sendtime
	Dim F_Filename,ftype,flag
	Dim showfile
	Dim Tempwrite
	Dim redcolor,blackcolor
	redcolor=Dvbbs.Mainsetting(1)
	blackcolor=Dvbbs.Mainsetting(3)
	If request("id")="" or Not IsNumeric(request("id")) Then 
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(23)&"&action=OtherErr"
	Else
		cid=clng(request("id"))
	End If

	If request("msmid")="" or Not IsNumeric(request("msmid")) Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(23)&"&action=OtherErr"
	Else
		msnid=clng(request("msmid"))
	End If

	'取出短信内容
	Set Rs=Dvbbs.Execute("select sender,incept,title,content,sendtime from Dv_message where id="&msnid&" and (incept='"&Dvbbs.MemberName&"' or sender='"&Dvbbs.MemberName&"') order by id desc")
	If not (rs.eof and rs.bof) Then
		sender=Dvbbs.htmlencode(trim(rs(0)))
		incept=Dvbbs.htmlencode(trim(rs(1)))
		title=Dvbbs.htmlencode(rs(2))
		body=rs(3)
		sendtime=rs(4)
	Else
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(50)&"&action=OtherErr"
	End If
	Rs.close

	'取出文件内容
	Set Rs=Dvbbs.Execute("select F_Filename,F_Type,F_Flag from [DV_Upfile] where F_ID="&cid&" order by F_ID desc")
	If  Not (Rs.EOF And Rs.BOF) Then
		F_Filename=rs(0)
		ftype=cint(rs(1))
		flag=Cint(rs(2))
		If flag<>3 Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(51)&"&action=OtherErr"
	Else
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(50)&"&action=OtherErr"
	End If
	Rs.close:Set Rs=Nothing

	'判断文件是否本论坛，若不是则采用表中的记录．
	If InStr(F_Filename,":")=0 or InStr(F_Filename,"//")=0 Then
		If Dvbbs.Forum_Setting(75)="0" Then
			F_Filename=bbsurl&Dvbbs.Forum_Setting(76)&F_Filename
		Else
			F_Filename="showimg.asp?Boardid="&Dvbbs.BoardID&"&filename="&F_Filename
		End If
	End If 
	Select Case ftype
	Case 1
		showfile="[img]"&F_Filename&"[/img]"
		ubblists=ubblist(showfile)
		showfile=dv_ubb.Dv_UbbCode(showfile,Dvbbs.UserGroupID,2,1)
	Case 2
		showfile="[flash=500,350]"&F_Filename&"[/flash]"
		ubblists=ubblist(showfile)
		showfile=dv_ubb.Dv_UbbCode(showfile,Dvbbs.UserGroupID,2,0)
	Case Else
		showfile="[upload="&F_FileType&"]viewfile.asp?ID="&F_ID&"[/upload]"
		ubblists=ubblist(showfile)
		showfile=dv_ubb.Dv_UbbCode(showfile,Dvbbs.UserGroupID,2,1)
	End Select
	Tempwrite=Template.html(15)
	Tempwrite=Replace(Tempwrite,"{$sendtime}",sendtime)
	Tempwrite=Replace(Tempwrite,"{$sender}",sender)
	Tempwrite=Replace(Tempwrite,"{$incept}",incept)
	Tempwrite=Replace(Tempwrite,"{$redcolor}",redcolor)
	Tempwrite=Replace(Tempwrite,"{$title}",title)
	Tempwrite=Replace(Tempwrite,"{$showfile}",showfile)
	Tempwrite=Replace(Tempwrite,"{$blackcolor}",blackcolor)
	Ubblists=Ubblist(body)
	Tempwrite=Replace(Tempwrite,"{$dvbody}",dv_ubb.Dv_UbbCode(body,Dvbbs.UserGroupID,2,1))
	Response.Write Tempwrite
End Sub

'贮存发送贺卡
Sub cardsave()
	Dvbbs.stats=Template.Strings(36)
	Dvbbs.Head_var 0,0,template.Strings(0),"show.asp"
	If Dvbbs.UserID=0 Then
		Dvbbs.AddErrCode(6)
		Dvbbs.ShowErr()
	End if
	Dim cid,sname,rname,ctitle,body
	Dim msmid,cardurl,msmbody,Rs,SQl
	cid = Dvbbs.checkStr(trim(request.form("saveid")))
	sname = Dvbbs.checkStr(trim(request.form("sname")))
	rname = Dvbbs.checkStr(trim(request.form("subject")))	'收信人名
	ctitle = Dvbbs.checkStr(trim(request.form("title")))
	body = Html2Ubb(request.form("Body"))
	body = Dvbbs.checkStr(body)
	If cid="" or Not IsNumeric(cid) Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(23)&"&action=OtherErr"
	If Not (IsEmpty(session("lastpost")) or Dvbbs.boardmaster or Dvbbs.master or Dvbbs.superboardmaster) Then
		If DateDiff("s",session("lastpost"),Now())<10 Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(37)&"&action=OtherErr"
	End If
	If Dvbbs.chkpost=False Then Dvbbs.AddErrCode(16)
	Dvbbs.ShowErr()
	If Replace(rname,",","")="" Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(38)&"&action=OtherErr"
	Else
		rname=split(rname,",")
	End If
	If ctitle="" Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(39)&"&action=OtherErr"
	ElseIf Dvbbs.strLength(ctitle)>50 Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(40)&"&action=OtherErr"
	End If
	If Dvbbs.strLength(body)>15360 Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(41)&"&action=OtherErr"
	if body="" Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(42)&"&action=OtherErr"

	Dim msg,ISOK,i,SendNum
	ISOK=False
	Dim k:K=0
	Dim OKlist
	SendNum=UBound(rname)+1
	If Dvbbs.UserToday(1)<SendNum Then
		SendNum=Dvbbs.UserToday(1)
	End if
	For i=0 to SendNum-1
		If Not IsFind(rname(i)) Then
			msg = msg &Template.Strings(43)
			msg = Replace(msg,"{$rname}",rname(i))
		Else
			If K>Cint(Dvbbs.GroupSetting(33))-1 Then
				msg = msg & Template.Strings(44)
				msg=Replace(msg,"{$rennum}",Dvbbs.GroupSetting(33))
				msg=Replace(msg,"{$rname}",rname(i))
			Else
				'插入短信并获得ID
				sql="insert into dv_message (incept,sender,title,content,sendtime,flag,issend) values ('"&rname(i)&"','"&Dvbbs.membername&"','"&ctitle&"','"&body&"',"&SqlNowString&",0,1)"
				Dvbbs.Execute(sql)
				update_user_msg(rname(i))
				set Rs=Dvbbs.Execute("select top 1 id from dv_message order by id desc")
				msmid=rs(0)
				rs.close
				cardurl=bbsurl&"fileshow.asp?action=cards&id="&cid&"&msmid="&msmid
				cardurl="[URL="&cardurl&"]"&Template.Strings(28)&"[/URL]"
				msmbody=body+chr(13)+chr(13)+chr(10)+chr(10)+chr(10)+cardurl
				Dvbbs.Execute("update [dv_message] set content='"&Dvbbs.checkStr(msmbody)&"' where id="&msmid)
				Dvbbs.Execute("update [DV_Upfile] set F_Flag=3 where F_ID="&cid)
				K=K+1
				ISOK=True
				OKlist=OKlist&Template.Strings(45)
				OKlist=Replace(OKlist,"{$rname}",rname(i))
			End If
		End If 
		cardurl=""
	Next
	Set Rs=Nothing
	'更新用户今日短信数据
	If SendNum > 0 Then
	Dim iUserInfo
		iUserInfo = Dvbbs.UserToday(0) & "|" & Dvbbs.UserToday(1) + SendNum & "|" & Dvbbs.UserToday(2)
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usertoday").text=iUserInfo
		Dvbbs.Execute("Update [Dv_User] Set UserToday='" & iUserInfo & "' Where UserID = " & Dvbbs.UserID)
	End If
	If ISOK Then 
		Dim sucmsg
		sucmsg=sucmsg+"<br>"+Template.Strings(46)&OKlist
		session("lastpost")=Now()
		If Msg<>"" Then sucmsg=sucmsg&Template.Strings(47)&msg
	Else
		Response.redirect "showerr.asp?ErrCodes="&msg&Template.Strings(48)&"&action=OtherErr"
	End If
	Dvbbs.Dvbbs_suc(sucmsg)
End Sub

 '编写贺卡内容
Sub card()
	Dvbbs.stats=Template.Strings(33)
	dvbbs.Head_var 0,0,template.Strings(0),"show.asp"
	Dim sid,showfile
	Dim F_Filename,F_Type
	Dim frs,Rs,SQl
	Dim Postubb
	Dim Tempwrite
	If Dvbbs.UserID=0 Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(34)&"&action=OtherErr"
	If request("id")="" or not isnumeric(request("id")) Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(23)&"&action=OtherErr"
	Else
		sid=CLng(request("id"))
	End If
	'F_ID,F_Username,F_Filename,F_FileType,F_Type,F_Readme,F_ViewNum,F_Flag,F_boardid
	Set Rs=Dvbbs.Execute("select * from [DV_Upfile] where F_ID="&sid)
	If Not (Rs.EOF And Rs.BOF) Then
		F_Filename=Dvbbs.htmlencode(rs("F_Filename"))
		'判断文件是否本论坛，若不是则采用表中的记录．
		If InStr(F_Filename,":")=0 or InStr(F_Filename,"//")=0 Then
			If Dvbbs.Forum_Setting(75)="0" Then
				F_Filename=bbsurl&Dvbbs.Forum_Setting(76)&F_Filename
			Else
				F_Filename="showimg.asp?Boardid="&Dvbbs.BoardID&"&filename="&F_Filename
			End If
		End If 
		F_Type=cint(rs("F_Type"))
		Select Case F_Type
		Case 1
			If Renzhen(Rs("F_boardid"),Dvbbs.Membername) then
				showfile="[img]"&F_Filename&"[/img]"
				ubblists=ubblist(showfile)
				showfile=dv_ubb.Dv_UbbCode(showfile,Dvbbs.UserGroupID,2,1)
			Else
				Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(35)&"&action=OtherErr"
			End if
		Case 2
			If Renzhen(Rs("F_boardid"),Dvbbs.Membername) then
				showfile="[flash=500,350]"&F_Filename&"[/flash]"
				ubblists=ubblist(showfile)&"39,"
				showfile=dv_ubb.Dv_UbbCode(showfile,Dvbbs.UserGroupID,2,0)
			Else
				Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(35)&"&action=OtherErr"
			End if
		Case Else
 			Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(35)&"&action=OtherErr"
		End Select
	Else
		Dvbbs.AddErrCode(35)
		Dvbbs.ShowErr()
	End If
	Rs.close:Set Rs=Nothing
	Tempwrite=Template.html(14)
	Tempwrite=Replace(Tempwrite,"{$showfile}",showfile)
	Tempwrite=Replace(Tempwrite,"{$friend}",OPTION_Friend)
	Tempwrite=Replace(Tempwrite,"{$sname}",Dvbbs.Membername)
	Tempwrite=Replace(Tempwrite,"{$sid}",Sid)
	Tempwrite=Replace(Tempwrite,"{$postubb}",Temp_UBB)
	Response.Write Tempwrite
End Sub 

Sub main()
	Dim Tempwrite
	Dim sid
	Dim F_ID, F_AnnounceID, F_BoardID, F_UserID ,F_Username, F_Filename, F_FileType, F_Type, F_FileSize, F_Readme, F_DownNum, F_ViewNum, F_DownUser, F_Flag, F_AddTime
	Dim F_typename,Selfiletype
	Dim golist,showfile,csend
	Selfiletype=Split(Dvbbs.lanstr(5),"||")
	If request("id")="" or not IsNumeric(request("id")) Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(23)&"&action=OtherErr"
	Else
		sid=clng(request("id"))
	End If
	If Dvbbs.boardid=0 Then
		dvbbs.Head_var 0,0,template.Strings(0),"show.asp"
		Sql="select F_ID,F_AnnounceID,F_BoardID,F_UserID,F_Username,F_Filename,F_FileType,F_Type ,F_FileSize,F_Readme,F_DownNum,F_ViewNum,F_DownUser,F_Flag,F_AddTime from [DV_Upfile] where F_ID="&sid
	Else
		Dvbbs.head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
		Sql="select F_ID,F_AnnounceID,F_BoardID,F_UserID,F_Username,F_Filename,F_FileType,F_Type ,F_FileSize,F_Readme,F_DownNum,F_ViewNum,F_DownUser,F_Flag,F_AddTime from [DV_Upfile] where F_ID="&sid&" and F_boardid="&Dvbbs.Boardid
	End if

	Dim Rs
	Set Rs=Dvbbs.Execute(Sql)
	If Not(Rs.EOF And Rs.BOF) Then
	Dvbbs.Execute("update [DV_Upfile] set F_ViewNum=F_ViewNum+1 where  F_ID="& sid)
	F_ID=rs(0)
	F_AnnounceID=rs(1)
	replyID=F_AnnounceID
	F_BoardID=rs(2)
	F_UserID=rs(3)
	F_Username=rs(4)
	F_Filename=rs(5)
	F_FileType=rs(6)
	F_Type=rs(7)
	F_FileSize=rs(8)
	F_Readme=Rs(9)
	F_DownNum=rs(10)
	F_ViewNum=rs(11)
	'F_DownUser=rs(12)
	'F_Flag=rs(13)
	F_AddTime=rs(14)
	End If
	Rs.Close:Set Rs=Nothing

	If F_Readme<>"" or Not IsNull(F_Readme) Then
		F_Readme=Dvbbs.HtmlEnCode(F_Readme)
	Else
		F_Readme="<font color=gray>"&Template.Strings(24)&"</font>"
	End If
	'判断文件是否本论坛，若不是则采用表中的记录．
	If InStr(F_Filename,":")=0 or InStr(F_Filename,"//")=0 Then
		If Dvbbs.Forum_Setting(75)="0" Then
			F_Filename=bbsurl&Dvbbs.Forum_Setting(76)&F_Filename
		Else
			F_Filename="showimg.asp?Boardid="&Dvbbs.BoardID&"&filename="&F_Filename
		End If
	End If 
	If Not IsNull(F_AnnounceID) And F_AnnounceID<>"" And InStr(F_AnnounceID,"|")>0 Then
		F_AnnounceID=split(F_AnnounceID,"|")
		golist="<a href=dispbbs.asp?Boardid="&F_BoardID&"&ID="&F_AnnounceID(0)&"&replyID="&F_AnnounceID(1)&"&skin=1 target=_blank title="&Template.Strings(9)&">"&Template.Strings(25)&"</a>"
	Else
		golist=Template.Strings(26)
	End If
	Select Case F_Type
	Case 1
		F_typename=Selfiletype(1) '图片集
		IF Renzhen(F_BoardID,Dvbbs.Membername) Then
			showfile="[IMG]"&F_Filename&"[/img]"
			ubblists=ubblist(showfile)&"39,"
			showfile=dv_ubb.Dv_UbbCode(showfile,Dvbbs.UserGroupID,2,1)
			csend="<a href=fileshow.asp?action=send&amp;id="&f_id&"><img title="&Template.Strings(32)&" src=skins/default/newmail.gif border=0 width=28 height=11></a>"
		Else
			csend=""
			showfile=Template.Strings(31)&F_typename
		End if
	case 2
		F_typename=Selfiletype(2) 'Flash集
		IF Renzhen(F_BoardID,Dvbbs.Membername) Then
			Showfile = "[FLASH=500,350]" & F_Filename & "[/FLASH]"
			Ubblists = Ubblist(Showfile) & "39,"
			showfile=dv_ubb.Dv_UbbCode(showfile,Dvbbs.UserGroupID,2,0)
			csend="<a href=fileshow.asp?action=send&amp;id="&f_id&"><img title="&Template.Strings(32)&" src=skins/default/newmail.gif border=0 width=28 height=11></a>"
		Else
			showfile=Template.Strings(31)&F_typename
			csend=""
		End if
	case 3
		F_typename=Selfiletype(3) '音乐集
		IF Renzhen(F_BoardID,Dvbbs.Membername) Then
			showfile="<img src=skins/default/filetype/"&F_FileType&".gif border=0><a href="&Dvbbs.htmlencode(F_Filename)&" target=_blank title="&Template.Strings(28)&">"&Dvbbs.htmlencode(F_Filename)&"</a>"
			csend="<a href=fileshow.asp?action=send&amp;id="&f_id&"><img title="&Template.Strings(32)&" src=skins/default/newmail.gif border=0 width=28 height=11></a>"
		Else
			showfile=Template.Strings(31)&F_typename
			csend=""
		End if
	Case 4
		F_typename=Selfiletype(4) '电影集
		IF Renzhen(F_BoardID,Dvbbs.Membername) Then
			showfile="<img src=skins/default/filetype/"&F_FileType&".gif border=0><a href="&Dvbbs.htmlencode(F_Filename)&" target=_blank title="&Template.Strings(28)&">"&Dvbbs.htmlencode(F_Filename)&"</a>"
			csend="<a href=fileshow.asp?action=send&amp;id="&f_id&"><img title="&Template.Strings(32)&" src=skins/default/newmail.gif border=0 width=28 height=11></a>"
		Else
			showfile=Template.Strings(31)&F_typename
			csend=""
		End if
	Case Else 
		F_typename=Selfiletype(0) '文件集
		IF Renzhen(F_BoardID,Dvbbs.Membername) Then
			showfile="[upload="&F_FileType&"]viewfile.asp?ID="&F_ID&"[/upload]"
			ubblists=ubblist(showfile)&"39,"
			showfile=dv_ubb.Dv_UbbCode(showfile,Dvbbs.UserGroupID,2,1)
			csend="<a href=fileshow.asp?action=send&amp;id="&f_id&"><img title="&Template.Strings(32)&" src=skins/default/newmail.gif border=0 width=28 height=11></a>"
		Else
			showfile=Template.Strings(31)&F_typename
			csend=""
		End if
	End Select
	Dim edit
	edit=""
	If Dvbbs.GroupSetting(48)=1 Then
		If Dvbbs.master or Dvbbs.superboardmaster or Dvbbs.boardmaster  Then
			edit="<a title="&Template.Strings(29)&" href=myfile.asp?action=edit&amp;editid="&Clng(F_ID)&"><img src=skins/default/editfile.gif border=0 width=10 height=10></a>&nbsp;<a title="&Template.Strings(30)&" href=myfile.asp?action=fdel&amp;delid="&Clng(F_ID)&"><img height=10 src=skins/default/delete.gif width=10 border=0></a>"
		ElseIf F_Username=Dvbbs.membername Then
			edit="<a title="&Template.Strings(29)&" href=myfile.asp?action=edit&amp;editid="&Clng(F_ID)&"><img src=skins/default/editfile.gif border=0 width=10 height=10></a>&nbsp;<a title="&Template.Strings(30)&" href=myfile.asp?action=fdel&amp;delid="&Clng(F_ID)&"><img height=10 src=skins/default/delete.gif width=10 border=0></a>"
		Else
			edit=""
		End If
	End If
	Tempwrite=Template.html(13)
	Tempwrite=Replace(Tempwrite,"{$f_userid}",Clng(F_UserID))
	Tempwrite=Replace(Tempwrite,"{$f_username}",Dvbbs.HtmlEnCode(f_username))
	Tempwrite=Replace(Tempwrite,"{$showfile}",showfile)
	Tempwrite=Replace(Tempwrite,"{$edit}",edit)
	Tempwrite=Replace(Tempwrite,"{$f_typename}",f_typename)
	Tempwrite=Replace(Tempwrite,"{$f_filesize}",f_filesize & "")
	Tempwrite=Replace(Tempwrite,"{$f_viewnum}",f_viewnum)
	Tempwrite=Replace(Tempwrite,"{$f_downnum}",f_downnum)
	Tempwrite=Replace(Tempwrite,"{$f_addtime}",f_addtime)
	Tempwrite=Replace(Tempwrite,"{$golist}",golist)
	Tempwrite=Replace(Tempwrite,"{$f_readme}",f_readme)
	Tempwrite=Replace(Tempwrite,"{$csend}",csend)
	Response.Write Tempwrite

End Sub

Function IsFind(UserName)
	IsFind=False
	If UserName<>"" Then
		USerName=replace(UserName,"'","")
		Dim Rs
		Set Rs=Dvbbs.Execute("select Count(*) from [dv_user] where username='"&USerName&"'")
		If Rs(0)>0 Then IsFind=True
		Set Rs=Nothing
	End If 
End Function

'用户好友下拉名单
Function OPTION_Friend()
DIM i,Rs
Sql="SELECT F_friend FROM Dv_Friend WHERE F_userid="&Dvbbs.userid&" ORDER BY F_addtime DESC"
Set Rs=Dvbbs.Execute(Sql)
If not Rs.eof Then
	SQL=Rs.GetRows(-1)
	Rs.Close:Set Rs=Nothing
End if
If IsArray(SQL) Then
For i=0 To Ubound(SQL,2)
OPTION_Friend=OPTION_Friend & "<OPTION value="""&SQL(0,i)&""">"&SQL(0,i)&"</OPTION> "
Next
Else
OPTION_Friend=""
End If
End Function

'黑名单验证
Function CHKHateName(name)
DIM Sql,Rs
CHKHateName=False
Sql="Select F_friend From Dv_Friend Where (F_userid="&Dvbbs.userid&" or F_username='"&name&"') And F_Mod=2"
Set Rs=Dvbbs.Execute(Sql)
If not Rs.eof Then
	Sql=Rs.GetString(,, ",", "", "")
	Rs.Close:Set Rs=Nothing
	If instr(Sql,name) or instr(Sql,Dvbbs.Membername) Then CHKHateName=True
End If
End Function

'更新用户短信通知信息（新短信条数||新短讯ID||发信人名）
Sub UPDATE_User_Msg(username)
	Dim msginfo,i,UP_UserInfo,newmsg
	newmsg=newincept(username)
	If newmsg>0 Then
		msginfo=newincept(username) & "||" & inceptid(1,username) & "||" & inceptid(2,username)
	Else
		msginfo="0||0||null"
	End If
	Dvbbs.execute("UPDATE [Dv_User] Set UserMsg='"&Dvbbs.CheckStr(msginfo)&"' WHERE username='"&Dvbbs.CheckStr(username)&"'")
	If username=Dvbbs.MemberName Then
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermsg").text=msginfo
	Else
		Call Dvbbs.NeedUpdateList(username,1)
	End If
End Sub

'统计留言
Function newincept(iusername)
Dim Rs
Rs=Dvbbs.execute("SELECT Count(id) FROM Dv_Message WHERE flag=0 And issend=1 And DelR=0 And incept='"& iusername &"'")
    newincept=Rs(0)
	Set Rs=nothing
	If isnull(newincept) Then newincept=0
End Function

Function inceptid(stype,iusername)
	Dim Rs
	Set Rs=Dvbbs.execute("SELECT top 1 id,sender FROM Dv_Message WHERE flag=0 And issend=1 And DelR=0 And incept='"& iusername &"'")
	If not rs.eof Then
		If stype=1 Then
			inceptid=Rs(0)
		Else
			inceptid=Rs(1)
		End If
	Else
		If stype=1 Then
			inceptid=0
		Else
			inceptid="null"
		End If
	End If
	Set Rs=nothing
End Function

'认证论坛的文件的判断
Function renzhen(boardid,username)
	Dim boarduser,rrs,Board_Setting,BoardMaster,i
	Dim sql
	renzhen=false
	If Dvbbs.Master then
		renzhen=true
	Elseif boardid=0 then
		renzhen=true
	Else
		sql="select boarduser,Board_Setting,BoardMaster from Dv_board where boardid="&boardid
		set rrs=Dvbbs.iCreateObject("adodb.recordset")
		rrs.open sql,conn,1,1
		Dvbbs.SqlQueryNum=Dvbbs.SqlQueryNum+1
		Board_Setting=split(rrs("board_setting"),",")
		If cint(Board_Setting(2))=1 then
			If not (isnull(rrs(2)) or rrs(2)="") then
				BoardMaster=split(rrs(2), "|")
				For i = 0 to ubound(BoardMaster)
					If trim(BoardMaster(i))=trim(username) then
						renzhen=true
						Exit for
					End if
				Next
			End if
			If renzhen=false then
				If isnull(rrs(0)) or rrs(0)="" then
					renzhen=false
				Else
					boarduser=split(rrs(0), ",")
					For i = 0 to ubound(boarduser)
						If trim(boarduser(i))=trim(username) then
							renzhen=true
							Exit for
						End if
					Next
				End if
			End if
		Else
			renzhen=true
		End if
		rrs.close
		Set rrs=nothing
	End if
End function

'只读，获得UBB模板
Function Temp_UBB()
	Dvbbs.Loadtemplates("post")
	Dim TempArray,i
	Temp_UBB = template.html(3)
	TempArray = Split(template.html(9),"|")
	For i = 1 To Ubound(TempArray)
		Temp_UBB = Replace(Temp_UBB,"{$ubb"&i&"}",TempArray(0) & TempArray(i))
	Next
End function
'发贴时用，为了减少入库量
Function Html2Ubb(str)
	If Str<>"" And Not IsNull(Str) Then
		Dim re
		Set re=new RegExp
		re.IgnoreCase =True
		re.Global=True
		re.Pattern = "(&nbsp;)"
		Str = re.Replace(Str,Chr(9))
		re.Pattern = "(<p>)"
		Str = re.Replace(Str,"")
		re.Pattern = "(<\/p>)"
		Str = re.Replace(Str,CHR(10) & CHR(10))
		re.Pattern = "(<STRONG>)"
		Str = re.Replace(Str,"<b>")
		re.Pattern = "(<\/STRONG>)"
		Str = re.Replace(Str,"</b>")
		Html2Ubb = Str
	Else
		Html2Ubb = ""
	End If
End Function
%>