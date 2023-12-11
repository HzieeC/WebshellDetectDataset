<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/dv_clsother.asp"-->
<!--#include file="inc/dv_ubbcode.asp"-->
<%
Dim ErrCodes,Rs,Sql,TempLateStr
Dim AnnounceID,TopicID,UserGroupID,RootID,ReplyID,Topic,Url
Dim abgcolor,dv_ubb
Dim username,PostBuyUser,bgcolor,EmotPath
Dim MailBody,Email,TotalUseTable
Dim T_GetMoneyType,replyid_a,AnnounceID_a,RootID_a
Dim IsThisBoardMaster '确定当前用户是否本版版主，防止下面的操作影响到 Dvbbs.BoardMaster导致出错
IsThisBoardMaster = Dvbbs.BoardMaster

If Request("action")="add" Then
	FavAdd_Main()
ElseIf Request("action")="boke" Then
	FavAdd_Boke()
Else
	Main()
End If
Dvbbs.ActiveOnline()
Dvbbs.Footer()
Dvbbs.PageEnd()

Sub FavAdd_Boke()
	Dvbbs.LoadTemplates("usermanager")
	If Dvbbs.Forum_Setting(99) = "0" Then Response.Redirect "showerr.asp?ErrCodes=<li>本系统未开放博客功能。&action=OtherErr"
	If Dvbbs.UserID = 0 Then Response.Redirect "showerr.asp?ErrCodes=<li>请登录系统后使用此功能。&action=OtherErr"
	Dim TopicMode
	TopicID = Request("ID")
	ReplyID = Request("replyID")
	If TopicID = "" Or Not IsNumeric(TopicID) Then
		Response.Redirect "showerr.asp?ErrCodes=<li>非法的帖子参数。&action=OtherErr"
		Exit Sub
	Else
		TopicID = cCur(TopicID)
		AnnounceID = TopicID
	End If
	If ReplyID = "" Or Not IsNumeric(ReplyID) Then
		Response.Redirect "showerr.asp?ErrCodes=<li>非法的帖子参数。&action=OtherErr"
		Exit Sub
	Else
		ReplyID = cCur(ReplyID)
	End If
	Set Rs=Dvbbs.Execute("Select PostTable,BoardID,TopicMode From Dv_Topic Where TopicID = " & TopicID)
	If Not(Rs.BOF and Rs.EOF) then
		If Rs(1)<>Dvbbs.BoardID Then Dvbbs.AddErrCode(29)
		TotalUseTable = Rs(0)
		TopicMode = Rs(2)
	Else
		Dvbbs.AddErrcode(32)
	End If
	Rs.Close
	Set Rs=Nothing
	Dvbbs.Showerr()
	Dim Body,tRs,iBody
	Set dv_ubb=new Dvbbs_UbbCode
	dv_ubb.PostType=1
	Set Rs=Dvbbs.Execute("Select * From "&TotalUseTable&" Where BoardID = "&Dvbbs.BoardID&" And AnnounceID = "&ReplyID&"")
	If Not(Rs.Bof And Rs.Eof) Then
		If Rs("IsBest") = 1 and Cint(Dvbbs.GroupSetting(41)) = 0 Then Dvbbs.AddErrCode(8)
		If Rs("LockTopic") = 444  Then Dvbbs.AddErrCode(8)
		If Dvbbs.UserID <> Rs("PostUserID") And Cint(Dvbbs.GroupSetting(2)) = 0 Then Dvbbs.AddErrCode(31)
		PostBuyUser=Rs("PostBuyUser")
		If Rs("GetMoneyType") = 3 And Rs("ParentID") = 0 And Not Dvbbs.Boardmaster Then
			If Instr(PostBuyUser,"|||"&Dvbbs.MemberName&"|||")=0 Then Response.Redirect "showerr.asp?ErrCodes=<li>该贴为金币购买贴，您没有浏览此贴的权限。&action=OtherErr"
		End If
		Dvbbs.Showerr()
		If Rs("PostUserID")=0 Then
			UserGroupID = 7
		Else
			Set tRs=Dvbbs.Execute("Select UserGroupID From Dv_User Where UserID = " & Rs("PostUserID"))
			If tRs.Eof And tRs.Bof Then
				UserGroupID = 0
			Else
				UserGroupID = Rs(0)
			End If
		tRs.Close:Set tRs=Nothing
		End If
		ReplyID_a = Rs("AnnounceID")
		AnnounceID_a = Rs("AnnounceID")
		RootID_a = Rs("RootID")
		Ubblists = Rs("Ubblist")
		Topic = Rs("Topic")
		If TopicMode <> "1" Then
			Topic = Replace(Topic,"<","&lt;")
		Else
			If Rs("ParentID")<>"0" Then Topic = Replace(Topic,"<","&lt;")
		End If
		Topic = Dvbbs.ChkBadWords(Topic)
		Topic = Dvbbs.Replacehtml(Topic)
		If Rs("signflag")=2 Then
			UserName = "匿名用户"
		ElseIf UserGroupID = 7 Then
			UserName = "客人"
		Else
			UserName = Dvbbs.ChkBadWords(Dvbbs.HtmlEncode(Rs("UserName")))
		End If
		Body = Dvbbs.ChkBadWords(Rs("Body"))
		If InStr(Ubblists,",39,") > 0  Then
			Body = dv_ubb.Dv_UbbCode(Body,UserGroupID,1,0)
		Else
			Body = dv_ubb.Dv_UbbCode(Body,UserGroupID,1,1)
		End If
		iBody = "标题：" & Topic & "<BR><BR>"
		iBody = iBody & "作者：" & UserName & "<BR><BR>"
		iBody = iBody & Body & "<BR><BR>"
		iBody = iBody & "原贴地址：" & Dvbbs.Get_ScriptNameUrl() & "dispbbs.asp?BoardID="&Dvbbs.BoardID&"&ID="&RootID_a&"&replyID="&ReplyID_a&"&skin=1"
		iBody = Replace(iBody,"onload=""javascript:if(this.width>500)this.style.width=500;""","")
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo").attributes.setNamedItem(Dvbbs.UserSession.createNode(2,"cachebokebody","")).text = iBody
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo").attributes.setNamedItem(Dvbbs.UserSession.createNode(2,"cacheboketopic","")).text = "[转]" & Topic
		'Response.Write Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@cacheboketopic").text
		'Response.Write Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@cachebokebody").text
		'Set Session(Dvbbs.CacheName & "UserID")= Dvbbs.UserSession.cloneNode(True)
		Response.Redirect "BokeManage.asp?s=1&t=1&m=1"
	Else
		Dvbbs.AddErrcode(32)
	End If
	Set dv_ubb=Nothing
	Dvbbs.Showerr()
	Response.End
