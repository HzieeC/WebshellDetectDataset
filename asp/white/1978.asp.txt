<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/dv_clsother.asp"-->
<%
Dvbbs.LoadTemplates("fmanage")
Dvbbs.Stats=template.Strings(1)
Dvbbs.Nav()
Dim rootid 
Dim id
Dim Lasttopic,Lastpost
Dim lastrootid,lastpostuser
Dim ip,url
Dim title,content
Dim TotalUseTable
Dim ars
Dim rs,sql,i,sucmsg,ErrCodes
ip=Dvbbs.UserTrueIP
If Dvbbs.UserID=0 Then Dvbbs.AddErrCode(6)
If Not Dvbbs.ChkPost() Then Dvbbs.AddErrCode(16)
Dim UpdateBoardID,UpdateBoardID_1
If Dvbbs.BoardParentID = 0 Then
	UpdateBoardID=Dvbbs.BoardID
Else
	UpdateBoardID=Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@parentstr").text & "," & Dvbbs.BoardID
End If
If Request.form("announceid")="" Then Dvbbs.AddErrCode(30)
Dim CanBatchTopic
CanBatchTopic=False
If Dvbbs.BoardID = 0 Then
	Dvbbs.Head_var 0,0,template.Strings(0),"admin_batch.asp"
Else
	Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
End If
If (Dvbbs.master or Dvbbs.superboardmaster or Dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(45))=1 Then CanBatchTopic=true
If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(45))=1 Then
	CanBatchTopic=true
ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(45))=0 Then
	CanBatchTopic=false
End If
If not CanBatchTopic Then Dvbbs.AddErrCode(28)
Dvbbs.ShowErr()
Main
Dvbbs.ActiveOnline
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub Main
	Select Case Request("action")
	Case "lock"
		Topic_Batch_Lock
	Case "dele"
		Topic_Batch_Delete
	Case "move"
		Topic_Batch_Move
	Case "istop"
		'Topic_Batch_Istop
	Case "isbest"
		Topic_Batch_Isbest
	Case "topicmode"
		Topic_SetTopicMode
	Case Else
		Dvbbs.AddErrCode(35)
	End Select
	If ErrCodes<>"" Then Response.redirect "showerr.asp?BoardID="&Dvbbs.BoardID&"ErrCodes="&ErrCodes&"&action=OtherErr"
	Dvbbs.ShowErr
End Sub

'批量专辑操作
Function Topic_SetTopicMode
Dim TopModeID
If Request.form("mode")<>"" and IsNumeric(Request.form("mode")) Then
	TopModeID=Cint(Request.form("mode"))
Else
	ErrCodes=ErrCodes+"<li>"+template.Strings(2)
	Exit Function
End If
Dim ID
For I=1 To Request.Form("Announceid").Count
	ID=Replace(Request.Form("Announceid")(I),"'","")
	ID=CLng(ID)
	Dvbbs.Execute("Update Dv_Topic Set Mode="&TopModeID&" Where BoardID="&Dvbbs.BoardID&" And TopicID=" & ID)
Next
	SQL="insert into Dv_Log (L_AnnounceID,L_BoardID,L_ToUser,L_UserName,L_Content,L_IP,l_type) Values ("&Id&","&Dvbbs.BoardID&",'More','"&Dvbbs.Membername&"','批量专辑操作','"&Ip&"',3)"
	Dvbbs.Execute(SQL)
	sucmsg="<li>批量专辑操作成功。<li>您的操作信息已经记录在案。"
	Dvbbs.Dvbbs_suc(sucmsg)
End Function

'批量锁定
Function Topic_Batch_Lock
	Dim ID
	For I=1 To Request.Form("Announceid").Count
		ID=Replace(Request.Form("Announceid")(I),"'","")
		ID=CLng(ID)
		Dvbbs.Execute("update dv_Topic Set LockTopic=1 Where BoardID="&Dvbbs.BoardID&" And TopicID=" & ID)
	Next
	SQL="insert into Dv_Log (L_AnnounceID,L_BoardID,L_ToUser,L_UserName,L_Content,L_IP,l_type) Values ("&Id&","&Dvbbs.BoardID&",'More','"&Dvbbs.Membername&"','批量锁定','"&Ip&"',3)"
	Dvbbs.Execute(SQL)
	sucmsg="<li>帖子批量锁定成功。<li>您的操作信息已经记录在案。"
	Dvbbs.Dvbbs_suc(sucmsg)
