<!-- #include file =conn.asp-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/dv_clsother.asp"-->
<!-- #include file="Dv_plus/Tools/plus_Tools_const.asp" -->
<%
Dim ToUserID,TopicID,ReplyID,Action,ChkAction,LogMsg
Dvbbs.ErrType = 1 '设置错误提示信息显示模式
ChkAction = True
ToUserID = Dv_Tools.CheckNumeric(Request("ToUserID"))	'目标用户
TopicID = Dv_Tools.CheckNumeric(Request("TopicID"))		'主题ID
ReplyID = Dv_Tools.CheckNumeric(Request("ReplyID"))		'回复ID
Action = Dv_Tools.CheckNumeric(Request("Action"))		'执行分类
If TopicID = 0 or ReplyID = 0 or Dvbbs.BoardID = 0 Then ChkAction = False
Dvbbs.stats = "论坛道具使用"
If Action=0 Then
	Dv_Tools.ChkToolsLogin
	Dvbbs.stats = "论坛道具使用=="&Dv_Tools.ToolsInfo(1)
End If
Dvbbs.LoadTemplates("")
Dvbbs.Head()
ToolsMain
Dvbbs.Showerr()
Dvbbs.mainsetting(0)="98%"
Dvbbs.Footer()
Dvbbs.PageEnd()
'---------------------------------------------------
'Dv_Tools.ToolsInfo 道具系统信息
'ID=0 ,ToolsName=1 ,ToolsInfo=2 ,IsStar=3 ,SysStock=4 ,UserStock=5 ,UserMoney=6 ,UserPost=7 ,UserWealth=8 ,UserEp=9 ,UserCp=10 ,UserGroupID=11 ,BoardID=12,UserTicket=13,BuyType=14,ToolsImg=15
'---------------------------------------------------
'事件记录过程：Call Dvbbs.ToolsLog(道具ID，发生数量，金币发生额，点券发生额，记录事件类型，备注内容，用户最后剩余金币和点券（金币|点券）)
'---------------------------------------------------
Sub ToolsMain()
	Dv_Tools.ChkUseTools '检查道具使用权限
	Select Case Dv_Tools.ToolsID
	Case 1 :  Tools_1
	Case 2 :  Tools_2
	Case 3 :  Tools_3
	Case 4 :  Tools_4
	Case 5 :  Tools_5
	Case 6 :  Tools_6
	Case 7 :  Tools_7
	Case 8 :  Tools_8
	Case 9 :  Tools_9
	Case 10 : Tools_10
	Case 11 : Tools_11
	Case 12 : Tools_12
	Case 13 : Tools_13
	Case 14 : Tools_14
	Case 16 : Tools_16
	Case 17 : Tools_17
	Case 18 : Tools_18
	Case 19 : Tools_19
	Case 20 : Tools_20
	Case 21 : Tools_21
	Case 22 : Tools_22
	Case 23 : Tools_23
	Case 24 : Tools_24
	Case 25 : Tools_25
	Case 26 : Tools_26
	Case 27 : Tools_27
	Case 28 : Tools_28
	Case 29 : Tools_29
	Case Else
		Dv_Tools.ShowErr(3)
	End Select
End Sub

'------------------------------------------------------------------------------------------------------
'道具处理过程
'------------------------------------------------------------------------------------------------------

'---------------------------------------------------
'道具:转让器，可进行道具、金币和点券的转让
'---------------------------------------------------
Sub Tools_1()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	Dim iUserInfo
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(Request("ToUserID"))

	If Request("ToolsAction")="SendTools" Then
		Dim SendToolsID,SendToolsNum,SendMoneyNum,SendTicketNum
		SendToolsID = Dv_Tools.CheckNumeric(Request("SendToolsID"))
		SendToolsNum = Dv_Tools.CheckNumeric(Request("SendToolsNum"))
		SendMoneyNum = CCur(Abs(Dv_Tools.CheckNumeric(Request("SendMoneyNum"))))
		SendTicketNum = CCur(Abs(Dv_Tools.CheckNumeric(Request("SendTicketNum"))))
		If (SendToolsID=0 Or SendToolsNum=0) And SendMoneyNum=0 And SendTicketNum=0 Then
			LogMsg = "由于您没有正确填写相应的转让内容，使用道具不成功！"
		Else
			If Dvbbs.UserID = Clng(Dv_Tools.ToUserInfo(0)) Then
				Dv_Tools.ShowErr(14)
				Exit Sub
			End If
			LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功"
			'金币转让
			If SendMoneyNum > 0 Then
				If CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text) < SendMoneyNum Then Dv_Tools.ShowErr(17) : Exit Sub
				Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text) - cCur(SendMoneyNum)
				LogMsg = LogMsg & "，转给"&Dv_Tools.ToUserInfo(1)&"<B>"&SendMoneyNum&"</B>个金币"
				Dvbbs.Execute("Update Dv_User Set UserMoney = UserMoney - "&SendMoneyNum&" Where UserID=" & Dvbbs.UserID)
				Dvbbs.Execute("Update Dv_User Set UserMoney = UserMoney + "&SendMoneyNum&" Where UserID=" & Dv_Tools.ToUserInfo(0))
			End If
			'点券转让
			If SendTicketNum > 0 Then
				If CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text) < SendTicketNum Then Dv_Tools.ShowErr(17) : Exit Sub
				Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text = cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text) - cCur(SendTicketNum)
				LogMsg = LogMsg & "，转给"&Dv_Tools.ToUserInfo(1)&"<B>"&SendTicketNum&"</B>张点券"
				Dvbbs.Execute("Update Dv_User Set UserTicket = UserTicket - "&SendTicketNum&" Where UserID=" & Dvbbs.UserID)
				Dvbbs.Execute("Update Dv_User Set UserTicket = UserTicket + "&SendTicketNum&" Where UserID=" & Dv_Tools.ToUserInfo(0))
			End If
			'道具转让
			If SendToolsID > 0 And SendToolsNum > 0 Then
				Dim Trs,UserToolsNum
				UserToolsNum = 0
				Sql = "Select ID,UserID,UserName,ToolsID,ToolsName,ToolsCount,SaleCount,UpdateTime From [Dv_Plus_Tools_Buss] Where ToolsCount>0 and UserID="& Dvbbs.UserID &" and ToolsID="& SendToolsID
				Set Trs = Dvbbs.Plus_Execute(Sql)
				If Trs.Eof Then
					Response.redirect "showerr.asp?ErrCodes=<li>所选取转让的道具不存在，请购买了相应的道具再执行转让！&action=NoHeadErr"
					Exit Sub
				Else
					UserToolsNum = Trs(5)
					If UserToolsNum<SendToolsNum Then
						Response.redirect "showerr.asp?ErrCodes=<li>你目前只能转让("&UserToolsNum&")个道具！&action=NoHeadErr"
						Exit Sub
					End If
				End If
				Trs.Close
				Set Trs = Dvbbs.Plus_Execute("Select ToolsName From Dv_Plus_Tools_Info Where ID=" & SendToolsID)
				If Not (Trs.Eof And Trs.Bof) Then
					LogMsg = LogMsg & "，转给"&Dv_Tools.ToUserInfo(1)&"<B>"&SendToolsNum&"</B>个"&Trs(0)&"道具"
				End If
				Trs.Close
				Set Trs=Nothing
				'更新用户和系统使用数量
				Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
				'更新用户道具数量
				Call UpdateBussTools(Dvbbs.UserID,SendToolsID,SendToolsNum)	
				Call UpdateBussTools(Dv_Tools.ToUserInfo(0),SendToolsID,-SendToolsNum)
			End If
			Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,2,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
		End If
		Dvbbs.Dvbbs_Suc(LogMsg)
	Else
%>
<table border="0" cellpadding=3 cellspacing=1 align=center class=Tableborder1 Style="Width:99%">
	<tr>
	<th height=23 colspan=2>使用道具 <%=Dv_Tools.ToolsInfo(1)%></th></tr>
	<tr><td height=23 class=Tablebody1 colspan=2>
	<B>说明</B>：<BR>1、使用本道具可将您自己的金钱、点券或道具转让给目标用户<BR>2、目标用户的选择方法：通常在论坛的各种位置只要点击用户名连接即可进入该用户资料页面，浏览帖子过程可点击该贴用户“信息”图标，进入用户资料页面后点击“使用道具”连接即可进入具体的道具操作页面</td></tr>
	<tr>
	<td height=23 class=Tablebody1 width="30%" align=right>目标用户：</td>
	<td height=23 class=Tablebody1 width="70%"><B><%=Dv_Tools.ToUserInfo(1)%></B></td>
	</tr>
	<FORM METHOD=POST ACTION="?ToolsAction=SendTools">
	<input type=hidden value="<%=ToUserID%>" name="ToUserID">
	<input type=hidden value="<%=Dv_Tools.ToolsID%>" name="ToolsID">
	<tr>
	<td height=23 class=Tablebody1 width="30%" align=right>转让道具：</td>
	<td height=23 class=Tablebody1 width="70%">
	<Select Size=1 Name="SendToolsID">
	<Option value=0 selected>请选择要转让的道具</option>
