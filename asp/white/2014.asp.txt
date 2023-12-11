<!--#include file="Conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/dv_clsother.asp"-->
<!--#include file="inc/chan_const.asp"-->
<!--#include file="inc/chkinput.asp"-->
<!--#include file="inc/Email_Cls.asp"-->
<!--#include file="inc/md5.asp"-->
<!--#include file="dv_dpo/Api_Config.asp"-->
<%
Dim XMLDom,XmlDoc,Node,Status,Messenge
Dim UserName,Act,appid
Status = 1
Messenge = ""
If Not DvApi_Enable Then Response.end
If Request.QueryString<>"" Then
	SaveUserCookie()
Else
	Set XmlDoc = Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument" & MsxmlVersion)
	XmlDoc.ASYNC = False
	If Not XmlDoc.LOAD(Request) Then
		Status = 1
		Messenge = "数据非法，操作中止！"
		appid = "未知"
	Else
		If Not (XmlDoc.documentElement.selectSingleNode("userip") is nothing) Then
			Dvbbs.UserTrueIP = Dvbbs.CheckStr(XmlDoc.documentElement.selectSingleNode("userip").text)
		End If
		If CheckPost() Then
			Select Case Act
				Case "checkname"
					Checkname()
				Case "reguser"
					Reguser()
				Case "login"
					UesrLogin()
				Case "logout"
					LogoutUser()
				Case "update"
					UpdateUser()
				Case "delete"
					Deleteuser()
				Case "lock"
					Lockuser()
				Case "getinfo"
					GetUserinfo()
			End Select
		End If
	End If
	ReponseData()
	Set XmlDoc = Nothing
End If
Dvbbs.PageEnd()

Sub ReponseData()
	'XmlDoc.loadxml "<root><appid>powereasy</appid><status>0</status><body><message/><email/><question/><answer/><savecookie/><truename/><gender/><birthday/><qq/><msn/><mobile/><telephone/><address/><zipcode/><homepage/><userip/><jointime/><experience/><ticket/><valuation/><balance/><posts/><userstatus/></body></root>"
	If Act <> "getinfo" Then
		XmlDoc.loadxml "<root><appid>powereasy</appid><status>0</status><body><message/></body></root>"
	End If
	XmlDoc.documentElement.selectSingleNode("appid").text = "Dvbbs"
	XmlDoc.documentElement.selectSingleNode("status").text = status
	XmlDoc.documentElement.selectSingleNode("body/message").text = ""
	Set Node = XmlDoc.createCDATASection(Replace(Messenge,"]]>","]]&gt;"))
	XmlDoc.documentElement.selectSingleNode("body/message").appendChild(Node)
	Response.Clear
	Response.ContentType="text/xml"
	Response.CharSet="gb2312"
	Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"&vbNewLine
	Response.Write XmlDoc.documentElement.XML
End Sub


Function CheckPost()
	CheckPost = False
	Dim Syskey
	If XmlDoc.documentElement.selectSingleNode("action") is Nothing or XmlDoc.documentElement.selectSingleNode("syskey") is Nothing or XmlDoc.documentElement.selectSingleNode("username")  is Nothing Then
		Status = 1
		Messenge = Messenge & "<li>非法请求。"
		Exit Function
	End If

	UserName = Dvbbs.checkstr(Trim(XmlDoc.documentElement.selectSingleNode("username").text))
	Syskey = XmlDoc.documentElement.selectSingleNode("syskey").text
	Act = XmlDoc.documentElement.selectSingleNode("action").text
	Appid = Dvbbs.CheckStr(XmlDoc.documentElement.selectSingleNode("appid").text)
	
	Dim NewMd5,OldMd5
	NewMd5 = Md5(UserName&DvApi_SysKey,16)
	Md5OLD = 1
	OldMd5 = Md5(UserName&DvApi_SysKey,16)
	Md5OLD = 0
	If DvApi_SysKey = "API_TEST" or Syskey = "Syskey" Then
		Status = 1
		Messenge = Messenge & "<li>默认非法请求。"
		Exit Function
	End If
	If Syskey=NewMd5 or Syskey=OldMd5 Then
		CheckPost = True
	Else
		Status = 1
		Messenge = Messenge & "<li>请求数据验证不通过，请与管理员联系。"
	End If
End Function


