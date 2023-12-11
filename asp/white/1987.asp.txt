<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<%
Dvbbs.LoadTemplates("dispuser")
Dim ErrCodes,UserName,ShowUserid
UserName=Dvbbs.CheckStr(Request("name"))
If IsNumeric(Request("id")) and Request("id")<>"" Then
	ShowUserid=Clng(Request("id"))
Else
	If UserName="" Then
		Dvbbs.AddErrCode(35)
	End If
End If
If Cint(Dvbbs.GroupSetting(1))=0 Then
	ErrCodes=ErrCodes+"<li>"+template.Strings(1)
End If
Dvbbs.Stats=Replace(template.Strings(0),"{$MemberName}",UserName)
Dvbbs.Nav()
Dvbbs.Head_var 2,0,Dvbbs.Stats,"dispuser.asp"
Dvbbs.Showerr()
If ErrCodes="" Then Main()
Showerr()
Dvbbs.Showerr()
Dvbbs.ActiveOnline
Dvbbs.NewPassword()
Dvbbs.Footer()
Dvbbs.PageEnd()
'0=UserID,1=UserName,2=UserPassword,3=UserEmail,4=UserPost,5=UserTopic,6=UserSign,7=UserSex,8=UserFace,9=UserWidth,10=UserHeight,11=UserIM,12=JoinDate,13=LastLogin,14=UserLogins,15=UserViews,16=Lockuser,17=Userclass,18=UserGroup,19=userWealth,20=userEP,21=userCP,22=UserTitle,23=UserBirthday,24=UserQuesion,25=UserAnswer,26=UserLastIP,27=UserPhoto,28=UserFav,29=UserPower,30=UserDel,31=UserIsBest,32=UserInfo,33=UserSetting,34=UserGroupID,35=TitlePic,36=UserHidden,37=UserMsg,38=IsChallenge,39=UserMobile
 
Sub Main()
Dim RS,SQL,UserInfo,i,UPSQL
Dim TempPart0,TempPart1,TempPart2,TempPart3,TempPart4,TempPart5
Dim FoundUseMagic,iMagicFace
		i	= 0
TempPart0	= template.html(0)
TempPart1	= template.html(1)
TempPart2	= template.html(2)
TempPart3	= template.html(3)
TempPart4	= template.html(4)
TempPart5	= template.html(5)

