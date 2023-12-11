<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/md5.asp"-->
<!--#include file="inc/chkinput.asp"-->
<!--#include file="dv_dpo/cls_dvapi.asp"-->
<%
Dvbbs.LoadTemplates("Usermanager")
If Request("t")="1" Then
	Dvbbs.Stats=Dvbbs.MemberName&template.Strings(2)
Else
	Dvbbs.Stats=Dvbbs.MemberName&template.Strings(3)
End If

Dvbbs.Nav()
Dvbbs.Head_var 0,0,template.Strings(0),"Usermanager.asp"
Dim ErrCodes
Dim psw,password,oldpassword,quesion,answer,Usercookies

If Dvbbs.Userid=0 Then
	Dvbbs.AddErrCode(6)
End If
If Cint(Dvbbs.GroupSetting(16))=0 Then
	Dvbbs.AddErrCode(28)
End If
Dvbbs.Showerr()
Response.write template.html(0)

If Request("t")="1" Then
	Psw_Main()
Else
	Main()
End If

Dvbbs.ActiveOnline()
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub Main()
	If Request("action")="updat" Then
		Call update()
		If ErrCodes<>"" Then Response.redirect "showerr.asp?ErrCodes="&ErrCodes&"&action=OtherErr"
		Dvbbs.Showerr()
		Dvbbs.Dvbbs_Suc("<li>"+template.Strings(26))
	Else
		Call Userinfo()
		Dvbbs.Showerr()
	End If
End Sub

Sub Psw_Main()
	If Request("action")="updat" Then
		Call Psw_Update()
		If ErrCodes<>"" Then Response.redirect "showerr.asp?ErrCodes="&ErrCodes&"&action=OtherErr"
		Dvbbs.Showerr()
		Dvbbs.Dvbbs_Suc("<li>"+template.Strings(26))
	Else
		Call Psw_Userinfo()
		Dvbbs.Showerr()
	End If
End Sub

Sub userinfo()
	Dim Rs,Sql,tempstr,userim
	tempstr=template.html(10)
	sql="Select Userid,UserEmail,UserIM from [Dv_User] where Userid="&Dvbbs.Userid
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.eof And Rs.bof Then
		Dvbbs.AddErrCode(32)
		Exit Sub
	Else
		tempstr=Replace(tempstr,"{$user_id}",Rs(0))
		tempstr=Replace(tempstr,"{$user_email}",Rs(1)&"")
		If rs(2)="" or isnull(rs(2)) Then
			tempstr=Replace(tempstr,"{$user_homepage}","")
			tempstr=Replace(tempstr,"{$user_oicq}","")
			tempstr=Replace(tempstr,"{$user_icq}","")
			tempstr=Replace(tempstr,"{$user_Msn}","")
			tempstr=Replace(tempstr,"{$user_Yahoo}","")
			tempstr=Replace(tempstr,"{$user_Aim}","")
			tempstr=Replace(tempstr,"{$user_UC}","")
		Else
			userim=split(rs(2),"|||")
			tempstr=Replace(tempstr,"{$user_homepage}",userim(0))
			tempstr=Replace(tempstr,"{$user_oicq}",userim(1))
			tempstr=Replace(tempstr,"{$user_icq}",userim(2))
			tempstr=Replace(tempstr,"{$user_Msn}",userim(3))
			tempstr=Replace(tempstr,"{$user_Aim}",userim(4))
			tempstr=Replace(tempstr,"{$user_Yahoo}",userim(5))
			tempstr=Replace(tempstr,"{$user_UC}",userim(6))
		End If
		Response.write tempstr
	End If
	Rs.Close:Set Rs =Nothing
End sub