Sub GetUserinfo()
	Dim Rs,Sql
	Dim Userinfo,UserIM
	
	XmlDoc.loadxml "<root><appid>powereasy</appid><status>0</status><body><message/><email/><question/><answer/><savecookie/><truename/><gender/><birthday/><qq/><msn/><mobile/><telephone/><address/><zipcode/><homepage/><userip/><jointime/><experience/><ticket/><valuation/><balance/><posts/><userstatus/></body></root>"
	
	Sql = "Select Top 1 * From Dv_User Where UserName='"&Dvbbs.Checkstr(UserName)&"'"
	Set Rs = Dvbbs.Execute(Sql)
	If Not Rs.Eof And Not Rs.Bof Then
		Userinfo = Split(Rs("UserInfo"),"|||")
		UserIM = Split(Rs("UserIM"),"|||")
		XmlDoc.documentElement.selectSingleNode("body/email").text = Rs("UserEmail")&""
		XmlDoc.documentElement.selectSingleNode("body/question").text = Rs("UserQuesion")&""
		XmlDoc.documentElement.selectSingleNode("body/answer").text = Rs("UserAnswer")&""
		'XmlDoc.documentElement.selectSingleNode("body/savecookie").text = ""
		XmlDoc.documentElement.selectSingleNode("body/gender").text = Rs("Usersex")&""
		XmlDoc.documentElement.selectSingleNode("body/birthday").text = Rs("UserBirthday")&""
		XmlDoc.documentElement.selectSingleNode("body/mobile").text = Rs("UserMobile")&""
		'XmlDoc.documentElement.selectSingleNode("body/zipcode").text = ""
		XmlDoc.documentElement.selectSingleNode("body/userip").text = Rs("UserLastIP")&""
		XmlDoc.documentElement.selectSingleNode("body/jointime").text = Rs("JoinDate")&""
		XmlDoc.documentElement.selectSingleNode("body/experience").text = Rs("userEP")&""
		XmlDoc.documentElement.selectSingleNode("body/ticket").text = Rs("UserTicket")&""
		XmlDoc.documentElement.selectSingleNode("body/valuation").text = Rs("userCP")&""
		XmlDoc.documentElement.selectSingleNode("body/balance").text = Rs("UserMoney")&""
		XmlDoc.documentElement.selectSingleNode("body/posts").text = Rs("UserPost")&""
		XmlDoc.documentElement.selectSingleNode("body/userstatus").text = Rs("Lockuser")&""
		XmlDoc.documentElement.selectSingleNode("body/homepage").text = UserIM(0)
		XmlDoc.documentElement.selectSingleNode("body/qq").text = UserIM(1)
		XmlDoc.documentElement.selectSingleNode("body/msn").text = UserIM(3)
		XmlDoc.documentElement.selectSingleNode("body/truename").text = Userinfo(0)
		XmlDoc.documentElement.selectSingleNode("body/telephone").text = Userinfo(13)
		XmlDoc.documentElement.selectSingleNode("body/address").text = Userinfo(14)
		Status = 0
		Messenge = Messenge & "<li>读取用户资料成功。"
	Else
		Status = 1
		Messenge = Messenge & "<li>该用户不存在。"
	End If
	Rs.Close
	Set Rs = Nothing
End Sub


Sub Deleteuser()
	Dim D_Users,i,UserID,AllUserID
	Dim Rs
	D_Users = Split(UserName,",")
	AllUserID = ""
	For i=0 To UBound(D_Users)
		Set Rs=Dvbbs.Execute("Select UserName,UserID from [dv_User] where UserName='"&Dvbbs.Checkstr(D_Users(i))&"'")
		If not (rs.eof and rs.bof) then
			AllUserID = AllUserID & Rs(1) & ","
			Dvbbs.Execute("update dv_message set delR=1 where incept='"&Dvbbs.Checkstr(rs(0))&"' and delR=0")
			Dvbbs.Execute("update dv_message set delS=1 where sender='"&Dvbbs.Checkstr(rs(0))&"' and delS=0 and issend=0")
			Dvbbs.Execute("update dv_message set delS=1 where sender='"&Dvbbs.Checkstr(rs(0))&"' and delS=0 and issend=1")
			Dvbbs.Execute("delete from dv_message where incept='"&Dvbbs.Checkstr(rs(0))&"' and delR=1") 
			Dvbbs.Execute("update dv_message set delS=2 where sender='"&Dvbbs.Checkstr(rs(0))&"' and delS=1")
			Dvbbs.Execute("delete from dv_friend where F_username='"&Dvbbs.Checkstr(rs(0))&"'") 
			Dvbbs.Execute("delete from dv_bookmark where username='"&Dvbbs.Checkstr(rs(0))&"'")
			Messenge = Messenge & "<li>用户（"&D_Users(i)&"）删除成功。"
		End If
	Next
	If AllUserID<>"" Then
		If Right(AllUserID,1) = "," Then AllUserID = Left(AllUserID,Len(AllUserID)-1)
		'删除用户的帖子和精华
		Dvbbs.Execute("Delete From dv_topic where PostUserID in ("&AllUserID&")")
		Dim PostTable
		PostTable = AllPostTable
		For i=0 to ubound(PostTable,2)
			Dvbbs.Execute("Delete From "&PostTable(0,i)&" where PostUserID in ("&AllUserID&")")
		Next
		Dvbbs.Execute("Delete From dv_besttopic where PostUserID in ("&AllUserID&")")
		'删除用户上传表
		Dvbbs.Execute("Delete From dv_upfile where F_UserID in ("&AllUserID&")")
		Dvbbs.Execute("Delete From [dv_user] where userid in ("&AllUserID&")")
	End If
	Status = 0
End Sub

Function AllPostTable()
	Dim Trs
	Set Trs = Dvbbs.Execute("select TableName from [Dv_TableList]")
	AllPostTable = TRs.GetRows(-1)
	Trs.Close 
	Set Trs = Nothing
End Function