End Function

'批量删除
Function Topic_Batch_Delete
	Dim ID,toprs,iBoardTopStr
	Dim ChildNum,istop,Forum_AllTopNum,j,AllTopNum,Board_TopNum,TopNum,BoardTopStr
	ChildNum = 0
	AllTopNum = Dvbbs.CacheData(28,0)
	Board_TopNum = Application(Dvbbs.CacheName &"_information_" & Dvbbs.Boardid).documentElement.selectSingleNode("information/@boardtopstr").text
	For I=1 To Request.Form("Announceid").Count
		ID=Replace(Request.Form("Announceid")(I),"'","")
		ID=CLng(ID)
		'更新发贴用户数据
		Set Rs=Dvbbs.Execute("Select PostTable,Child,istop from dv_Topic Where BoardID="&Dvbbs.BoardID&" And TopicID="&ID)
		If Not Rs.Eof Then
		TotalUseTable=Rs(0)
		ChildNum=ChildNum+Rs(1)
		istop=Rs(2)
		Dvbbs.Execute("Update [dv_User] Set Userwealth=Userwealth-"&Dvbbs.Forum_user(3)&",Usercp=Usercp-"&Dvbbs.Forum_user(13)&",Userep=Userep-"&Dvbbs.Forum_user(8)&" Where Userid In (Select PostUserID From "&TotalUseTable&" Where BoardID="&Dvbbs.BoardID&" And Rootid="&ID&")")

		'更新帖子数据
		Dvbbs.Execute("Update "&TotalUseTable&" Set Locktopic=BoardID,BoardID=444,isbest=0 Where BoardID="&Dvbbs.BoardID&" And RootID=" & ID)
		Dvbbs.Execute("update dv_Topic Set Locktopic=BoardID,BoardID=444,isbest=0 Where BoardID="&Dvbbs.BoardID&" And TopicID="&ID)
		'更新上传附件数据
		Dvbbs.Execute("update Dv_Upfile Set F_flag=4 Where F_BoardID="&Dvbbs.BoardID&" And F_AnnounceID LIKE  '"&ID&"|"&"%' ")
		Dvbbs.Execute("delete from dv_besttopic where rootid="&ID)
		End If
		'判断是否为固顶贴，是则更新固顶数据
		If istop=3 Then
			'将总固顶ID从总设置表去除
			Forum_AllTopNum=Split(AllTopNum,",")
			TopNum=""
			For j=0 to UBound(Forum_AllTopNum)
				If CStr(ID)<> CStr(Trim(Forum_AllTopNum(j))) Then
					If TopNum="" Then
						TopNum=Trim(Forum_AllTopNum(j))
					Else
						TopNum=TopNum&","&Trim(Forum_AllTopNum(j))
					End If
				End If
			Next
			AllTopNum=TopNum
		ElseIf istop=1 Then
			'固顶贴去除版面固顶ID
			Forum_AllTopNum=Split(Board_TopNum,",")
			TopNum=""
			For j=0 to UBound(Forum_AllTopNum)
				If CStr(ID)<> CStr(Trim(Forum_AllTopNum(j))) Then
					If TopNum="" Then
						TopNum=Trim(Forum_AllTopNum(j))
					Else
						TopNum=TopNum&","&Trim(Forum_AllTopNum(j))
					End If
				End If
			Next
			Board_TopNum=TopNum
		ElseIf istop=2 Then
			'查询此贴区固顶的所有版面ID
			Set toprs=Dvbbs.Execute("Select BoardID,BoardTopStr From Dv_Board Where BoardTopStr Like '%"&ID&"%'")
			Do While Not TopRs.Eof
			If TopRs(1)="" Or IsNull(TopRs(1)) Then
				iBoardTopStr = ""
			Else
				If InStr(","&TopRs(1)&",",","&ID&",")>0 Then
					BoardTopStr = "," & topRs(1) & ","
					BoardTopStr = Split(BoardTopStr,",")
					For j = 1 To Ubound(BoardTopStr)-1
						If Cstr(Trim(BoardTopStr(j)))<>Cstr(ID) Then
							If iBoardTopStr="" Then
								iBoardTopStr = BoardTopStr(j)
							Else
								iBoardTopStr = iBoardTopStr & "," & BoardTopStr(j)
							End If
						End If
															
					Next
				Else
					iBoardTopStr = topRs(1)
				End If
			End If
			Dvbbs.Execute("Update Dv_Board Set BoardTopStr='"&iBoardTopStr&"' Where BoardID="&TopRs(0))
			Dvbbs.ReloadBoardInfo  TopRs(0)
			BoardTopStr = ""
			iBoardTopStr = ""
			topRs.Movenext
			Loop
		End If
	Next
	'总固顶数据发生变化,则更新数据
	If AllTopNum <> Dvbbs.CacheData(28,0) Then
		Dvbbs.Execute("Update Dv_Setup Set Forum_AllTopNum='"&AllTopNum&"'")
		Dvbbs.ReloadSetupCache AllTopNum,28
	End If
	'固顶数据发生变化,则更新数据
	If Board_TopNum <> Application(Dvbbs.CacheName &"_information_" & Dvbbs.Boardid).documentElement.selectSingleNode("information/@boardtopstr").text Then
		Dvbbs.Execute("Update Dv_Board Set BoardTopStr='"&Board_TopNum&"' Where BoardID="&Dvbbs.boardid)
	End If
	'更新版面和总数据
	Set Rs=Dvbbs.Execute("Select ParentStr,ParentID from dv_Board Where BoardID="&Dvbbs.BoardID)
	If Rs(1)=0 Then
		UpdateBoardID = Dvbbs.BoardID
	Else
		UpdateBoardID = Rs(0) & "," & Dvbbs.BoardID
	End If
	Del_Math_Forum_Count ChildNum,i-1
	Math_Forum_Today(Dvbbs.BoardID)
	LastCount(Dvbbs.BoardID)
	SQL="insert into Dv_Log (L_AnnounceID,L_BoardID,L_ToUser,L_UserName,L_Content,L_IP,l_type) Values ("&ID&","&Dvbbs.BoardID&",'More','"&Dvbbs.Membername&"','批量删除','"&IP&"',3)"
	Dvbbs.Execute(SQL)
	sucmsg="<li>帖子批量删除成功。<li>您的操作信息已经记录在案。"

	Dvbbs.ReloadBoardInfo(UpdateBoardID)
	Dvbbs.loadSetup()
	Dvbbs.Dvbbs_suc(sucmsg)
	Set Rs=Nothing