<%
	Set Rs=Dvbbs.Plus_Execute("Select ToolsID,ToolsName,ToolsCount From [Dv_Plus_Tools_Buss] where UserID="& Dvbbs.UserID &" ORDER BY ToolsCount Desc")
	Do While Not Rs.Eof
		Response.Write "<option value="""&Rs(0)&""">拥有"&Rs(1)&Rs(2)&"个</option>"
	Rs.MoveNext
	Loop
	Rs.Close
	Set Rs=Nothing
%>
	</Select>
	转让数量：
	<input type=text size=5 value="0" name="SendToolsNum">
	个
	</td>
	</tr>
	<tr><td height=23 class=Tablebody1 colspan=2 align=center>
	您有 <B><font color=red><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text%></font></B> 个金币和 <B><font color=red><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text%></font></B> 张点券可供转让
	</td></tr>
	<tr>
	<td height=23 class=Tablebody1 width="30%" align=right>转让金币：</td>
	<td height=23 class=Tablebody1 width="70%">
	<input type=text size=5 value="0" name="SendMoneyNum">
	个</td>
	</tr>
	<tr>
	<td height=23 class=Tablebody1 width="30%" align=right>转让点券：</td>
	<td height=23 class=Tablebody1 width="70%">
	<input type=text size=5 value="0" name="SendTicketNum">
	个</td>
	</tr>
	<tr><td height=23 class=Tablebody2 colspan=2 align=center>
	<input type=submit value="确认转让" name=submit>
	</td></tr>
	</FORM>
</table>
<%
	End If
End Sub
'---------------------------------------------------
'道具:后悔药，可删除自己发表的帖子，有回复则不能删
'---------------------------------------------------
Sub Tools_2()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable,ToolsParnetID,ToolsIsToday
	ToolsIsToday = 0
'	If ToUserID = 0 Then ChkAction = False
'	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(Dvbbs.UserID)
	If Dvbbs.UserID <> Clng(Dv_Tools.ToUserInfo(0)) Then
		Dv_Tools.ShowErr(15)
		Exit Sub
	End If
	Sql = "Select Title,UseTools,PostTable,Child From [Dv_Topic] Where TopicID="&TopicID&" And PostUserID="&Dvbbs.UserID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在！&action=NoHeadErr"
		Exit Sub
	Else
		If Rs(3)>0 Then
			Response.redirect "showerr.asp?ErrCodes=<li>该贴已有人回复，不能删除，您可自行编辑清除该贴相关内容！&action=NoHeadErr"
			Exit Sub
		End If
		T_PostTable = Rs(2)
	End If
	Rs.Close
	Set Rs=Dvbbs.Execute("Select Topic,UseTools,Body,ParentID,DateAndTime From "&T_PostTable&" Where AnnounceID="&ReplyID&" And PostUserID=" & Dvbbs.UserID)
	If Rs.Eof Then
		Response.redirect "showerr.asp?ErrCodes=<li>该帖子不存在！&action=NoHeadErr"
		Exit Sub
	Else
		If Rs(0)="" Or IsNull(Rs(0)) Then
			T_Title = Left(Rs(2),25)
		Else
			T_Title = Rs(0)
		End If
		ToolsParnetID = Rs(3)
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
		If DateDiff("d",Rs(4),Now())=0 Then ToolsIsToday = 1
	End If
	Rs.Close
	If ToolsParnetID = 0 Then
		Sql = "Update Dv_Topic Set BoardID=444,locktopic="&Dvbbs.BoardID&",UseTools='"& T_UseTools &"' Where TopicID=" & TopicID
		Dvbbs.Execute(Sql)
		Sql = "Update "&T_PostTable&" Set BoardID=444,locktopic="&Dvbbs.BoardID&",UseTools='"& T_UseTools &"' Where AnnounceID=" & ReplyID
		Dvbbs.Execute(Sql)
	Else
		Sql = "Update "&T_PostTable&" Set BoardID=444,locktopic="&Dvbbs.BoardID&",UseTools='"& T_UseTools &"' Where AnnounceID=" & ReplyID
		Dvbbs.Execute(Sql)
	End If
	'更新所有版面帖子数
	AllboardNumSub ToolsIsToday,1,1
	'更新相关版面帖子数
	Call BoardNumSub(Dvbbs.BoardID,1,1,ToolsIsToday)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"已成功删除入论坛回收站！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)

End Sub
'---------------------------------------------------
'道具:一级特赦令，可解除单贴屏蔽
'---------------------------------------------------
Sub Tools_3()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
'	If ToUserID = 0 Then ChkAction = False
'	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
'	Dv_Tools.ChkToUseTools()
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在！&action=NoHeadErr"
		Exit Sub
	Else
		T_PostTable = Rs(2)
	End If
	Rs.Close
	Set Rs=Dvbbs.Execute("Select topic,UseTools,Body,postuserid From "&T_PostTable&" Where AnnounceID="&ReplyID&" And LockTopic=2")
	If Rs.Eof Then
		Response.redirect "showerr.asp?ErrCodes=<li>该帖子不存在或不是屏蔽状态！&action=NoHeadErr"
		Exit Sub
	Else
		If Rs(0)="" Or IsNull(Rs(0)) Then
			T_Title = Left(Rs(2),25)
		Else
			T_Title = Rs(0)
		End If
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
	End If
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(Rs(3))
	Rs.Close
	Sql = "Update "&T_PostTable&" Set LockTopic=0,UseTools='"& T_UseTools &"' Where AnnounceID=" & ReplyID
	Dvbbs.Execute(Sql)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"已成功解除单贴屏蔽状态！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub
'---------------------------------------------------
'道具:二级特赦令，可解除主题锁定
'---------------------------------------------------
Sub Tools_4()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID&" And LockTopic=1"
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在或不是锁定状态！&action=NoHeadErr"
		Exit Sub
	Else
		T_Title = Rs(0)
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
		T_PostTable = Rs(2)
	End If
	Rs.Close
	Sql = "Update [Dv_Topic] Set LockTopic=0,UseTools='"& T_UseTools &"' Where TopicID="&TopicID
	Dvbbs.Execute(Sql)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"已成功解除锁定！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub

'---------------------------------------------------
'道具:三级特赦令，解除自己或他人的屏蔽或锁定状态
'---------------------------------------------------
Sub Tools_5()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(ToUserID)
	Sql = "Select UserID From Dv_User Where UserID="&ToUserID&" And LockUser>0"
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该用户不存在或不是屏蔽或锁定状态！&action=NoHeadErr"
		Exit Sub
	Else
		Dvbbs.Execute("Update Dv_User Set LockUser=0 Where UserID="& Rs(0))
	End If
	Rs.Close
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，用户<B>"&Dv_Tools.ToUserInfo(1)&"</B>已成功解除锁定或屏蔽状态！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub
'---------------------------------------------------
'道具:吖噗鸡，可使帖子提升到第一页
'---------------------------------------------------
Sub Tools_6()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID&" And LockTopic=1"
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在或不是锁定状态！&action=NoHeadErr"
		Exit Sub
	Else
		T_Title = Rs(0)
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
		T_PostTable = Rs(2)
	End If
	Rs.Close
	Sql = "Update [Dv_Topic] Set LastPostTime="&SqlNowString&",UseTools='"& T_UseTools &"' Where TopicID="&TopicID
	Dvbbs.Execute(Sql)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"已成功提升到第一页！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub
'---------------------------------------------------
'道具:醒目灯，可将主题变色
'---------------------------------------------------
Sub Tools_7()
	Dim Rs,Sql,i
	Dim T_Title,T_UseTools,T_PostTable,ToolsColorList
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在或不是锁定状态！&action=NoHeadErr"
		Exit Sub
	Else
		T_Title = Rs(0)
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
		T_PostTable = Rs(2)
	End If
	Rs.Close
	ToolsColorList = "#000000,#F0F8FF,#FAEBD7,#00FFFF,#7FFFD4,#F0FFFF,#F5F5DC,#FFE4C4,#000000,#FFEBCD,#0000FF,#8A2BE2,#A52A2A,#DEB887,#5F9EA0,#7FFF00,#D2691E,#FF7F50,#6495ED,#FFF8DC,#DC143C,#00FFFF,#00008B,#008B8B,#B8860B,#A9A9A9,#006400,#BDB76B,#8B008B,#556B2F,#FF8C00,#9932CC,#8B0000,#E9967A,#8FBC8F,#483D8B,#2F4F4F,#00CED1,#9400D3,#FF1493,#00BFFF,#696969,#1E90FF,#B22222,#FFFAF0,#228B22,#FF00FF,#DCDCDC,#F8F8FF,#FFD700,#DAA520,#808080,#008000,#ADFF2F,#F0FFF0,#FF69B4,#CD5C5C,#4B0082,#FFFFF0,#F0E68C,#E6E6FA,#FFF0F5,#7CFC00,#FFFACD,#ADD8E6,#F08080,#E0FFFF,#FAFAD2,#90EE90,#D3D3D3,#FFB6C1,#FFA07A,#20B2AA,#87CEFA,#778899,#B0C4DE,#FFFFE0,#00FF00,#32CD32,#FAF0E6,#FF00FF,#800000,#66CDAA,#0000CD,#BA55D3,#9370DB,#3CB371,#7B68EE,#00FA9A,#48D1CC,#C71585,#191970,#F5FFFA,#FFE4E1,#FFE4B5,#FFDEAD,#000080,#FDF5E6,#808000,#6B8E23,#FFA500,#FF4500,#DA70D6,#EEE8AA,#98FB98,#AFEEEE,#DB7093,#FFEFD5,#FFDAB9,#CD853F,#FFC0CB,#DDA0DD,#B0E0E6,#800080,#FF0000,#BC8F8F,#4169E1,#8B4513,#FA8072,#F4A460,#2E8B57,#FFF5EE,#A0522D,#C0C0C0,#87CEEB,#6A5ACD,#708090,#FFFAFA,#00FF7F,#4682B4,#D2B48C,#008080,#D8BFD8,#FF6347,#40E0D0,#EE82EE,#F5DEB3,#FFFFFF,#F5F5F5,#FFFF00,#9ACD32"
	If Request("ToolsAction")="SendColor" Then
		If Instr("," & ToolsColorList & ",","," & Request("color") & ",")=0 Then
			Response.redirect "showerr.asp?ErrCodes=<li>错误的颜色参数！&action=NoHeadErr"
			Exit Sub
		End If
		T_Title = "<font color="&Request("color")&">"&T_Title&"</font>"
		Dvbbs.Execute("Update Dv_Topic Set Title='"&Replace(T_Title,"'","''")&"',TopicMode=1,UseTools='"& T_UseTools &"' Where TopicID=" & TopicID)
		Dvbbs.Execute("Update "&T_PostTable&" Set Topic='"&Replace(T_Title,"'","''")&"',UseTools='"& T_UseTools &"' Where RootID="&TopicID&" And ParentID=0")
		Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
		LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&Replace(Replace(LoadTitle(T_Title),"&lt;","<"),"&gt;",">")&"已成功操作！"
		Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
		Dvbbs.Dvbbs_Suc(LogMsg)
	Else
	ToolsColorList = Split(ToolsColorList,",")
%>
<table border="0" cellpadding=3 cellspacing=1 align=center class=Tableborder1 Style="Width:99%">
	<tr>
	<th height=23 colspan=2>使用道具 <%=Dv_Tools.ToolsInfo(1)%></th></tr>
	<tr><td height=23 class=Tablebody1 colspan=2>
	<B>说明</B>：本道具可使目标帖子标题变成您所选择的颜色，请在下面选择您所需要的颜色</td></tr>
	<FORM METHOD=POST ACTION="?ToolsAction=SendColor" name="theForm">
<!--	<input type=hidden value="<%=ToUserID%>" name="ToUserID">  -->
	<input type=hidden value="<%=Dvbbs.BoardID%>" name="BoardID">
	<input type=hidden value="<%=TopicID%>" name="TopicID">
	<input type=hidden value="<%=ReplyID%>" name="ReplyID">
	<input type=hidden value="<%=Dv_Tools.ToolsID%>" name="ToolsID">
	<tr>
	<td height=23 class=Tablebody1 width="30%" align=right>颜色列表：</td>
	<td height=23 class=Tablebody1 width="70%">
	<SELECT onChange="document.getElementById('TopicColor').color=options[selectedIndex].value;" name="color"> 
	<%
	For i=0 To Ubound(ToolsColorList)
		Response.Write "<option style=""background-color:"&ToolsColorList(i)&";color: "&ToolsColorList(i)&""" value="""&ToolsColorList(i)&""">"&ToolsColorList(i)&"</option>"
	Next
	%>
	</SELECT>
	</td>
	</tr>
	<tr>
	<td height=23 class=Tablebody1 width="30%" align=right>使用效果：</td>
	<td height=23 class=Tablebody1 width="70%"><font id=TopicColor><%=Server.HtmlEncode(T_Title)%></font></td>
	</tr>
	<tr><td height=23 class=Tablebody2 colspan=2 align=center>
	<input type=submit value="确认使用" name=submit>
	</td></tr>
	</FORM>