Sub SaveUserCookie()
	Dim S_syskey,Password,SaveCookie,TruePassWord,userclass,Userhidden
	S_syskey = Request.QueryString("syskey")
	UserName = Request.QueryString("UserName")
	Password = Request.QueryString("Password")
	SaveCookie = Request.QueryString("savecookie")
	If UserName="" or S_syskey="" Then Exit Sub
	Dim NewMd5,OldMd5
	NewMd5 = Md5(UserName&DvApi_SysKey,16)
	Md5OLD = 1
	OldMd5 = Md5(UserName&DvApi_SysKey,16)
	Md5OLD = 0
	If Not (S_syskey=NewMd5 or S_syskey=OldMd5) Then
		Exit Sub
	End If
	If EnabledSession Then
		Session(Dvbbs.CacheName & "UserID")=Empty
		Set Dvbbs.UserSession=Nothing
	End If
	If SaveCookie="" or Not IsNumeric(SaveCookie) Then SaveCookie = 0
	'用户退出
	If Password = "" Then
		Response.Cookies(Dvbbs.Forum_sn).path=Dvbbs.cookiepath
		Response.Cookies(Dvbbs.Forum_sn)("username")=""
		Response.Cookies(Dvbbs.Forum_sn)("password")=""
		Response.Cookies(Dvbbs.Forum_sn)("userclass")=""
		Response.Cookies(Dvbbs.Forum_sn)("userid")=""
		Response.Cookies(Dvbbs.Forum_sn)("userhidden")=""
		Response.Cookies(Dvbbs.Forum_sn)("usercookies")=""
		Session("flag")=Empty
		Exit Sub
	End If

	'用户登陆
	'Password = Md5(Password,16)
	TruePassWord = Dvbbs.Createpass
	Dim Rs,Sql
	If Not IsObject(Conn) Then ConnectionDatabase
	Set Rs = Dvbbs.iCreateObject("Adodb.RecordSet")
	Sql = "Select Top 1 UserID,UserName,UserPassword,Userclass,Userhidden,TruePassWord From Dv_User Where UserName='"&Dvbbs.Checkstr(UserName)&"'"
	Rs.Open Sql,Conn,1,3
	If Not Rs.Eof And Not Rs.Bof Then
		If Rs(2)<>Password Then
			Exit Sub
		End If
		Dvbbs.UserID = Rs(0)
		UserName = Rs(1)
		UserClass = Rs(3)
		Userhidden = Rs(4)
		Rs(5) = TruePassword
		Rs.Update
	Else
		Exit Sub
	End If
	Rs.Close
	Set Rs = Nothing
	'Response.Write "document.write("""&Dvbbs.cookiepath&""");"
	Select case SaveCookie
		case 0
			Response.Cookies(Dvbbs.Forum_sn)("usercookies") = SaveCookie
		case 1
			Response.Cookies(Dvbbs.Forum_sn).Expires=Date+1
			Response.Cookies(Dvbbs.Forum_sn)("usercookies") = SaveCookie
		case 2
			Response.Cookies(Dvbbs.Forum_sn).Expires=Date+31
			Response.Cookies(Dvbbs.Forum_sn)("usercookies") = SaveCookie
		case 3
			Response.Cookies(Dvbbs.Forum_sn).Expires=Date+365
			Response.Cookies(Dvbbs.Forum_sn)("usercookies") = SaveCookie
	End Select
	Response.Cookies(Dvbbs.Forum_sn).path = Dvbbs.cookiepath
	Response.Cookies(Dvbbs.Forum_sn)("username") = UserName
	Response.Cookies(Dvbbs.Forum_sn)("userid") = Dvbbs.UserID
	Response.Cookies(Dvbbs.Forum_sn)("password") = TruePassWord
	Response.Cookies(Dvbbs.Forum_sn)("userclass") = UserClass
	Response.Cookies(Dvbbs.Forum_sn)("userhidden") = Userhidden
	rem 清除图片上传数的限制
	Response.Cookies("upNum")=0
	'Response.Write "document.write(""OK"");"
End Sub


