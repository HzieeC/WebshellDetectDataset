<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/chkinput.asp"-->
<%
'mymodify.asp
Dvbbs.LoadTemplates("usermanager")
Dvbbs.Stats=Dvbbs.MemberName&template.Strings(1)
Dvbbs.Nav()
Dvbbs.Head_var 0,0,template.Strings(0),"usermanager.asp"
Dim Sql,Rs,TempStr,ErrCodes

If Cint(Dvbbs.GroupSetting(16))=0 Then
	Dvbbs.AddErrCode(28)
	Dvbbs.Showerr()
End If

If Dvbbs.userid=0 Then
	Dvbbs.AddErrCode(6)
	Dvbbs.Showerr()
Else
	Response.Write Template.Html(0)
	If Request("action")="updat" Then
		update()
	Else
		Userinfo()
	End If
	If ErrCodes<>"" Then Response.redirect "showerr.asp?ErrCodes="&ErrCodes&"&action=OtherErr"	
End If
Dvbbs.ActiveOnline()
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub Userinfo()
	Dim CanUseTitle,CanUseTitle1,CanUseTitle2,i,CanUserInfo
	Dim My_info,My_infotemp,My_Cookies,ShowUserInfo
	Dim UseRsetting,SetUserInfo,SetUserTrue,ShowRe
	Dim signtrue
	Dim FoundUseMagic,iMagicFace,mRs,iMagicMoney,iMagicTicket
	FoundUseMagic = False
	My_infotemp=Template.Html(5)
	My_Cookies=Request.Cookies(Dvbbs.Forum_sn)("usercookies")
	CanUseTitle=False
	CanUseTitle1=False
	CanUseTitle2=False
	CanUserInfo=False
	'UserID=0,UserName=1,UserEmail=2,UserPost=3,UseRsign=4,UseRsex=5,UserFace=6,UserWidth=7,UserHeight=8,JoinDate=9,UserGroup=10,UserTitle=11,UserBirthday=12,UserPhoto=13,UserInfo=14,UseRsetting=15
	sql="Select UserID,UserName,UserEmail,UserPost,UseRsign,UseRsex,UserFace,UserWidth,UserHeight,JoinDate,UserGroup,UserTitle,UserBirthday,UserPhoto,UserInfo,UseRsetting from [DV_User] where userid="&Dvbbs.userid
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.eof And Rs.bof Then
		Dvbbs.AddErrCode(32)
		Exit Sub
	Else
		Sql=Rs.GetString(,,"#§§#","","")
	End If
	Rs.close :Set Rs=Nothing
	My_info= Split(Sql,"#§§#")
	If Cint(Dvbbs.Forum_Setting(6))=1 Then CanUseTitle=True

	If CanUseTitle And Cint(Dvbbs.Forum_Setting(60)) > 0 And Clng(My_info(3)) > Cint(Dvbbs.Forum_Setting(60)) Then
		CanUseTitle1 = True
	ElseIf CanUseTitle And Cint(Dvbbs.Forum_Setting(60)) = 0 Then
		CanUseTitle1 = True
	Else 
		CanUseTitle1 = False
	End If

	If CanUseTitle and Cint(Dvbbs.Forum_Setting(61))>0 And DateDiff("d",My_info(9),Now())>Cint(Dvbbs.Forum_Setting(61)) Then
		CanUseTitle2=True
	ElseIf CanUseTitle And Cint(Dvbbs.Forum_Setting(61))=0 Then
		CanUseTitle2=True
	Else
		CanUseTitle2=False
	End If

	If CanUseTitle And Cint(Dvbbs.Forum_Setting(62))=1 Then 
		If CanUseTitle1 And CanUseTitle2 Then 
			CanUseTitle=True 
		Else
			CanUseTitle=False
		End If 
	ElseIf CanUseTitle And (CanUseTitle1 or CanUseTitle2) Then
		CanUseTitle=True 
	Else
		CanUseTitle=False 
	End If
	signtrue=My_info(4)
	If My_info(14)<>"" Then
		ShowUserInfo=split(My_info(14),"|||")
		If ubound(ShowUserInfo)=14 Then
		CanUserInfo=True
		End If
	End If
	Dim userdsindex

	UseRsetting=split(My_info(15),"|||")
	If UBound(UseRsetting)>=3 Then
		If isnumeric(UseRsetting(0)) Then Setuserinfo=cint(UseRsetting(0)) Else Setuserinfo=1
		If isnumeric(UseRsetting(1)) Then Setusertrue=cint(UseRsetting(1)) Else Setusertrue=0
		If isnumeric(UseRsetting(2)) Then ShowRe=cint(UseRsetting(2)) Else ShowRe=0
		If isnumeric(UseRsetting(3)) Then userdsindex=cint(UseRsetting(3)) Else userdsindex = 0
	Else
		Setuserinfo=1
		Setusertrue=0
		ShowRe=0
		userdsindex = 0
	End If
	'魔法头像部分
	If Dvbbs.Forum_Setting(98)="1" And Dvbbs.GroupSetting(69)="1" Then My_infotemp=Replace(My_infotemp,"{$usermagicface}",Template.Html(20))
	My_infotemp=Replace(My_infotemp,"{$usermagicface}","")
	iMagicFace = Split(My_info(6),"|")
	If Ubound(iMagicFace) = 1 Then
		My_info(6) = iMagicFace(1)
		If Dvbbs.Forum_Setting(98)="1" And IsNumeric(iMagicFace(0)) Then
			Set mRs = Dvbbs.Plus_Execute("Select iMoney,iTicket From Dv_Plus_Tools_MagicFace Where MagicFace_s = " & Dvbbs.CheckNumeric(iMagicFace(0)))
			If Not (mRs.Eof And mRs.Bof) Then
				FoundUseMagic = True
				iMagicMoney = mRs(0)
				iMagicTicket = mRs(1)
			End If
			mRs.Close
			Set mRs=Nothing
		End If
	End If
	If Dvbbs.Forum_Setting(98)="1" Then
	If FoundUseMagic Then
		My_infotemp=Replace(My_infotemp,"{$isselect}","checked")
		My_infotemp=Replace(My_infotemp,"{$firstmagicface}",iMagicFace(0))
		My_infotemp=Replace(My_infotemp,"{$magicmoney}",iMagicMoney)
		My_infotemp=Replace(My_infotemp,"{$magicticket}",iMagicTicket)
	Else
		My_infotemp=Replace(My_infotemp,"{$isselect}","")
		Set mRs = Dvbbs.Plus_Execute("Select Top 1 MagicFace_s,iMoney,iTicket From Dv_Plus_Tools_MagicFace Order By ID")
		If Not (mRs.Eof And mRs.Bof) Then
			My_infotemp = Replace(My_infotemp,"{$firstmagicface}",mRs(0))
			My_infotemp = Replace(My_infotemp,"{$magicmoney}",mRs(1))
			My_infotemp = Replace(My_infotemp,"{$magicticket}",mRs(2))
		Else
			My_infotemp = Replace(My_infotemp,"{$firstmagicface}",0)
			My_infotemp = Replace(My_infotemp,"{$magicmoney}",0)
			My_infotemp = Replace(My_infotemp,"{$magicticket}",0)
		End If
		mRs.Close
		Set mRs=Nothing
	End If
	End If
	'魔法头像部分
	'管理员与超版自定义头像不受发帖数限制 2005-3-10 Dv.Yz
	If (Dvbbs.Master Or Dvbbs.SuperBoardMaster) And Clng(My_info(3)) < Cint(Dvbbs.Forum_Setting(54)) Then My_info(3) = Cint(Dvbbs.Forum_Setting(54))
	'用My_info(3)判断是否有自定义头像权限，更新page_usermanager模板界面(5)、(7) Dv.Yz 2005-1-27
	My_infotemp = Replace(My_infotemp, "{$SetFace_info}", SetUserFace(Cint(Dvbbs.Forum_UploadSetting(0)), My_info(6)&"", My_info(7), My_info(8), My_info(3)))

	'If cint(Dvbbs.Forum_Setting(32))=1 Then
	'	My_infotemp=Replace(My_infotemp,"{$SetGroup_info}",SetUserGroup(My_info(10)))
	'Else
		My_infotemp=Replace(My_infotemp,"{$SetGroup_info}","")
	'End If

	My_infotemp = Replace(My_infotemp,"{$signlength}",Clng(Dvbbs.GroupSetting(56)))
	My_infotemp=Replace(My_infotemp,"{$user_Id}",My_info(0))
	My_infotemp=Replace(My_infotemp,"'{$Dvbbs.FoundIsChallenge}'",Lcase(Dvbbs.FoundIsChallenge))
	If CanUseTitle Then
		My_Infotemp = Replace(My_Infotemp, "{$SetTitle_info}", SetUserTitle(Dvbbs.Htmlencode(My_info(11))))
	Else
		My_Infotemp = Replace(My_infotemp, "{$SetTitle_info}", "")
	End If
	My_infotemp=Replace(My_infotemp,"{$checked_sex}",My_info(5))
	My_infotemp=Replace(My_infotemp,"{$user_Birthday}",My_info(12))
	My_infotemp=Replace(My_infotemp,"{$user_Photo}",Dvbbs.htmlencode(Trim(My_info(13))))
	My_infotemp=Replace(My_infotemp,"{$user_Signature}",signtrue)
	My_infotemp=Replace(My_infotemp,"{$showRe}",ShowRe)
	My_infotemp=Replace(My_infotemp,"{$user_Cookies}",My_Cookies)
	My_infotemp=Replace(My_infotemp,"{$user_Setuserinfo}",Setuserinfo)
	My_infotemp=Replace(My_infotemp,"{$user_Setusertrue}",Setusertrue)
	My_infotemp=Replace(My_infotemp,"{$userdsindex}",userdsindex)
	
	If CanUserInfo=True Then
		My_infotemp=Replace(My_infotemp,"{$user_Realname}",ShowUserInfo(0))
		My_infotemp=Replace(My_infotemp,"{$user_character}",Chk_KidneyType("character",ShowUserInfo(1),template.Strings(15)))
		My_infotemp=Replace(My_infotemp,"{$user_Personal}",ShowUserInfo(2))
		My_infotemp=Replace(My_infotemp,"{$user_Country}",ShowUserInfo(3))
		My_infotemp=Replace(My_infotemp,"{$user_Province}",ShowUserInfo(4))
		My_infotemp=Replace(My_infotemp,"{$user_City}",ShowUserInfo(5))
		My_infotemp=Replace(My_infotemp,"{$user_College}",ShowUserInfo(12))
		My_infotemp=Replace(My_infotemp,"{$user_Phone}",ShowUserInfo(13))
		My_infotemp=Replace(My_infotemp,"{$user_Address}",ShowUserInfo(14))
		My_infotemp=Replace(My_infotemp,"{$user_shengxiao}",chk_select(ShowUserInfo(6),template.Strings(11)))
		My_infotemp=Replace(My_infotemp,"{$user_blood}",chk_select(ShowUserInfo(7),"A,B,AB,O"))
		My_infotemp=Replace(My_infotemp,"{$user_belief}",chk_select(ShowUserInfo(8),template.Strings(16)))
		My_infotemp=Replace(My_infotemp,"{$user_occupation}",chk_select(ShowUserInfo(9),template.Strings(12)))
		My_infotemp=Replace(My_infotemp,"{$user_marital}",chk_select(ShowUserInfo(10),template.Strings(13)))
		My_infotemp=Replace(My_infotemp,"{$user_education}",chk_select(ShowUserInfo(11),template.Strings(14)))
	Else
		My_infotemp=Replace(My_infotemp,"{$user_Realname}","")
		My_infotemp=Replace(My_infotemp,"{$user_character}",Chk_KidneyType("character","",template.Strings(15)))
		My_infotemp=Replace(My_infotemp,"{$user_Personal}","")
		My_infotemp=Replace(My_infotemp,"{$user_Country}","")
		My_infotemp=Replace(My_infotemp,"{$user_Phone}","")
		My_infotemp=Replace(My_infotemp,"{$user_Address}","")
		My_infotemp=Replace(My_infotemp,"{$user_Province}","")
		My_infotemp=Replace(My_infotemp,"{$user_City}","")
		My_infotemp=Replace(My_infotemp,"{$user_Cartype}","")	
		My_infotemp=Replace(My_infotemp,"{$user_College}","")
		My_infotemp=Replace(My_infotemp,"{$user_shengxiao}",chk_select("",template.Strings(11)))
		My_infotemp=Replace(My_infotemp,"{$user_blood}",chk_select("","A,B,AB,O"))
		My_infotemp=Replace(My_infotemp,"{$user_belief}",chk_select("",template.Strings(16)))
		My_infotemp=Replace(My_infotemp,"{$user_occupation}",chk_select("",template.Strings(12)))
		My_infotemp=Replace(My_infotemp,"{$user_marital}",chk_select("",template.Strings(13)))
		My_infotemp=Replace(My_infotemp,"{$user_education}",chk_select("",template.Strings(14)))
	End If
	Response.write My_infotemp