UPSQL="update [Dv_user] set UserViews=UserViews+1 "
SQL=" Select UserID,UserName,UserPassword,UserEmail,UserPost,UserTopic,UserSign,UserSex,UserFace,UserWidth,UserHeight,UserIM,JoinDate,LastLogin,UserLogins,UserViews,Lockuser,Userclass,UserGroup,userWealth,userEP,userCP,UserTitle,UserBirthday,UserQuesion,UserAnswer,UserLastIP,UserPhoto,UserFav,UserPower,UserDel,UserIsBest,UserInfo,UserSetting,UserGroupID,TitlePic,UserHidden,UserMsg,IsChallenge,UserMobile,RLActTimeT From [Dv_User] "
	If ShowUserid="" Then
		UPSQL=UPSQL + " Where Username='"&UserName&"'"
		SQL=SQL + " Where Username='"&UserName&"'"
	Else
		UPSQL=UPSQL + " Where Userid="&ShowUserid
		SQL=SQL + " Where Userid="&ShowUserid
	End If
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.Eof And Rs.Bof Then
		Dvbbs.AddErrCode(32)
		Exit Sub
	Else
		Dvbbs.Execute(UPSQL)
		'UserInfo=Rs.GetRows(1)
		SQL=Rs.GetString(,1, "@@@", "", "")
		Rs.Close:Set Rs=Nothing
	End if
	UserInfo=Split(Sql,"@@@")
	ShowUserid=Clng(UserInfo(0))
	UserName=UserInfo(1)
	Dim UserStats,UserOnTime
	If Dvbbs.Boardmaster or Dvbbs.Superboardmaster or Dvbbs.Master Then
		Set Rs=Dvbbs.Execute("Select Stats,Startime From Dv_Online Where Userid="&ShowUserid)
	Else
		Set Rs=Dvbbs.Execute("Select Stats,Startime From Dv_Online Where Userid="&ShowUserid&" And Userhidden=2")
	End If
	If Rs.eof and Rs.bof Then
		UserStats=template.Strings(4)
		UserOnTime=template.Strings(4)
	Else
		UserStats=Rs(0)
		UserOnTime=DateDiff("n",Rs(1),Now())
		UserOnTime=Replace(template.Strings(3),"{$UserOnTime}",UserOnTime)
	End If
	Rs.close:Set Rs=Nothing
	Dim UserSetting,SetUserInfo,SetUserTrue
	UserSetting=split(UserInfo(33),"|||")
	If Ubound(UserSetting)>1 Then
		If not isnumeric(UserSetting(0)) Then SetUserInfo=1 Else SetUserInfo=cint(UserSetting(0))
		If not isnumeric(UserSetting(1)) Then SetUserTrue=0 Else SetUserTrue=cint(UserSetting(1))
	Else
		SetUserInfo=1
		SetUserTrue=0
	End If

	'魔法表情部分
	iMagicFace = Split(UserInfo(8),"|")
	If Ubound(iMagicFace) = 1 Then
		UserInfo(8) = iMagicFace(1)
		FoundUseMagic = Clng(iMagicFace(0))
	End If
	If FoundUseMagic > 0 Then
		TempPart0=Replace(TempPart0,"{$usermagicface}",template.html(6))
		TempPart0=Replace(TempPart0,"{$magicfaceid}",FoundUseMagic)
	Else
		TempPart0=Replace(TempPart0,"{$usermagicface}","")
	End If
	TempPart4=Replace(TempPart4,"{$UserName}",UserName)
	TempPart0=Replace(TempPart0,"{$TableWidth}",Dvbbs.mainsetting(0))
	TempPart0=Replace(TempPart0,"{$UserFace}",Dv_FilterJS(UserInfo(8)))
	TempPart0=Replace(TempPart0,"{$UserName}",UserName)
	TempPart0=Replace(TempPart0,"{$WhereUser}",UserStats)
	TempPart0=Replace(TempPart0,"{$Pic_Stats}",template.pic(0))
	TempPart0=Replace(TempPart0,"{$UserStats}",LockUser(UserInfo(16)))
	TempPart0=Replace(TempPart0,"{$UserOnTime}",UserOnTime)
	
	Response.Write TempPart4
	Response.Write TempPart0