Sub Checkname()
	Dim UserEmail
	Dim Temp_tr,i,Rs,Sql
	UserEmail = Dvbbs.checkstr(Trim(XmlDoc.documentElement.selectSingleNode("email").text))
	Dvbbs.LoadTemplates("login")
	LoadRegSetting()
	'信息验证
	If strLength(UserName)>Cint(Dvbbs.Forum_Setting(41)) or strLength(UserName)<Cint(Dvbbs.Forum_Setting(40)) Then
		Temp_tr = Template.Strings(28)
		Temp_tr = Replace(Temp_tr,"{$RegMaxLength}",Dvbbs.Forum_Setting(41))
		Temp_tr = Replace(Temp_tr,"{$RegLimLength}",Dvbbs.Forum_Setting(40))
		Messenge = Messenge & "<li>"+Temp_tr
		Temp_tr = ""
	Else
		If XMLDom.documentElement.selectSingleNode("@checknumeric").text = "1" Then
			If IsNumeric(UserName) Then
				Messenge = Messenge & "<li>论坛不接受全数字的用户名注册."
			End If
		End If
		If Instr(UserName,"=")>0 or Instr(UserName,"%")>0 or Instr(UserName,chr(32))>0 or Instr(UserName,"?")>0 or Instr(UserName,"&")>0 or Instr(UserName,";")>0 or Instr(UserName,",")>0 or Instr(UserName,"'")>0 or Instr(UserName,",")>0 or Instr(UserName,chr(34))>0 or Instr(UserName,chr(9))>0 or Instr(UserName,"")>0 or Instr(UserName,"$")>0 Then
			Messenge = Messenge & "<li>"+template.Strings(46)
		End If
		Dim RegSplitWords
		RegSplitWords = Split(Dvbbs.Forum_setting(4),",")
		For i = 0 to Ubound(RegSplitWords)
			If Instr(UserName,RegSplitWords(i))>0 Then
				Messenge = Messenge & "<li>"+template.Strings(46)
			End If
		Next
	End If
	If UserEmail<>"" Then
		If IsValidEmail(UserEmail)=false then
			Messenge = Messenge & "<li>"+template.Strings(30)
		End If
	End If
	If Messenge<>"" Then
		'输出错误信息
		Status = 1
		Exit Sub
	End If
	If Cint(Dvbbs.Forum_Setting(24))=1 Then
		Sql="Select * From [Dv_user] Where Username='"&UserName&"' or useremail='"&UserEmail&"'"
	Else
		Sql="Select * From [Dv_user] Where Username='"&UserName&"'"
	End If
	Set Rs = Dvbbs.Execute(Sql)
	If Not Rs.Eof And Not Rs.Bof Then
		If Cint(Dvbbs.Forum_Setting(24))=1 Then
			Messenge = "您填写的用户名已经被注册或者已经有用户使用了您填写的电子邮件地址。"
		Else
			Messenge = "您填写的用户名已经被注册。"
		End If
		Status = 1
		Exit Sub
	Else
		Status = 0
		Messenge = "验证通过。"
	End If
	Rs.Close
	Set Rs = Nothing
End Sub

