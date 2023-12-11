<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/DV_ubbcode.asp"-->
<!--#include file="inc/ubblist.asp"-->
<%
Dvbbs.LoadTemplates("usermanager")
Dvbbs.Stats=Dvbbs.MemberName&template.Strings(4)
Dvbbs.mainsetting(0)="98%"
Dvbbs.ErrType = 1	'转到不显示顶部和导航的错误显示页
Dvbbs.Head()
Dim Rs,Sql,SqlStr,ErrCodes,TempLateStr,id,sendtime,sender,temptxt
Dim top_TempLateStr,send_TempLateStr,Read_TempLateStr
DIM title,ActInfo
Dim EmotPath
Dim replyid,Announceid,UserName
EmotPath=Split(Dvbbs.Forum_emot,"|||")(0)		'em心情路径
If Dvbbs.userid=0 Then Dvbbs.AddErrCode(6):Dvbbs.Showerr()								'判断用户是否在线。
If Cint(Dvbbs.GroupSetting(32))=0 Then ErrCodes=ErrCodes+"<li>"+template.Strings(33)	'判断用户是否有权限。
id=Request("id")
TempLateStr=split(template.html(11),"||")
If Dvbbs.forum_setting(80)="0" Then
	TempLateStr(1)=Replace(TempLateStr(1),"{$getcode}","")
Else
	TempLateStr(4)=Replace(TempLateStr(4),"{$codestr}",Dvbbs.GetCode)
	TempLateStr(1)=Replace(TempLateStr(1),"{$getcode}",TempLateStr(4))
End If
top_TempLateStr=TempLateStr(0)
top_TempLateStr=Replace(top_TempLateStr,"{$sms__write}",template.pic(5))
top_TempLateStr=Replace(top_TempLateStr,"{$sms__reply}",template.pic(6))
top_TempLateStr=Replace(top_TempLateStr,"{$sms__fw}",template.pic(7))
top_TempLateStr=Replace(top_TempLateStr,"{$sms_delete}",template.pic(8))
Send_TempLateStr=top_TempLateStr&TempLateStr(1)
Read_TempLateStr=top_TempLateStr&TempLateStr(2)
ActInfo=split(template.Strings(62),",")
If ErrCodes="" Then
Dim dv_ubb,abgcolor
Set dv_ubb=new Dvbbs_UbbCode
dv_ubb.PostType=2
SELECT Case Request("action")
	Case "new"
		Response.Cookies("Dvbbs")=""
		IF id<>"" Then
			title="RW: "
			SqlStr="SELECT id,title,content,incept,sender,sendtime FROM Dv_Message WHERE incept='"&Dvbbs.MemberName&"' And id="&Dvbbs.checkStr(id)
			temptxt=template.Strings(56)
		End If
		Dvbbs.Stats=ActInfo(0)		'"发送短信"
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		'Response.Write "<script src=""images/post/post.js"" type=""text/javascript""></script>"
		call sendmsg()		
	Case "read"
		Dvbbs.Stats=ActInfo(1)		'"阅读短信"
		call read()
		Dvbbs.NewPassword()
	Case "outread"
		Dvbbs.Stats=ActInfo(1)		'"阅读短信"
		call read()
		Dvbbs.NewPassword()
	Case  "newmsg"
		Dvbbs.Stats=ActInfo(0)		'"发送短信"
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		Call newmsg()
	Case "send"
		Dvbbs.Stats=ActInfo(2)		'"保存发送短信"
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		call savemsg()
	Case "fw"
		Response.Cookies("Dvbbs")=""
		title="FW: "
		Dvbbs.Stats=ActInfo(3)		'"转发短信"
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		SqlStr="SELECT id,title,content,incept,sender,sendtime FROM Dv_Message WHERE (incept='"&Dvbbs.MemberName&"' or sender='"&Dvbbs.MemberName&"') And id="&Dvbbs.checkStr(id)
		temptxt=template.Strings(57)
		call sendmsg()
	Case "edit"
		Response.Cookies("Dvbbs")=""
		Dvbbs.Stats=ActInfo(4)		'"修改短信"
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		SqlStr="SELECT id,title,content,incept,sender,sendtime FROM Dv_Message WHERE sender='"&Dvbbs.MemberName&"' And issend=0 And id="&Dvbbs.checkStr(id)
		call sendmsg()
	Case "savedit"
		Call savedit()
	Case "delet"
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		call Delete()
	Case ActInfo(5)					'"删除收件箱"
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		Dvbbs.Stats=ActInfo(5)
		call Delinbox()
	Case ActInfo(6)					'"清空收件箱"
		Dvbbs.Stats=ActInfo(6)
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		call AllDelinbox()
	Case ActInfo(7)					'"删除草稿箱"
		Dvbbs.Stats=ActInfo(7)
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		call Deloutbox()
	Case ActInfo(8)					'"清空草稿箱"
		Dvbbs.Stats=ActInfo(8)
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 	
		call AllDeloutbox()
	Case ActInfo(9)					'"删除已发送的消息"
		Dvbbs.Stats=ActInfo(9)
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		call Delissend()
	Case ActInfo(10)				'"清空已发送的消息"
		Dvbbs.Stats=ActInfo(10)
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		call AllDelissend()
	case ActInfo(11)				'"删除垃圾箱"
		Dvbbs.Stats=ActInfo(11)
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		call Delrecycle()
	Case ActInfo(12)				'"清空垃圾箱"
		Dvbbs.Stats=ActInfo(12)
		If Dvbbs.IsReadonly()  And Not Dvbbs.Master Then Response.redirect "showerr.asp?action=readonly&boardid="&dvbbs.boardID&"" 
		Call AllDelrecycle()
	Case Else
  		ErrCodes=ErrCodes+"<li>"+template.Strings(51)