Sub update()
	Dim Rs,Sql
	Dim Email,NewUserIM
	Dim HomePage
	If Dvbbs.chkpost=False Then
		Dvbbs.AddErrCode(16)
		Exit Sub
	End If
	Dim userpassword
	userpassword=Request.form("password")
	If userpassword="" Then
		Dvbbs.AddErrCode(11)
		Exit Sub
	Else
		userpassword=md5(userpassword,16)
	End If
	'校验密码，
	SQL="Select userpassword from dv_user where userid="&Dvbbs.UserID&""

	Set Rs=Dvbbs.Execute(SQL)
	If Not Rs.eof Then
		If Rs(0)<> userpassword Then
			Response.redirect "showerr.asp?ErrCodes=您输入的密码错误&action=OtherErr"
		End If
	Else
		Response.redirect "showerr.asp?ErrCodes=您输入的密码错误&action=OtherErr"
	End If
	Set Rs=Nothing
	If IsValidEmail(Request.form("Email"))=false Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(31)		'Dvbbs.AddErrmsg "您的Email有错误。"
		Exit Sub
	Else
		If Not IsNull(Dvbbs.forum_setting(52)) And Dvbbs.forum_setting(52)<>"" And Dvbbs.forum_setting(52)<>"0" Then
			Dim SplitUserEmail,i
			SplitUserEmail=split(Dvbbs.forum_setting(52),"|")
			For i=0 to ubound(SplitUserEmail)
				If instr(Request.form("email"),SplitUserEmail(i))>0 Then
					ErrCodes=ErrCodes+"<li>"+template.Strings(32)		'Dvbbs.AddErrmsg "您填写的Email地址含有系统禁止字符。"
					Exit Sub
				End If
			Next
		End If
		Email=Dvbbs.checkstr(Request.form("Email"))
	End If
	If Trim(Request.Form("Oicq")) <> "" Then
		If Not IsNumeric(Trim(Request.form("Oicq"))) or Len(Trim(Request.Form("Oicq"))) > 12 Then
			Dvbbs.AddErrCode(18)
			Exit Sub
		End If
	End If
	If Trim(Request.Form("Icq")) <> "" Then
		If Not IsNumeric(Trim(Request.Form("Icq"))) Or Len(Trim(Request.Form("Icq"))) > 12 Then
			Dvbbs.AddErrCode(18)
			Exit Sub
		End If
	End If
	'主页加http://开头 2004-10-7 Dv.Yz
	HomePage = Trim(Request.Form("homepage"))
	If Not (Left(HomePage, 7) = "http://" Or HomePage = "") Then HomePage = "http://" & HomePage
	'HomePage,UserOicq,UserIcq,UserMsn,UserAim,UserYahoo,UserUC
	NewUserIM = checkreal(HomePage) & "|||" & checkreal(Trim(Request.Form("Oicq"))) & "|||" & checkreal(Trim(Request.Form("Icq"))) & "|||" & checkreal(Request.Form("Msn")) & "|||" & checkreal(Request.Form("UserAim")) & "|||" & checkreal(Request.Form("Yahoo")) & "|||" & checkreal(Request.Form("UC"))

	NewUserIM = Dvbbs.Checkstr(Server.Htmlencode(NewUserIM))
	'update data
	Sql = "UPDATE [Dv_User] SET UserEmail = '" & Email & "', UserIM = '" & NewUserIM & "' WHERE Userid = " & Dvbbs.Userid & ""
	Set Rs = Dvbbs.Execute(Sql)
End Sub

Function checkreal(v)
	Dim w
	If not isnull(v) Then
		w=replace(v,"|","")
		checkreal=w
	End If
End Function

Sub Psw_Userinfo()
	If Dvbbs.chkpost=False Then
		Dvbbs.AddErrCode(16)
		Exit Sub
	End If
	Dim Rs,Sql,tempstr
	tempstr=template.html(9)
	Sql="Select Userid,UserAnswer,UserQuesion from [Dv_User] where Userid="&Dvbbs.Userid
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.eof And Rs.bof Then
		Dvbbs.AddErrCode(32)
		Exit Sub
	Else
		tempstr=Replace(tempstr,"{$user_id}",Rs(0))
		tempstr=Replace(tempstr,"{$user_answer}","")
		'tempstr=Replace(tempstr,"{$user_answer}",Rs(1) & "")'密码答案
		'tempstr=Replace(tempstr,"{$user_quesion}",Rs(2) & "")'密码问题不显示
		tempstr=Replace(tempstr,"{$user_quesion}","")
		tempstr=Replace(tempstr,"{$color}",Dvbbs.mainsetting(1))
		Response.write tempstr
	End If
	Rs.Close:Set Rs=Nothing
