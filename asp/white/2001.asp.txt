<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/Email_Cls.asp"-->
<!--#include file="inc/md5.asp"-->
<!--#include file="dv_dpo/cls_dvapi.asp"-->
<%
Dim topic,mailbody,sendmsg,useremail
Dim username,password,repassword,Rs,SQl
Dim answer
Dim SendMail
SendMail = False
Dvbbs.LoadTemplates("login")
If Request("action")="GetPass" Then
	Dvbbs.Stats=template.Strings(21)
Else
	Dvbbs.Stats=template.Strings(2)
End If
Dvbbs.Nav()
Dvbbs.Head_var 0,"",template.Strings(0),""
If Request("action")="step1" Then
	call step1()
ElseIf Request("action")="step2" Then
	call step2()
ElseIf Request("action")="step3" Then
	call step3()
ElseIf Request("action")="GetPass" Then
	GetPass()
Else
	Call main()
End If
Dvbbs.activeonline()
Dvbbs.footer()
Dvbbs.PageEnd()
Sub step1()
	If Dvbbs.chkpost=False Then 
   		showerr template.Strings(10)
		Exit Sub
	End If
	If request.Form("username")="" Then
		showerr template.Strings(6)
		Exit Sub
	Else
		username=replace(request("username"),"'","")
	End If
	If Dvbbs.forum_setting(81)="1"  Then
		If Not Dvbbs.CodeIsTrue() Then
			 Response.redirect "showerr.asp?ErrCodes=<li>验证码校验失败，请返回刷新页面后再输入验证码。&action=OtherErr"
		End If
	End If
	If Dvbbs.Forum_Setting(2)<>"0" Then
		Set Rs=Dvbbs.execute("Select UserQuesion,userAnswer,Username,Usergroupid from [Dv_user] where username='"&username&"'")
	Else
		Set Rs=Dvbbs.execute("Select UserQuesion,userAnswer,Username,Usergroupid from [Dv_user] where username='"&username&"' and UserGroupID>3")
	End If
	If rs.eof and rs.bof then
		showerr template.Strings(8)
		Exit Sub 
	ElseIf  rs(3) < 4 then
		showerr template.Strings(7)
		Exit Sub 
	Else 
		If  rs(0)="" or isnull(rs(0)) Then 
			showerr template.Strings(9)
			Exit Sub 
		Else
			Dim Tpl6
			Tpl6=Replace(template.html(6),"{$Quesion}",Rs(0))
			Tpl6=Replace(Tpl6,"{$username}",username)
			If Dvbbs.forum_setting(81)="0"  Then
				Tpl6=Replace(Tpl6,"{$getcode}","")
			Else
				Tpl6=Replace(Tpl6,"{$getcode}",Dvbbs.GetCode())
			End If 
			Response.Write Tpl6		
		End If
	End If
	Rs.Close  
	Set Rs=Nothing 
End Sub 

Sub step2()
	Dim answer,UserToday,UpUserToday,answerpass,Tpl7
	If request.Form("username")="" Then
		showerr template.Strings(6)
		Exit Sub
	Else
		username=replace(request("username"),"'","")
	End If
	If Dvbbs.chkpost=False Then 
   		showerr template.Strings(10)
		Exit Sub
	End If
	If request.form("answer")="" then
		showerr template.Strings(11)
		Exit Sub
	Else
		answer=md5(request("answer"),16)
	End If
	If Dvbbs.forum_setting(81)="1"  Then
		If Not Dvbbs.CodeIsTrue() Then
			 Response.redirect "showerr.asp?ErrCodes=<li>验证码校验失败，请返回刷新页面后再输入验证码。&action=OtherErr"
		End If
	End If
	sql="select useranswer,userquesion,useranswer,UserToday,UserID from [Dv_user] where username='"&username&"'"
	Set Rs=Dvbbs.Execute(sql)
	If Rs.EOF And Rs.BOF Then
		showerr template.Strings(12)
		Exit Sub
	Else
		If Ubound(Split(Rs(3),"|"))=4 Then
			UpUserToday = Rs(3)
		Else
			UpUserToday = "0|0|0|0|0"
		End If
		UserToday = Split(UpUserToday,"|")
		UserToday(3) = Cint(UserToday(3))
		If UserToday(3)>Cint(Dvbbs.forum_setting(84)) and Cint(Dvbbs.forum_setting(84))<>0 Then
			showerr "您取回密码次数已超出系统限制，请24小时后再使用取回密码功能！"
			Exit Sub
		End If
		answerpass =False
		If Rs(2)<>answer Then
			Md5OLD = 1
			answer=md5(request("answer"),16)
			If Rs(2)=answer Then
					answerpass =True
			End If
		Else
			answerpass =True
		End If
		If Not answerpass  Then
			If Cint(Dvbbs.Forum_setting(84))<>0 Then
				UserToday(3)=UserToday(3)+1
				UpUserToday=Clng(UserToday(0))&"|"&Clng(UserToday(1))&"|"&Clng(UserToday(2))&"|"&UserToday(3)&"|"&Clng(UserToday(4))
				Dvbbs.Execute("update [Dv_user] Set UserToday='"&UpUserToday&"' Where UserID="&Rs(4))
			End If
			showerr template.Strings(12)
			Exit Sub
		Else
			If Dvbbs.Forum_Setting(2)<>"0" Then
				Tpl7=Replace(template.html(7),"{$readme}",template.Strings(3))
			Else
				Tpl7=Replace(template.html(7),"{$readme}",template.Strings(4))
			End If
			Tpl7=Replace(Tpl7,"{$Quesion}",Rs(1))
			Tpl7=Replace(Tpl7,"{$answer}",request.form("answer"))
			Tpl7=Replace(Tpl7,"{$username}",username)
			Response.Write Tpl7
		End If
	End If
	Rs.Close 
	Set Rs=Nothing