End Function

Function Topic_Batch_Move
	Dim ID,Newboard,Trs,ChildNum
	Dim F_announceID,SplitUpBoardID
	ChildNum=0
	If Request.form("newboard")="" or isnull(Request.form("newboard")) or not isnumeric(Request.form("newboard")) Then
		Response.redirect "showerr.asp?ErrCodes=<li>如果您是批量转移帖子请选择相关论坛。&action=OtherErr"
		Exit Function
	ElseIf Cint(Request.form("newboard"))=Cint(Dvbbs.BoardID) Then
		Response.redirect "showerr.asp?ErrCodes=<li>请不要选择相同的论坛进行转移操作。&action=OtherErr"
		Exit Function
	Else
		newboard=Request.form("newboard")
	End If

	For i=1 To Request.Form("Announceid").Count
		Id=Replace(Request.Form("Announceid")(i),"'","")
		ID=CLng(ID)
		Set Rs=Dvbbs.Execute("Select PostTable,Isbest,Child,IsTop from dv_Topic Where BoardID="&Dvbbs.BoardID&" And TopicID="&ID)
		If Not(Rs.Eof And Rs.Bof) Then
			TotalUseTable = Rs(0)
			ChildNum = ChildNum + Rs(2)
			Dvbbs.Execute("UPDATE " & TotalUseTable & " SET BoardID = " & Newboard & " WHERE BoardID = " & Dvbbs.BoardID & " AND RootID = " & ID)
			'更新版块ID，并取消专题所属
			Dvbbs.Execute("UPDATE Dv_Topic SET BoardID = " & Newboard & ",mode=0 WHERE BoardID = " & Dvbbs.BoardID & " AND TopicID = " & ID)
			If Rs(1) = 1 Then
				Dvbbs.Execute("UPDATE Dv_Besttopic Set BoardID = " & Newboard & " WHERE BoardID = " & Dvbbs.BoardID & " AND RootID = " & ID)
			End If
			'移动时判断是否固顶并作相关处理 2004-4-25 Dvbbs.YangZheng
			If Rs("istop") > 0 Then
				Dim Yrs, TopstrinfoN, TopstrinfoO
				'读取新旧版面的固顶信息
				Set Yrs = Dvbbs.Execute("SELECT BoardTopStr From Dv_Board Where Boardid = " & Dvbbs.Boardid)
				TopstrinfoO = Yrs(0)
				Set Yrs = Dvbbs.Execute("SELECT BoardTopStr From Dv_Board Where Boardid = " & Newboard)
				TopstrinfoN = Yrs(0)
				Yrs.Close:Set Yrs = Nothing
				'删除原固顶主题ID
				TopstrinfoO = Replace(TopstrinfoO, Cstr(ID)&",", "")
				TopstrinfoO = Replace(TopstrinfoO, ","&Cstr(ID), "")
				TopstrinfoO = Replace(TopstrinfoO, Cstr(ID), "")
				If TopstrinfoN = "" Or Isnull(TopstrinfoN) Then
					TopstrinfoN = Cstr(ID)
				ElseIf TopstrinfoN = Cstr(ID) Then
					TopstrinfoN = TopstrinfoN
				ElseIf Instr(TopstrinfoN, ","&Cstr(ID)) > 0 Then
					TopstrinfoN = TopstrinfoN
				Else
					TopstrinfoN = TopstrinfoN & "," & Cstr(ID)
				End If
				'更新当前版面固顶信息及缓存
				Sql = "UPDATE Dv_Board SET BoardTopStr = '" & TopstrinfoO & "' WHERE BoardID = " & Dvbbs.Boardid
				Dvbbs.Execute(Sql)
				'更新新版面固顶信息及缓存
				Sql = "UPDATE Dv_Board SET BoardTopStr = '" & TopstrinfoN & "' WHERE Boardid = " & Newboard
				Dvbbs.Execute(Sql)
			End If
			'shinzeal加入批量移动上传文件数据
			F_announceID = ID & "|"
			Dvbbs.Execute("UPDATE DV_Upfile SET F_Boardid = " & Newboard & " WHERE F_boardid = " & Dvbbs.Boardid & " AND F_AnnounceID LIKE '" & F_announceID & "%'")
			'shinzeal加入批量移动上传文件数据结束
		End If
	Next
	'更新版面和总数据
	Dim TempUpdateBoardID
	Set Rs=Dvbbs.Execute("Select ParentStr from dv_Board Where BoardID="&Dvbbs.BoardID)
	UpdateBoardID=Rs(0) & "," & Dvbbs.BoardID
	Rs.Close
	TempUpdateBoardID = UpdateBoardID
	Del_Math_Forum_Count ChildNum,i-1
	LastCount(Dvbbs.BoardID)
	Math_Forum_Today(Dvbbs.BoardID)
	'Dvbbs.ReloadBoardInfo(UpdateBoardID)
	Set Rs=Dvbbs.Execute("Select ParentStr from dv_Board Where BoardID="&Newboard)
	UpdateBoardID=Rs(0) & "," & Newboard
	Rs.Close
	Set Rs=Nothing
	TempUpdateBoardID = TempUpdateBoardID & "," & UpdateBoardID
	Add_Math_Forum_Count ChildNum,i
	LastCount(Newboard)
	Math_Forum_Today(Newboard)
	Dvbbs.ReloadBoardInfo(TempUpdateBoardID)
	SQL="insert into Dv_Log (L_AnnounceID,L_BoardID,L_ToUser,L_UserName,L_Content,L_IP,l_type) Values ("&ID&","&Dvbbs.BoardID&",'More','"&Dvbbs.Membername&"','批量移动','"&IP&"',3)"
	Dvbbs.Execute(SQL)
	sucmsg="<li>帖子批量移动成功。<li>您的操作信息已经记录在案。"
	Dvbbs.Dvbbs_suc(sucmsg)
	Dvbbs.loadSetup()