</table>
<%
	End If
End Sub

'---------------------------------------------------
'道具:水晶球，可查看发贴用户IP
'---------------------------------------------------
Sub Tools_8()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable,ToUserToolsIP
'	If ToUserID = 0 Then ChkAction = False
'	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
'	Dv_Tools.ChkToUseTools()
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在！&action=NoHeadErr"
		Response.write "1"
		Exit Sub
	Else
		T_PostTable = Rs(2)
	End If
	Rs.Close
	Set Rs=Dvbbs.Execute("Select Topic,UseTools,Body,IP,postuserid From "&T_PostTable&" Where AnnounceID="&ReplyID)
	If Rs.Eof Then
		Response.redirect "showerr.asp?ErrCodes=<li>该帖子不存在！&action=NoHeadErr"
		Exit Sub
	Else
		If Rs(0)="" Or IsNull(Rs(0)) Then
			T_Title = Left(Rs(2),25)
		Else
			T_Title = Rs(0)
		End If
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
		ToUserToolsIP = Rs(3)
	End If
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(Rs(4))
	Rs.Close
	Sql = "Update "&T_PostTable&" Set UseTools='"& T_UseTools &"' Where AnnounceID=" & ReplyID
	Dvbbs.Execute(Sql)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"中帖子编号为"&ReplyID&"的发贴IP是："&ToUserToolsIP&"！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub
'---------------------------------------------------
'道具:追踪器，可查看发贴用户的IP和来源
'---------------------------------------------------
Sub Tools_9()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable,ToUserToolsIP,ToUserToolsIP_1,ToUserToolsAddress
'	If ToUserID = 0 Then ChkAction = False
'	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
'	Dv_Tools.ChkToUseTools()
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在！&action=NoHeadErr"
		Exit Sub
	Else
		T_PostTable = Rs(2)
	End If
	Rs.Close
	Set Rs=Dvbbs.Execute("Select Topic,UseTools,Body,IP,postuserid From "&T_PostTable&" Where AnnounceID="&ReplyID)
	If Rs.Eof Then
		Response.redirect "showerr.asp?ErrCodes=<li>该帖子不存在！&action=NoHeadErr"
		Exit Sub
	Else
		If Rs(0)="" Or IsNull(Rs(0)) Then
			T_Title = Left(Rs(2),25)
		Else
			T_Title = Rs(0)
		End If
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
		ToUserToolsIP = Rs(3)
	End If
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(Rs(4))
	Rs.Close
	Sql = "Update "&T_PostTable&" Set UseTools='"& T_UseTools &"' Where AnnounceID=" & ReplyID
	Dvbbs.Execute(Sql)
	ToUserToolsIP_1 = ToUserToolsIP
	ToUserToolsAddress = lookaddress(ToUserToolsIP_1)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"中帖子编号为"&ReplyID&"的发贴IP是："&ToUserToolsIP&"，来源是："&ToUserToolsAddress&"！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)

End Sub
'---------------------------------------------------
'道具:一星龙珠，可将用户所有负分转为0
'---------------------------------------------------
Sub Tools_10()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(ToUserID)
	'更新用户分值信息
	Sql = "Select UserWealth,UserEP,UserCP,UserPower,UserDel From Dv_User Where UserID= " & Dv_Tools.ToUserInfo(0)
	Set Rs = Dvbbs.iCreateObject ("adodb.recordset")
	If Not IsObject(Conn) Then ConnectionDatabase
	Rs.Open Sql,Conn,1,3
	If Rs("UserWealth") < 0 Then Rs("UserWealth") = 0
	If Rs("UserEP") < 0 Then Rs("UserEP") = 0
	If Rs("UserCP") < 0 Then Rs("UserCP") = 0
	If Rs("UserPower") < 0 Then Rs("UserPower") = 0
	If Rs("UserDel") < 0 Then Rs("UserDel") = 0
	Rs.Update
	Rs.Close
	Set Rs=Nothing
	'更新用户和系统使用数量
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，成功将用户<b>"&Dv_Tools.ToUserInfo(1)&"</b>的所有负分转正！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)

End Sub
'---------------------------------------------------
'道具:二星龙珠，可将用户积分负分转为0
'---------------------------------------------------
Sub Tools_11()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(ToUserID )
	'更新用户分值信息
	Sql = "Select UserEP From Dv_User Where UserID= " & Dv_Tools.ToUserInfo(0)
	Set Rs = Dvbbs.iCreateObject ("adodb.recordset")
	If Not IsObject(Conn) Then ConnectionDatabase
	Rs.Open Sql,Conn,1,3
	If Rs("UserEP") < 0 Then Rs("UserEP") = 0
	Rs.Update
	Rs.Close
	Set Rs=Nothing
	'更新用户和系统使用数量
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，成功将用户<b>"&Dv_Tools.ToUserInfo(1)&"</b>的积分负分转正！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub
'---------------------------------------------------
'道具:狗仔队，可在用户上线第一时间获知
'---------------------------------------------------
Sub Tools_12()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(ToUserID )
	If Dvbbs.UserID = Clng(Dv_Tools.ToUserInfo(0)) Then
		Dv_Tools.ShowErr(14)
		Exit Sub
	End If
	'更新用户信息
	Sql = "Select FollowMsgID From Dv_User Where UserID= " & Dv_Tools.ToUserInfo(0)
	Set Rs = Dvbbs.iCreateObject ("adodb.recordset")
	If Not IsObject(Conn) Then ConnectionDatabase
	Rs.Open Sql,Conn,1,3
	If Rs(0)="" Or IsNull(Rs(0)) Then
		Rs(0) = Dvbbs.Membername
	Else
		Rs(0) = Rs(0) & "," & Dvbbs.Membername
	End If
	Rs.Update
	Rs.Close
	Set Rs=Nothing
	'更新用户和系统使用数量
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，成功跟踪用户<b>"&Dv_Tools.ToUserInfo(1)&"</b>，用户上线后会第一时间通知您！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub

'---------------------------------------------------
'道具:救生圈，可将帖子固顶6小时
'---------------------------------------------------
Sub Tools_13()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable,LastPostTime
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在！&action=NoHeadErr"
		Exit Sub
	Else
		T_Title = Rs(0)
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
		T_PostTable = Rs(2)
	End If
	Rs.Close
	LastPostTime = DateAdd("h",6,now)
	Sql = "Update [Dv_Topic] Set LastPostTime='"&LastPostTime&"',UseTools='"& T_UseTools &"' Where TopicID="&TopicID
	Dvbbs.Execute(Sql)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"已成功固顶6小时！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub
'---------------------------------------------------
'道具:大救生圈，可将帖子固顶12小时
'---------------------------------------------------
Sub Tools_14()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable,LastPostTime
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在！&action=NoHeadErr"
		Exit Sub
	Else
		T_Title = Rs(0)
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
		T_PostTable = Rs(2)
	End If
	Rs.Close
	LastPostTime = DateAdd("h",12,now)
	Sql = "Update [Dv_Topic] Set LastPostTime='"&LastPostTime&"',UseTools='"& T_UseTools &"' Where TopicID="&TopicID
	Dvbbs.Execute(Sql)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"已成功固顶12小时！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub

'---------------------------------------------------
'道具:时空转移机 可将自已的帖子移动到任意版面（隐含、特殊限定版面除外）。
'---------------------------------------------------
Sub Tools_15()

End Sub

'---------------------------------------------------
'道具:照妖镜 可查看匿名发帖用户名。
'---------------------------------------------------
Sub Tools_16()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable,ToUserToolsName
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在！&action=NoHeadErr"
		Exit Sub
	Else
		T_PostTable = Rs(2)
	End If
	Rs.Close
	Set Rs=Dvbbs.Execute("Select Topic,UseTools,Body,postuserid From "&T_PostTable&" Where AnnounceID="&ReplyID)
	If Rs.Eof Then
		Response.redirect "showerr.asp?ErrCodes=<li>该帖子不存在！&action=NoHeadErr"
		Exit Sub
	Else
		If Rs(0)="" Or IsNull(Rs(0)) Then
			T_Title = Left(Rs(2),25)
		Else
			T_Title = Rs(0)
		End If
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
		ToUserToolsName = Rs(3)
	End If
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(Rs(3))
	Rs.Close
	Sql = "Update "&T_PostTable&" Set UseTools='"& T_UseTools &"' Where AnnounceID=" & ReplyID
	Dvbbs.Execute(Sql)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"的发贴人是："&Dv_Tools.ToUserInfo(1)&"！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub

'---------------------------------------------------
'道具:晶体探测器 可将匿名发帖用户信息直接转为真实信息，并公开显示状态。
'---------------------------------------------------
Sub Tools_17()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable,ToUserToolsName
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在！&action=NoHeadErr"
		Exit Sub
	Else
		T_PostTable = Rs(2)
	End If
	Rs.Close
	Set Rs=Dvbbs.Execute("Select Topic,UseTools,Body,postuserid,ParentID From "&T_PostTable&" Where AnnounceID="&ReplyID)
	If Rs.Eof Then
		Response.redirect "showerr.asp?ErrCodes=<li>该帖子不存在！&action=NoHeadErr"
		Exit Sub
	Else
		If Rs(0)="" Or IsNull(Rs(0)) Then
			T_Title = Left(Rs(2),25)
		Else
			T_Title = Rs(0)
		End If
		T_UseTools = LoadUserTools(Rs(1),Dv_Tools.ToolsID)
		ToUserToolsName = Rs(3)
	End If
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(Rs(3))
	If Rs(4)=0 Then Dvbbs.Execute("Update Dv_Topic Set HideName=0 Where TopicID="&TopicID)
	Rs.Close
	Sql = "Update "&T_PostTable&" Set UseTools='"& T_UseTools &"',signflag=0 Where AnnounceID=" & ReplyID
	Dvbbs.Execute(Sql)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"的发贴人用户信息已转为显示状态！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub


'---------------------------------------------------
'道具:精灵弓，可破坏小救生圈效果，对大救生圈效果破坏1/6
'---------------------------------------------------
Sub Tools_18()
	Dim Rs,Sql,CanUserTools,UseTools
	Dim T_Title,T_UseTools,T_PostTable
	CanUserTools = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在！&action=NoHeadErr"
		Exit Sub
	Else
		T_Title = Rs(0)
		UseTools = Rs(1)
		If UseTools="" Or IsNull(UseTools) Then
			Response.redirect "showerr.asp?ErrCodes=<li>该主题没有被使用相关道具！&action=NoHeadErr"
			Exit Sub
		End If
		If InStr("," & UseTools & ",",",13,") Then CanUserTools = True
		If InStr("," & UseTools & ",",",14,") Then CanUserTools = True
		If Not CanUserTools Then
			Response.redirect "showerr.asp?ErrCodes=<li>该主题没有被使用相关道具！&action=NoHeadErr"
			Exit Sub
		End If
		T_UseTools = LoadUserTools(UseTools,Dv_Tools.ToolsID)
		T_PostTable = Rs(2)
	End If
	Rs.Close
	Sql = ""
	If IsSqlDataBase=1 Then
		If InStr("," & UseTools & ",",",13,")>0 Then
			Sql = "Update [Dv_Topic] Set LastPostTime=DateAdd(hour,-6,"&SqlNowString&"),UseTools='"& T_UseTools &"' Where TopicID="&TopicID
		ElseIf InStr("," & UseTools & ",",",14,")>0 Then
			Sql = "Update [Dv_Topic] Set LastPostTime=DateAdd(hour,-2,"&SqlNowString&"),UseTools='"& T_UseTools &"' Where TopicID="&TopicID
		End If
	Else
		If InStr("," & UseTools & ",",",13,")>0 Then
			Sql = "Update [Dv_Topic] Set LastPostTime=DateAdd('h',-6,"&SqlNowString&"),UseTools='"& T_UseTools &"' Where TopicID="&TopicID
		ElseIf InStr("," & UseTools & ",",",14,")>0 Then
			Sql = "Update [Dv_Topic] Set LastPostTime=DateAdd('h',-2,"&SqlNowString&"),UseTools='"& T_UseTools &"' Where TopicID="&TopicID
		End If
	End If
	If Sql<>"" Then Dvbbs.Execute(Sql)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"对目标帖子操作已成功！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub
'---------------------------------------------------
'道具:水之母，可延长大小救生圈固顶效果时限的1/6
'---------------------------------------------------
Sub Tools_19()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable,CanUserTools,UseTools
	CanUserTools = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	Sql = "Select Title,UseTools,PostTable From [Dv_Topic] Where TopicID="&TopicID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.Eof Then 
		Response.redirect "showerr.asp?ErrCodes=<li>该主题不存在！&action=NoHeadErr"
		Exit Sub
	Else
		T_Title = Rs(0)
		UseTools = Rs(1)
		If UseTools="" Or IsNull(UseTools) Then
			Response.redirect "showerr.asp?ErrCodes=<li>该主题没有被使用相关道具！&action=NoHeadErr"
			Exit Sub
		End If
		If InStr("," & UseTools & ",",",13,") Then CanUserTools = True
		If InStr("," & UseTools & ",",",14,") Then CanUserTools = True
		If Not CanUserTools Then
			Response.redirect "showerr.asp?ErrCodes=<li>该主题没有被使用相关道具！&action=NoHeadErr"
			Exit Sub
		End If
		T_UseTools = LoadUserTools(UseTools,Dv_Tools.ToolsID)
		T_PostTable = Rs(2)
	End If
	Rs.Close
	Sql = ""
	If IsSqlDataBase=1 Then
		If InStr("," & UseTools & ",",",13,")>0 Then
			Sql = "Update [Dv_Topic] Set LastPostTime=DateAdd(hour,1,"&SqlNowString&"),UseTools='"& T_UseTools &"' Where TopicID="&TopicID
		ElseIf InStr("," & UseTools & ",",",14,")>0 Then
			Sql = "Update [Dv_Topic] Set LastPostTime=DateAdd(hour,2,"&SqlNowString&"),UseTools='"& T_UseTools &"' Where TopicID="&TopicID
		End If
	Else
		If InStr("," & UseTools & ",",",13,")>0 Then
			Sql = "Update [Dv_Topic] Set LastPostTime=DateAdd('h',1,"&SqlNowString&"),UseTools='"& T_UseTools &"' Where TopicID="&TopicID
		ElseIf InStr("," & UseTools & ",",",14,")>0 Then
			Sql = "Update [Dv_Topic] Set LastPostTime=DateAdd('h',2,"&SqlNowString&"),UseTools='"& T_UseTools &"' Where TopicID="&TopicID
		End If
	End If
	If Sql<>"" Then Dvbbs.Execute(Sql)
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，"&LoadTitle(T_Title)&"对目标帖子操作已成功！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub

'---------------------------------------------------
'道具:转生之炎,可将更改自已在论坛的用户名。。
'---------------------------------------------------
Sub Tools_20()
	ChkAction = True
	Dim NewUserName,TempLateStr,i,Rs,Sql
	If Request("ToolsAction")="ChangID" Then
		NewUserName = Dvbbs.Checkstr(Trim(Request.Form("name")))
		If Dvbbs.chkpost = False Then Dvbbs.AddErrCode(16):Exit Sub
		'验证用户名字符长度是否符合论坛标准
		If NewUserName = "" or Dvbbs.strLength(NewUserName)>Cint(Dvbbs.Forum_Setting(41)) or Dvbbs.strLength(NewUserName)<Cint(Dvbbs.Forum_Setting(40)) Then
			Dvbbs.AddErrCode(17)
			ChkAction = False
		End If
		'验证用户名是否含有禁止字符；
		If Instr(NewUserName,"=")>0 or Instr(NewUserName,"%")>0 or Instr(NewUserName,chr(32))>0 or Instr(NewUserName,"?")>0 or Instr(NewUserName,"&")>0 or Instr(NewUserName,";")>0 or Instr(NewUserName,",")>0 or Instr(NewUserName,"'")>0 or Instr(NewUserName,chr(34))>0 or Instr(NewUserName,chr(9))>0 or Instr(NewUserName,"")>0 or Instr(NewUserName,"$")>0 or Instr(NewUserName,"§")>0 Then
			Dvbbs.AddErrCode(19)
			ChkAction = False
		End If
		'验证用户名是注册过禁止字符；
		Dim RegSplitWords
		RegSplitWords = Split(Dvbbs.Forum_setting(4),",")
		For i = 0 To Ubound(RegSplitWords)
			If Instr(NewUserName,RegSplitWords(i))>0 Then
				Dvbbs.AddErrCode(19)
				ChkAction = False
				Exit For
			End If
		Next
		If ChkAction = False Then Exit Sub
		'新用户名是否有重复
		Set Rs = Dvbbs.Execute("Select top 1 UserID From [Dv_user]  Where UserName = '"&NewUserName&"'")
		If Not Rs.eof Then
			Dvbbs.AddErrCode(21)	'您输入的用户名已经被注册。
			Exit Sub
		End If
		Rs.close

		'-------------------------------------------------------------------------------------------------------------
		'更新道具表用户名数据
		Dvbbs.Plus_Execute("update [Dv_Plus_Tools_Buss] set UserName = '"&NewUserName&"' Where UserID = "&Dvbbs.UserID)
		Dvbbs.Plus_Execute("update [Dv_MoneyLog] set AddUserName = '"&NewUserName&"' Where AddUserID = "&Dvbbs.UserID)
		'更新论坛用户名所有数据
		Conn.Execute("update [Dv_Admin] set adduser = '"&NewUserName&"' Where adduser = '"&Dvbbs.MemberName&"'")
		Dvbbs.Execute("update [Dv_User] set username = '"&NewUserName&"' Where UserID = "&Dvbbs.UserID)
		Dvbbs.Execute("update [Dv_BbsNews] set username = '"&NewUserName&"' Where username = '"&Dvbbs.MemberName&"'")
		Dvbbs.Execute("update [Dv_BestTopic] set PostUserName = '"&NewUserName&"' Where PostUserID = "&Dvbbs.UserID)
		Dvbbs.Execute("update [Dv_BookMark] set username = '"&NewUserName&"' Where username = '"&Dvbbs.MemberName&"'")
		Dvbbs.Execute("update [Dv_Friend] set F_username = '"&NewUserName&"' Where F_userid = "&Dvbbs.UserID)
		Dvbbs.Execute("update [Dv_Friend] set F_friend = '"&NewUserName&"' Where F_friend = '"&Dvbbs.MemberName&"'")
		Dvbbs.Execute("update [Dv_Log] set l_username = '"&NewUserName&"' Where l_username = '"&Dvbbs.MemberName&"'")
		Dvbbs.Execute("update [Dv_Message] set sender = '"&NewUserName&"' Where sender = '"&Dvbbs.MemberName&"'")
		Dvbbs.Execute("update [Dv_Message] set incept = '"&NewUserName&"' Where incept = '"&Dvbbs.MemberName&"'")
		Dvbbs.Execute("update [Dv_Online] set username = '"&NewUserName&"' Where UserID = "&Dvbbs.UserID)
		Dvbbs.Execute("update [Dv_SmallPaper] set S_UserName = '"&NewUserName&"' Where S_UserName = '"&Dvbbs.MemberName&"'")
		Dvbbs.Execute("update [DV_Upfile] set F_Username = '"&NewUserName&"' Where F_UserID = "&Dvbbs.UserID)
		Dvbbs.Execute("update [Dv_Topic] set PostUserName = '"&NewUserName&"' Where PostUserID = "&Dvbbs.UserID)
		'更新圈子用户名所有数据
		Dvbbs.Execute("update [Dv_GroupName] set AppUserName = '"&NewUserName&"' Where AppUserID = "&Dvbbs.UserID)
		Dvbbs.Execute("update [Dv_GroupUser] set UserName = '"&NewUserName&"' Where UserID = "&Dvbbs.UserID)
		Dvbbs.Execute("update [Dv_Group_Topic] set PostUserName = '"&NewUserName&"' Where PostUserID = "&Dvbbs.UserID)
		Dvbbs.Execute("update [Dv_Group_BBS] set UserName = '"&NewUserName&"' Where PostUserID = "&Dvbbs.UserID)

		SQL = "Select TableName,TableType From Dv_TableList"
		Set Rs = Dvbbs.Execute(SQL)
		Do while Not Rs.eof
			Dvbbs.Execute("update "&Rs(0)&" set UserName = '"&NewUserName&"' Where PostUserID = "&Dvbbs.UserID)
			'Response.Write SQL(1,i)&"更新完成！<br>"
		Rs.Movenext
		Loop
		Rs.close
		
		'更新版块出现的用户名
		TempStr1=""
		SQL = "Select Boardid,BoardMaster,LastPost,boarduser From [Dv_Board]"
		SET Rs = Dvbbs.Execute(SQL)
		If Not Rs.eof Then
			SQL = Rs.GetRows(-1)
			Rs.close
			For i = 0 To Ubound(SQL,2)
				If GetInstr(SQL(1,i),Dvbbs.MemberName,"|")=True or GetInstr(SQL(2,i),Dvbbs.MemberName,"$")=True or GetInstr(SQL(3,i),Dvbbs.MemberName,",")=True Then
					TempStr1 = StrReplace(SQL(1,i),"|",Dvbbs.MemberName,NewUserName)
					TempStr2 = StrReplace(SQL(2,i),"$",Dvbbs.MemberName,NewUserName)
					TempStr3 = StrReplace(SQL(3,i),",",Dvbbs.MemberName,NewUserName)
					Dvbbs.Execute("update [Dv_Board] set BoardMaster = '"& TempStr1 &"',LastPost = '"& TempStr2 &"',boarduser = '"& TempStr3 &"' Where Boardid = "&SQL(0,i))
					'更新版面缓存
					Dvbbs.ReloadBoardInfo(SQL(0,i))
				End If
			Next
		Else
			Rs.close
		End If
		Dim TempStr1,TempStr2,TempStr3
		'更新主题回复用户名==================>考虑取消(>_<)
		TempStr2 = Dvbbs.MemberName & "$"
		SQL = "Select TopicID,LastPost From [Dv_Topic] where LastPost Like '"&TempStr2&"%'"
		SET Rs = Dvbbs.Execute(SQL)
		If Not Rs.eof Then
			SQL = Rs.GetRows(-1)
			Rs.close:Set Rs = Nothing
			For i = 0 To Ubound(SQL,2)
				If GetInstr(SQL(1,i),Dvbbs.MemberName,"$")=True Then
					TempStr1 = StrReplace(SQL(1,i),"$",Dvbbs.MemberName,NewUserName)
					Dvbbs.Execute("update [Dv_Topic] set LastPost = '"& TempStr1 &"' Where TopicID = "&SQL(0,i))
				End If
			Next
		Else
			Rs.close:Set Rs = Nothing
		End If
		
		'更新用户和系统使用数量
		Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
		LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，成功将用户名：<b>"&Dvbbs.MemberName&"</b>改为：<b>"&NewUserName&"</b>！"
		Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
		'更新用户Session,Cookies信息
		Dvbbs.MemberName = NewUserName
		Dvbbs.TrueCheckUserLogin
		Dvbbs.NewPassword
		Dvbbs.Dvbbs_Suc(LogMsg)
	Else
	%>
		<FORM name=theForm action="?ToolsAction=ChangID" method=post>
		<input type=hidden value="<%=Dv_Tools.ToolsID%>" name="ToolsID">
		<table cellpadding=3 cellspacing=1 align=center class=tableborder1 Style="Width:99%">
		<tr><th height=25 id="tabletitlelink" colspan="2">使用道具 <%=Dv_Tools.ToolsInfo(1)%></th></tr>
		<tr><td colspan="2" Class=tablebody2>
		<li><%=Dv_Tools.ToolsInfo(2)%>
		<li>注册用户名长度限制为：<font color="<%=Dvbbs.mainsetting(1)%>"><%=Dvbbs.Forum_Setting(40)%></font> －<font color="<%=Dvbbs.mainsetting(1)%>"><%=Dvbbs.Forum_Setting(41)%></font>字符；
		<li>改名后下一次将会用新用户名ID登陆，所以更改前务必记下新用户ID；
		<li>注意事项，由于大量数据更新，所以更新时请不要刷新，直到提示完成为止！
		</td></tr>
		<tr>
		<td width="25%" height="25" Class=tablebody1 align=right>新论坛用户名：</td>
		<td width="75%" Class=tablebody1>
		<INPUT maxLength="<%=Dvbbs.Forum_Setting(41)%>" size=30 name=name>
		</td></tr>
		<tr><td height=25 colspan="2" Class=tablebody2 align=center >
		<INPUT TYPE="submit" value="确定" onclick ="if (document.theForm.name.value == ''){alert('用户名不能为空！');return false;}">&nbsp;&nbsp;&nbsp;
		<INPUT TYPE="reset" value="重置">
		</td></tr>
		</table>
		</FORM>
	<%
	End If