End Sub

Sub Main()
	Dvbbs.LoadTemplates("usermanager")
	Dvbbs.Stats=Dvbbs.MemberName&template.Strings(6)
	Dvbbs.Nav()
	Dvbbs.Head_var 0,0,template.Strings(0),"usermanager.asp"
	If Dvbbs.UserID=0 Then
		Dvbbs.AddErrCode(6)
		Dvbbs.Showerr()
	End If
	Response.Write Template.Html(0)
	TempLateStr=Split(template.html(17),"||")
	TempLateStr(1)=Replace(TempLateStr(1),"{$fav_del}",template.pic(13))

	Response.Write(TempLateStr(1))

	If request("action")="delet" Then
		call delete()
	Else
		Response.Write TempLateStr(0)
		Response.Write TempLateStr(1)
		call favlist()
	End If
	If ErrCodes<>"" Then Response.redirect "showerr.asp?ErrCodes="&ErrCodes&"&action=OtherErr"
	Dvbbs.Showerr()
End Sub

Sub FavAdd_Main()
	Dvbbs.LoadTemplates("postjob")
	Dvbbs.stats=template.Strings(7)
	Dvbbs.nav()
	If Dvbbs.UserID=0 Then
		Dvbbs.AddErrCode(6)
	End If
	If Request("id")="" Then
		Dvbbs.AddErrCode(43)
	ElseIf Not Isnumeric(Request("id")) Then
		Dvbbs.AddErrCode(30)
	Else
		AnnounceID=Clng(Request("id"))
	End If
	Dvbbs.ShowErr()
	Url = "dispbbs.asp?"
	Url = Url & "boardid="&Dvbbs.BoardID&"&id="&AnnounceID
	Call chkurl()
	Dvbbs.ShowErr()
	Call favadd()
	Dvbbs.ShowErr()
	Dvbbs.head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
	Dvbbs.Dvbbs_suc("<li>"&template.Strings(8))
End Sub