'用户注册
Sub Reguser()
	Dim UserPass,UserEmail,Question,Answer,usercookies
	Dim Temp_tr,i
	UserPass = Dvbbs.checkstr(XmlDoc.documentElement.selectSingleNode("password").text)
	UserEmail = Dvbbs.checkstr(Trim(XmlDoc.documentElement.selectSingleNode("email").text))
	Question = Dvbbs.checkstr(XmlDoc.documentElement.selectSingleNode("question").text)
	Answer = Dvbbs.checkstr(XmlDoc.documentElement.selectSingleNode("answer").text)
	usercookies = 1
	If UserName="" or UserPass="" or Question="" or Answer = "" Then
		Status = 1
		Messenge = Messenge & "<li>请填写用户名或密码。"
		Exit Sub
	End If
	UserPass = Md5(UserPass,16)
	Answer = Md5(Answer,16)
	Dvbbs.LoadTemplates("login")
	LoadRegSetting()
	'信息验证
	If strLength(UserName)>Cint(Dvbbs.Forum_Setting(41)) or strLength(UserName)<Cint(Dvbbs.Forum_Setting(40)) Then
		Temp_tr = Template.Strings(28)
		Temp_tr = Replace(Temp_tr,"{$RegMaxLength}",Dvbbs.Forum_Setting(41))
		Temp_tr = Replace(Temp_tr,"{$RegLimLength}",Dvbbs.Forum_Setting(40))
		Messenge = Messenge & "<li>"+Temp_tr
		Temp_tr = ""
	Else
		If XMLDom.documentElement.selectSingleNode("@checknumeric").text = "1" Then
			If IsNumeric(UserName) Then
				Messenge = Messenge & "<li>论坛不接受全数字的用户名注册."
			End If
		End If
		If Instr(UserName,"=")>0 or Instr(UserName,"%")>0 or Instr(UserName,chr(32))>0 or Instr(UserName,"?")>0 or Instr(UserName,"&")>0 or Instr(UserName,";")>0 or Instr(UserName,",")>0 or Instr(UserName,"'")>0 or Instr(UserName,",")>0 or Instr(UserName,chr(34))>0 or Instr(UserName,chr(9))>0 or Instr(UserName,"")>0 or Instr(UserName,"$")>0 Then
			Messenge = Messenge & "<li>"+template.Strings(46)
		End If
		Dim RegSplitWords
		RegSplitWords = Split(Dvbbs.Forum_setting(4),",")
		For i = 0 to Ubound(RegSplitWords)
			If Instr(UserName,RegSplitWords(i))>0 Then
				Messenge = Messenge & "<li>"+template.Strings(46)
			End If
		Next
	End If

	If IsValidEmail(UserEmail)=false then
		Messenge = Messenge & "<li>"+template.Strings(30)
	End If

	If Messenge<>"" Then
		'输出错误信息
		Status = 1
		Exit Sub
	End If

	Dim Rs,Sql
	Dim Titlepic,UserClass
	Dim TruePassWord

	Dim RegUserFace,RegTitlePic,RegClassName
	'随机产生用户头像
	Dim ForumAllFace,FaceTotalNum,RegUserFaceNum
	ForumAllFace = Split(Dvbbs.Forum_userface,"|||")
	FaceTotalNum = Ubound(ForumAllFace)-1
	Randomize
	RegUserFaceNum = Int(Rnd * FaceTotalNum)
	RegUserFace = ForumAllFace(0)&ForumAllFace(RegUserFaceNum)

	TruePassWord = Dvbbs.Createpass
	Set Rs = Dvbbs.Execute("Select UserTitle,GroupPic,UserGroupID,IsSetting,ParentGID From Dv_UserGroups Where ParentGID=3 Order By MinArticle")
	UserClass = Rs(0)
	TitlePic = Rs(1)
	Dvbbs.UserGroupID = Rs(2)
	Set Rs = Dvbbs.iCreateObject("Adodb.RecordSet")
	If Cint(Dvbbs.Forum_Setting(24))=1 Then
		Sql="Select * From [Dv_user] Where Username='"&UserName&"' or useremail='"&UserEmail&"'"
	Else
		Sql="Select * From [Dv_user] Where Username='"&UserName&"'"
	End If
	Rs.Open Sql,Conn,1,3
	If Not Rs.Eof And Not Rs.Bof Then
		If Cint(Dvbbs.Forum_Setting(24))=1 Then
			Messenge = "您填写的用户名已经被注册或者已经有用户使用了您填写的电子邮件地址。"
		Else
			Messenge = "您填写的用户名已经被注册。"
		End If
		Status = 1
		Exit Sub
	Else
		Status = 0
		Rs.AddNew
		Rs("UserName") = UserName
		Rs("UserPassword") = UserPass
		Rs("UserEmail") = UserEmail
		Rs("Userclass") = UserClass
		Rs("TitlePic") = TitlePic
		Rs("UserQuesion") = Question
		Rs("UserAnswer") = Answer

		Rs("TruePassWord") = TruePassword
		Rs("UserIM") = "||||||||||||||||||"
		Rs("UserPost") = 0
		Rs("Usersex") = 0
		Rs("Lockuser")=0
		If Dvbbs.Forum_Setting(25)="1" Then
			Rs("UserGroupID")=5
		Else
		   	Rs("UserGroupID")=Dvbbs.UserGroupID
		End If
		Rs("JoinDate")=NOW()
		Rs("UserFace") = RegUserFace
		Rs("UserWidth") = 32
		Rs("Usertoday") = "0|0|0|0|0"
		Rs("UserHeight") = 32
		Rs("UserLogins") = 1
		Rs("LastLogin") = NOW()
		Rs("userWealth") = dvbbs.Forum_user(0)
		Rs("userEP") = dvbbs.Forum_user(5)
		Rs("usercP") = dvbbs.Forum_user(10)
		Rs("UserInfo") = "||||||||||||||||||||||||||||||||||||||||||"
		Rs("Usersetting") = "0|||0|||1"
		Rs("UserPower") = 0
		Rs("UserDel") = 0
		Rs("UserIsbest") = 0
		Rs("UserFav") = "陌生人,我的好友,黑名单"
		Rs("IsChallenge") = 0
		Rs("UserHidden") = 0
		Rs("UserLastIP") = Dvbbs.UserTrueIP
		Rs.Update
		Dvbbs.Execute("UpDate Dv_Setup Set Forum_UserNum=Forum_UserNum+1,Forum_lastUser='"&Dvbbs.HtmlEncode(username)&"'")

	End If
	Rs.Close
	Set Rs = Nothing
	If Status = 0 Then
		Set Rs=Dvbbs.execute("select top 1 userid from [Dv_user] order by userid desc")
		Dvbbs.userid=rs(0)
		Rs.close
		Set Rs=nothing
		Dvbbs.ReloadSetupCache UserName,14
		Dvbbs.ReloadSetupCache (CLng(Dvbbs.CacheData(10,0))+1),10
		Saveregcount(UserName)

		If Cint(Dvbbs.Forum_Setting(23))=0 and Cint(Dvbbs.Forum_Setting(25))=0 Then
			If EnabledSession Then
				Set Dvbbs.UserSession = Nothing 
				Session(Dvbbs.CacheName & "UserID")= empty
			End If
			Dim StatUserID,UserSessionID
			StatUserID = Dvbbs.checkStr(Trim(Request.Cookies(Dvbbs.Forum_sn)("StatUserID")))
			If IsNumeric(StatUserID) = 0 or StatUserID = "" Then
				StatUserID = Replace(Dvbbs.UserTrueIP,".","")
				UserSessionID = Replace(Startime,".","")
				If IsNumeric(StatUserID) = 0 or StatUserID = "" Then StatUserID = 0
				StatUserID = Ccur(StatUserID) + Ccur(UserSessionID)
			End If
			StatUserID = Ccur(StatUserID)
			Dvbbs.Execute("Delete from dv_online where username='"&dvbbs.membername&"' Or id="&StatUserID&"")
			'Response.Cookies(Dvbbs.Forum_sn)("StatUserID") = StatUserID
			'select case usercookies
			'case 0
			'	Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
			'Case 1
			'	Response.Cookies(Dvbbs.Forum_sn).Expires=Date+1
			'	Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
			'Case 2
			'	Response.Cookies(Dvbbs.Forum_sn).Expires=Date+31
			'	Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
			'case 3
			'	Response.Cookies(Dvbbs.Forum_sn).Expires=Date+365
			'	Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
			'end select
			'Response.Cookies(Dvbbs.Forum_sn)("username") = username
			'Response.Cookies(Dvbbs.Forum_sn)("password") = TruePassWord
			'Response.Cookies(Dvbbs.Forum_sn)("userclass") = userclass
			'Response.Cookies(Dvbbs.Forum_sn)("userid") = Dvbbs.userid
			'Response.Cookies(Dvbbs.Forum_sn)("userhidden") = 2
			'Response.Cookies(Dvbbs.Forum_sn).path = Dvbbs.cookiepath
			Dvbbs.membername = Username
			Dvbbs.userhidden = 2
			Dvbbs.MemberClass = Userclass		
		End If
		Session("regtime")=now()
		Messenge = "注册成功。"
	End If

