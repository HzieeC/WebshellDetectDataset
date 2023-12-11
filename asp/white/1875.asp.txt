<!--#include file="Conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/dv_clsother.asp"-->
<!--#include file="inc/chan_const.asp"-->
<!--#include file="inc/chkinput.asp"-->
<!--#include file="inc/Email_Cls.asp"-->
<!--#include file="inc/md5.asp"-->
<!--#include file="dv_dpo/cls_dvapi.asp"-->
<%
Dim ajaxPro
Dim ajaxpostwin
ajaxPro=CInt(Request.Form("ajaxPost"))
ajaxpostwin=Request.Form("ajaxpostwin")
'Response.Write(ajaxPro):Response.End()

If ajaxPro=1 Then
	ajaxPro=True
Else
	ajaxPro=False Rem 非ajax登陆
End If

Dim comeurl
Dim TruePassWord
'-----------------'owz
Dim EnterCount
If Request.Cookies("count")<>"" Then
	EnterCount=Request.Cookies("count")
Else
	EnterCount=Request.QueryString("count")'地址栏带参数
End If
'----------------------
session("flag")=empty
Dvbbs.LoadTemplates("login")
Dvbbs.stats=template.Strings(1)
Dvbbs.Nav()
Dvbbs.Head_var 0,0,template.Strings(0),"login.asp"
TruePassWord=Dvbbs.Createpass

Select Case request("action")
	Case "chk"
		Dvbbs_ChkLogin
		Dvbbs.Showerr()
	Case "redir"
		redir		
		Dvbbs.Showerr()		
	Case "save_redir_reg"
		call save_redir_reg()	
		Dvbbs.Showerr()
	Case Else
		Main
End Select
Dvbbs.ActiveOnline
Dvbbs.Footer()
Dvbbs.PageEnd()

Sub Main()
	Dim TempStr
	TempStr = template.html(0)
		If Dvbbs.forum_setting(79)="0" Then
		TempStr = Replace(TempStr,"{$getcode}","")
		Else 
		TempStr = Replace(TempStr,"{$getcode}",Replace(template.html(23),"{$codestr}",Dvbbs.GetCode()))
		End If 
	TempStr = Replace(TempStr,"{$rayuserlogin}",template.html(1))
	Dim Comeurl,tmpstr
	If Request("f")<>"" Then
		Comeurl=Request("f")
	ElseIf Request.ServerVariables("HTTP_REFERER")<>"" Then 
		tmpstr=split(Request.ServerVariables("HTTP_REFERER"),"/")
		Comeurl=tmpstr(UBound(tmpstr))
	Else
		If isUrlreWrite = 1 Then
			Comeurl="index.html"
		Else
			Comeurl="index.asp"
		End If
	End If

	If UBound(Split(Comeurl,"."))>=2 Or Trim(Comeurl)="" Then
		Comeurl = "index.asp"
	End If

	TempStr = Replace(TempStr,"{$comeurl}",Comeurl)
	Response.Write TempStr
	TempStr=""	
End Sub