End Sub

Sub Psw_Update()
	Dim Rs,Sql
	Sql="Select Userpassword from [Dv_User] where Userid="&Dvbbs.Userid
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.Eof And Rs.Bof Then
		Dvbbs.AddErrCode(32)
	Else
		If Request.Form("oldpsw")="" Then
	  		ErrCodes=ErrCodes+"<li>"+template.Strings(27)'Dvbbs.AddErrMsg "请输入您的旧密码,才能完成修改。"
		ElseIf md5(trim(Request.Form("oldpsw")),16)<>trim(RS("Userpassword")) then
	  		ErrCodes=ErrCodes+"<li>"+template.Strings(28)'Dvbbs.AddErrMsg "输入的旧密码错误，请重新输入。"
		Else
			oldpassword=Request.Form("oldpsw")
		End If
		If Request.Form("psw")<>"" Then
			password=md5(Request.Form("psw"),16)
		Else
			password=RS("Userpassword")
		End If

        If Request.Form("quesion")="" And Request.Form("answer")="" Then
            quesion=""
            answer=""
        Else
            If Request.Form("quesion")="" Then
                ErrCodes=ErrCodes+"<li>密码问题和答案必须同时为空，或同时填写！</li>"
            Else
                quesion=Request.Form("quesion")
                If quesion <> Dvbbs.CheckStr(quesion) Then
                    ErrCodes=ErrCodes+"<li>您的问题中含有非法字符</li>"
                End If
            End If

            If Request.Form("answer")<>"" Then
                answer=md5(Request.Form("answer"),16)
            Else
                ErrCodes=ErrCodes+"<li>密码问题和答案必须同时为空，或同时填写！</li>"
            End If
        End If
	End If

	If ErrCodes<>"" Then Exit sub
	Dvbbs.Showerr()
	'-----------------------------------------------------------------
	'系统整合
	'-----------------------------------------------------------------
	Dim DvApi_Obj,DvApi_SaveCookie,SysKey
	If DvApi_Enable Then
		'SysKey = Md5(DvApi_SysKey&Dvbbs.MemberName,16)
		Set DvApi_Obj = New DvApi
			DvApi_Obj.NodeValue "syskey",SysKey,0,False
			DvApi_Obj.NodeValue "action","update",0,False
			DvApi_Obj.NodeValue "username",Dvbbs.MemberName,1,False
			Md5OLD = 1
			SysKey = Md5(DvApi_Obj.XmlNode("username")&DvApi_SysKey,16)
			Md5OLD = 0
			DvApi_Obj.NodeValue "syskey",SysKey,0,False
			DvApi_Obj.NodeValue "password",Request.Form("psw"),1,False
			If Request.Form("answer")<>Request.Form("oldanswer") and Request.Form("answer")<>"" Then
				DvApi_Obj.NodeValue "answer",Request.Form("answer"),1,False
			End If
			DvApi_Obj.NodeValue "question",Request.Form("quesion"),1,False
			DvApi_Obj.SendHttpData
			If DvApi_Obj.Status = "1" Then
				Response.redirect "showerr.asp?ErrCodes="& DvApi_Obj.Message &"&action=OtherErr"
			End If
		Set DvApi_Obj = Nothing
	End If
	'-----------------------------------------------------------------

	Set Rs=Dvbbs.iCreateObject("Adodb.Recordset")
	Sql="Select * from [Dv_User] where Userid="&Dvbbs.Userid
	Rs.Open Sql,Conn,1,3
	If Rs.Eof And Rs.Bof Then
		Dvbbs.AddErrCode(32)
		Exit Sub
	Else
		'If Not Dvbbs.FoundIsChallenge Then
		Rs("Userpassword")=password
        If quesion<>"" Then'如果问题和答案为空则不修改
		    Rs("UserQuesion")=quesion
        End If
		If answer<>"" Then
            Rs("UserAnswer")=answer
        End If
		Rs.Update
	End If
	Rs.Close:set Rs=Nothing


End Sub
%>