End SELECT
End If
If ErrCodes<>"" Then Showerr
Dvbbs.ActiveOnline()
Response.Write TempLateStr(3)
Response.Write "</div>"
Dvbbs.Footer()
Dvbbs.PageEnd()
'发送信息,回复，转发,编辑
Sub sendmsg()
	Dim content,i,textarea,touser,incept
	Dim chatloglist
	textarea=""
	If Clng(Dvbbs.GroupSetting(53))>0 And DateDiff("s",Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@joindate").text,Now)<Clng(Dvbbs.GroupSetting(53))*60 Then
		ErrCodes=ErrCodes+"<li>"+Replace(template.Strings(39),"{$Lim_Time}",Dvbbs.GroupSetting(53))
		Exit sub
	End If
	If Dvbbs.GroupSetting(63)<>"0" Then
		If Clng(Dvbbs.GroupSetting(63))<=Clng(Dvbbs.UserToday(1)) Then
			ErrCodes=ErrCodes+"<li>"+Replace(template.Strings(65),"{$smslimited}",Dvbbs.GroupSetting(63))
			Exit sub
		End If
	End If
	touser=Dvbbs.Checkstr(Request("touser"))
	If id<>"" And isNumeric(id) And SqlStr<>"" Then
		Set Rs=Dvbbs.execute(SqlStr)
		If not(Rs.eof And Rs.bof) Then
			If Request("action")="new" or Request("action")="edit" Then touser=Rs("sender")
				incept=Rs("incept")
				sender=Rs("sender")
				sendtime=Rs("sendtime")
				title=title & Rs("title")
				content=Rs("content")
				temptxt=Replace(temptxt,"{$sendtime}",sendtime)
				temptxt=Replace(temptxt,"{$sender}",sender)
				'temptxt=Replace(temptxt,"{$br}",Chr(10))
				temptxt=Replace(temptxt,"{$br}","<br/>")
			If Request("action")="new" or Request("action")="fw" Then 
				textarea=temptxt+content+"<br/>======================================<br/>"
			Else
				textarea=content
			End If
			If Not Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@postdata") Is nothing  Then
				textarea=server.htmlencode(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@postdata").text)
			Else
				textarea=server.htmlencode(textarea)
			End If 
		Else
			ErrCodes=ErrCodes+"<li>"+template.Strings(35):Exit Sub
		End If
		Set Rs=Nothing
	Else
		If Not Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@postdata") Is nothing  Then
				textarea=server.htmlencode(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@postdata").text)
			Else
				textarea=server.htmlencode(textarea)
		End If 
	End If
	If Request("reaction")="chatlog" Then
	Dim temp_chat1,temp_chat2
			Touser=Dvbbs.checkStr(Request("touser"))
		Sql="SELECT sender,incept,title,content,sendtime FROM Dv_Message WHERE ((sender='"&Dvbbs.MemberName&"' And incept='"&Touser&"') or (sender='"&Touser&"' And incept='"&Dvbbs.MemberName&"')) And DelS=0 ORDER BY sendtime DESC"
		Set Rs=Dvbbs.Execute(Sql)
			If Rs.eof And Rs.bof Then
			Chatloglist="<tr><td class=tablebody1 colspan=3>"&template.Strings(58)&"</td></tr>"
			Else
				SQL=Rs.GetRows(-1)
				Rs.close:Set Rs=nothing

				For i=0 to Ubound(SQL,2)
					temp_chat1=template.Strings(59)
					temp_chat2=template.Strings(60)
					chatloglist=chatloglist & "<tr><td class=tablebody2 height=25 colspan=3>"
					If Trim(SQL(0,i))=Dvbbs.MemberName Then
						temp_chat1=Replace(temp_chat1,"{$sendtime}",SQL(4,i))
						temp_chat1=Replace(temp_chat1,"{$incept}",Dvbbs.HtmlEncode(SQL(1,i)))
						chatloglist=chatloglist & temp_chat1
					Else
						temp_chat2=Replace(temp_chat2,"{$sendtime}",SQL(4,i))
						temp_chat2=Replace(temp_chat2,"{$senduser}",Dvbbs.HtmlEncode(SQL(0,i)))
						chatloglist=chatloglist & temp_chat2
					End If
					chatloglist=chatloglist & "</td></tr><tr><td class=tablebody1 valign=top align=left colspan=3><b>"
					chatloglist=chatloglist & template.Strings(61)&Dvbbs.HtmlEncode(SQL(2,i))
					chatloglist=chatloglist & "</b><hr size=1>"
					Ubblists=Ubblist(SQL(3,i))&"39,"
					chatloglist=chatloglist & dv_ubb.Dv_UbbCode(SQL(3,i),Dvbbs.UserGroupID,2,0)
					chatloglist=chatloglist & "</td></tr>"
				Next
			End If
		End If
		If Request("action")="edit" Then 
			send_TempLateStr=Replace(send_TempLateStr,"{$Sms_SendAct}","messanger.asp?action=savedit&id="&id)
		Else
			send_TempLateStr=Replace(send_TempLateStr,"{$Sms_SendAct}","messanger.asp?action=send")
		End If
		send_TempLateStr=Replace(send_TempLateStr,"{$mo_send}","")
	send_TempLateStr=Replace(send_TempLateStr,"{$sender}",sender)
	send_TempLateStr=Replace(send_TempLateStr,"{$touser}",touser)
	send_TempLateStr=Replace(send_TempLateStr,"{$Friend_option}",OPTION_Friend)
	send_TempLateStr=Replace(send_TempLateStr,"{$title}","value="""&title&"""")
	send_TempLateStr=Replace(send_TempLateStr,"{$sms_id}",id)
	send_TempLateStr=Replace(send_TempLateStr,"{$sendtime}",sendtime)
	send_TempLateStr=Replace(send_TempLateStr,"{$content}",content)
	send_TempLateStr=Replace(send_TempLateStr,"{$Sms_senduser}",Dvbbs.GroupSetting(33))
	send_TempLateStr=Replace(send_TempLateStr,"{$Sms_maxbody}",(Dvbbs.GroupSetting(34)))
	send_TempLateStr=Replace(send_TempLateStr,"{$reaction}",Request("reaction"))
	send_TempLateStr=Replace(send_TempLateStr,"{$action}",Request("action"))
	send_TempLateStr=Replace(send_TempLateStr,"{$chatloglist}",EncodeJS(chatloglist))
	send_TempLateStr=Replace(send_TempLateStr,"{$myname}",Dvbbs.membername)
	send_TempLateStr=Replace(send_TempLateStr,"{$textarea}",textarea)
	If Dvbbs.GroupSetting(63)<>"0" Then
		send_TempLateStr=Replace(send_TempLateStr,"{$smslimited}",Clng(Dvbbs.GroupSetting(63))-Clng(Dvbbs.UserToday(1)))
	Else
		send_TempLateStr=Replace(send_TempLateStr,"{$smslimited}",9999)
	End If
	Response.Write send_TempLateStr
End Sub

'读取信息
sub read()
	If id<>"" and isNumeric(id) Then
		id=Clng(id)
	Else
		ErrCodes=ErrCodes+"<li>"+template.Strings(51)
		Exit Sub
	End If
	Dim incept,content
	Dim nextid,nextsender

	Sql="SELECT * FROM Dv_Message WHERE (incept='"&Dvbbs.MemberName&"' or sender='"&Dvbbs.MemberName&"') And id="&id
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.eof And Rs.bof Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(34)
		exit sub
	Else
	incept=Rs("incept")
	sender=Rs("sender")
	title=Dvbbs.htmlencode(Rs("title"))
	Sendtime = Rs("sendtime")
	If Not Isdate(Sendtime) Then Sendtime = Now()
	Ubblists=Ubblist(Rs("content"))&"39,"
	content=dv_ubb.Dv_UbbCode(Rs("content"),Dvbbs.UserGroupID,2,0)
	End If
	Rs.close:Set Rs=Nothing

	If Request("action")="read" Then
   		Sql="UPDATE [Dv_message] Set flag=1 WHERE ID="&id
		Dvbbs.execute(sql)
		UPDATE_User_Msg(Dvbbs.MemberName)
	End If

	Sql="SELECT id,sender FROM Dv_Message WHERE incept='"&Dvbbs.MemberName&"' And flag=0 And issend=1 And id>"&id&" ORDER BY sendtime "
	Set Rs=Dvbbs.execute(sql)
	If not (Rs.eof And Rs.bof) Then
		nextid=Rs(0)
		nextsender=Rs(1)
	End If
	Rs.close

	Read_TempLateStr=Replace(Read_TempLateStr,"{$incept}",incept)
	Read_TempLateStr=Replace(Read_TempLateStr,"{$sender}",sender)
	Read_TempLateStr=Replace(Read_TempLateStr,"{$read_title}",title)
	Read_TempLateStr=Replace(Read_TempLateStr,"{$sms_id}",id)
	Read_TempLateStr=Replace(Read_TempLateStr,"{$sendtime}",sendtime)
	Read_TempLateStr=Replace(Read_TempLateStr,"{$textarea}",content)
	Read_TempLateStr=Replace(Read_TempLateStr,"{$nextid}",nextid)
	Read_TempLateStr=Replace(Read_TempLateStr,"{$nextsender}",nextsender)
	Response.Write Read_TempLateStr
End Sub


'保存
Sub savemsg()
	Dim i,BoxName
	Dim incept,title,message,subtype
	Dim Http_Referer
	'把提交的数据保存到session
	Dvbbs.UserSession.documentElement.selectSingleNode("userinfo").attributes.setNamedItem(Dvbbs.UserSession.createNode(2,"postdata","")).text=Request.form("message")
	BoxName=split(template.Strings(63),",")
	If Clng(Dvbbs.GroupSetting(53))>0 And DateDiff("s",Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@joindate").text,Now)<Clng(Dvbbs.GroupSetting(53))*60 Then
		ErrCodes=ErrCodes+"<li>"+Replace(template.Strings(39),"{$Lim_Time}",Dvbbs.GroupSetting(53))
		Exit Sub
	End If
	If Dvbbs.GroupSetting(63)<>"0" Then
		If Clng(Dvbbs.GroupSetting(63))<=Clng(Dvbbs.UserToday(1)) Then
			ErrCodes=ErrCodes+"<li>"+Replace(template.Strings(65),"{$smslimited}",Dvbbs.GroupSetting(63))
			Exit sub
		End If
	End If
	If Dvbbs.forum_setting(80)="1" Then
		If Not Dvbbs.CodeIsTrue() Then
			Http_Referer = "messanger.asp?action=new&touser="&Request("touser")
			ErrCodes=ErrCodes+"<meta http-equiv=refresh content=""2;URL="&Http_Referer&"""><li>验证码校验失败，请返回刷新页面再试。两秒后自动返回"
			Exit Sub
		End If
	End If

	If CStr(Request.Cookies("Dvbbs"))=CStr(Dvbbs.Boardid) Then
		Dvbbs.Dvbbs_Suc("<li>"+template.Strings(26))	'您的修改信息已成功提交！
		Exit Sub
	End If

	If Request.form("touser")="" Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(35)
		Exit Sub
	Else
		incept=replace(Request.form("touser"),"'","")
		incept=split(incept,",")
	End If

	If Request.form("title")="" or Dvbbs.StrLength(Request.form("title")) > 50 Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(53)
		Exit Sub
	Else
		title=Dvbbs.checkStr(Request.form("title"))
	End If
	If Request.form("message")="" or (Dvbbs.StrLength(Request.form("message")) > CLng(Dvbbs.GroupSetting(34)) And CLng(Dvbbs.GroupSetting(34)) > 0) Then
		ErrCodes=ErrCodes+"<li>"+Replace(template.Strings(54),"{$MaxLen}",Dvbbs.GroupSetting(34))
		Exit Sub
	Else
		message=checkXHTML(Request.form("message"))
		If message <>"" Then
			ErrCodes=ErrCodes+"<li>"& message
		Exit Sub
		End If
		message=Request.form("message")
		message=Dvbbs.checkStr(message)
	End If

	Dim InceptName,SendNum
	SendNum = 0
	FOR i=0 TO ubound(incept)
		Sql="SELECT UserName FROM [Dv_User] WHERE UserName = '"& Trim(incept(i)) &"' "
		Set Rs=Dvbbs.Execute(Sql)
		If Rs.eof And Rs.bof Then
			ErrCodes=ErrCodes+"<li>"+template.Strings(35):Exit Sub
		ELSE
			InceptName=RS(0)
		End If
		Rs.close
		If CHKHateName(InceptName) Then
			ErrCodes=ErrCodes+"<li>"+Replace(template.Strings(64),"{$incept}",InceptName)
			Exit Sub
		Else
			If Request.Form("sms_act")="Sms_Issend" Then
				Sql="insert into Dv_Message (incept,sender,title,content,sendtime,flag,issend) values ('"& Dvbbs.CheckStr(InceptName) &"','"&Dvbbs.MemberName&"','"&title&"','"&message&"',"&SqlNowString&",0,1)"
				subtype=BoxName(2)		'已发送的消息
				SendNum = SendNum + 1
			ElseIf Request.Form("sms_act")="Sms_Issave" Then
				Sql="insert into Dv_Message (incept,sender,title,content,sendtime,flag,issend) values ('"& Dvbbs.CheckStr(InceptName) &"','"&Dvbbs.MemberName&"','"&title&"','"&message&"',"&SqlNowString&",0,0)"
				subtype=BoxName(4)		'发件箱
			Else
				Sql="insert into Dv_Message (incept,sender,title,content,sendtime,flag,issend) values ('"& Dvbbs.CheckStr(InceptName) &"','"&Dvbbs.MemberName&"','"&title&"','"&message&"',"&SqlNowString&",0,1)"
				subtype=BoxName(2)		'已发送的消息
				SendNum = SendNum + 1
			End If
			Dvbbs.execute(sql)
			UPDATE_User_Msg(InceptName)
		End If
		If i>Cint(Dvbbs.GroupSetting(33))-1 Then
			ErrCodes=ErrCodes+"<li>"+Replace(template.Strings(55),"{$Sms_MaxSend}",Dvbbs.GroupSetting(33))
			Exit Sub
		Exit For
		End If
	Next
	'更新用户当日发短信数据以及缓存
	If SendNum > 0 Then
		Dim iUserInfo
		iUserInfo = Dvbbs.UserToday(0) & "|" & Dvbbs.UserToday(1) + SendNum & "|" & Dvbbs.UserToday(2) &"|"& Clng(Dvbbs.UserToday(3)) &"|"& Clng(Dvbbs.UserToday(4))
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usertoday").text=iUserInfo
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@postdata").text=""
		Dvbbs.Execute( "Update [Dv_User] Set UserToday='" & Dvbbs.CheckStr(iUserInfo) & "' Where UserID = " & Dvbbs.UserID)
	End If
	Response.Cookies("Dvbbs")=Dvbbs.Boardid
	Dvbbs.Dvbbs_Suc("<li>"+Replace(template.Strings(38),"{$SmsBOX}",subtype))
End Sub

'保存修改
Sub savedit()
	Dim incept,title,message,subtype
	If Clng(Dvbbs.GroupSetting(53))>0 And DateDiff("s",Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@joindate").text,Now)<Clng(Dvbbs.GroupSetting(53))*60 Then
		ErrCodes=ErrCodes+"<li>"+Replace(template.Strings(39),"{$Lim_Time}",Dvbbs.GroupSetting(53))
		Exit Sub
	End If
	If CheckID(id) = False Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(51)
		Exit Sub
	End If
	If Request("touser")="" Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(35)
		Exit Sub
	Else
		incept=Dvbbs.checkStr(Request.Form("touser"))
	End If
	If Request("title")="" or Dvbbs.StrLength(Request("title")) > 50 Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(53)
		Exit Sub
	Else
		title=Dvbbs.checkStr(Request.Form("title"))
	End If
	If Request("message")="" or (Dvbbs.StrLength(Request("message")) > CLng(Dvbbs.GroupSetting(34)) And CLng(Dvbbs.GroupSetting(34)) > 0) Then
		ErrCodes=ErrCodes+"<li>"+Replace(template.Strings(54),"{$MaxLen}",Dvbbs.GroupSetting(34))
		Exit Sub
	Else
		message=checkXHTML(Request.form("message"))
		If message <>"" Then
			ErrCodes=ErrCodes+"<li>"& message
		Exit Sub
		End If
		message=Request.form("message")
		message=Dvbbs.checkStr(message)
	End If

	Dim SendNum
	SendNum = 0
	Sql="SELECT UserName FROM [Dv_User] WHERE UserName='"&incept&"'"
	Set Rs=Dvbbs.execute(sql)
	If Rs.eof And Rs.bof Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(35)
		Exit Sub
	End If
	Rs.close:Set Rs=Nothing
	If Request("sms_act")="Sms_Issend" Then
		Sql="UPDATE Dv_Message Set incept='"&incept&"',title='"&title&"',content='"&message&"',sendtime="&SqlNowString&",flag=0,issend=1 WHERE id="&Dvbbs.checkStr(id) &" And Sender ='"&Dvbbs.MemberName&"'" Rem modify by Dv.Jastby 只允许修改自己发送的短信 2008-1-10
		subtype="发送箱"
		SendNum = 1
	Else
		Sql="UPDATE Dv_Message Set incept='"&incept&"',title='"&title&"',content='"&message&"',sendtime="&SqlNowString&",flag=0,issend=0 WHERE id="&Dvbbs.checkStr(id) &" And Sender ='"&Dvbbs.MemberName&"'" Rem modify by Dv.Jastby 只允许修改自己发送的短信 2008-1-10
		subtype="发件箱"
	End If
	Dvbbs.execute(sql)

	'更新用户当日发短信数据以及缓存
	If SendNum > 0 Then
		Dim iUserInfo
		iUserInfo = Dvbbs.UserToday(0) & "|" & Dvbbs.UserToday(1) + SendNum & "|" & Dvbbs.UserToday(2) &"|"& Clng(Dvbbs.UserToday(3)) &"|"& Clng(Dvbbs.UserToday(4))
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermsg").text=iUserInfo
		Dvbbs.Execute("Update [Dv_User] Set UserToday='" & iUserInfo & "' Where UserID = " & Dvbbs.UserID)
	End If
	UPDATE_User_Msg(incept)
	UPDATE_User_Msg(Dvbbs.membername)
	Dvbbs.Dvbbs_Suc("<li>"+Replace(template.Strings(38),"{$SmsBOX}",subtype))
End Sub

'-------------------------------------------------------------逻辑删除-----------------------------------------
'收件逻辑删除，置于回收站，入口字段DelR，可用于批量及单个删除
Sub Delinbox()
If CheckID(id) = False Then
	ErrCodes=ErrCodes+"<li>"+template.Strings(51)
else
	Dvbbs.execute("UPDATE Dv_Message Set DelR=1 WHERE incept='"&Dvbbs.MemberName&"' And id in ("&Dvbbs.checkStr(id)&")")
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(36))
	UPDATE_User_Msg(Dvbbs.membername)
End If 
End Sub

Sub AllDelinbox()
	Dvbbs.execute("UPDATE Dv_Message Set DelR=1 WHERE incept='"&Dvbbs.MemberName&"' And DelR=0")
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(36))
	UPDATE_User_Msg(Dvbbs.membername)
End Sub

'发件逻辑删除，置于回收站，入口字段DelS，可用于批量及单个删除
Sub Deloutbox()
If CheckID(id) = False Then
	ErrCodes=ErrCodes+"<meta http-equiv=refresh content=""2;URL="&Request.ServerVariables("HTTP_REFERER")&"""><li>"+template.Strings(51)&"2秒后自动返回上一页"
Else
	Dvbbs.execute("UPDATE Dv_Message Set DelS=1 WHERE sender='"&Dvbbs.MemberName&"' And issend=0 And id in ("&Dvbbs.checkStr(id)&")")
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(36))
	UPDATE_User_Msg(Dvbbs.membername)
End If
End Sub

Sub AllDeloutbox()
	Dvbbs.execute("UPDATE Dv_Message Set DelS=1 WHERE sender='"&Dvbbs.MemberName&"' And DelS=0 And issend=0")
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(36))
	UPDATE_User_Msg(Dvbbs.membername)
End Sub

'已发送逻辑删除，置于回收站，入口字段DelS，可用于批量及单个删除
'DelS：0未操作，1发送者删除，2发送者从回收站删除
Sub DelISsend()
If CheckID(id) = False Then
	ErrCodes=ErrCodes+"<meta http-equiv=refresh content=""2;URL="&Request.ServerVariables("HTTP_REFERER")&"""><li>"+template.Strings(51)&"两秒后自动返回"
Else 
	Dvbbs.execute("UPDATE Dv_Message Set DelS=1 WHERE sender='"&Dvbbs.MemberName&"' And issend=1 And id in ("&Dvbbs.checkStr(id)&")")
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(36))
	UPDATE_User_Msg(Dvbbs.membername)
End If
End Sub

'将已发送的短信移到回收站。
Sub AllDelIssend()
	Dvbbs.execute("UPDATE Dv_Message Set DelS=1 WHERE sender='"&Dvbbs.MemberName&"' And DelS=0 And issend=1")
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(36))
	UPDATE_User_Msg(Dvbbs.membername)
End Sub

'用户能完全删除收到信息和逻辑删除所发送信息，逻辑删除所发送信息设置入口字段DelS参数为2
sub Delrecycle()
If CheckID(id) = False Then
	ErrCodes=ErrCodes+"<meta http-equiv=refresh content=""2;URL="&Request.ServerVariables("HTTP_REFERER")&"""><li>"+template.Strings(51)
	Exit Sub
Else
	Dvbbs.execute("DelETE FROM Dv_Message WHERE incept='"&Dvbbs.MemberName&"' And DelR=1 And id in ("&Dvbbs.checkStr(id)&")")
	Dvbbs.execute("UPDATE Dv_Message Set DelS=2 WHERE sender='"&Dvbbs.MemberName&"' And DelS=1 And id in ("&Dvbbs.checkStr(id)&")")
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(37))
	UPDATE_User_Msg(dvbbs.membername)
End If
End Sub

'收信人回收站： incept=收信人 DelR=1
'发信人回收站： sender=收信人 DelS=2
'清空及删除回收站记录，将不在回收站的记录放到回收站内
sub AllDelrecycle()
	Dvbbs.execute("DelETE FROM Dv_Message WHERE incept='"&Dvbbs.MemberName&"' And DelR=1")	
	Dvbbs.execute("UPDATE Dv_Message Set DelS=2 WHERE sender='"&Dvbbs.MemberName&"' And DelS=1")
	'sucmsg=sucmsg+"<br/>"+"<li>删除短信息成功。删除的消息将不可恢复。"
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(37))
	UPDATE_User_Msg(Dvbbs.Membername)
End Sub

'删除的消息将置于您的回收站
Sub Delete()
If CheckID(id) = False Then
	ErrCodes=ErrCodes+"<meta http-equiv=refresh content=""2;URL="&Request.ServerVariables("HTTP_REFERER")&"""><li>"+template.Strings(51)
Else
	Dvbbs.execute("UPDATE Dv_Message Set DelR=1 WHERE incept='"&Dvbbs.MemberName&"' And id="&Dvbbs.checkStr(id))
	Dvbbs.execute("UPDATE Dv_Message Set DelS=1 WHERE sender='"&Dvbbs.MemberName&"' And id="&Dvbbs.checkStr(id))
	UPDATE_User_Msg(Dvbbs.membername)
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(36))
End If
End Sub