End Function

Function Topic_Batch_Istop
	Dim ID
	For I=1 To Request.Form("Announceid").Count
		ID=replace(Request.form("Announceid")(i),"'","")
		ID=CLng(ID)
		If IsSqlDataBase=1 Then
		Dvbbs.Execute("update dv_Topic Set Istop=1,LastPostTime=Dateadd(day,100,LastPostTime) Where BoardID="&Dvbbs.BoardID&" And TopicID="&ID)
		else
		Dvbbs.Execute("update dv_Topic Set Istop=1,LastPostTime=Dateadd('d',100,LastPostTime) Where BoardID="&Dvbbs.BoardID&" And TopicID="&ID)
		end if
	Next
	SQL="insert into Dv_Log (L_AnnounceID,L_BoardID,L_ToUser,L_UserName,L_Content,L_IP,l_type) Values ("&ID&","&Dvbbs.BoardID&",'More','"&Dvbbs.Membername&"','批量固顶','"&IP&"',4)"
	Dvbbs.Execute(SQL)
	sucmsg="<li>帖子批量固顶成功。<li>您的操作信息已经记录在案。"
	Dvbbs.ReloadBoardInfo(Dvbbs.BoardID)
	Dvbbs.Dvbbs_suc(sucmsg)
End Function

Function Topic_Batch_Isbest
	Dim ID
	For I=1 To Request.Form("Announceid").Count
		ID=replace(Request.form("Announceid")(i),"'","")
		ID=CLng(ID)
		Set Rs=Dvbbs.Execute("Select PostTable from dv_Topic Where BoardID="&Dvbbs.BoardID&" And TopicID="&ID)
		TotalUseTable=Rs(0)
		Set Rs=Dvbbs.Execute("Select Top 1 * From "&TotalUseTable&" Where RootID="&ID&" Order By AnnounceID")
		If Not (Rs.Eof And Rs.Bof) Then
		SQL="insert into Dv_bestTopic (Title,BoardID,Announceid,Rootid,Postusername,Postuserid,Dateandtime,Expression) Values ('"&Dvbbs.Checkstr(Rs("Topic"))&"',"&Rs("BoardID")&","&Rs("Announceid")&","&Rs("Rootid")&",'"&Dvbbs.Checkstr(Rs("Username"))&"',"&Rs("Postuserid")&",'"&Rs("Dateandtime")&"','"&Rs("Expression")&"')"
		Dvbbs.Execute(SQL)
		Dvbbs.Execute("Update "&TotalUseTable&" Set Isbest=1 Where AnnounceID=" & Rs("AnnounceID"))
		Dvbbs.Execute("update dv_Topic Set Isbest=1 Where TopicID="&ID)
		End If
	Next
	SQL="insert into Dv_Log (L_AnnounceID,L_BoardID,L_ToUser,L_UserName,L_Content,L_IP,l_type) values ("&ID&","&Dvbbs.BoardID&",'more','"&Dvbbs.Membername&"','批量精华','"&IP&"',3)"
	Dvbbs.Execute(SQL)
	sucmsg="<li>帖子批量精华成功。<li>您的操作信息已经记录在案。"
	Dvbbs.Dvbbs_suc(sucmsg)
	Set Rs=Nothing