End Sub

Sub update()
	If Dvbbs.chkpost=False Then
		Dvbbs.AddErrCode(16)		
		Exit Sub
	End If
	Dim CanUseTitle,CanUseTitle1,CanUseTitle2
	Dim SplitUserTitle,i,sex,showRe,face,width,height,birthday,usercookies,usertitle
	Dim tMagicFace,iMagicFace,tMagicMoney,tMagicTicket,FoundUseMagic,iFace,Get_GroupNameId
	CanUseTitle=false
	CanUseTitle1=false
	CanUseTitle2=false
	If Not Dvbbs.FoundIsChallenge Then
		If Request.Form("sex")="" Then
			Dvbbs.AddErrCode(18)
		ElseIf isInteger(Request.Form("sex")) Then
			sex=Request.Form("sex")
		Else
			Dvbbs.AddErrCode(18)
		End If
	End If
	
	If Request.Form("showRe")="" Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(17)		
	ElseIf isInteger(Request.Form("showRe")) Then
		showRe=cint(Request.Form("showRe"))
	Else
		Dvbbs.AddErrCode(18)
	End If
	'管理员与超版自定义头像不受发帖数限制 2005-3-10 Dv.Yz
	Dim Mypost
	Mypost=Clng(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userpost").text) 
	If (Dvbbs.Master Or Dvbbs.SuperBoardMaster) And Mypost < Cint(Dvbbs.Forum_Setting(54)) Then Mypost = Cint(Dvbbs.Forum_Setting(54))
	If Request.Form("myface") <> "" And ((Clng(Dvbbs.Forum_Setting(54)) > 0 And Not Mypost < Clng(Dvbbs.Forum_Setting(54))) Or Clng(Dvbbs.Forum_Setting(54))=0) And Not InStr(LCase(Request.Form("myface")),".asp")>0 Then
		If Request.Form("width")="" or Request.Form("height")="" Then
			ErrCodes=ErrCodes+"<li>"+template.Strings(18)
		ElseIf not isInteger(Request.Form("width")) or not isInteger(Request.Form("height")) Then
			Dvbbs.AddErrCode(18)
		ElseIf Cint(Request.Form("width"))>Cint(Dvbbs.Forum_Setting(57)) Then
			ErrCodes=ErrCodes+"<li>"+template.Strings(19)
		ElseIf Cint(Request.Form("height"))>Cint(Dvbbs.Forum_Setting(57)) Then
			ErrCodes=ErrCodes+"<li>"+template.Strings(20)
		Else
			If Cint(Dvbbs.Forum_Setting(55))=0 Then
				If InStr(lcase(Request.Form("myface")),"http://")>0 or instr(lcase(Request.Form("myface")),"www.")>0 Then
					ErrCodes=ErrCodes+"<li>"+template.Strings(21)
				End If
			End If
			Face=Request.Form("myface")
			width=Request.Form("width")
			height=Request.Form("height")
		End If
	Else
		Dvbbs.Forum_userface = Split(Dvbbs.Forum_userface,"|||")
		If Request.Form("face")="" Then
			Face=Dvbbs.Forum_userface(0)&Dvbbs.Forum_userface(1)
		Else
			Face=Request.Form("face")
		End If
	End If
	face=Dv_FilterJS(Replace(face,"'",""))
	face=Replace(face,"..","")
	face=Replace(face,"\","/")
	face=Replace(face,"^","")
	face=Replace(face,"#","")
	face=Replace(face,"%","")
	face=Replace(face,"|","")
	face=Server.htmlencode(Left(face,200))
	'魔法表情检查部分
	tMagicFace = Request("tMagicFace")
	If tMagicFace = "" Or Not IsNumeric(tMagicFace) Then tMagicFace = 0
	tMagicFace = Cint(tMagicFace)
	iMagicFace = Request("iMagicFace")
	If iMagicFace = "" Or Not IsNumeric(iMagicFace) Then iMagicFace = 0
	iMagicFace = Clng(iMagicFace)
	If tMagicFace = 1 And iMagicFace > 0 And Dvbbs.Forum_Setting(98)="1" Then
		Set Rs = Dvbbs.Plus_Execute("Select iMoney,iTicket,MagicSetting From Dv_Plus_Tools_MagicFace Where MagicFace_s = " & iMagicFace)
		If Rs.Eof And Rs.Bof Then
			face = "0|" & face
			tMagicMoney = 0
			tMagicTicket = 0
		Else
			If cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text) < Rs(1) And cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text) < Rs(0) Then Response.redirect "showerr.asp?ErrCodes=<li>您没有足够的金币或点券使用魔法表情，2秒后自动返回上一页面。&action=OtherErr&autoreload=1"
			Dim iMagicSetting
			iMagicSetting = Split(Rs(2),"|")
			If cCur(iMagicSetting(0)) > cCur(Mypost) Then Response.redirect "showerr.asp?ErrCodes=<li>您的帖子数没有达到使用魔法表情的标准，2秒后自动返回上一页面。&action=OtherErr&autoreload=1"
			If cCur(iMagicSetting(1)) > cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userwealth").text) Then Response.redirect "showerr.asp?ErrCodes=<li>您的金钱数没有达到使用魔法表情的标准，2秒后自动返回上一页面。&action=OtherErr&autoreload=1"
			If cCur(iMagicSetting(2)) > cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userep").text) Then Response.redirect "showerr.asp?ErrCodes=<li>您的积分数没有达到使用魔法表情的标准，2秒后自动返回上一页面。&action=OtherErr&autoreload=1"
			If cCur(iMagicSetting(3)) > cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usercp").text) Then Response.redirect "showerr.asp?ErrCodes=<li>您的魅力数没有达到使用魔法表情的标准，2秒后自动返回上一页面。&action=OtherErr&autoreload=1"
			If cCur(iMagicSetting(4)) > cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userpower").text) Then Response.redirect "showerr.asp?ErrCodes=<li>您的威望数没有达到使用魔法表情的标准，2秒后自动返回上一页面。&action=OtherErr&autoreload=1"
			face = iMagicFace & "|" & face
			tMagicMoney = Rs(0)
			tMagicTicket = Rs(1)
			FoundUseMagic = True
		End If
		Rs.Close
		Set Rs=Nothing
	Else
		face = "0|" & face
	End If
	If width="" or height="" Then
		width=Dvbbs.Forum_Setting(38)
		height=Dvbbs.Forum_Setting(39)
	End If
	If Dvbbs.StrLength(Request.Form("Signature")) > 250 Then
		ErrCodes = ErrCodes + "<li>" + Template.Strings(23)
	ElseIf Dvbbs.StrLength(Request.Form("Signature")) > Cint(Dvbbs.GroupSetting(56)) And Cint(Dvbbs.GroupSetting(56)) > 0 Then
		ErrCodes = ErrCodes + "<li>" + Replace(Template.Strings(23), "250", Cint(Dvbbs.GroupSetting(56)))
	End If
	birthday=trim(Request.Form("birthday"))
	If Not IsDate(birthday) Then birthday=""
	Dim userinfo,useRsetting
	userinfo=checkreal(Request.Form("realname")) & "|||" & checkreal(Request.Form("character")) & "|||" & checkreal(Request.Form("peRsonal")) & "|||" & checkreal(Request.Form("country")) & "|||" & checkreal(Request.Form("province")) & "|||" & checkreal(Request.Form("city")) & "|||" & Request.Form("shengxiao") & "|||" & Request.Form("blood") & "|||" & Request.Form("belief") & "|||" & Request.Form("occupation") & "|||" & Request.Form("marital") & "|||" & Request.Form("education") & "|||" & checkreal(Request.Form("college")) & "|||" & checkreal(Request.Form("userphone")) & "|||" & checkreal(Request.Form("address"))
	userinfo=Server.htmlencode(userinfo)
	usersetting=Request.Form("setuserinfo") & "|||" & Request.Form("setusertrue") & "|||" & showRe & "|||" & Request.Form("userdsindex")
	usersetting=Server.htmlencode(usersetting)
	If ErrCodes<>"" Then Exit Sub
	Set Rs=Dvbbs.iCreateObject("adodb.recordset")
	If Not IsObject(Conn) Then ConnectionDatabase
	sql="Select * from [Dv_User] where userid="&Dvbbs.UserID
	Rs.open sql,conn,1,3
	If Rs.EOF And Rs.BOF Then
		Dvbbs.AddErrCode(12)
	Else
		iFace = Split(Rs("UserFace"),"|")
		If Ubound(iFace) = 1 And Dvbbs.Forum_Setting(98)="1" Then
			If Clng(iFace(0)) <> Clng(Split(Face,"|")(0)) Then
				If cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text) > tMagicMoney Then
					Rs("UserMoney") = Rs("UserMoney") - tMagicMoney
					Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text=CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text) - tMagicMoney
					Dvbbs.ToolsLog -88,1,tMagicMoney,0,1,"使用金币购买魔法头像",Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text & "|" & Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text
				Else
					Rs("UserTicket") = Rs("UserTicket") - tMagicTicket
					Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text=CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text) - tMagicMoney
					Dvbbs.ToolsLog -88,1,0,tMagicTicket,1,"使用点券购买魔法头像",Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text & "|" & Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text
				End If
			End If
		End If
		Rs("UserFace")=face
		'Response.Write("<script>alert('"&face&"');</script>")
		Rs("UserWidth")=Replace(width,"|","")
		Rs("UserHeight")=Replace(height,"|","")
		If Not Dvbbs.FoundIsChallenge Then Rs("UseRsex")=sex
		Rs("UserSign")=Replace(Request.Form("Signature"),"|","")
		Rs("UserPhoto")=Replace(Dv_FilterJS(Request.Form("userphoto")),"|","")

		'判断是否允许提交头衔
		If Cint(Dvbbs.Forum_Setting(6))=1 Then
			CanUseTitle=True 
		End If
		If CanUseTitle and Cint(Dvbbs.Forum_Setting(60))>0 and Rs("UserPost")>Cint(Dvbbs.Forum_Setting(60)) Then
			CanUseTitle1=True 
		ElseIf CanUseTitle and Cint(Dvbbs.Forum_Setting(60))=0 Then
			CanUseTitle1=True 
		Else
			CanUseTitle1=False 
		End If
		If CanUseTitle and Cint(Dvbbs.Forum_Setting(61))>0 and DateDiff("d",Rs("JoinDate"),Now())>Cint(Dvbbs.Forum_Setting(61)) Then
			CanUseTitle2=True 
		ElseIf CanUseTitle and Cint(Dvbbs.Forum_Setting(61))=0 Then
			CanUseTitle2=True 
		Else
			CanUseTitle2=False 
		End If
		If CanUseTitle and Cint(Dvbbs.Forum_Setting(62))=1 Then
			If CanUseTitle1 and CanUseTitle2 Then
				CanUseTitle=True
			Else
				CanUseTitle=False
			End If
		ElseIf CanUseTitle and (CanUseTitle1 or CanUseTitle2) Then
			CanUseTitle=True 
		Else
			CanUseTitle=False
		End If
		usertitle = Replace(Dvbbs.iHtmlencode(Request.Form("title")),"|","")
		If CanUseTitle Then
			If Trim(Dvbbs.Forum_Setting(63))<>"" Then
				SplitUserTitle=split(Dvbbs.Forum_Setting(63),"|")
				For i=0 to ubound(SplitUserTitle)
					If InStr(lcase(usertitle),lcase(SplitUserTitle(i)))>0 Then
						ErrCodes=ErrCodes+"<li>"+template.Strings(24)
						Exit sub
					End If
				Next
			End If
			If Len(usertitle)>Cint(Dvbbs.Forum_Setting(59)) Then
				ErrCodes=ErrCodes+"<li>"+Replace(template.Strings(25),"{$MaxTitleLen}",Dvbbs.Forum_Setting(59))
				Exit Sub
			End If
			Rs("UserTitle")=usertitle
		End If
		If birthday<>"" Then Rs("UserBirthday")=birthday
		Rs("Userinfo")=Userinfo
		Rs("UseRsetting")=Trim(UseRsetting)
		Rs.Update
		usercookies=Request.Form("usercookies")
		If IsNumeric(usercookies) Then usercookies=Cint(usercookies) Else usercookies=0
		Select Case usercookies
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
	End If
	Rs.Close


	Dvbbs.TrueCheckUserLogin()
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(26))
End Sub