'-------------------------------------------------------------------------------------------------------------
'显示错误信息
Sub Showerr()
Dim Show_Errmsg
	If ErrCodes<>"" Then 
		Show_Errmsg=Dvbbs.mainhtml(14)
		ErrCodes=Replace(ErrCodes,"{$color}",Dvbbs.mainSetting(1))
		Show_Errmsg=Replace(Show_Errmsg,"{$color}",Dvbbs.mainSetting(1))
		Show_Errmsg=Replace(Show_Errmsg,"{$errtitle}",Dvbbs.Forum_Info(0)&"-"&Dvbbs.Stats)
		Show_Errmsg=Replace(Show_Errmsg,"{$action}",Dvbbs.Stats)
		Show_Errmsg=Replace(Show_Errmsg,"{$ErrString}",ErrCodes)
	End If
	Response.write Show_Errmsg
End Sub

'用户好友下拉名单
Function OPTION_Friend()
DIM i
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
Sql="Select F_friend From Dv_Friend Where (F_userid="&Dvbbs.userid&" or F_username='"& Dvbbs.CheckStr(Name) &"') And F_Mod=2"
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
Rs=Dvbbs.execute("SELECT Count(id) FROM Dv_Message WHERE flag=0 And issend=1 And DelR=0 And incept='"& Dvbbs.CheckStr(iusername) &"'")
    newincept=Rs(0)
	Set Rs=nothing
	If isnull(newincept) Then newincept=0