End Sub

'更新用户状态
Sub Lockuser()
	Dim UserStatus,Rs,Sql,locktype
	If XmlDoc.documentElement.selectSingleNode("userstatus") is Nothing Then
		Messenge = "<li>参数非法，中止请求。"
		Status = 1
		Exit Sub
	ElseIf Not IsNumeric(XmlDoc.documentElement.selectSingleNode("userstatus").text) Then
		Messenge = "<li>参数非法，中止请求。"
		Status = 1
		Exit Sub
	Else
		UserStatus = Clng(XmlDoc.documentElement.selectSingleNode("userstatus").text)
	End If
	Select Case UserStatus
		Case 1
			locktype="锁定"
		Case 2
			locktype="屏蔽"
		Case Else
			locktype="解锁"
	End Select
	If Not IsObject(Conn) Then ConnectionDatabase

	Set Rs = Dvbbs.iCreateObject("Adodb.RecordSet")
	Sql  = "Select Lockuser From [Dv_user] Where UserGroupID>1 and Username='"&UserName&"'"
	Rs.Open Sql,Conn,1,3
	If Not Rs.Eof And Not Rs.Bof Then
		Status = 0
		Messenge = "<li>"&locktype&"成功。"
		Rs("Lockuser") = UserStatus
		Rs.Update
		sql="insert into Dv_log (l_touser,l_username,l_content,l_ip,l_type) values ('"&username&"','"&appid&"','用户操作："&locktype& "','"&Dvbbs.UserTrueIP&"',6)"
		Dvbbs.Execute(SQL)
	End If
	Rs.close
	Set Rs = Nothing
End Sub


'用户信息修改
Sub UpdateUser()
	Dim Rs,Sql
	Dim UserPass,UserEmail,Question,Answer
	UserPass = Dvbbs.checkstr(XmlDoc.documentElement.selectSingleNode("password").text)
	UserEmail = Dvbbs.checkstr(Trim(XmlDoc.documentElement.selectSingleNode("email").text))
	Question = Dvbbs.checkstr(XmlDoc.documentElement.selectSingleNode("question").text)
	Answer = Dvbbs.checkstr(XmlDoc.documentElement.selectSingleNode("answer").text)
	If UserPass<>"" Then
		UserPass = Md5(UserPass,16)
	End If
	If Answer<>"" THen
		Answer = Md5(Answer,16)
	End If
	Set Rs = Dvbbs.iCreateObject("Adodb.RecordSet")
	Sql="Select Top 1 * From [Dv_user] Where Username='"&UserName&"'"
	If Not IsObject(Conn) Then ConnectionDatabase
	Rs.Open Sql,Conn,1,3
	If Not Rs.Eof And Not Rs.Bof Then
		If UserPass<>"" Then Rs("UserPassword") = UserPass
		If Answer<>"" THen Rs("UserAnswer") = Answer
		If UserEmail<>"" Then Rs("UserEmail") = UserEmail
		If Question<>"" Then Rs("UserQuesion") = Question
		Rs.update
		Status = 0
		Messenge = "<li>基本资料修改成功。"
	Else
		Status = 1
		Messenge = "<li>该用户不存在，修改资料失败。"
	End If
	Rs.Close
	Set Rs = Nothing
End Sub

'用户退出
Sub LogoutUser()
	If Not IsObject(Conn) Then ConnectionDatabase
	Dim activeuser,TempNum
	If Not CLng(DVbbs.UserSession.documentElement.selectSingleNode("userinfo/@userid").text)=0 Then
		activeuser="delete from Dv_online where userid= "& DVbbs.UserSession.documentElement.selectSingleNode("userinfo/@userid").text
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
	If EnabledSession Then
		Session(Dvbbs.CacheName & "UserID")=Empty
	End If
	Set Dvbbs.UserSession=Nothing
	Session("flag")=Empty
	Status = 0
	Messenge = "退出成功。"
End Sub