End Function

'更新指定论坛信息
Function LastCount(BoardID)
	Dim LastTopic,body,LastRootid,LastPostTime,LastPostUser
	Dim LastPost,uploadpic_n,Lastpostuserid,Lastid
	Set Rs=Dvbbs.Execute("Select Top 1 T.title,B.Announceid,B.Dateandtime,B.Username,B.Postuserid,B.Rootid From "&dvbbs.NowUseBBS&" b Inner Join dv_Topic T On b.rootid=T.TopicID Where B.BoardID="&BoardID&" Order By B.Announceid Desc")
	If Not(Rs.Eof And Rs.Bof) Then
		Lasttopic=replace(left(Rs(0),15),"$","")
		LastRootid=Rs(1)
		LastPostTime=Rs(2)
		LastPostUser=Rs(3)
		LastPostUserid=Rs(4)
		Lastid=Rs(5)
	Else
		LastTopic="无"
		LastRootid=0
		LastPostTime=now()
		LastPostUser="无"
		LastPostUserid=0
		Lastid=0
	End If
	Set Rs=Nothing
	LastPost=LastPostUser & "$" & LastRootid & "$" & LastPostTime & "$" & LastTopic & "$" & uploadpic_n & "$" & LastPostUserID & "$" & LastID & "$" & BoardID
	LastPost = Dvbbs.CheckStr(LastPost)
	Dim SplitUpBoardID,SplitLastPost
	SplitUpBoardID=Split(UpdateBoardID,",")
	For i=0 to Ubound(SplitUpBoardID)
		Set Rs=Dvbbs.Execute("Select LastPost from dv_Board Where BoardID="&SplitUpBoardID(i))
		If Not (Rs.Eof And Rs.Bof) Then
			SplitLastPost=Split(Rs(0),"$")
			If SplitLastPost(1)="" or Isnull(SplitLastPost(1)) Then SplitLastPost(1)=0
			If Ubound(SplitLastPost)=7 and Clng(LastRootID)<>Clng(SplitLastPost(1)) Then
				Dvbbs.Execute("update dv_Board Set LastPost='"&LastPost&"' Where BoardID="&SplitUpBoardID(i))
			End If
		End If
	Next
	Set Rs=Nothing