End Function

Function inceptid(stype,iusername)
	Set Rs=Dvbbs.execute("SELECT top 1 id,sender FROM Dv_Message WHERE flag=0 And issend=1 And DelR=0 And incept='"& Dvbbs.CheckStr(iusername) &"'")
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

Function Get_ForumCSS()
	Dim Sid
	sid = Request.Cookies("skin")("skinid_0")
	If Not IsNumeric(sid) Or sid = "" Then Sid=Application(Forum_CacheName & "_Dv_Setup")(17,0)
	Get_ForumCSS=Application(Forum_CacheName &"_Forum_CSS"&Sid)
End Function  

Function CheckID(CHECK_ID)
	Dim Delid,Fixid
	CheckID=True
	Delid=replace(CHECK_ID,"'","")
	Delid=replace(Delid,";","")
	Delid=replace(Delid,"--","")
	Delid=replace(Delid,")","")
	Fixid=replace(Delid,",","")
	Fixid=Trim(replace(fixid," ",""))
	If Delid="" or isnull(Delid) Then  CheckID=False
	If Not IsNumeric(fixid) Then CheckID=False
End Function

Function EncodeJS(str)
EncodeJS = Replace(Replace(Replace(Replace(Replace(str,chr(10),""),"\","\\"),"'","\'"),VbCrLf,"\n"),chr(13),"")
End Function

%>