'用户登陆
Sub UesrLogin()
	Dim UserPass
	Dim i
	
	UserPass = Dvbbs.checkstr(XmlDoc.documentElement.selectSingleNode("password").text)
	If UserName="" or UserPass="" Then
		Status = 1
		Messenge = Messenge & "<li>请填写用户名或密码。"
		Exit Sub
	End If
	UserPass = Md5(UserPass,16)
	'判断更新cookies目录
	Dim cookies_path_s,cookies_path_d,cookies_path
	cookies_path_s=split(Request.ServerVariables("PATH_INFO"),"/")
	cookies_path_d=ubound(cookies_path_s)
	cookies_path="/"
	For i=1 to cookies_path_d-1
		If not (cookies_path_s(i)="upload" or cookies_path_s(i)="admin") Then cookies_path=cookies_path&cookies_path_s(i)&"/"
	Next
	If dvbbs.cookiepath<>cookies_path Then
		Cookies_path = replace(cookies_path,"'","")
		Dvbbs.execute("update dv_setup set Forum_Cookiespath='"&cookies_path&"'")
		Dim setupData 
		Dvbbs.CacheData(26,0)=cookies_path
		Dvbbs.Name="setup"
		Dvbbs.value = Dvbbs.CacheData
	End If
	'判断用户是否登录
    If ChkUserLogin(UserName, UserPass, "", 3, 1)=False Then
		Status = 1
		Messenge = Messenge & "<li>登陆失败。"
	Else
		Status = 0
		Messenge = Messenge & "<li>登陆成功。"
	End If
End Sub

Rem ==========论坛登录函数=========
Rem 判断用户登录
Function ChkUserLogin(username,password,mobile,usercookies,ctype)
	Dim TruePassWord
	'产生随机密码
	TruePassword = Dvbbs.Createpass
	Dim rsUser,article,userclass,titlepic
	Dim userhidden,lastip,UserLastLogin
	Dim GroupID,ClassSql,FoundGrade
	Dim regname,iMyUserInfo
	Dim sql,sqlstr,OLDuserhidden

	FoundGrade=False
	lastip=Dvbbs.UserTrueIP
	'userhidden=request.form("userhidden")
	If userhidden <> "1" Then userhidden=2
	ChkUserLogin=false
	If mobile<>"" Then
		sqlstr=" Passport='"&mobile&"'"
	Else
		sqlstr=" UserName='"&username&"'"
	End If
	Sql="Select UserID,UserName,UserPassword,UserEmail,UserPost,UserTopic,UserSex,UserFace,UserWidth,UserHeight,JoinDate,LastLogin,lastlogin as cometime , LastLogin as activetime,UserLogins,Lockuser,Userclass,UserGroupID,UserGroup,userWealth,userEP,userCP,UserPower,UserBirthday,UserLastIP,UserDel,UserIsBest,UserHidden,UserMsg,IsChallenge,UserMobile,TitlePic,UserTitle,TruePassWord,UserToday,UserMoney,UserTicket,FollowMsgID,Vip_StarTime,Vip_EndTime,userid as boardid"
	Sql=Sql & " From [Dv_User] Where "&sqlstr&""
	set rsUser=Dvbbs.Execute(sql)
	If rsUser.eof and rsUser.bof Then
		Messenge = Messenge & "<li>该用户不存在。"
		ChkUserLogin=False
		Exit Function
	Else
		If rsUser("Lockuser") =1 Or rsUser("UserGroupID") =5 Then
			Status = 1
			Messenge = Messenge & "<li>该用户已被系统锁定。"
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
				If Article < 0  Then Article=0
				'Set Dvbbs.UserSession=Dvbbs.RecordsetToxml(rsUser,"userinfo","xml")
				'Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@cometime").text=Now()
				'Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@activetime").text=DateAdd("s",-3600,Now())
				'Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@boardid").text=0
				'Dvbbs.UserSession.documentElement.selectSingleNode("userinfo").attributes.setNamedItem(Dvbbs.UserSession.createNode(2,"isuserpermissionall","")).text=Dvbbs.FoundUserPermission_All()
				If OLDuserhidden <> CLng(userhidden) Then
					'Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userhidden").text=userhidden
					Dvbbs.Execute("update Dv_user set userhidden="&userhidden&" where UserId=" & Dvbbs.UserID)
				End If
				'Dim BS
				'Set Bs=Dvbbs.GetBrowser()
				'Dvbbs.UserSession.documentElement.appendChild(Bs.documentElement)
				'If EnabledSession Then 	Session(Dvbbs.CacheName & "UserID")=Dvbbs.UserSession.xml
			Else
				Messenge = Messenge & "<li>请检查您填写的密码是否正确。"
				ChkUserLogin=False
				Exit Function
			End If
		End If
	End If
	If ChkUserLogin Then
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
		Set rsUser=nothing
		If Not FoundGrade Then
			Status = 1
			Messenge = Messenge & "<li>系统没有找到您的注册用户组资料，请联系管理员进行修正。"
			Exit Function
		End If
		select case ctype
		case 1
			If Datediff("d",UserLastLogin,Now())=0 Then
				sql="update [Dv_User] set LastLogin="&SqlNowString&",UserLogins=UserLogins+1,UserLastIP='"&lastip&"',userclass='"&userclass&"',titlepic='"&titlepic&"',UserGroupID="&GroupID&",TruePassWord='"&TruePassWord&"' where userid="&dvbbs.UserID
			Else
				sql="update [Dv_User] set userWealth=userWealth+"&Dvbbs.Forum_user(4)&",userEP=userEP+"&Dvbbs.Forum_user(9)&",userCP=userCP+"&Dvbbs.Forum_user(14)&",LastLogin="&SqlNowString&",UserLogins=UserLogins+1,UserLastIP='"&lastip&"',userclass='"&userclass&"',titlepic='"&titlepic&"',UserGroupID="&GroupID&",TruePassWord='"&TruePassWord&"' where userid="&dvbbs.UserID
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
		'If trim(username)<>trim(Dvbbs.membername) Then
			'Response.Cookies(Dvbbs.Forum_sn)("username")=""
			'Response.Cookies(Dvbbs.Forum_sn)("password")=""
			'Response.Cookies(Dvbbs.Forum_sn)("userclass")=""
			'Response.Cookies(Dvbbs.Forum_sn)("userid")=""
			'Response.Cookies(Dvbbs.Forum_sn)("userhidden")=""
			'Response.Cookies(Dvbbs.Forum_sn)("usercookies")=""
			'Dvbbs.Execute("delete from dv_online where username='"&Dvbbs.membername&"'")
		'End If
		'If isnull(usercookies) or usercookies="" Then usercookies="0"
		'select case usercookies
		'case "0"
			'Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
		'case 1
			'Response.Cookies(Dvbbs.Forum_sn).Expires=Date+1
			'Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
		'case 2
			'Response.Cookies(Dvbbs.Forum_sn).Expires=Date+31
			'Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
		'case 3
			'Response.Cookies(Dvbbs.Forum_sn).Expires=Date+365
			'Response.Cookies(Dvbbs.Forum_sn)("usercookies") = usercookies
		'end select
		
		'Response.Cookies(Dvbbs.Forum_sn).path = Dvbbs.cookiepath
		'Response.Cookies(Dvbbs.Forum_sn)("username") = regname
		'Response.Cookies(Dvbbs.Forum_sn)("userid") = Dvbbs.UserID
		'Response.Cookies(Dvbbs.Forum_sn)("password") = TruePassWord
		'Response.Cookies(Dvbbs.Forum_sn)("userclass") = userclass
		'Response.Cookies(Dvbbs.Forum_sn)("userhidden") = userhidden
		rem 清除图片上传数的限制
		'Response.Cookies("upNum")=0
		'Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@truepassword").text= TruePassWord
		Dvbbs.Membername=Dvbbs.Checkstr(regname)
		Dvbbs.Memberclass=Dvbbs.Checkstr(userclass)
		Dvbbs.UserGroupID=GroupID
	End If