End Sub

'---------------------------------------------------
'道具:群发器,可发送论坛短消息给所有在线用户。
'---------------------------------------------------
Sub Tools_21()
	ChkAction = True
	If Request("ToolsAction")="SendMsg" Then
		If Dvbbs.chkpost = False Then Dvbbs.AddErrCode(16):Exit Sub
		Dim MsgTitle,MsgBody,ErrCodes,Rs,Sql,SendUser,i
		MsgTitle = Dvbbs.Checkstr(Trim(Request.Form("MsgTitle")))
		MsgBody = Dvbbs.Checkstr(Trim(Request.Form("MsgBody")))
		If MsgTitle="" or Dvbbs.strLength(MsgTitle)>50 Then ErrCodes = ErrCodes & "<li>标题内容不能为空或超过50字节！"
		If MsgBody="" or Dvbbs.strLength(MsgBody)>Clng(Dvbbs.GroupSetting(34)) Then ErrCodes = ErrCodes & "<li>内容不能为空或超过"&Dvbbs.GroupSetting(34)&"字节！"
		If ErrCodes<>"" Then
			Response.redirect "showerr.asp?ErrCodes="&ErrCodes&"&action=NoHeadErr"
			Exit Sub
		End If
		LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，在线短信已发送完毕！"
		Sql = "Select username From Dv_online Where UserID>0 and UserID<>"&Dvbbs.UserID
		SET Rs = Dvbbs.Execute(SQL)
		If Not Rs.eof Then
			SendUser=Rs.GetRows(-1)
		Else
			Dvbbs.Dvbbs_Suc(LogMsg)
			Exit Sub
		End If
		Rs.close:Set Rs = Nothing
		MsgTitle = Dv_Tools.ToolsInfo(1)&"--"&MsgTitle
		MsgTitle = Dvbbs.Checkstr(MsgTitle)
		For i=0 To Ubound(SendUser,2)
			SendUser(0,i) = Dvbbs.Checkstr(SendUser(0,i))
			Sql="Insert into Dv_Message (incept,sender,title,content,sendtime,flag,issend) values ('"& SendUser(0,i)&"','"&Dvbbs.MemberName&"','"&MsgTitle&"','"&MsgBody&"',"&SqlNowString&",0,1)"
			Dvbbs.Execute(SQL)
			UPDATE_User_Msg(SendUser(0,i))
		Next
		'更新用户和系统使用数量
		Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
		LogMsg = LogMsg &"共发出"& i &"条论坛短信。"
		Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
		Dvbbs.Dvbbs_Suc(LogMsg)
	Else
%>
		<FORM name=theForm action="?ToolsAction=SendMsg" method=post>
		<input type=hidden value="<%=Dv_Tools.ToolsID%>" name="ToolsID">
		<table cellpadding=3 cellspacing=1 align=center class=tableborder1 Style="Width:99%">
		<tr><th height=25 id="tabletitlelink" colspan="2">使用道具 <%=Dv_Tools.ToolsInfo(1)%></th></tr>
		<tr><td colspan="2" Class=tablebody2><li><%=Dv_Tools.ToolsInfo(2)%></td></tr>
		<tr><td width="25%" height="25" Class=tablebody1 align=right><b>短信标题：</td>
		<td width="75%" Class=tablebody1><INPUT maxLength="50" size=50 name="MsgTitle">(*请在50个字符以内！)
		</td></tr>
		<tr><td height="25" Class=tablebody1 align=right Valign=Top><b>短信内容：</td>
		<td Class=tablebody1><TEXTAREA NAME="MsgBody" Style="width:100%;height:250;"></TEXTAREA>
		</td></tr>
		<tr><td height=25 colspan="2" Class=tablebody2 align=center >
		<INPUT TYPE="submit" value="确定" onclick ="if (document.theForm.name.value == ''){alert('用户名不能为空！');return false;}">&nbsp;&nbsp;&nbsp;
		<INPUT TYPE="reset" value="重置">
		</td></tr>
		</table>
		</FORM>
<%
	End If
End Sub

'---------------------------------------------------
'道具:偷窥器，可查看他人金币、点券和道具等保密信息
'---------------------------------------------------
Sub Tools_22()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(ToUserID )
%>
<table border="0" cellpadding=3 cellspacing=1 align=center class=Tableborder1 Style="Width:99%">
	<tr>
	<th height=23 colspan=2>使用道具 <%=Dv_Tools.ToolsInfo(1)%></th></tr>
	<tr>
	<td height=23 class=Tablebody1 width="30%" align=right>目标用户：</td>
	<td height=23 class=Tablebody1 width="70%"><B><%=Dv_Tools.ToUserInfo(1)%></B></td>
	</tr>
	<tr>
	<td height=23 class=Tablebody1 width="30%" align=right>金币数量：</td>
	<td height=23 class=Tablebody1 width="70%">
	<B><%=Dv_Tools.ToUserInfo(5)%></B>
	个
	</td>
	</tr>
	<tr>
	<td height=23 class=Tablebody1 width="30%" align=right>点券数量：</td>
	<td height=23 class=Tablebody1 width="70%">
	<B><%=Dv_Tools.ToUserInfo(6)%></B>
	张
	</td>
	</tr>
	<tr><td height=23 class=Tablebody1 colspan=2 align=center>
	<B>该用户道具信息</B>
	</td></tr>
<%
	Set Rs=Dvbbs.Plus_Execute("Select ToolsName,ToolsCount From [Dv_Plus_Tools_Buss] Where ToolsCount>0 and UserID="& Dv_Tools.ToUserInfo(0)&" Order By ToolsCount Desc")
	Do While Not Rs.Eof
%>
	<tr>
	<td height=23 class=Tablebody1 width="30%" align=right><%=Rs(0)%></td>
	<td height=23 class=Tablebody1 width="70%">
	<B><%=Rs(1)%></B>
	个
	</td>
	</tr>
<%
	Rs.MoveNext
	Loop
	Rs.Close
	Set Rs=Nothing
%>
</table>
<%
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，查看<B>"&Dv_Tools.ToUserInfo(1)&"</B>的用户信息！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	'Dvbbs.Dvbbs_Suc(LogMsg)
End Sub

'---------------------------------------------------
'道具:查税卡，可对用户的金币执行一定比例的税收，收入归使用者，比例由系统定义
'---------------------------------------------------
Sub Tools_23()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	Dim GetPercent,GetPercentMoney
	Dim iUserInfo
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(ToUserID)
	If Dvbbs.UserID = Clng(Dv_Tools.ToUserInfo(0)) Then
		Dv_Tools.ShowErr(14)
		Exit Sub
	End If
	GetPercent = cCur(Dv_Tools.ToolsSetting(4))
	GetPercentMoney = cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text) + cCur(Dv_Tools.ToUserInfo(5)) * GetPercent
	Dvbbs.Execute("Update Dv_User Set UserMoney="&GetPercentMoney&" Where UserID="&Dvbbs.UserID)
	Dvbbs.Execute("Update Dv_User Set UserMoney=UserMoney - "&cCur(Dv_Tools.ToUserInfo(5)) * GetPercent&" Where UserID="&Dv_Tools.ToUserInfo(0))
	Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = GetPercentMoney
	'发短信通知目标用户
	Sql="Insert into Dv_Message (incept,sender,title,content,sendtime,flag,issend) values ('"& Dv_Tools.ToUserInfo(1)&"','"&Dvbbs.MemberName&"','您在论坛上被"&Dvbbs.Membername&"使用了查税卡','您在论坛上被"&Dvbbs.Membername&"使用了查税卡，被该用户收走了 "&cCur(Dv_Tools.ToUserInfo(5)) * GetPercent&" 个金币，被收走的税额为您总金币值的比例为 "&GetPercent&"，关于查税卡和相关的道具信息请看论坛道具中心，感谢您的参与。',"&SqlNowString&",0,1)"
	Dvbbs.Execute(SQL)
	UPDATE_User_Msg(Dv_Tools.ToUserInfo(1))

	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，收取<B>"&Dv_Tools.ToUserInfo(1)&"</B>用户<B>"&cCur(Dv_Tools.ToUserInfo(5)) * GetPercent&"</B>个金币！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,GetPercentMoney&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub

'---------------------------------------------------
'道具:均富卡，可使使用用户和目标用户金币数相同
'---------------------------------------------------
Sub Tools_24()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	Dim GetPercentMoney
	Dim iUserInfo
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(ToUserID )
	If Dvbbs.UserID = Clng(Dv_Tools.ToUserInfo(0)) Then
		Dv_Tools.ShowErr(14)
		Exit Sub
	End If
	GetPercentMoney = cCur(Dv_Tools.ToUserInfo(5))
	Dvbbs.Execute("Update Dv_User Set UserMoney="&GetPercentMoney&" Where UserID="&Dvbbs.UserID)
	Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = GetPercentMoney
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，利润为<B>"&GetPercentMoney - cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)&"</B>个金币！"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,GetPercentMoney&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub

'---------------------------------------------------
'道具:均贫卡，可使目标用户和使用用户金币数相同
'---------------------------------------------------
Sub Tools_25()
	Dim Rs,Sql
	Dim T_Title,T_UseTools,T_PostTable
	Dim GetPercentMoney
	Dim iUserInfo
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(ToUserID )
	If Dvbbs.UserID = Clng(Dv_Tools.ToUserInfo(0)) Then
		Dv_Tools.ShowErr(14)
		Exit Sub
	End If
	If cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)<cCur(Dv_Tools.ToUserInfo(5)) Then
		'更新目标用户
		GetPercentMoney = cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)
		Dvbbs.Execute("Update Dv_User Set UserMoney="&GetPercentMoney&" Where UserID="&Dv_Tools.ToUserInfo(0))
		LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，目标用户<b>"& Dv_Tools.ToUserInfo(1) &"</b>的金币数已经和您相等！"
		'短信通知目标用户
		Sql="Insert into Dv_Message (incept,sender,title,content,sendtime,flag,issend) values ('"& Dv_Tools.ToUserInfo(1)&"','"&Dvbbs.MemberName&"','您在论坛上被"&Dvbbs.Membername&"使用了均贫卡','您在论坛上被"&Dvbbs.Membername&"使用了均贫卡，目前金币值为 "&GetPercentMoney&" 个，被系统收走了 "&cCur(Dv_Tools.ToUserInfo(5)) - cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)&" 个金币，关于均贫卡和相关的道具信息请看论坛道具中心，感谢您的参与。',"&SqlNowString&",0,1)"
		Dvbbs.Execute(SQL)
		UPDATE_User_Msg(Dv_Tools.ToUserInfo(1))
	Else
		'更新使用用户
		GetPercentMoney = cCur(Dv_Tools.ToUserInfo(5))
		Dvbbs.Execute("Update Dv_User Set UserMoney="&GetPercentMoney&" Where UserID="&Dvbbs.UserID)
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = GetPercentMoney
		LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，您的金币数已经和目标用户<b>"& Dv_Tools.ToUserInfo(1) &"</b>相等，利润为<B>"&GetPercentMoney - cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)&"</B>个金币！"
	End If

	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub

'---------------------------------------------------
'道具:抢夺卡，可随机抢夺目标用户身上1/3到1/2的卡片
'---------------------------------------------------
Sub Tools_26()
	Dim Rs,Sql
	Dim rndnum,n,i
	Dim ToolsCount,SucMsg
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(ToUserID )
	If Dvbbs.UserID = Clng(Dv_Tools.ToUserInfo(0)) Then
		Dv_Tools.ShowErr(14)
		Exit Sub
	End If
	Randomize
	rndnum=Cint(9999*rnd+1)
	If rndnum Mod 2 = 0 Then
		n = 1
	Else
		n = 2
  	End If
	Sql = "Select Count(ID) From [Dv_Plus_Tools_Buss] Where ToolsCount>0 and UserID="&ToUserID
	ToolsCount = Dvbbs.Plus_Execute(Sql)(0)
	If ToolsCount=0 Then 
		Dv_Tools.ShowErr(18)
		Exit Sub
	End If
	Select Case n
	'获取目标用户1/3道具	
	Case 1
		ToolsCount = ToolsCount \ 3
	'获取目标用户1/2道具
	Case 2
		ToolsCount = ToolsCount \ 2
	End Select
	Sql = "Select TOP "&ToolsCount&" ID,UserName,ToolsID,ToolsName From [Dv_Plus_Tools_Buss] Where ToolsCount>0 and  UserID="&ToUserID
	Set Rs = Dvbbs.Plus_Execute(SQL)
	SQL = Rs.GetRows(-1)
	Rs.close 
	Set Rs = Nothing
	For i=0 To Ubound(SQL,2)
		Call UpdateBussTools(Dvbbs.UserID,Sql(2,i),-1)	'给用户添加道具
		Dvbbs.Plus_Execute("Update [Dv_Plus_Tools_Buss] Set ToolsCount = ToolsCount-1 Where ID="&Sql(0,i))
		LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，你已成功从<b>"&Sql(1,i)&"</b>用户获取道具卡《<b>"&Sql(3,i)&"</b>》。"
		Call Dvbbs.ToolsLog(Sql(2,i),1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
		SucMsg = SucMsg &"<li>"& LogMsg
	Next
	SucMsg = SucMsg & "<li>总共获得"&Ubound(SQL,2)+1&"张道具。"
	Dvbbs.Dvbbs_Suc(SucMsg)
End Sub

'---------------------------------------------------
'道具:复仇卡，当被人使用恶性道具引起损失，此道具会自动使用并惩罚所使用恶性道具方用户
'---------------------------------------------------
Sub Tools_27()
	Dim Rs,Sql
	ChkAction = True
	If ToUserID = 0 Then ChkAction = False
	If ChkAction = False Then Dvbbs.AddErrCode(42) : Exit Sub
	'判断目标用户使用权限并取出目标用户信息
	Dv_Tools.ChkToUseTools(ToUserID)
	If Dvbbs.UserID = Clng(Dv_Tools.ToUserInfo(0)) Then
		Dv_Tools.ShowErr(14)
		Exit Sub
	End If
	Dim GetPercent,GetPercentMoney

	LogMsg = "使用：<B>"& Dv_Tools.ToolsInfo(1) &"</B>成功，您的金币数已经和目标用户相等，利润为<B>"&GetPercentMoney - cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)&"</B>个金币！"
	Call UpdateUserTools(Dvbbs.UserID,Dv_Tools.ToolsID,1)
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,1,0,0,1,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	Dvbbs.Dvbbs_Suc(LogMsg)
End Sub

'---------------------------------------------------
'道具:获赠金币帖 回复用户可自定数量金币赠送给帖主，赠送总数量在帖子中特殊显示。
'---------------------------------------------------
Sub Tools_28()
Response.redirect "showerr.asp?ErrCodes=<li>该道具只能对采用相关属性类型的帖子进行操作！&action=NoHeadErr"
End Sub
'---------------------------------------------------
'道具:金币购买帖 发帖者可以定义帖子出售金币值，浏览者需要支付金币购买才可以查看帖子全部内容。
'---------------------------------------------------
Sub Tools_29()
Response.redirect "showerr.asp?ErrCodes=<li>该道具只能对采用相关属性类型的帖子进行操作！&action=NoHeadErr"
End Sub
'------------------------------------------------------------------------------------------------------
'
'------------------------------------------------------------------------------------------------------
'更新用户及系统道具数量(用户ID，道具ID，减少数量)
Sub UpdateUserTools(U_UserID,U_ToolsID,n)
	Dim Sql,Rs,UpdateNum
	If Clng(n)<0 Then
		UpdateNum = "+" & -n
	Else
		UpdateNum = "-" & n
	End If
	Set Rs = Dvbbs.Plus_Execute("Select ID From [Dv_Plus_Tools_Buss] Where UserID="& U_UserID &" and ToolsID="& U_ToolsID)
	If Rs.Eof And Rs.Bof Then
		Dim Trs
		Set Trs = Dvbbs.Plus_Execute("Select ToolsName From Dv_Plus_Tools_Info Where ID=" & U_ToolsID)
		If Not (Trs.Eof And Trs.Bof) Then
			Sql = "Insert Into [Dv_Plus_Tools_Buss] (UserID,UserName,ToolsID,ToolsName,ToolsCount) Values ("&U_UserID&",'"&Dv_Tools.ToUserInfo(1)&"',"&U_ToolsID&",'"&Trs(0)&"',"&Clng(Replace(Replace(UpdateNum,"+",""),"-",""))&")"
			Dvbbs.Plus_Execute(Sql)
		End If
		Trs.Close
		Set Trs=Nothing
	Else
		Sql = "Update [Dv_Plus_Tools_Buss] Set ToolsCount = ToolsCount"&UpdateNum&" Where UserID="& U_UserID &" and ToolsID="& U_ToolsID
		Dvbbs.Plus_Execute(Sql)
	End If
	Rs.Close
	Set Rs=Nothing
	Sql = "Update [Dv_Plus_Tools_Info] Set UserStock =  UserStock"&UpdateNum&" Where ID="& U_ToolsID
	Dvbbs.Plus_Execute(Sql)