End Sub

Sub step3()
	Dim answerOK
	If Dvbbs.chkpost=False Then 
   		showerr template.Strings(10)
		Exit Sub
	End If
	If request.Form("username")="" Then 
		showerr template.Strings(6)
		Exit Sub 
	Else 
		username=replace(request("username"),"'","")
	End If
	If request.Form("answer")="" Then
		showerr template.Strings(11)
		Exit Sub
	Else
		answer=md5(request("answer"),16)
	End  If 
	if request.Form("password")="" or Len(request("password"))>10 or len(request("password"))<6 then
		showerr template.Strings(13)
		Exit  Sub 
	ElseIf request.Form("repassword")="" Then
		showerr template.Strings(14)
		Exit Sub 
	ElseIf request.Form("password")<>request("repassword") Then
		showerr template.Strings(15)
		Exit Sub
	Else 
		password=md5(request.Form("password"),16)
	End If 
	set rs=Dvbbs.iCreateObject("adodb.recordset")
	sql="select userpassword,useremail,userquesion,userclass,UserGroupID,useranswer from [Dv_user] where username='"&username&"'"
	If Not IsObject(Conn) Then ConnectionDatabase
	rs.open sql,conn,1,3
	If rs.eof and rs.bof Then 
		showerr template.Strings(16)
		Exit Sub 
	Else
		answer=md5(request("answer"),16)
		If Rs("useranswer")=answer Then
			answerok=True
		Else
			 Md5OLD = 1
			 answer=md5(request("answer"),16)
			 Md5OLD = 0
			If Rs("useranswer")=answer Then
				answerok=True
			End If
		End If
		If answerok Then
				If Dvbbs.Forum_Setting(2)>0 Then
					repassword=request.form("password")
					answer=request.form("answer")
					password=rs("userpassword")
					useremail=rs("useremail")
					call sendusermail()
					If SendMail Then
						sendmsg=template.Strings(17)
					ElseIf Rs("UserGroupID")<4 Then
						sendmsg=template.Strings(18)
					Else
						rs("userpassword")=md5(repassword,16)
						Rs.Update
						sendmsg=template.Strings(19)
					End If
				Else
					Rs("userpassword")=password
					Rs.Update 
				End If
				Dim Tpl8
				Tpl8=Replace(template.html(8),"{$Quesion}",Rs(2))
				Tpl8=Replace(Tpl8,"{$answer}",request.form("answer"))
				Tpl8=Replace(Tpl8,"{$password}",request.form("password"))
				If Dvbbs.Forum_Setting(2)<>"0" Then
					Tpl8=Replace(Tpl8,"{$readme}",sendmsg)
				Else
					Tpl8=Replace(Tpl8,"{$readme}",template.Strings(5))
				End If 
				Response.Write Tpl8
				'-----------------------------------------------------------------
				'系统整合
				'-----------------------------------------------------------------
				Dim DvApi_Obj,DvApi_SaveCookie,SysKey
				If DvApi_Enable Then
					'SysKey = Md5(DvApi_SysKey&UserName,16)
					Set DvApi_Obj = New DvApi
						'DvApi_Obj.NodeValue "syskey",SysKey,0,False
						DvApi_Obj.NodeValue "action","updateuser",0,False
						DvApi_Obj.NodeValue "username",UserName,1,False
						Md5OLD = 1
						SysKey = Md5(DvApi_Obj.XmlNode("username")&DvApi_SysKey,16)
						Md5OLD = 0
						DvApi_Obj.NodeValue "syskey",SysKey,0,False
						DvApi_Obj.NodeValue "password",Request.Form("password"),0,False
						DvApi_Obj.SendHttpData
					Set DvApi_Obj = Nothing
				End If
				'-----------------------------------------------------------------
		Else
				showerr template.Strings(16)
		End If
End If
	Rs.Close
	Set Rs=Nothing 
End Sub 

Sub  main()
	Dim Tpl5
	If Dvbbs.forum_setting(81)="0"  Then
		Tpl5=Replace(template.html(5),"{$getcode}","")
	Else
		Tpl5=Replace(template.html(5),"{$getcode}",Dvbbs.GetCode())
	End If 
	Response.Write Tpl5