Function Dvbbs_ChkLogin	
	Dim UserIP
	Dim username
	Dim userclass
	Dim password
	Dim article
	Dim usercookies
	Dim mobile
	Dim chrs,i
	UserIP=Dvbbs.UserTrueIP
	mobile=trim(Dvbbs.CheckStr(request("passport")))
    If Dvbbs.forum_setting(79)="1" Then			
        If mobile="" And Not Dvbbs.CodeIsTrue() Then
			    If  ajaxPro Then
				    If ajaxpostwin="1" Then 'modifty by reoaiq at 091022
						If Dvbbs.forum_setting(120)="1" Then 
						strString("语音验证码校验失败@@@@2")
						Else 
						strString("验证码校验失败@@@@0")
						End If
					else 
						If Dvbbs.forum_setting(120)="1" Then 
						strString("语音验证码校验失败@@@@2")
						Else 
						strString("验证码校验失败@@@@0")
						End If
					end if  
				Else 
				    If ajaxpostwin="1" Then 
						If Dvbbs.forum_setting(120)="1" Then 
						strString("语音验证码校验失败@@@@2")
						Else 
						strString("验证码校验失败@@@@0")
						End If
					Else 
                    Response.redirect "showerr.asp?ErrCodes=<li>验证码校验失败，请返回刷新页面后再输入验证码。&action=OtherErr"
					End If 
                End If 			
        End If
    End If
	
	If Request("username")="" Then
		If Request("passport")="" Then	
			If Not ajaxPro Then
				Dvbbs.AddErrCode(10)
			Else			
				strString("用户名不能为空@@@@0") 'o
			End If
		End If
	Else
		username=trim(Dvbbs.CheckStr(request("username")))
		If ajaxPro Then username = Dvbbs.CheckStr(unescape(username))
	End If
	If request("password")="" and mobile="" Then
		If Not ajaxPro Then
			Dvbbs.AddErrCode(11)
		Else 
			strString("密码不能为空!@@@@0") 'o
		End If
	Else
		password=md5(trim(Dvbbs.CheckStr(request("password"))),16)
		If Request("password") = "" Then password = ""
	End If

	If Dvbbs.ErrCodes<>"" Then Exit Function

	'-----------------------------------------------------------------以上注释(o)
	'系统整合
	'-----------------------------------------------------------------
	Dim DvApi_Obj,DvApi_SaveCookie,SysKey
	If DvApi_Enable Then
		Set DvApi_Obj = New DvApi
			'DvApi_Obj.NodeValue "syskey",SysKey,0,False
			DvApi_Obj.NodeValue "action","login",0,False
			DvApi_Obj.NodeValue "username",UserName,1,False
			Md5OLD = 1
			SysKey = Md5(DvApi_Obj.XmlNode("username")&DvApi_SysKey,16)
			Md5OLD = 0
			DvApi_Obj.NodeValue "syskey",SysKey,0,False
			DvApi_Obj.NodeValue "password",Request("password"),0,False
			DvApi_Obj.SendHttpData
			'strString(UserName&"---"&SysKey&"---"&Md5(Request("password"),16))
			If DvApi_Obj.Status = "1" Then
				If Not ajaxPro Then
					Response.Redirect "showerr.asp?ErrCodes="& DvApi_Obj.Message &"&action=OtherErr"
				Else
					strString(DvApi_Obj.Message &".@@@@0")
				End If
			Else
				DvApi_SaveCookie = DvApi_Obj.SetCookie(SysKey,UserName,Password,request("CookieDate"))
			End If
		Set DvApi_Obj = Nothing
	End If
	'-----------------------------------------------------------------

	usercookies=request("CookieDate")
	'判断更新cookies目录
	Dim cookies_path_s,cookies_path_d,cookies_path
	cookies_path_s=split(Request.ServerVariables("PATH_INFO"),"/")
	cookies_path_d=ubound(cookies_path_s)
	cookies_path="/"
	For i=1 to cookies_path_d-1
		If not (cookies_path_s(i)="upload" or cookies_path_s(i)="admin") Then cookies_path=cookies_path&cookies_path_s(i)&"/"
	Next
	If dvbbs.cookiepath<>cookies_path Then
		cookies_path=replace(cookies_path,"'","")
		Dvbbs.execute("update dv_setup set Forum_Cookiespath='"&cookies_path&"'")
		Dim setupData 
		Dvbbs.CacheData(26,0)=cookies_path
		Dvbbs.Name="setup"
		Dvbbs.value=Dvbbs.CacheData
	End If
	
	If ChkUserLogin(username,password,mobile,usercookies,1)=false Then
		Set chrs=Dvbbs.Execute("select Passport,IsChallenge from [Dv_User] where username='"&username&"' and IsChallenge=1")
		If chrs.eof and chrs.bof Then
			If Not ajaxPro Then
				Dvbbs.AddErrCode(12)
			Else 
				strString("本论坛不存在该用户名.@@@@0")'o
			End If
			Exit Function
		End If
		set chrs=nothing
	End If

	Dim comeurlname
	If instr(lcase(request("comeurl")),"reg.asp")>0 or instr(lcase(request("comeurl")),"login.asp")>0 or trim(request("comeurl"))="" or instr(lcase(request("comeurl")),"index.asp")>0 Or instr(lcase(request("comeurl")),"showerr.asp")>0 Then
		comeurlname=""
		If isUrlreWrite = 1 Then
			Comeurl="index.html"
		Else
			comeurl="index.asp"
		End If
	Else
		comeurl=request("comeurl")		
		comeurlname="<li><a href="&request("comeurl")&">"&request("comeurl")&"</a></li>"
	End If
	
	Dim TempStr
	TempStr = template.html(2)
	'-----------------------------------------------------------------
	'系统整合
	'-----------------------------------------------------------------
	If DvApi_Enable Then
		Response.Write DvApi_SaveCookie
		Response.Flush
	End If
	'-----------------------------------------------------------------
	TempStr = Replace(TempStr,"{$ray_logininfo}","")
	TempStr = Replace(TempStr,"{$comeurl}",comeurl)
	TempStr = Replace(TempStr,"{$comeurlinfo}",comeurlname)
	TempStr = Replace(TempStr,"{$forumname}",Dvbbs.Forum_Info(0))

	Session.Contents.Remove("xcount")

	If Not ajaxPro And DvApi_Enable Then'非ajax
		Response.Write TempStr
	ElseIf Not ajaxPro And Not DvApi_Enable Then
		Response.Redirect(comeurl)
	Else
		Response.Cookies("count")=""'o(清空ajax里写入的cookies)
		strString(comeurl&"@@@@1")'o
	End If 