End Sub

'更新用户道具数量(用户ID，道具ID，减少数量)
Sub UpdateBussTools(U_UserID,U_ToolsID,n)
	Dim Sql,Rs,UpdateNum
	If n<0 Then
		UpdateNum = "+" & -n
	Else
		UpdateNum = "-" & n
	End If
	Set Rs = Dvbbs.Plus_Execute("Select ID From [Dv_Plus_Tools_Buss] Where UserID="& U_UserID &" and ToolsID="& U_ToolsID)
	If Rs.Eof And Rs.Bof Then
		Dim Trs
		Set Trs = Dvbbs.Plus_Execute("Select ToolsName From Dv_Plus_Tools_Info Where ID=" & U_ToolsID)
		If Not (Trs.Eof And Trs.Bof) Then
			Sql = "Insert Into [Dv_Plus_Tools_Buss] (UserID,UserName,ToolsID,ToolsName,ToolsCount) Values ("&U_UserID&",'"&Dv_Tools.ToUserInfo(1)&"',"&U_ToolsID&",'"&Trs(0)&"',"&Clng(Replace(Replace(UpdateNum,"+",""),"-",""))&")"
			Dvbbs.Plus_Execute(Sql)
		End If
		Trs.Close
		Set Trs=Nothing
	Else
		Sql = "Update [Dv_Plus_Tools_Buss] Set ToolsCount = ToolsCount"&UpdateNum&" Where UserID="& U_UserID &" and ToolsID="& U_ToolsID
		Dvbbs.Plus_Execute(Sql)
	End If
	Rs.Close
	Set Rs=Nothing
End Sub

Function LoadTitle(Str)
	LoadTitle = "帖子《<a href=""dispbbs.asp?BoardID="&Dvbbs.BoardID&"&ID="&TopicID&"&ReplyID="&ReplyID&"&Skin=1"" target=_blank><b>"&Server.HtmlEncode(Str)&"</b></a>》"
	LoadTitle = Dvbbs.Checkstr(LoadTitle)
End Function
Function LoadUserTools(Str,AddID)
	If Str="" or IsNull(Str) Then _
		LoadUserTools = AddID _
	Else _
		LoadUserTools = Str & "," & AddID
	LoadUserTools = Dvbbs.Checkstr(LoadUserTools)
End Function

'参数：旧原始值，分隔字符，旧名，替换的新名
Function StrReplace(str,splits,oldstr,newstr)
	If str<>"" or Not Isnull(str) Then
		Dim TempStr
		TempStr = splits & str & splits
		TempStr = Replace(TempStr , splits & oldstr & splits , splits & newstr & splits)
		TempStr = Mid(TempStr,2,Len(TempStr)-2)	'去掉先后临时分隔符
		StrReplace = Dvbbs.CheckStr(TempStr)
	Else
		StrReplace = ""
	End If
End Function
'参数：目标字符，比较字符，分隔字符
Function GetInstr(Str1,Str2,Strings)
	GetInstr = False
	If Str1="" or IsNull(Str1) Then Exit Function
	If Instr(Strings & Str1 & Strings,Strings & Str2 & Strings) Then GetInstr = True
End Function

'短信相关更新
Sub UPDATE_User_Msg(username)
	Dim msginfo
	If newincept(username)>0 Then
		msginfo=newincept(username) & "||" & inceptid(3,username)
	Else
		msginfo="0||0||null"
	End If
	Dvbbs.Execute("update [dv_user] set UserMsg='"&Dvbbs.CheckStr(msginfo)&"' where username='"&Dvbbs.CheckStr(username)&"'")
End Sub
Function inceptid(stype,iusername)
	Dim Rs
	Set Rs=Dvbbs.Execute("Select top 1 id,sender From Dv_Message Where Flag=0 and issend=1 and delR=0 And incept ='"& iusername &"'")
		If stype=1 then
			inceptid=Rs(0)
		ElseIf stype=2 Then
			inceptid=Rs(1)
		Else
			inceptid=Rs(0) &"||"& Rs(1)
		End If
	Set Rs = Nothing
End Function
'统计留言
Function newincept(iusername)
	Dim Rs
	Rs=Dvbbs.Execute("Select Count(id) From Dv_Message Where Flag=0 and issend=1 and delR=0 And incept='"& iusername &"'")
	newincept = Rs(0)
	Set Rs=Nothing
	If IsNull(newincept) Then newincept=0
End Function

'查看用户来源
Function lookaddress(sip)
	Dim str1,str2,str3,str4
	Dim num
	Dim irs,SQL
	If isnumeric(left(sip,2)) Then
		If sip="127.0.0.1" Then sip="192.168.0.1"
		str1=left(sip,instr(sip,".")-1)
		sip=mid(sip,instr(sip,".")+1)
		str2=left(sip,instr(sip,".")-1)
		sip=mid(sip,instr(sip,".")+1)
		str3=left(sip,instr(sip,".")-1)
		str4=mid(sip,instr(sip,".")+1)
		If isNumeric(str1)=0 Or isNumeric(str2)=0 Or isNumeric(str3)=0 Or isNumeric(str4)=0 Then

		Else
			num=cint(str1)*256*256*256+cint(str2)*256*256+cint(str3)*256+cint(str4)-1
			Dim adb,aConnStr,AConn
			adb = "data/ipaddress.mdb"
			aConnStr = "Provider = Microsoft.Jet.OLEDB.4.0;Data Source = " & Server.MapPath(adb)
			Set AConn = Dvbbs.iCreateObject("ADODB.Connection")
			aConn.Open aConnStr
			sql="select country,city from dv_address where ip1 <="&num&" and ip2 >="&num
			Set irs=aConn.Execute(sql)
			If irs.eof And irs.bof Then 
				lookaddress=template.Strings(12)
			Else
				Do While Not irs.eof
					lookaddress=lookaddress & "<br>" &irs(0) & irs(1)
				irs.movenext
				Loop
			End If
			irs.close
			Set irs=nothing
			Set AConn=Nothing
		End If
	Else
		lookaddress=template.Strings(12)
	End If
End Function

'所有论坛发帖数减少
Function AllboardNumSub(todayNum,postNum,topicNum)
	Dvbbs.Execute("Update dv_Setup Set Forum_TodayNum=Forum_TodayNum-"&todaynum&",Forum_PostNum=Forum_PostNum-"&postNum&",Forum_TopicNum=Forum_TopicNum-"&TopicNum)
	Dvbbs.ReloadSetupCache CLng(Dvbbs.CacheData(7,0))-TopicNum,7
	Dvbbs.ReloadSetupCache CLng(Dvbbs.CacheData(8,0))-postNum,8
	Dvbbs.ReloadSetupCache CLng(Dvbbs.CacheData(9,0))-todaynum,9
End Function

'版面发帖数减少
Sub BoardNumSub(boardID,topicNum,postNum,todayNum)
	Dim iUpdateBoardID,UpdateBoardID
	Dim trs,LastPostTime,LastpostuserID,Lastid,uploadpic_n
	Dim Lasttopic,LastRootid,LastPostUser,LastPost,i,sql
	UpdateBoardID = Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@parentstr").text & "," & Dvbbs.BoardID
	Set trs=Dvbbs.Execute("select top 1 T.title,b.Announceid,b.dateandtime,b.username,b.postuserid,b.rootid from "&Dvbbs.NowUseBBS&" b inner join dv_Topic T on b.rootid=T.TopicID where b.boardid="&boardid&" order by b.Announceid desc")
	If not(trs.eof and trs.bof) Then
		Lasttopic=replace(left(trs(0),15),"$","")
		LastRootid=trs(1)
		LastPostTime=trs(2)
		LastPostUser=trs(3)
		LastPostUserid=trs(4)
		Lastid=trs(5)
	else
		LastTopic="无"
		LastRootid=0
		LastPostTime=now()
		LastPostUser="无"
		LastPostUserid=0
		Lastid=0
	End If
	trs.close
	Set trs=nothing	
	LastPost=LastPostUser & "$" & LastRootid & "$" & LastPostTime & "$" & LastTopic & "$" & uploadpic_n & "$" & LastPostUserID & "$" & LastID & "$" & BoardID
	'更新最后回复及帖子数 2005-12-13 Dv.Yz
	Dvbbs.Execute("UPDATE Dv_Board SET Postnum = Postnum - " & PostNum & ", TopicNum = TopicNum - " & TopicNum & ", TodayNum = TodayNum - " & TodayNum & ", LastPost = '" & Dvbbs.CheckStr(LastPost) & "' WHERE BoardID IN (" & UpdateBoardID & ")")
	iUpdateBoardID = Split(UpdateBoardID,",")
	For i=0 To Ubound(iUpdateBoardID)
		Dvbbs.LoadBoardinformation iUpdateBoardID(i)
	Next
End Sub
%>
