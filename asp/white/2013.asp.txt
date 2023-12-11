<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/chkinput.asp"-->
<%
Dvbbs.LoadTemplates("usermanager")
Dvbbs.Stats=Dvbbs.MemberName&template.Strings(5)
Dvbbs.Nav()
Dvbbs.Head_var 0,0,template.Strings(0),"usermanager.asp"
Response.Write Template.Html(0)
If Dvbbs.Userid=0 Or Dvbbs.Userid="" Then Dvbbs.AddErrCode(6):Dvbbs.Showerr()

Dim ErrCodes,Rs,Sql,Redcolor
Dim UserFavName,FavName_id
Redcolor=Dvbbs.mainsetting(1)

Set Rs=Dvbbs.Execute("Select UserFav From [Dv_User] Where UserID="&Dvbbs.userid)
	If Rs(0)<>"" Then UserFavName=Rs(0) Else UserFavName=""
	Rs.close
Set Rs=Nothing

If Request("fid")<>"" and IsNumeric(Request("fid")) Then
	FavName_id=cint(Request("fid"))
Else
	FavName_id=""
End If

Response.Write "<div id=Fiend_act style=""display:none"">"
Select Case Request("action")
	Case "creat"
		Call creat_fav()
	Case "editfav"
		Call save_fav()
	Case "favdel"
		Call del_fav()
	Case "addF"
		Call saveF()
	Case "saveF"
		Call saveF()
	Case "移动"
		Call MovFriend()
	Case "删除"
		Call DelFriend()
	case "清空好友"
		Call AllDelFriend()	
End Select
If ErrCodes<>"" Then Showerr
Response.Write "</div>"

Call Main()

Dvbbs.Showerr()
Dvbbs.ActiveOnline()
Dvbbs.Footer()
Dvbbs.PageEnd()
'主页面
Sub	Main()
Dim TempLateStr,TempWrite
	TempLateStr=template.html(13)
	TempLateStr=Replace(TempLateStr,"{$TableWidth}",Dvbbs.mainsetting(0))
	TempLateStr=Replace(TempLateStr,"{$fav_name}",UserFavName)
	UserFavName=Split(UserFavName,",")
	TempWrite="<font color="&Redcolor&">"&Ubound(UserFavName)+1&"</font>"
	TempLateStr=Replace(TempLateStr,"{$Fav_Select}",fav_select)
	TempLateStr=Replace(TempLateStr,"{$redcolor}",redcolor)
	TempLateStr=Replace(TempLateStr,"{$Fav_total}",TempWrite)
	TempLateStr=Replace(TempLateStr,"{$FavName_List}",UserFavName_List)
	TempLateStr=Replace(TempLateStr,"{$Friend_list}",UserFriend_List)
	Response.Write TempLateStr
End Sub

Function fav_select()
Dim i
For i=0 to Ubound(UserFavName)
	fav_select=fav_select+"<option value="""&i&""" >"&UserFavName(i)&"</option>"
Next
End Function

Function UserFavName_List
	Dim ShowList,i,ShowName
		If Ubound(UserFavName)<0 Then 
		UserFavName_List="<tr><td height=""20"" colspan=""3"" class=tablebody1 align=center style=""color:"&Redcolor&""">"
		UserFavName_List=UserFavName_List&template.Strings(8)&"</td></tr>"
		Exit Function
		End If
	For i=0 to Ubound(UserFavName)
		ShowList=template.html(14)
		If FavName_id=i Then
			ShowName="<font color="&Redcolor&">"&UserFavName(i)&"</font>"
			ShowList=Replace(ShowList,"{$FavName_pic}",Dvbbs.mainpic(12)) '打开
		Else
			ShowName=UserFavName(i)
			ShowList=Replace(ShowList,"{$FavName_pic}",Dvbbs.mainpic(13)) '关闭
		End If
		ShowList=Replace(ShowList,"{$FavName_Name}",ShowName)
		ShowList=Replace(ShowList,"{$FavName_id}",i)
		UserFavName_List=UserFavName_List+ShowList
	Next
End Function

Function UserFriend_List()
If Dvbbs.chkpost=False Then
	Dvbbs.AddErrCode(16)
	Exit Function
End If
Dim CurrentPage,page_count,totalrec,Pcount,endpage,i
Dim SearchStr,ShowList
Dim Friend_IM,HomePage_Img,Oicq_img,Sms_Img,Msg_Img
CurrentPage = Request("page")
page_count=0
If CurrentPage <> "" And IsNumeric(CurrentPage) Then
	CurrentPage = Clng(CurrentPage)
	Else
	CurrentPage = 1