End Function

Function strAnsi2Unicode(asContents)
	Dim len1,i,varchar,varasc
	strAnsi2Unicode = ""
	len1=LenB(asContents)
	If len1=0 Then Exit Function
	  For i=1 to len1
	  	varchar=MidB(asContents,i,1)
	  	varasc=AscB(varchar)
	  	If varasc > 127  Then
	  		If MidB(asContents,i+1,1)<>"" Then
	  			strAnsi2Unicode = strAnsi2Unicode & chr(ascw(midb(asContents,i+1,1) & varchar))
	  		End If
	  		i=i+1
	     Else
	     	strAnsi2Unicode = strAnsi2Unicode & Chr(varasc)
	     End If	
	  Next
End Function

Sub save_redir_reg()
	If Session("re_challenge_reg_temp")="" Then
		Dvbbs.AddErrCode(14)
		Exit Sub
	End If

	Dim username,sex,pass1,pass2,password,ErrCodes
	Dim useremail,face,width,height
	Dim oicq,sign,showRe,birthday
	Dim mailbody,sendmsg,rndnum,num1
	Dim quesion,answer,topic
	Dim userinfo,usersetting
	Dim userclass,UserIM
	Dim re_challenge_reg_temp
	Dim rs,sql,i,namebadword,SplitWords
	Dim t
	Dim StatUserID,UserSessionID
	Dim TempStr
	t = Request("t")
	If t = "" Or Not IsNumeric(t) Then t = 1
	t = Cint(t)
	If t <> 1 And t <> 2 Then t = 1
	re_challenge_reg_temp=split(Session("re_challenge_reg_temp"),"|||")

	If Request("name")="" or strLength(Request("name"))>Cint(Dvbbs.Forum_Setting(41)) or strLength(Request("name"))<Cint(Dvbbs.Forum_Setting(40)) Then
		Dvbbs.AddErrCode(17)
	Else
		username=Dvbbs.CheckStr(Trim(Request("name")))
	End If

	If Instr(username,"=")>0 or Instr(username,"%")>0 or Instr(username,chr(32))>0 or Instr(username,"?")>0 or Instr(username,"&")>0 or Instr(username,";")>0 or Instr(username,",")>0 or Instr(username,"'")>0 or Instr(username,",")>0 or Instr(username,chr(34))>0 or Instr(username,chr(9))>0 or Instr(username,"")>0 or Instr(username,"$")>0 Then
		Dvbbs.AddErrCode(19)
	End If

	If Request.form("psw")="" or len(Request.form("psw"))>10 or len(Request.form("psw"))<6 Then
		ErrCodes=ErrCodes+"<li>请输入您的密码，密码长度为6-10字节。"
	Else
		pass1=Request.form("psw")
	End If

	If Request.form("pswc")="" or strLength(Request.form("pswc"))>10 or len(Request.form("pswc"))<6 Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(13)
	Else
		pass2=Request.form("pswc")
	End If
	If pass1<>pass2 Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(29)
	Else
		password=md5(pass2,16)
	End If

	Dim RegSplitWords
	If Trim(Dvbbs.cachedata(1,0))<>"" Then
	RegSplitWords=split(Dvbbs.cachedata(1,0),"|||")(4)
	RegSplitWords=split(RegSplitWords,",")
		For i = 0 to ubound(RegSplitWords)
			If Trim(RegSplitWords(i))<>"" Then
				If instr(username,RegSplitWords(i))>0 Then
					Dvbbs.AddErrCode(19)
					Exit For
				End If
			End If 
		next
	End If 
	sex=1
	'password=md5(re_challenge_reg_temp(1),16)
	useremail=re_challenge_reg_temp(0) & "@dvbbs.net"
	showRe=1
	face="images/userface/image1.gif"
	width=32
	height=32

	If request.Form("birthyear")="" or request.form("birthmonth")="" or request.form("birthday")="" Then
		birthday=""
	Else
		birthday=trim(Request.Form("birthyear"))&"-"&trim(Request.Form("birthmonth"))&"-"&trim(Request.Form("birthday"))
		If not isdate(birthday) Then birthday=""
	End If

	userinfo=checkreal(request.Form("realname")) & "|||" & checkreal(request.Form("character")) & "|||" & checkreal(request.Form("personal")) & "|||" & checkreal(request.Form("country")) & "|||" & checkreal(request.Form("province")) & "|||" & checkreal(request.Form("city")) & "|||" & request.Form("shengxiao") & "|||" & request.Form("blood") & "|||" & request.Form("belief") & "|||" & request.Form("occupation") & "|||" & request.Form("marital") & "|||" & request.Form("education") & "|||" & checkreal(request.Form("college")) & "|||" & checkreal(request.Form("userphone")) & "|||" & checkreal(request.Form("address"))
	usersetting=request.Form("setuserinfo") & "|||" & request.Form("setusertrue") & "|||" & showRe

	If ErrCodes<>"" Then
		Response.Redirect "showerr.asp?ErrCodes="&ErrCodes&"&action=OtherErr"
		Exit Sub
	End If
	If Dvbbs.ErrCodes<>"" Then Exit Sub
	Dim titlepic,iUserGroupID
	set rs=Dvbbs.Execute("select usertitle,grouppic,UserGroupID from Dv_UserGroups where ParentGID=3 order by minarticle")
	userclass=rs(0)
	titlepic=rs(1)
	iUserGroupID=rs(2)
	UserIM = "||||||||||||||||||"
	set rs=Dvbbs.iCreateObject("adodb.recordset")
	sql="select * from [Dv_User] where username='"&username&"' or Passport='"&re_challenge_reg_temp(0)&"'"
	rs.open sql,conn,1,3
	If not rs.eof and not rs.bof Then
		Dvbbs.AddErrCode(21)
		'strString("此用户已经存在")'o
		Exit Sub
	Else
		rs.addnew
		rs("IsChallenge")=1
		rs("username")=username
		rs("userpassword")=password
		rs("TruePassWord")=TruePassWord
		rs("useremail")=useremail
		rs("userclass")=userclass
		rs("titlepic")=titlepic
		rs("Passport")=re_challenge_reg_temp(0)
		Rs("UserIM")=UserIM
		Rs("UserPost")=0
		Rs("usergroupid")=iUserGroupID
		rs("lockuser")=0
		Rs("Usersex")=sex
		rs("JoinDate")=NOW()
		rs("Userface")=replace(face,"'","")
		rs("UserWidth")=width
		rs("UserHeight")=height
		rs("UserLogins")=1
		Rs("lastlogin")=NOW()
		Rs("LastMsg")=Now()
		rs("userWealth")=Dvbbs.Forum_user(0)
		rs("userEP")=Dvbbs.Forum_user(5)
		rs("usercP")=Dvbbs.Forum_user(10)
		rs("userinfo")=userinfo
		rs("usersetting")=usersetting
		rs("UserFav")="陌生人,我的好友,黑名单"
		rs.update
		Dvbbs.Execute("update Dv_Setup set Forum_usernum=Forum_usernum+1,Forum_lastuser='"&username&"'")
	End If
	rs.close
	set rs=Dvbbs.Execute("select top 1 userid from [Dv_User] order by userid desc")
	dvbbs.userid=rs(0)
	set rs=nothing
	Dvbbs.ReloadSetupCache username,14
	Dvbbs.ReloadSetupCache (CLng(Dvbbs.CacheData(10,0))+1),10 

	If Dvbbs.Forum_Setting(47)=1 and Cint(Dvbbs.Forum_Setting(2))>0 Then
		'on error resume next
		'发送注册邮件
		Dim getpass
		topic=Replace(template.Strings(35),"{$Forumname}",Dvbbs.Forum_Info(0))
		mailbody = template.html(17)
		mailbody = Replace(mailbody,"{$username}",Dvbbs.HtmlEncode(username))
		mailbody = Replace(mailbody,"{$password}",password)
		mailbody = Replace(mailbody,"{$copyright}",Dvbbs.Forum_Copyright)
		mailbody = Replace(mailbody,"{$version}",Dvbbs.Forum_Version)
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
				If Cint(Dvbbs.Forum_Setting(23))=1 Then
					sendmsg=template.Strings(38)
				Else
					sendmsg=template.Strings(39)
				End If
			Else
				sendmsg=template.Strings(37)
			End If
		Else
			sendmsg=template.Strings(37)
		End If
		Set DvEmail = Nothing
		Dvbbs.ErrCodes=""
	Else
		sendmsg = template.Strings(36)
	End If

	If Dvbbs.Forum_Setting(46)=1 Then
		'发送注册短信
		Dim sender,title,body,UserMsg,MsgID
		sender=Dvbbs.Forum_info(0)
		title=Dvbbs.Forum_info(0)&"欢迎您的到来"

		body = template.html(18)
		body = Replace(body,"{$Forumname}",Dvbbs.Forum_Info(0))
		'response.write body
		sql="insert into dv_message(incept,sender,title,content,sendtime,flag,issend) values('"&username&"','"&sender&"','"&title&"','"&body&"',"&SqlNowString&",0,1)"
		Dvbbs.Execute(sql)
		Set rs=Dvbbs.execute("select top 1 ID from [Dv_message] order by ID desc")
		MsgID=rs(0)
		Rs.close:Set Rs=Nothing
		UserMsg="1||"& MsgID &"||"& sender
		Dvbbs.execute("UPDATE [Dv_User] Set UserMsg='"&Dvbbs.CheckStr(UserMsg)&"' WHERE UserID="&Dvbbs.userid)
	End If

	If cint(Dvbbs.Forum_Setting(25))=1 Then

	Else
		Response.Cookies(Dvbbs.Forum_sn).path=dvbbs.cookiepath
		Response.Cookies(Dvbbs.Forum_sn)("username")=""
		Response.Cookies(Dvbbs.Forum_sn)("password")=""
		Response.Cookies(Dvbbs.Forum_sn)("userclass")=""
		Response.Cookies(Dvbbs.Forum_sn)("userid")=""
		Response.Cookies(Dvbbs.Forum_sn)("userhidden")=""
		Response.Cookies(Dvbbs.Forum_sn)("usercookies")=""
		

		StatUserID = Dvbbs.checkStr(Trim(Request.Cookies(Dvbbs.Forum_sn)("StatUserID")))
		If IsNumeric(StatUserID) = 0 or StatUserID = "" Then
			StatUserID = Replace(Dvbbs.UserTrueIP,".","")
			UserSessionID = Replace(Startime,".","")
			If IsNumeric(StatUserID) = 0 or StatUserID = "" Then StatUserID = 0
			StatUserID = Ccur(StatUserID) + Ccur(UserSessionID)
		End If
		StatUserID = Ccur(StatUserID)
		Response.Cookies(Dvbbs.Forum_sn).path=Dvbbs.cookiepath
		Response.Cookies(Dvbbs.Forum_sn)("StatUserID") = StatUserID
 		Response.Cookies(Dvbbs.Forum_sn)("usercookies") = "0"
		Response.Cookies(Dvbbs.Forum_sn)("username") = username
		Response.Cookies(Dvbbs.Forum_sn)("password") = TruePassWord
		Response.Cookies(Dvbbs.Forum_sn)("userclass") = userclass
		Response.Cookies(Dvbbs.Forum_sn)("userid") = dvbbs.userid
		Response.Cookies(Dvbbs.Forum_sn)("userhidden") = 2
		Dvbbs.Execute("delete from dv_online where username='"&dvbbs.membername&"' Or id="&StatUserID&"")
	End If
	If ChkUserLogin(username,password,"",0,1) Then password=""

	TempStr = template.html(22)
	TempStr = Replace(TempStr,"{$ray_logininfo}","")
	TempStr = Replace(TempStr,"{$reuserpassword}",re_challenge_reg_temp(1))