End Function



'-------------------------------------------------------------------------------------------------------
Sub LoadRegSetting()
	Dim Node
	Set XMLDom=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	If XMLDom.loadxml(Dvbbs.CacheData(27,0)) Then
		If XMLDom.documentElement.nodeName<>"regsetting" Then
			ToDefaultsetting()
		End If
	End If
End Sub

Sub ToDefaultsetting()
	Dim Node
	Set XMLDom=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XMLDom.appendChild(XMLDom.createElement("regsetting"))
	Set Node=XMLDom.documentElement.appendChild(XMLDom.createNode(1,"checkip",""))
	Node.attributes.setNamedItem(XMLDom.createNode(2,"use","")).text="0"
	Node.appendChild(XMLDom.createElement("iplist1"))
	Node.appendChild(XMLDom.createElement("iplist2"))
	XMLDom.documentElement.attributes.setNamedItem(XMLDom.createNode(2,"postipinfo","")).text="0"
	XMLDom.documentElement.attributes.setNamedItem(XMLDom.createNode(2,"checknumeric","")).text="0"
	XMLDom.documentElement.attributes.setNamedItem(XMLDom.createNode(2,"checktime","")).text="0"
	XMLDom.documentElement.attributes.setNamedItem(XMLDom.createNode(2,"usevarform","")).text="0"
	XMLDom.documentElement.attributes.setNamedItem(XMLDom.createNode(2,"checkregcount","")).text="0"
	Dvbbs.Execute("update dv_setup set Forum_Boards='"&Dvbbs.checkstr(XMLDom.XML)&"'")
	Dvbbs.loadSetup()
End Sub

Sub Saveregcount(username)
	Dim Node,rs,XMLDom1
	Set XMLDom1=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	Set Rs=Dvbbs.Execute("select Forum_Ad From Dv_setup")
	If Not XMLDom1.loadxml(Rs(0)) Then
		XMLDom1.LoadXML "<?xml version=""1.0""?><reglist/>"
	Else
		For Each Node in XMLDom1.documentElement.selectNodes("ip")
			If Datediff("d",Node.selectSingleNode("@datetime").text,Now()) > 0 Then
					XMLDom1.documentElement.removeChild(node)	
			End If
		Next
	End If
	Set Node=XMLDom1.documentElement.appendChild(XMLDom1.createNode(1,"ip",""))
	Node.text=Dvbbs.userTrueIP
	Node.attributes.setNamedItem(XMLDom1.createNode(2,"datetime","")).text=Now()
	Node.attributes.setNamedItem(XMLDom1.createNode(2,"username","")).text=username
	Dvbbs.Execute("update Dv_setup set Forum_Ad='"&Dvbbs.checkstr(XMLDom1.xml)&"'")
End Sub

%>