Sub favlist()
	Dim currentPage,page_count,totalrec,Pcount,PageListNum,i
	PageListNum=Cint(Dvbbs.Forum_Setting(11))
	currentPage=Request("page")
	If currentpage="" or not IsNumeric(currentpage) Then
		currentpage=1
	Else
		currentpage=clng(currentpage)
	End If
	set Rs=Dvbbs.iCreateObject("adodb.recordset")
	Sql="Select * From Dv_bookmark Where UserName='"&Dvbbs.membername&"' Order By id Desc"
	Dvbbs.SqlQueryNum=Dvbbs.SqlQueryNum+1
	If Not IsObject(Conn) Then ConnectionDatabase
	Rs.Open SQL,Conn,1,1
	If Rs.eof And Rs.bof Then
		ErrCodes=ErrCodes+"<li>"+template.Strings(50)
		Exit Sub
	Else
		Rs.PageSize = PageListNum
		Rs.AbsolutePage=currentpage
		page_count=0
		totalrec=Rs.recordcount
		Do While Not Rs.eof And (Not page_count = Rs.PageSize)
		Response.Write "<script>dvbbs_favlist_loop('"&rs("url")&"','"&EncodeJS(rs("topic"))&"','"&rs("addtime")&"',"&rs("id")&")</script>"
		page_count = page_count + 1
		Rs.movenext
		Loop
	End If
	Rs.close:Set rs=nothing
	If totalrec mod PageListNum=0 Then
     	Pcount= totalrec \ PageListNum
  	Else
     	Pcount= totalrec \ PageListNum+1
  	End If
	If page_count=0 Then CurrentPage=0
	Response.Write ShowPage(CurrentPage,Pcount,totalrec,PageListNum)
	Response.Write TempLateStr(2)
End Sub

Sub delete()
	If Dvbbs.chkpost=False Then
		Dvbbs.AddErrCode(16)
		Exit Sub
	End If
	If IsNumeric(request("id")) Then
		Sql="Delete From Dv_bookmark where Username='"&Dvbbs.membername&"' And Id="&cstr(request("id"))
		Dvbbs.Execute Sql
	End If
	Dvbbs.Dvbbs_Suc("<li>"+template.Strings(46))
End Sub

'分页输出
Function ShowPage(CurrentPage,Pcount,totalrec,PageNum)
	Dim SearchStr
	SearchStr=Request("action")
	ShowPage=template.html(16)
	ShowPage=Replace(ShowPage,"{$colSpan}",3)
	ShowPage=Replace(ShowPage,"{$CurrentPage}",CurrentPage)
	ShowPage=Replace(ShowPage,"{$Pcount}",Pcount)
	ShowPage=Replace(ShowPage,"{$PageNum}",PageNum)
	ShowPage=Replace(ShowPage,"{$totalrec}",totalrec)
	ShowPage=Replace(ShowPage,"{$SearchStr}",SearchStr)
	ShowPage=Replace(ShowPage,"{$redcolor}",Dvbbs.mainsetting(1))
End Function

Function EncodeJS(str)
	EncodeJS = Replace(Replace(Replace(Replace(str,"\","\\"),"'","\'"),VbCrLf,"\n"),chr(13),"")
End Function

Sub ChkUrl()
	Sql="Select Title From Dv_Topic Where TopicID="&AnnounceID
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.Eof And Rs.Bof Then
		Dvbbs.AddErrCode(48)
	Else
		Topic=Dvbbs.HtmlEnCode(rs(0))
	End If
	Rs.Close:Set Rs=Nothing
End Sub

Sub favadd()
	Sql="Select * From Dv_bookmark Where UserName='"&Dvbbs.Membername&"' And Url='"&Url&"'"
	Set Rs=Dvbbs.iCreateObject("Adodb.Recordset")
	If Not IsObject(Conn) Then ConnectionDatabase
	Rs.Open Sql,Conn,1,3
	If Not (Rs.Eof And Rs.Bof) Then
		Dvbbs.AddErrCode(53)
	Else
		Rs.Addnew
		Rs("username")=Dvbbs.membername
		Rs("topic")=Left(Dvbbs.checkStr(trim(topic)),100)
		Rs("url")=Dvbbs.checkStr(trim(url))
		Rs("addtime")=Now()
		Rs.Update
	End If
	Rs.Close:set Rs=Nothing
End Sub
%>