End If
If Request("action")="search" Then
	If Request("fid")<>"" And IsNumeric(Request("fid")) Then
	SearchStr=SearchStr+" And F_Mod="&cint(Request("fid"))
	End If
	If Request("SearchInfo")<>"" Then
	SearchStr=SearchStr+" And F_friend='"&Dvbbs.Checkstr(Request("SearchInfo"))&"'"
	End If
Else
End If

HomePage_Img = ImgSrc(template.pic(15))
Oicq_img = ImgSrc(template.pic(16))
Msg_Img = ImgSrc(template.pic(17))
Sms_Img = ImgSrc(template.pic(18))

Dim PageListNum
PageListNum=Cint(Dvbbs.Forum_Setting(11))
Sql="select count(F_id) From [Dv_Friend] where F_userid="&Dvbbs.userid&" "&SearchStr
Set Rs=Dvbbs.Execute(Sql)
totalrec=Rs(0)
Rs.close
If totalrec mod PageListNum=0 Then
	Pcount= totalrec \ PageListNum
Else
	Pcount= totalrec \ PageListNum+1
End If
if currentpage > Pcount then currentpage = Pcount
if currentpage<1 then currentpage=1
Set Rs=Dvbbs.iCreateObject("adodb.recordset")
Sql="select F.F_id,F.F_userid,F.F_Friend,F_Mod,U.UserEmail,U.UserIM From [Dv_Friend] F inner join [Dv_user] U on F.F_Friend=U.username where F.F_userid="&Dvbbs.userid&" "&SearchStr
Sql=Sql+" order by F.f_addtime desc"
Rs.Open Sql,Conn,1,1
Dvbbs.SqlQueryNum=Dvbbs.SqlQueryNum+1
If Rs.eof and Rs.bof Then
	UserFriend_List="<tr><td height=""20"" colspan=""7"" class=tablebody1 align=center style=""color:"&Redcolor&""">"
	UserFriend_List=UserFriend_List+template.Strings(8)
	UserFriend_List=UserFriend_List+"</td></tr>"
	Exit Function
Else
	'Rs.MoveFirst
	Rs.Move (currentpage-1) * Cint(PageListNum)
	SQL=Rs.GetRows(PageListNum)
	Rs.Close:Set Rs=Nothing
End If
For i=0 To Ubound(SQL,2)
		ShowList=template.html(15)
		If SQL(5,i)="" or isnull(SQL(5,i)) Then
			ShowList=Replace(ShowList,"{$Friend_HomePage}","")
			ShowList=Replace(ShowList,"{$Friend_Oicq}","无")
		Else
			Friend_IM=split(SQL(5,i),"|||")
			ShowList=Replace(ShowList,"{$Friend_HomePage}",Friend_IM(0))
			If Isnumeric(Friend_IM(1)) Then
				ShowList = Replace(ShowList,"{$Friend_Oicq}","<a target=blank href=http://wpa.qq.com/msgrd?V=1&Uin=" & Friend_IM(1) & "&Site=" & Dvbbs.Forum_Info(0) & "&Menu=yes><img border=0 SRC=http://wpa.qq.com/pa?p=1:" & Friend_IM(1) & ":9 alt=点击这里发消息给" & Friend_IM(1) & "></a>")
			Else
				ShowList = Replace(ShowList,"{$Friend_Oicq}","无")
			End If
		End If
		ShowList=Replace(ShowList,"{$F_id}",SQL(0,i))
		ShowList=Replace(ShowList,"{$FavName}",UserFavName(SQL(3,i)))
		ShowList=Replace(ShowList,"{$Friend_UserName}",SQL(2,i))
		ShowList=Replace(ShowList,"{$Friend_Email}",SQL(4,i)&"")
		ShowList=Replace(ShowList,"{$Img_HomePage}",HomePage_Img)
		ShowList=Replace(ShowList,"{$Img_Oicq}",Oicq_img)
		ShowList=Replace(ShowList,"{$Img_Msg}",Msg_Img)
		'ShowList=Replace(ShowList,"{$Img_sms}",Sms_Img)
		UserFriend_List=UserFriend_List+ShowList
		page_count=page_count+1
Next
UserFriend_List=UserFriend_List+ShowPage(CurrentPage,Pcount,totalrec,PageListNum)
End Function

'图片输出
Function ImgSrc(str)
	If str="" Then Exit Function
	ImgSrc = "<img src="&str&" border=0>"
End Function

'分页输出
Function ShowPage(CurrentPage,Pcount,totalrec,PageNum)
	Dim SearchStr
	SearchStr=Request("action")
	ShowPage=template.html(16)
	ShowPage=Replace(ShowPage,"{$colSpan}",7)
	ShowPage=Replace(ShowPage,"{$CurrentPage}",CurrentPage)
	ShowPage=Replace(ShowPage,"{$Pcount}",Pcount)
	ShowPage=Replace(ShowPage,"{$PageNum}",PageNum)
	ShowPage=Replace(ShowPage,"{$totalrec}",totalrec)
	ShowPage=Replace(ShowPage,"{$SearchStr}",SearchStr)
	ShowPage=Replace(ShowPage,"{$redcolor}",redcolor)
End Function

'创建分组
Sub Creat_fav()
If Dvbbs.chkpost=False Then
	Dvbbs.AddErrCode(16)
	Exit Sub
End If
	Dim fav_name
	fav_name=Dvbbs.checkstr(Replace(Request("FavName"),",",""))
	If fav_name="" Then  
		ErrCodes=ErrCodes+"<li>"+template.Strings(49)
		Exit Sub
	ElseIf strLength(fav_name)>12 Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(42)
		Exit Sub
	Else
		fav_name=","+Dvbbs.htmlencode(Trim(fav_name))
		Sql="Update [Dv_User] Set UserFav=UserFav+'"&fav_name&"' Where UserId="&Dvbbs.UserID
		Set Rs=Dvbbs.Execute(Sql)
		Dvbbs.Dvbbs_Suc("<li>"+template.Strings(48))
	End If
End Sub

'修改分组
Sub save_fav()
If Dvbbs.chkpost=False Then
	Dvbbs.AddErrCode(16)
	Exit Sub
End If
	Dim fav_name
	Dim Old_FavName
	Old_FavName=Split(UserFavName,",")
	fav_name=Dvbbs.checkstr(Request("fav_name"))
	If instr(left(fav_name,1),",") or instr(right(fav_name,1),",") Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(49)
		Exit Sub
	End If
	If strLength(fav_name)>250 or Ubound(Split(fav_name,","))>9 or Ubound(Split(fav_name,","))<Ubound(Old_FavName) Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(42)
		Exit Sub
	End If

	If Replace(fav_name,",","")="" Then  
		ErrCodes=ErrCodes+"<li>"+template.Strings(49)
		Exit Sub
	End If

	Sql="Update [Dv_User] Set UserFav='"&Dvbbs.htmlencode(fav_name)&"' Where UserId="&Dvbbs.UserID
	Set Rs=Dvbbs.Execute(Sql)
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(48))
End Sub

'批量移动
Sub MovFriend()
If Dvbbs.chkpost=False Then
	Dvbbs.AddErrCode(16)
	Exit Sub
End If
	Dim Fav_id
	Dim f_id,fixid
	If Request("Fav_id")<>"" And IsNumeric(Request("Fav_id")) Then
		Fav_id=Cint(Request("Fav_id"))
	Else
		Dvbbs.AddErrCode(35)
		Exit Sub
	End If
	f_id=replace(Request.form("id"),"'","")
	f_id=replace(f_id,";","")
	f_id=replace(f_id,"--","")
	f_id=replace(f_id,")","")
	fixid=replace(f_id,",","")
	fixid=Trim(replace(fixid," ",""))
	If f_id="" or isnull(f_id) Then
		Dvbbs.AddErrCode(35)
		Exit Sub
	ElseIf Not IsNumeric(fixid) Then
		Dvbbs.AddErrCode(35)
		Exit Sub
	Else
		Dvbbs.execute("Update Dv_Friend set F_Mod = "&Fav_id&" where F_userid="&Dvbbs.UserId&" and F_id in ("&f_id&")")
		Dvbbs.Dvbbs_Suc("<li>"+template.Strings(47))
	End If
End Sub

'删除分组
Sub Del_Fav()
Dim Old_FavName,New_FavName,Del_FavName,i
If Dvbbs.chkpost=False Then
	Dvbbs.AddErrCode(16)
	Exit Sub
End If
Old_FavName=Split(UserFavName,",")
Del_FavName=Old_FavName(FavName_id)
For i=0 To Ubound(Old_FavName)
	If Old_FavName(i)<>Del_FavName Then
		New_FavName=New_FavName+Old_FavName(i)
		If i<>Ubound(Old_FavName) Then
			If (i=(Ubound(Old_FavName)-1) and FavName_id=Ubound(Old_FavName)) Then
				New_FavName=New_FavName
			Else
				New_FavName=New_FavName+","
			End If
		End If
	End If
Next
New_FavName = Replace(New_FavName,",,",",")
If instr(left(New_FavName,1),",") Then New_FavName = Replace(New_FavName,",","",1,1)
If Replace(New_FavName,",","")<>"" And FavName_id>2 Then
	Sql="Update [Dv_User] Set UserFav='"&Dvbbs.checkstr(New_FavName)&"' Where UserId="&Dvbbs.UserID
	Set Rs=Dvbbs.Execute(Sql)
	Sql="Delete From Dv_Friend where F_userid="&Dvbbs.UserId&" and F_Mod="&FavName_id
	Set Rs=Dvbbs.Execute(Sql)
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(46))
Else
	ErrCodes=ErrCodes+"<li>"+template.Strings(49)
	Exit Sub
End If
End Sub

'删除好友
Sub DelFriend()
If Dvbbs.chkpost=False Then
	Dvbbs.AddErrCode(16)
	Exit Sub
End If
	Dim delid,fixid
	delid=replace(Request.form("id"),"'","")
	delid=replace(delid,";","")
	delid=replace(delid,"--","")
	delid=replace(delid,")","")
	fixid=replace(delid,",","")
	fixid=Trim(replace(fixid," ",""))
	If delid="" Or isnull(delid) Then
		Dvbbs.AddErrCode(35)
		Exit Sub
	ElseIf Not IsNumeric(fixid) Then
		Dvbbs.AddErrCode(35)
		Exit Sub
	Else
		Dvbbs.execute("Delete From Dv_Friend where F_userid="&Dvbbs.UserId&" and F_id in ("&delid&")")
		Dvbbs.Dvbbs_Suc("<li>"+template.Strings(46))
	End If
End Sub

'清空好友
Sub AllDelFriend()
	If Dvbbs.chkpost=False Then
		Dvbbs.AddErrCode(16)
		Exit Sub
	End If
	Dvbbs.execute("Delete From Dv_Friend where F_userid="&Dvbbs.UserId)
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(45))
	Session("ispost")="0"
End Sub

'保存添加好友
Sub saveF()
If Dvbbs.chkpost=False Then
	Dvbbs.AddErrCode(16)
	Exit Sub
End If
Dim i,incept,Fav_id,Friend_Name

If Request("myFriend")="" Then
	ErrCodes=ErrCodes+"<li>"+template.Strings(35)
	Exit Sub
Else
	incept=Dvbbs.checkStr(Request("myFriend"))
	incept=split(incept,",")
End If
If Request("Fav_id")<>"" And IsNumeric(Request("Fav_id")) then 
	Fav_id=cint(Request("Fav_id"))
Else
	Fav_id=0
End If
For i=0 To ubound(incept)
	Friend_Name=trim(incept(i))
	Sql="select username from [Dv_User] where username='"&Friend_Name&"'"
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.eof and Rs.bof Then
		ErrCodes=ErrCodes+"<li>"+RePlace(template.Strings(41),"{$NoUser}",Friend_Name)
		Exit Sub
	Else
		Friend_Name=rs(0)
	End If
	Rs.close
	If Dvbbs.membername=trim(Friend_Name) Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(40)
		Exit Sub
	End If
	Sql="Select F_id From Dv_Friend Where F_userid="&Dvbbs.userid&" and F_friend='"&Friend_Name&"'"
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.eof and Rs.bof Then
		Sql="Insert into Dv_Friend (F_Userid,F_UserName,F_Friend,F_addTime,F_Mod) values ("& Dvbbs.Userid &",'"& Dvbbs.membername &"','"& Friend_Name &"',"& SqlNowString &","& Fav_id &") "
		DVBBS.execute(sql)
		Else
		ErrCodes=ErrCodes+"<li>"+RePlace(template.Strings(44),"{$IsUser}",Friend_Name)
		Exit Sub
	End If
	If i>4 Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(42)
		Exit Sub
	End If
next
Dvbbs.Dvbbs_Suc("<li>"+template.Strings(43))
End Sub

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
%>