'基本资料部分
If SetUserInfo=1 or ShowUserid=Dvbbs.userid Then
	Dim UserIM,Sex,UserPhoto	'UserIM=========HomePage,UserOicq,UserIcq,UserMsn,UserAim,UserYahoo,UserUC
	UserIM=DVbbs.htmlencode(UserInfo(11))
	UserIM=split(UserIM,"|||")
	If not IsArray(UserIM) Then ReDim UserIM(6)
	If UserInfo(7)="1" Then
		Sex=split(template.Strings(5),",")(1)
	Else
		Sex=split(template.Strings(5),",")(0)
	End If
	If UserInfo(27)<>"" Then UserPhoto="<img src="""&Dv_FilterJS(UserInfo(27)) &""" >"
	Rem 老迷加入管理员和超版可以看用户签名内容。2006-1-3
	If Dvbbs.Master Or Dvbbs.Superboardmaster Then
		UserPhoto=UserPhoto&"<br /><b>签 名 内 容</b><br />"& Server.htmlencode(userinfo(6))
	End If 
	TempPart1=Replace(TempPart1,"{$UserBirthday}",UserInfo(23))
	TempPart1=Replace(TempPart1,"{$UserName}",UserName)
	TempPart1=Replace(TempPart1,"{$UserPhoto}",UserPhoto)
	TempPart1=Replace(TempPart1,"{$UserSex}",Sex)
	TempPart1=Replace(TempPart1,"{$Pic_Star}",astro(UserInfo(23)))
	TempPart1=Replace(TempPart1,"{$UserEmail}",UserInfo(3))
	TempPart1=Replace(TempPart1,"{$UserHomePage}",UserIM(0))
	TempPart1=Replace(TempPart1,"{$UserOicq}",UserIM(1))
	TempPart1=Replace(TempPart1,"{$UserIcq}",UserIM(2))
	TempPart1=Replace(TempPart1,"{$UserMsn}",UserIM(3))
	TempPart1=Replace(TempPart1,"{$UserAIM}",UserIM(4))
	TempPart1=Replace(TempPart1,"{$UserYahoo}",UserIM(5))
	TempPart1=Replace(TempPart1,"{$UserUC}",UserIM(6))
	TempPart1=Replace(TempPart1,"{$UserMobile}",UserInfo(39))
	TempPart1=Replace(TempPart1,"{$UserID}",ShowUserid)
	Response.Write TempPart1
End If

'详细资料部分
If SetUserTrue=1 or ShowUserid=Dvbbs.userid Then
	Dim UserTrueInFo
	UserTrueInFo=DVbbs.htmlencode(UserInfo(32))
	UserTrueInFo=Split(UserTrueInFo,"|||")
	If Not IsArray(UserTrueInFo) Or Ubound(UserTrueInFo)<>14 Then ReDim UserTrueInFo(14)
	TempPart2=Replace(TempPart2,"{$UserRealName}",UserTrueInFo(0))
	TempPart2=Replace(TempPart2,"{$UserCharacter}",UserTrueInFo(1))
	TempPart2=Replace(TempPart2,"{$User_Personal}",UserTrueInFo(2))
	TempPart2=Replace(TempPart2,"{$UserCountry}",UserTrueInFo(3))
	TempPart2=Replace(TempPart2,"{$UserProvince}",UserTrueInFo(4))
	TempPart2=Replace(TempPart2,"{$UserCity}",UserTrueInFo(5))
	TempPart2=Replace(TempPart2,"{$UserShengXiao}",UserTrueInFo(6))
	TempPart2=Replace(TempPart2,"{$UserBlood}",UserTrueInFo(7))
	TempPart2=Replace(TempPart2,"{$UserBelief}",UserTrueInFo(8))
	TempPart2=Replace(TempPart2,"{$UserOccupation}",UserTrueInFo(9))
	TempPart2=Replace(TempPart2,"{$UserMarital}",UserTrueInFo(10))
	TempPart2=Replace(TempPart2,"{$UserEducation}",UserTrueInFo(11))
	TempPart2=Replace(TempPart2,"{$UserCollege}",UserTrueInFo(12))
	TempPart2=Replace(TempPart2,"{$UserPhone}",UserTrueInFo(13))
	TempPart2=Replace(TempPart2,"{$UserAddress}",UserTrueInFo(14))
	Response.Write TempPart2
End If

'论坛属性部分
TempPart3=Replace(TempPart3,"{$color}",Dvbbs.mainsetting(1))
REM 修正发帖数为空值时显示出错 2004-5-22 Dv.Yz
If Isnull(UserInfo(4)) Or Not Isnumeric(UserInfo(4)) Then UserInfo(4) = 0
TempPart3=Replace(TempPart3,"{$UserPost}",UserInfo(4))
TempPart3=Replace(TempPart3,"{$UserJoinDate}",UserInfo(12))
TempPart3=Replace(TempPart3,"{$UserLastLogin}",UserInfo(13))
TempPart3=Replace(TempPart3,"{$UserLogins}",UserInfo(14))
TempPart3=Replace(TempPart3,"{$UserClass}",UserInfo(17))

TempPart3=Replace(TempPart3,"{$UserWealth}",UserInfo(19))
TempPart3=Replace(TempPart3,"{$UserEP}",UserInfo(20))
TempPart3=Replace(TempPart3,"{$UserCP}",UserInfo(21))
TempPart3=Replace(TempPart3,"{$UserPower}",UserInfo(29))
If UserInfo(4)<>0 Or UserInfo(30) *(-1) <> 0 Then
	TempPart3=Replace(TempPart3,"{$UserDelPC}",FormatPercent(UserInfo(30) *(-1)/(UserInfo(4)+(UserInfo(30) *(-1)))))
Else
	TempPart3=Replace(TempPart3,"{$UserDelPC}",UserInfo(30))
End If
TempPart3=Replace(TempPart3,"{$UserDel}",UserInfo(30) *(-1))
TempPart3=Replace(TempPart3,"{$UserBest}",UserInfo(31))
TempPart3=Replace(TempPart3,"{$UserStock}",0)
TempPart3=Replace(TempPart3,"{$UserBank}",0)
TempPart3=Replace(TempPart3,"{$UserAssets}",UserInfo(19))
TempPart3=Replace(TempPart3,"{$UserAdmin}",GetAdminBoard(UserInfo(34),UserName))
Response.Write TempPart3

'快捷管理选项部分
If Dvbbs.Superboardmaster or Dvbbs.Master or (Dvbbs.GroupSetting(43)=1 and Dvbbs.GroupSetting(28)=1 and Dvbbs.GroupSetting(29)=1 and Dvbbs.GroupSetting(30)=1) Then
	TempPart5=Replace(TempPart5,"{$UserName}",UserName)
	TempPart5=Replace(TempPart5,"{$UserID}",ShowUserid)
	TempPart5=Replace(TempPart5,"{$UserIP}",UserInfo(26))
	TempPart5=Replace(TempPart5,"{$UseTable}",UseTable)
	Response.Write TempPart5
End IF
End Sub

'(用户组ＩＤ，用户名)
Function GetAdminBoard(UserGroupID,username)
	Dim Srs,BoardMaster,i,ii,MyBoardMaster
	ii=0
	GetAdminBoard="<font color=gray>无职务</font>"
	If UserGroupID=1 Then
		GetAdminBoard="论坛管理员"
	ElseIf UserGroupID<=3 Then
		GetAdminBoard=""
		Set Srs=Dvbbs.Execute("Select Boardmaster,Boardid,Boardtype From Dv_Board Where Boardmaster<>'' Order By Rootid,Orders")
		If not Srs.eof Then
			BoardMaster=Srs.GetRows(-1)
		Srs.Close:Set Srs=Nothing
		For i=0 to Ubound(BoardMaster,2)
			MyBoardMaster="|" & Trim(BoardMaster(0,i)) & "|"
			If instr(MyBoardMaster,"|" & username & "|")>0 Then
			ii=ii+1
				GetAdminBoard=GetAdminBoard&(ii)&": <a href=list.asp?boardid="&BoardMaster(1,i)&">"&BoardMaster(2,i)&"</a>  版主<br>"
			End If
			MyBoardMaster=""
		Next
		End if
		If GetAdminBoard="" Then GetAdminBoard="<font color=gray>无职务</font>"
	End If
End Function

'用户状态验证
Function LockUser(str)
If not IsNumeric(str) Then Exit Function
	Select case Cint(str)
	case 1 
		LockUser="锁定"
	case 2
		LockUser="屏蔽"
	case else
		LockUser="正常"
	End Select
End Function

'数据帖子列表
Function UseTable()
	DIM RS,SQL,i
	SET RS=Dvbbs.Execute("Select TableName,TableType From Dv_TableList")
	If Not RS.EOF Then
		SQL=RS.GetRows(-1)
		RS.Close:Set RS=Nothing
		For i=0 to Ubound(SQL,2)
			UseTable=UseTable+"<option value="""&SQL(0,i)&""" "
			If Dvbbs.nowusebbs=SQL(0,i) Then UseTable=UseTable+"selected"
			UseTable=UseTable+">"&SQL(1,i)&"</option>"
		Next
	End If
End Function

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
'日期转换星座函数
'白羊座,金牛座,双子座,巨蟹座,狮子座,处女座,天秤座,天蝎座,射手座,魔羯座,水瓶座,双鱼座
function astro(birth)
if birth="" or not isdate(birth) Then birth=now()
Dim birthday,birthmonth
Dim Star_name,Star_src
Star_name=Split(template.Strings(6),",")
IF not IsArray(Star_name) Then ReDim Star_name(12)
	birthday=day(birth)
	birthmonth=month(birth)
	select case birthmonth
	case 1
		if birthday>=21 then
			astro="<img src="""&template.pic(11)&""" alt="&Star_name(10)&birth&">"
		else
			astro="<img src="""&template.pic(10)&""" alt="&Star_name(9)&birth&">"
		end if
	case 2
		if birthday>=20 then
			astro="<img src="""&template.pic(12)&""" alt="&Star_name(11)&birth&">"
		else
			astro="<img src="""&template.pic(10)&""" alt="&Star_name(10)&birth&">"
		end if
	case 3
		if birthday>=21 then
			astro="<img src="""&template.pic(1)&""" alt="&Star_name(0)&birth&">"
		else
			astro="<img src="""&template.pic(12)&""" alt="&Star_name(11)&birth&">"
		end if
	case 4
		if birthday>=21 then
			astro="<img src="""&template.pic(2)&""" alt="&Star_name(1)&birth&">"
		else
			astro="<img src="""&template.pic(1)&""" alt="&Star_name(0)&birth&">"
		end if
	case 5
		if birthday>=22 then
			astro="<img src="""&template.pic(3)&""" alt="&Star_name(2)&birth&">"
		else
			astro="<img src="""&template.pic(2)&""" alt="&Star_name(1)&birth&">"
		end if
	case 6
		if birthday>=22 then
			astro="<img src="""&template.pic(4)&""" alt="&Star_name(3)&birth&">"
		else
			astro="<img src="""&template.pic(3)&""" alt="&Star_name(2)&birth&">"
		end if
	case 7
		if birthday>=23 then
			astro="<img src="""&template.pic(5)&""" alt="&Star_name(4)&birth&">"
		else
			astro="<img src="""&template.pic(4)&""" alt="&Star_name(3)&birth&">"
		end if
	case 8
		if birthday>=24 then
			astro="<img src="""&template.pic(6)&""" alt="&Star_name(5)&birth&">"
		else
			astro="<img src="""&template.pic(5)&""" alt="&Star_name(4)&birth&">"
		end if
	case 9
		if birthday>=24 then
			astro="<img src="""&template.pic(7)&""" alt="&Star_name(6)&birth&">"
		else
			astro="<img src="""&template.pic(6)&""" alt="&Star_name(5)&birth&">"
		end if
	case 10
		if birthday>=24 then
			astro="<img src="""&template.pic(8)&""" alt="&Star_name(7)&birth&">"
		else
			astro="<img src="""&template.pic(7)&""" alt="&Star_name(6)&birth&">"
		end if
	case 11
		if birthday>=23 then
			astro="<img src="""&template.pic(9)&""" alt="&Star_name(8)&birth&">"
		else
			astro="<img src="""&template.pic(8)&""" alt="&Star_name(7)&birth&">"
		end if
	case 12
		if birthday>=22 then
			astro="<img src="""&template.pic(10)&""" alt="&Star_name(9)&birth&">"
		else
			astro="<img src="""&template.pic(9)&""" alt="&Star_name(8)&birth&">"
		end if
	case else
		astro=""
	end select
end function
%>