Function checkreal(v)
	Dim w
	If not isnull(v) Then
		w=replace(v,"|","")
		checkreal=w
	End If
End Function

'用户头衔输出
Function SetUserTitle(str)
	SetUserTitle=template.html(6)
	SetUserTitle=Replace(SetUserTitle,"{$user_Title}",str)
End Function

'str=0 关闭显示上传头像表单
Function SetUserFace(str,face,wid,hig,mypostnum)
Dim tempstr,facetemp,userregface,i
	tempstr = Split(template.html(7),"||")
	Dvbbs.Forum_userface = split(Dvbbs.Forum_userface,"|||")
	For i = 1 to Ubound(Dvbbs.Forum_userface)-1
		userregface = userregface+"<option value="+Dvbbs.Forum_userface(0)&Dvbbs.Forum_userface(i)
		If trim(lcase(Dvbbs.Forum_userface(0) & Dvbbs.Forum_userface(i))) = trim(lcase(face)) then userregface = userregface+" selected=""selected"" "
		userregface = userregface+">"+Dvbbs.Forum_userface(i)+"</option>"
	Next
	If str = 0 Then
	SetUserFace = tempstr(0) & tempstr(2)
	Else
	SetUserFace = tempstr(0) & tempstr(1) & tempstr(2)
	End If
	SetUserFace=Replace(SetUserFace,"{$Face_select}",userregface)
	SetUserFace=Replace(SetUserFace,"{$color}",Dvbbs.mainsetting(1))
	SetUserFace=Replace(SetUserFace,"{$user_Face}",Dv_FilterJS(face))
	SetUserFace=Replace(SetUserFace,"{$user_FaceWidth}",wid)
	SetUserFace=Replace(SetUserFace,"{$user_FaceHeight}",hig)
	SetUserFace=Replace(SetUserFace,"{$forum_Mwidth}",Dvbbs.Forum_Setting(57))
	SetUserFace=Replace(SetUserFace,"{$forum_Mheight}",Dvbbs.Forum_Setting(57))
	SetUserFace=Replace(SetUserFace,"{$mypostnum}",Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userpost").text)
	SetUserFace=Replace(SetUserFace,"{$facepostnum}",Dvbbs.Forum_Setting(54))
End Function

'下拉菜单转换输出
Function Chk_select(str1,str2)
	Dim k
	str2=Split(str2,",")
	If IsEmpty(str1) Or str1="" Then chk_select="<option value='' selected>...</option>"
	For k=0 to ubound(str2)
		chk_select=chk_select+"<option value="+str2(k)
		If str2(k)=str1 Then chk_select=chk_select+" selected "
		chk_select=chk_select+" >"+str2(k)+"</option>"
	Next
End Function

'多项选取转换输出
Function Chk_KidneyType(str0,str1,str2)
	Dim k
	str2=split(str2,",")
	For k = 0 to ubound(str2)	
		chk_KidneyType=chk_KidneyType+"<input type=""checkbox"" class=""chkbox"" name="""&str0&""" value="""&trim(str2(k))&""" "	 
		If instr(str1,trim(str2(k)))>0 Then '如果有此项性格
		chk_KidneyType=chk_KidneyType + "checked" 
		End If 
		chk_KidneyType=chk_KidneyType + ">"&trim(str2(k))&" "
	If ((k+1) mod 5)=0 Then chk_KidneyType=chk_KidneyType +  "<br>"  '每行显示六个性格进行换行
	Next
End Function

Rem 判断数字是否整形
Function isInteger(Para)
	isInteger=False
	If Not (IsNull(Para) Or Trim(Para)="" Or Not IsNumeric(Para)) Then
		isInteger=True
	End If
End Function

Function Dv_FilterJS(v)
	If  Not Isnull(V) Then
		Dim t
		Dim re
		Dim reContent
		Set re=new RegExp
		re.IgnoreCase =True
		re.Global=True
		re.Pattern="(%)"
		t=re.Replace(v,"<I>%</I>")
		re.Pattern="(&#)"
		t=re.Replace(v,"<I>&#</I>")
		re.Pattern="(script)"
		t=re.Replace(t,"<I>script</I>")
		re.Pattern="(js:)"
		t=re.Replace(t,"<I>js:</I>")
		re.Pattern="(value)"
		t=re.Replace(t,"<I>value</I>")
		re.Pattern="(about:)"
		t=re.Replace(t,"<I>about:</I>")
		re.Pattern="(file:)"
		t=re.Replace(t,"<I>file:</I>")
		re.Pattern="(Document.cookie)"
		t=re.Replace(t,"<I>Documents.cookie</I>")
		re.Pattern="(vbs:)"
		t=re.Replace(t,"<I>vbs:</I>")
		re.Pattern="(on(mouse|Exit|error|click|key))"
		t=re.Replace(t,"<I>on$2</I>")
		Dv_FilterJS=Trim(t)
		Set Re=Nothing
	End If 
End Function
%>