'	TempStr = Replace(TempStr,"{$sendmsg}","<li>论坛通行证快速注册论坛用户成功！")
	TempStr = Replace(TempStr,"{$forumname}",Dvbbs.Forum_Info(0))
	Response.Write TempStr
	TempStr=""
	Session("re_challenge_reg_temp")=""
	Session("challengeUserID") = Empty
	Session("challengeWord_key") = Empty
End Sub

Function checkreal(v)
Dim w
If not isnull(v) Then
	w=replace(v,"|||","§§§")
	checkreal=w
End If
End Function


Rem ==========论坛登录函数=========
Rem 判断用户登录
Function ChkUserLogin(username,password,mobile,usercookies,ctype)

	Dim rsUser,article,userclass,titlepic,lastipinfo,lastipinfo_arr
	Dim userhidden,lastip,UserLastLogin
	Dim GroupID,ClassSql,FoundGrade
	Dim regname,iMyUserInfo
	Dim sql,sqlstr,OLDuserhidden

	FoundGrade=False
	lastip=Dvbbs.UserTrueIP
	userhidden=request.form("userhidden")
	If userhidden <> "1" Then userhidden=2
	ChkUserLogin=false
	If mobile<>"" Then
		sqlstr=" Passport='"&mobile&"'"
	Else
		sqlstr=" UserName='"&username&"'"
	End If
	Sql="Select UserID,UserName,UserPassword,UserEmail,UserPost,UserTopic,UserSex,UserFace,UserWidth,UserHeight,JoinDate,LastLogin,LastMsg,lastlogin as cometime , LastLogin as activetime,UserLogins,Lockuser,Userclass,UserGroupID,UserGroup,userWealth,userEP,userCP,UserPower,UserBirthday,UserLastIP,UserDel,UserIsBest,UserHidden,UserMsg,IsChallenge,UserMobile,TitlePic,UserTitle,TruePassWord,UserToday,UserMoney,UserTicket,FollowMsgID,Vip_StarTime,Vip_EndTime,userid as boardid,lastipinfo"
	Sql=Sql & " From [Dv_User] Where "&sqlstr&""
	set rsUser=Dvbbs.Execute(sql)
	If rsUser.eof and rsUser.bof Then
		'strString("本论坛不存在该用户名.@@@@0")
		ChkUserLogin=False
		Exit Function
	Else
		If rsUser("Lockuser") =1 Or rsUser("UserGroupID") =5 Then
			ChkUserLogin=False
			Exit Function
		Else
			If Trim(password)=Trim(rsUser("UserPassword")) Then
				ChkUserLogin=True
				Dvbbs.UserID=RsUser("UserID")
				RegName = RsUser("UserName")
				Article= RsUser("UserPost")
				UserLastLogin = RsUser("cometime")
				UserClass = RsUser("Userclass")		
				GroupID = RsUser("userGroupID")
				OLDuserhidden=RsUser("UserHidden")
				TitlePic = RsUser("UserTitle")
				lastipinfo=RsUser("lastipinfo")
				If Article < 0  Then Article=0
				Set Dvbbs.UserSession=Dvbbs.RecordsetToxml(rsUser,"userinfo","xml")
				Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@cometime").text=Now()
				Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@activetime").text=DateAdd("s",-3600,Now())
				Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@boardid").text=0
				Dvbbs.UserSession.documentElement.selectSingleNode("userinfo").attributes.setNamedItem(Dvbbs.UserSession.createNode(2,"isuserpermissionall","")).text=Dvbbs.FoundUserPermission_All()
				If OLDuserhidden <> CLng(userhidden) Then
					Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userhidden").text=userhidden
					Dvbbs.Execute("update Dv_user set userhidden="&userhidden&" where UserId=" & Dvbbs.UserID)
				End If
				Dim BS
				Set Bs=Dvbbs.GetBrowser()
				Dvbbs.UserSession.documentElement.appendChild(Bs.documentElement)
				If EnabledSession Then 	Session(Dvbbs.CacheName & "UserID")=Dvbbs.UserSession.xml
			Else
				If ajaxPro Then
					strString("用户名或者密码不正确.@@@@0")
				End If
				ChkUserLogin=False
				Exit Function
			End If
		End If
	End If
	If ChkUserLogin Then

	Rem 检查是否有新的群发短信	2010-3-4，Dv.Fish
	'Dvbbs.TyUserMsg username,1

	REM 判断用户组(等级)资料，当用户级别为跟随文章数增长则自动更新用户组(等级)
	REM 自动更新用户数据
	REM 如果属于系统或特殊或多属性组
	Set rsUser=Dvbbs.Execute("Select MinArticle,IsSetting,ParentGID,UserTitle,GroupPic From Dv_UserGroups Where UserGroupID="&GroupID)
	If Not (rsUser.Eof And rsUser.Bof) Then
		If rsUser(2)=1 Or rsUser(2)=2 Or rsUser(2)=4 Or rsUser(2)=5 Then
			'用户等级不按照文章升级，用户为系统或特殊或多属性组
			UserClass=rsUser(3)
			TitlePic=rsUser(4)
			FoundGrade=True
		End If
	End If
	If Not FoundGrade Then
		'如果不属于系统或特殊或多属性组,则将该用户属于注册用户组且按照其文章数自动更新其用户组(等级)
		Set rsUser=Dvbbs.Execute("Select Top 1 usertitle,GroupPic,UserGroupID From Dv_UserGroups Where ParentGID=3 And Minarticle<="&Article&" Order By MinArticle Desc,UserGroupID")
		If Not (rsUser.Eof And rsUser.Bof) Then
			UserClass=rsUser(0)
			TitlePic=rsUser(1)
			GroupID=rsUser(2)
			FoundGrade=True
		End If
	End If
	Set rsUser=Nothing
	If Not FoundGrade Then Response.Redirect "showerr.asp?ErrCodes=<li>系统没有找到您的注册用户组资料，请联系管理员进行修正。&action=OtherErr"
	'记录最后五个IP地址，同时更新dv_user中的lastipinfo字段 by 牛头
	Dim myuseripinfo
	myuseripinfo=dvbbs.UserTrueIP
	If myuseripinfo="" Or IsNull(myuseripinfo) Then myuseripinfo="127.0.0.1"
	If lastipinfo="" Or IsNull(lastipinfo) Then lastipinfo=myuseripinfo
	If lastipinfo="" Or IsNull(lastipinfo) Then lastipinfo="127.0.0.1"
	lastipinfo_arr=Split(lastipinfo,"|")
	If IsArray(lastipinfo_arr) Then
	    Select Case UBound(lastipinfo_arr)
		Case 0
		    If Trim(lastipinfo)<>Trim(myuseripinfo) Then
			    lastipinfo=lastipinfo&"|"&myuseripinfo
			End If
		Case 1
		    If InStr(lastipinfo,myuseripinfo)=0 Then lastipinfo=lastipinfo_arr(0)&"|"&lastipinfo_arr(1)&"|"&myuseripinfo
		Case 2
		    If InStr(lastipinfo,myuseripinfo)=0 Then lastipinfo=lastipinfo_arr(0)&"|"&lastipinfo_arr(1)&"|"&lastipinfo_arr(2)&"|"&myuseripinfo
		Case 3
			If InStr(lastipinfo,myuseripinfo)=0 Then lastipinfo=lastipinfo_arr(0)&"|"&lastipinfo_arr(1)&"|"&lastipinfo_arr(2)&"|"&lastipinfo_arr(3)&"|"&myuseripinfo
		Case Else
			lastipinfo=lastipinfo_arr(1)&"|"&lastipinfo_arr(2)&"|"&lastipinfo_arr(3)&"|"&lastipinfo_arr(4)&"|"&myuseripinfo
		End Select
	End If
	select case ctype
	case 1
		If Datediff("d",UserLastLogin,Now())=0 Then
			sql="update [Dv_User] set LastLogin="&SqlNowString&",UserLogins=UserLogins+1,UserLastIP='"&lastip&"',userclass='"&userclass&"',titlepic='"&titlepic&"',lastipinfo='"&lastipinfo&"',UserGroupID="&GroupID&",TruePassWord='"&TruePassWord&"' where userid="&dvbbs.UserID
		Else
			sql="update [Dv_User] set userWealth=userWealth+"&Dvbbs.Forum_user(4)&",userEP=userEP+"&Dvbbs.Forum_user(9)&",userCP=userCP+"&Dvbbs.Forum_user(14)&",LastLogin="&SqlNowString&",UserLogins=UserLogins+1,UserLastIP='"&lastip&"',userclass='"&userclass&"',lastipinfo='"&lastipinfo&"',titlepic='"&titlepic&"',UserGroupID="&GroupID&",TruePassWord='"&TruePassWord&"' where userid="&dvbbs.UserID
		End If
	case 2
		sql="update [Dv_User] set UserPost=UserPost+1,UserTopic=UserTopic+1,userWealth=userWealth+"&Dvbbs.Forum_user(1)&",userEP=userEP+"&Dvbbs.Forum_user(6)&",userCP=userCP+"&Dvbbs.Forum_user(11)&",LastLogin="&SqlNowString&",UserLastIP='"&lastip&"',userclass='"&userclass&"',titlepic='"&titlepic&"',UserGroupID="&GroupID&",TruePassWord='"&TruePassWord&"' where userid="&dvbbs.UserID
	case 3
		sql="update [Dv_User] set UserPost=UserPost+1,userWealth=userWealth+"&Dvbbs.Forum_user(2)&",userEP=userEP+"&Dvbbs.Forum_user(7)&",userCP=userCP+"&Dvbbs.Forum_user(12)&",LastLogin="&SqlNowString&",UserLastIP='"&lastip&"',userclass='"&userclass&"',titlepic='"&titlepic&"',UserGroupID="&GroupID&",TruePassWord='"&TruePassWord&"' where userid="&dvbbs.UserID
	end select
	Dvbbs.Execute(sql)
	Dim StatUserID,UserSessionID
		StatUserID = Dvbbs.checkStr(Trim(Request.Cookies(Dvbbs.Forum_sn)("StatUserID")))
		If IsNumeric(StatUserID) = 0 or StatUserID = "" Then
			StatUserID = Replace(Dvbbs.UserTrueIP,".","")
			UserSessionID = Replace(Startime,".","")
			If IsNumeric(StatUserID) = 0 or StatUserID = "" Then StatUserID = 0
			StatUserID = Ccur(StatUserID) + Ccur(UserSessionID)
		End If
	StatUserID = Ccur(StatUserID)
	Dvbbs.Execute("delete from dv_online where  id="&StatUserID&"")
	Response.Cookies(Dvbbs.Forum_sn).path = Dvbbs.cookiepath
	If trim(username)<>trim(Dvbbs.membername) Then
		Response.Cookies(Dvbbs.Forum_sn)("username")=""
		Response.Cookies(Dvbbs.Forum_sn)("password")=""
		Response.Cookies(Dvbbs.Forum_sn)("userclass")=""
		Response.Cookies(Dvbbs.Forum_sn)("userid")=""
		Response.Cookies(Dvbbs.Forum_sn)("userhidden")=""
		Response.Cookies(Dvbbs.Forum_sn)("usercookies")=""
		Dvbbs.Execute("delete from dv_online where username='"&Dvbbs.membername&"'")
	End If
	If isnull(usercookies) or usercookies="" Then usercookies="0"

	Select case usercookies
	case "0"
		Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
	case 1
   		Response.Cookies(Dvbbs.Forum_sn).Expires=Date+1
		Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
	case 2
		Response.Cookies(Dvbbs.Forum_sn).Expires=Date+31
		Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
	case 3
		Response.Cookies(Dvbbs.Forum_sn).Expires=Date+365
		Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
	End Select
	Response.Cookies(Dvbbs.Forum_sn)("username") = regname
	Response.Cookies(Dvbbs.Forum_sn)("userid") = Dvbbs.UserID
	Response.Cookies(Dvbbs.Forum_sn)("password") = TruePassWord
	Response.Cookies(Dvbbs.Forum_sn)("userclass") = userclass
	Response.Cookies(Dvbbs.Forum_sn)("userhidden") = userhidden

	rem 清除图片上传数的限制
	Response.Cookies("upNum")=0
	Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@truepassword").text= TruePassWord
	Dvbbs.Membername=Dvbbs.Checkstr(regname)
	Dvbbs.Memberclass=Dvbbs.Checkstr(userclass)
	Dvbbs.UserGroupID=GroupID
	End If
End Function

'-------------------------------------------------------------owz
Function strString(strValue)	
	Response.Clear
	'Response.ClearContent()
	'Response.ClearHeaders()
	Response.Write(strValue)
	Response.End
End Function

'Function BroswerType() rem 浏览是否为IE
	'Dim BrowserString 
	'BrowserString = Request.ServerVariables("HTTP_USER_AGENT") 
	'BrowserString = Lcase(BrowserString)
	'If Instr(BrowserString, "msie") <> 0 Then
	'	BroswerType=True
	'Else
	'	BroswerType=False
	'End If
'End Function

'----------------------------------------------------------
%>