End Function

Function Del_Math_Forum_Count(TopicNum,BBSNUM)
	Dvbbs.Execute("Update dv_setup Set Forum_postNum=Forum_postNum-"&TopicNum&",Forum_TopicNum=Forum_TopicNum-"&BBSNUM&"")
	Dvbbs.Execute("update dv_Board Set postnum=postnum-"&TopicNum&",topicnum=topicnum-"&BBSNUM&" Where BoardID In ("&UpdateBoardID&")")
End Function

Function Add_Math_Forum_Count(TopicNum,BBSNUM)
	Dvbbs.Execute("Update dv_setup Set Forum_postNum=Forum_postNum+"&TopicNum&",Forum_TopicNum=Forum_topicNum+"&BBSNUM&"")
	Dvbbs.Execute("update dv_Board Set postnum=postnum+"&TopicNum&",topicnum=topicnum+"&BBSNUM&" Where BoardID In ("&UpdateBoardID&")")
End Function


'今日帖子
Function Math_Forum_Today(BoardID)
	Dim MathForumToday
	if IsSqlDataBase=1 then
	Set Rs=Dvbbs.Execute("Select Count(*) From "&dvbbs.NowUseBBS&" Where BoardID="&BoardID&" And Datediff(d,Dateandtime,"&SqlNowString&")=0")
	else
	Set Rs=Dvbbs.Execute("Select Count(*) From "&dvbbs.NowUseBBS&" Where BoardID="&BoardID&" And Datediff('d',Dateandtime,"&SqlNowString&")=0")
	end if
	MathForumToday=Rs(0)
	Set Rs=Nothing
	If Isnull(MathForumToday) Then MathForumToday=0
	Dvbbs.Execute("update dv_Board Set Todaynum="&MathForumToday&" Where Boardid="&BoardID)
	if IsSqlDataBase=1 then
	Set Rs=Dvbbs.Execute("Select Count(*) From "&dvbbs.NowUseBBS&" Where (Not BoardID in (444,777)) And Datediff(d,Dateandtime,"&SqlNowString&")=0")
	else
	Set Rs=Dvbbs.Execute("Select Count(*) From "&dvbbs.NowUseBBS&" Where (Not BoardID in (444,777)) And Datediff('d',Dateandtime,"&SqlNowString&")=0")
	end if
	MathForumToday=Rs(0)
	Set Rs=Nothing
	If Isnull(MathForumToday) Then MathForumToday=0
	Dvbbs.Execute("Update dv_setup Set Forum_Todaynum="&MathForumToday)
End Function 
%>