End  Sub 
Sub showerr(errmsg)
	Dim Tpl9
	Tpl9=Replace(template.html(9),"{$Errmsg}",errmsg)
	Response.Write Tpl9
End Sub 
Sub sendusermail()
	'on error resume Next
	Dim activepassurl,Tpl10,Tpl12
	activepassurl=Dvbbs.Get_ScriptNameUrl()&"lostpass.asp?action=GetPass&username="&Dvbbs.htmlencode(username)&"&pass="&password&"&repass="&repassword&"&answer="&answer
	Tpl12=Replace(template.html(12),"{$Forumname}",Dvbbs.Forum_info(0))
	Topic = Template.Strings(21)
	Tpl10=Replace(template.html(10),"{$username}",Dvbbs.htmlencode(username))
	Tpl10=Replace(Tpl10,"{$Copyright}",Dvbbs.Forum_Copyright)
	Tpl10=Replace(Tpl10,"{$activepassurl}","<a href="&activepassurl&">"&activepassurl&"</a>")
	Tpl10=Replace(Tpl10,"{$Version}","Dvbbs"&Dvbbs.Forum_Version)
	mailbody=Tpl10

	If Cint(Dvbbs.Forum_Setting(2))>0 Then
		Dim DvEmail
		Set DvEmail = New Dv_SendMail
		DvEmail.SendObject = Cint(Dvbbs.Forum_Setting(2))	'设置选取组件 1=Jmail,2=Cdonts,3=Aspemail
		DvEmail.ServerLoginName = Dvbbs.Forum_info(12)	'您的邮件服务器登录名
		DvEmail.ServerLoginPass = Dvbbs.Forum_info(13)	'登录密码
		DvEmail.SendSMTP = Dvbbs.Forum_info(4)			'SMTP地址
		DvEmail.SendFromEmail = Dvbbs.Forum_info(5)		'发送来源地址
		DvEmail.SendFromName = Dvbbs.Forum_info(0)		'发送人信息
		If DvEmail.ErrCode = 0 Then
			DvEmail.SendMail useremail,topic,mailbody	'执行发送邮件
			If DvEmail.Count>0 Then
				SendMail = True
				sendmsg=template.Strings(20)&"<a href="&activepassurl&"><B>"&template.Strings(21)&"</B></a>"
			Else
				sendmsg=template.Strings(20)&"<a href="&activepassurl&"><B>"&template.Strings(21)&"</B></a>"
			End If
		Else
			sendmsg=template.Strings(20)&"<a href="&activepassurl&"><B>"&template.Strings(21)&"</B></a>"
		End If
		Set DvEmail = Nothing
	Else
		sendmsg=template.Strings(20)&"<a href="&activepassurl&"><B>"&template.Strings(21)&"</B></a>"
	End If
End Sub

Sub GetPass()
	Dim username
	Dim password
	Dim repassword
	Dim answer
	Dim Rs,SQL
	If request("username")="" or request("pass")="" or request("repass")="" or request("answer")="" then
		showerr template.Strings(22)
	Else 
		username=Dvbbs.checkStr(request("username"))
		password=Dvbbs.checkStr(request("pass"))
		repassword=md5(Dvbbs.checkStr(request("repass")),16)
		answer=md5(request("answer"),16)
		Sql="select userpassword,userclass,UserGroupID,useranswer from [Dv_user] where username='"&username&"'"
		Set Rs=Dvbbs.iCreateObject("Adodb.Recordset")
		If Not IsObject(Conn) Then ConnectionDatabase
		Rs.Open Sql,Conn,1,3
		If Rs.Eof And Rs.Bof Then 
			showerr template.Strings(23) 
			Exit Sub 
		Else 
			If Rs("usergroupid")<4 Then
				showerr template.Strings(7) 
				Exit Sub
			ElseIf Rs(0)=password And Rs(3)=answer Then
				Rs("userpassword")=repassword
				Rs.Update
				Response.Write template.html(11)
				'-----------------------------------------------------------------
				'系统整合
				'-----------------------------------------------------------------
				Dim DvApi_Obj,DvApi_SaveCookie,SysKey
				If DvApi_Enable Then
					Set DvApi_Obj = New DvApi
						'DvApi_Obj.NodeValue "syskey",SysKey,0,False
						DvApi_Obj.NodeValue "action","updateuser",0,False
						DvApi_Obj.NodeValue "username",UserName,1,False
						Md5OLD = 1
						SysKey = Md5(DvApi_Obj.XmlNode("username")&DvApi_SysKey,16)
						Md5OLD = 0
						DvApi_Obj.NodeValue "syskey",SysKey,0,False
						DvApi_Obj.NodeValue "password",Request("repass"),0,False
						DvApi_Obj.SendHttpData
					Set DvApi_Obj = Nothing
				End If
				'-----------------------------------------------------------------
				Rs.Close
				Set Rs=Nothing 
			Else
				showerr template.Strings(23) 
				Exit Sub
			End If 
		End If
	End If
End Sub
%>