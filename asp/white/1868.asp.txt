<!-- #include file="conn.asp" -->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/Dv_ubbcode.asp" -->
<!-- #include file="inc/dv_clsother.asp" -->
<!--#include file="inc/ubblist.asp"-->
<%
Dim DvUbb
Set DvUbb = New Dvbbs_UbbCode
DvUbb.PostType=1
Select Case Request("t")
Case "1"
	ViewVoters_Main()
Case "2"
	Dim Rootid,Action,TopicInfo,BBsInfo,BBsReplyInfo,PostTable,ReplyID
	ViewTopicInfo_Main()
Case Else
	Dim dv_ubb,abgcolor
	ViewPaper_Main()
End Select
Set dv_ubb=Nothing

Dvbbs.PageEnd()
Sub ViewPaper_Main()
	Dvbbs.LoadTemplates("paper_even_toplist")
	Dvbbs.stats=template.Strings(3)
	Dvbbs.Head()
	Dim paperid
	Dim username
	If Request("id")="" Then
		Dvbbs.AddErrCode(35)
	ElseIf Not IsNumeric(Request("id")) Then
		Dvbbs.AddErrCode(35)
	Else
		paperID=clng(Request("id"))
	End If
	Dvbbs.ShowErr()
	Set dv_ubb=new Dvbbs_UbbCode
	dv_ubb.PostType=2
	Dim Rs,Sql
	Set Rs=Dvbbs.iCreateObject("Adodb.Recordset")
	Sql="Select * From Dv_SmallPaper Where s_id="&paperid
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.Eof And Rs.Bof Then
		Dvbbs.AddErrCode(32)
		Rs.Close
		Set Rs=Nothing	
		Dvbbs.ShowErr()
	Else
		Dvbbs.Execute("Update Dv_SmallPaper Set s_hits=s_hits+1 Where s_id="&paperid)
		Dim TempStr
		TempStr = template.html(4)
		TempStr = Replace(TempStr,"{$title}",Dvbbs.Htmlencode(rs("s_title")))
		TempStr = Replace(TempStr,"{$username}",Dvbbs.Htmlencode(rs("s_username")))
		TempStr = Replace(TempStr,"{$hits}",rs("s_hits"))
		ubblists=ubblist(Rs("s_content"))&"39,"
		TempStr = Replace(TempStr,"{$content}",Dvbbs.HTMLEncode(dv_ubb.Dv_UbbCode(Rs("s_content"),4,2,1)))
		TempStr = Replace(TempStr,"{$addtime}",rs("s_addtime"))
		Response.Write TempStr
		Rs.Close
		Set Rs=Nothing
	End If

	Dvbbs.ActiveOnline()
	Dvbbs.Footer()
End Sub

Sub ViewVoters_Main()
	Dim voteid,votetype
	Dim title,votevalue,votevaluestr,voteoption
	Dim TempArray,TempStr,TempStr1,TempStr2,TempStr3
	Dim UserID
	Dvbbs.Loadtemplates("dispbbs")
	Dvbbs.Stats=template.Strings(12)
	Dvbbs.head
	If Request("id")="" then
		Dvbbs.AddErrCode(30)
	ElseIf Not IsNumeric(Request("id")) then
		Dvbbs.AddErrCode(30)
	Else
		VoteID=Request("id")
	End If
	UserID = 0

	If Request("UserID")="" or Not Isnumeric(Request("UserID")) Then
		If Dvbbs.Userid>0 Then
			UserID = Dvbbs.Userid
		End If
	Else
		UserID = Int(Request("UserID"))
	End If

	Dvbbs.ShowErr
	TempArray = Split(template.html(1),"||")
	TempStr = TempArray(0)
	Dim Rs,i
	Dim Sql,k
	Dim LockVote,TimeOut
	Dim Viewvote
	Dim votenum
	Viewvote = False
	Set Rs=Dvbbs.Execute("select vote,votetype,LockVote,TimeOut,votenum from dv_vote where voteid="&voteid)
	If Not (rs.eof And rs.bof) Then
		votevalue=Split(rs(0),"|")
		votetype = Rs(1)
		LockVote = Rs(2)
		TimeOut = Rs(3)
		votenum = Rs(4)
	Else
		Dvbbs.AddErrCode(30)
		Dvbbs.ShowErr
	End If

	If LockVote=0 Then
		Viewvote = True
	Else
		If Dvbbs.master Or Dvbbs.superboardmaster Or Dvbbs.boardmaster Then
			Viewvote = True
		End If
	End If
	Set Rs=Dvbbs.Execute("select title,postuserid from dv_topic where pollid="&voteid)
	If Not (Rs.EOF And rs.bof) Then
		Title=Dvbbs.HtmlEncode(rs(0))
		If Dvbbs.UserID = Rs(1) Then
			Viewvote = True
		End If
	Else
		Dvbbs.AddErrCode(30)
		Dvbbs.ShowErr
	End If
	
	If Not Viewvote Then
		If cCur(DateDiff("s", Now, TimeOut))<0 Then
			Viewvote = True
		End If
	End If
	
	TempStr = Replace(TempStr,"{$title}",title)
	Sql = "select v.UserID,v.voteoption,v.VoteDate,u.username from dv_voteuser v inner join [dv_user] u on v.userid=u.userid where voteid="&voteid
	If (votetype=2 and Request.QueryString("showall")="1") or Not Viewvote Then
		Sql = Sql & " and v.userid="&UserID
	End If
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.Eof And Rs.Bof Then
		TempStr = Replace(TempStr,"{$voteinfo}",TempArray(2)&TempArray(4))
	Else
		Sql = Rs.GetRows(-1)
	End If
	Rs.Close
	Set Rs =Nothing


	Dim Votechilds,Votechilds_Title,Votechilds_Type,Votechilds_Ep,VotechildsCheBox_Ep
	Dim VoteEp,j,n,VoteChild_Ep,TempStr4,TempStr5
	VoteEp = 0
	If IsArray(Sql) Then
		If votetype=2 and (Request.QueryString("showall")="1" or Not Viewvote) Then
			'调查设票显示
			voteoption = Split(Sql(1,0),"|")
			For k=0 to Ubound(votevalue)
				Votechilds = Split(votevalue(k),"@@")
				Votechilds_Type = Votechilds(1)
				Votechilds_Title = Split(Votechilds(2),"$$")
				Votechilds_Ep = Split(Votechilds(3),"$$")
				TempStr2 = TempArray(6)
				TempStr2 = Replace(TempStr2,"{$votetopic}",Votechilds(0))
					If Votechilds_Type = "2" Then
					'问题形式
						TempStr4 = Replace(TempArray(10),"{$childtitle1}",DvUbb.Dv_UbbCode(voteoption(k),Dvbbs.UserGroupid,1,0))
						
						If Trim(Votechilds_Title(0)) = "null" or Not Viewvote Then
							TempStr4 = Replace(TempStr4,"{$childtitle2}","无")
						Else
							TempStr4 = Replace(TempStr4,"{$childtitle2}",DvUbb.Dv_UbbCode(Votechilds_Title(0),Dvbbs.UserGroupid,1,0))
						End If
					Else
						If Votechilds_Type = "1" Then
							VotechildsCheBox_Ep = Split(voteoption(k),"$$")
							TempStr4 = ""
							For j=0 to Ubound(VotechildsCheBox_Ep)
								If Isnumeric(VotechildsCheBox_Ep(j)) Then
									VoteChild_Ep = cCur(VoteChild_Ep) + cCur(Votechilds_Ep(VotechildsCheBox_Ep(j)))
									TempStr4 = TempStr4 & Replace(TempArray(9),"{$childtitle}",Votechilds_Title(VotechildsCheBox_Ep(j)))
								End If
							Next
						Else
							VoteChild_Ep = cCur(Votechilds_Ep(voteoption(k)))
							TempStr4 =  Replace(TempArray(8),"{$childtitle}",Votechilds_Title(voteoption(k)))
						End If
						TempStr4 = Replace(TempStr4,"{$childep}",VoteChild_Ep)
						If Isnumeric(VoteChild_Ep) Then
							VoteEp = VoteEp+cCur(VoteChild_Ep)
							VoteChild_Ep = 0
						End If
					End If
				TempStr5 = Replace(TempArray(7),"{$childitems}",TempStr4)
				TempStr2 = Replace(TempStr2,"{$childinfo}",TempStr5)
				TempStr3 = TempStr3 & TempStr2
			Next
			TempStr1 = Replace(TempArray(5),"{$votechildinfo}",TempStr3)
			TempStr1 = Replace(TempStr1,"{$username}",Sql(3,0))
			TempStr1 = Replace(TempStr1,"{$userid}",Sql(0,0))
			TempStr1 = Replace(TempStr1,"{$votep}",VoteEp)
			TempStr = Replace(TempStr,"{$voteinfo}",TempStr1)
		ElseIf Request.QueryString("showall")="2" and votetype=2 Then
			TempStr1 = TempArray(11)
			votenum = Split(votenum,"|")
			voteoption = Split(Sql(1,0),"|")
			For k=0 to Ubound(votevalue)
				TempStr5 = ""
				Votechilds = Split(votevalue(k),"@@")
				Votechilds_Type = Votechilds(1)
				Votechilds_Title = Split(Votechilds(2),"$$")
				Votechilds_Ep = Split(votenum(k),"$$")
				TempStr2 = TempArray(6)
				TempStr2 = Replace(TempStr2,"{$votetopic}",Votechilds(0))
				TempStr2 = Replace(TempStr2,"{$childinfo}",TempArray(7))
				For i=0 to Ubound(Votechilds_Title)-1
					TempStr4 = TempArray(12)
					TempStr4 = Replace(TempStr4,"{$childtopic}",Votechilds_Title(i))
					TempStr4 = Replace(TempStr4,"{$childnum}",Votechilds_Ep(i))
					
					TempStr5 = TempStr5 & TempStr4
				Next
				TempStr2 = Replace(TempStr2,"{$childitems}",TempStr5)
				TempStr3 = TempStr3 & TempStr2
			Next
			TempStr1 = Replace(TempStr1,"{$votechildinfo}",TempStr3)
			TempStr = Replace(TempStr,"{$voteinfo}",TempStr1)
		Else
			'多选与单选显示或查看用户调查得分
			'TempStr1 = TempArray(1)&TempArray(3)
			For K=0 to ubound(Sql,2)
				VoteEp = 0
				TempStr2 = TempArray(3)
				TempStr2 = Replace(TempStr2,"{$userid}",Sql(0,k))
				If Dvbbs.UserID = Sql(0,k) Then
					TempStr2 = Replace(TempStr2,"{$username}","<font class=""redfont"">"&Sql(3,k)&"</font>")
				Else
					TempStr2 = Replace(TempStr2,"{$username}",Sql(3,k))
				End If

				If votetype=2 Then
					voteoption = Split(Sql(1,k),"|")
					For i=0 to Ubound(votevalue)
					Votechilds = Split(votevalue(i),"@@")
					Votechilds_Type = Votechilds(1)
					Votechilds_Title = Split(Votechilds(2),"$$")
					Votechilds_Ep = Split(Votechilds(3),"$$")
						If Votechilds_Type = "1" Then
							VotechildsCheBox_Ep = Split(voteoption(i),"$$")
							For j=0 to Ubound(VotechildsCheBox_Ep)
								If Isnumeric(VotechildsCheBox_Ep(j)) Then
									VoteChild_Ep = cCur(VoteChild_Ep) + cCur(Votechilds_Ep(VotechildsCheBox_Ep(j)))
								End If
							Next
						ElseIf Votechilds_Type = "0" Then
							VoteChild_Ep = cCur(Votechilds_Ep(voteoption(i)))
							
						Else
							VoteChild_Ep = 0
						End If
						If Isnumeric(VoteChild_Ep) Then
							VoteEp = VoteEp+cCur(VoteChild_Ep)
							VoteChild_Ep = 0
						End If
					Next
					TempStr2 = Replace(TempStr2,"{$uservote}","总得分：<font class=""redfont"">"&VoteEp&"</font>,[<a href=""viewinfo.asp?showall=1&t=1&userid="&Sql(0,k)&"&boardid="&Dvbbs.boardid&"&id="&voteid&""">查看该用户投票信息</a>]")
				Else
					TempStr2 = TempArray(3)
					voteoption = Split(Sql(1,k),",")
					TempStr2 = Replace(TempStr2,"{$userid}",Sql(0,k))
					If Dvbbs.UserID = Sql(0,k) Then
						TempStr2 = Replace(TempStr2,"{$username}","<font class=""redfont"">"&Sql(3,k)&"</font>")
					Else
						TempStr2 = Replace(TempStr2,"{$username}",Sql(3,k))
					End If
					For i = 0 To Ubound(voteoption)
						If IsNumeric(voteoption(i)) Then
							If i<>0 Then votevaluestr = votevaluestr & "<br />"
							votevaluestr = votevaluestr & votevalue(voteoption(i))
						End If
					Next
					TempStr2 = Replace(TempStr2,"{$uservote}",votevaluestr)
					votevaluestr = ""
				End If
				TempStr3 = TempStr3 & TempStr2
			Next
			TempStr = Replace(TempStr,"{$voteinfo}",TempArray(1)&TempStr3)
		End If

	End If
	TempStr = Replace(TempStr,"{$tourl}","viewinfo.asp?showall=0&t=1&boardid="&Dvbbs.boardid&"&id="&voteid)
	TempStr = Replace(TempStr,"{$counttourl}","viewinfo.asp?showall=2&t=1&boardid="&Dvbbs.boardid&"&id="&voteid)
	Response.Write TempStr
End Sub

Sub ViewTopicInfo_Main()
	Dvbbs.LoadTemplates("dispbbs")
	Dvbbs.ErrType = 1 '设置错误提示信息显示模式
	Dvbbs.mainsetting(0)="98%"
	Action = Request("action")
	Rootid = Request("ID")
	PostTable = Request("PostTable")
	'PostTable = Checktable(PostTable)

	ReplyID = Request("ReplyID")
	If Rootid="" Or Not IsNumeric(Rootid) Then Dvbbs.AddErrCode(35)
	If Dvbbs.GroupSetting(2)<>1 Then Dvbbs.AddErrCode(31)
	Dvbbs.ShowErr()
	Rootid  = Clng(Rootid)
	Select Case Action
		Case "View" : Dvbbs.stats="查看贴子的信息"
		Case Else
		Dvbbs.stats="购买帖子"
	End Select
	'Dvbbs.Nav
	'Dvbbs.Head_var 1,Dvbbs.Board_Data(4,0),"",""
	Dvbbs.Head()
	view_Topic()
	If IsNumeric(ReplyID) and ReplyID<>"" Then
		ReplyID = cCur(ReplyID)
		If cCur(BBsInfo(5,0)) <> ReplyID Then view_Dispbbs
	End If
	FootInfo()
	Dvbbs.ShowErr()
	Dvbbs.Activeonline()
	Dvbbs.Footer
End Sub

Sub view_Dispbbs()
	GetBBsReplyInfo
	Dvbbs.ShowErr()
%>
	<table border="0" cellpadding="4" cellspacing="1" align="center" class="tableborder1" style="width:99%">
		<tr><th colspan=2 height=23>该回复帖信息</th></tr>
		<tr><td class="tablebody2" width="20%" height=23 align="right"><b>回复作者</b>：</td><td class="tablebody1" width="80%" >
		<% If BBsReplyInfo(8,0)=2 and Dvbbs.Board_Setting(68)="1" and Not Dvbbs.master Then%>
		匿名用户
		<% Else%>
		<%=UserInfoUrl(BBsReplyInfo(0,0))%>
		<% End If%>		
		</td></tr>
		<tr><td class="tablebody2" height="23" align="right"><b>回复时间</b>：</td><td class="tablebody1"><%=BBsReplyInfo(2,0)%></td></tr>
		<%If DVbbs.Forum_Setting(90)="1" Then %>
		<tr><td class="tablebody2" height="23" align="right"><b>使用道具</b>：</td><td class="tablebody1"><%=GetTopicToolsInfo(BBsReplyInfo(6,0))%></td></tr>
		<%End If%>
	</table>
<%
End Sub

Sub view_Topic()
	GetTopicInfo()
	GetBBsInfo()
	Dvbbs.ShowErr()
	If TopicInfo(12,0)<>1 Then TopicInfo(0,0) = Dvbbs.iHtmlencode(TopicInfo(0,0))
%>
	<table border="0" cellpadding="4" cellspacing="1" align="center" class="tableborder1" Style="width:99%">
	<tr><th colspan=4 height=23>《<%=TopicInfo(0,0)%>》 主题信息</th></tr>
	<tr><td class="tablebody2" width="20%" height="23" align="right"><b>主题作者</b>：</td><td class="tablebody1" width="80%" colspan="3">
	<% If TopicInfo(13,0)=1  and Dvbbs.Board_Setting(68)="1" and Not Dvbbs.master Then%>
	匿名用户
	<% Else%>
	<%=UserInfoUrl(TopicInfo(1,0))%>
	<% End If%>
	</td></tr>
	<tr><td class="tablebody2" height="23" align="right"><b>发表时间</b>：</td><td class="tablebody1" colspan="3"><%=TopicInfo(3,0)%></td></tr>
	<tr>
	<td class="tablebody2" width="20%" height="23" align="right"><b>回复帖数</b>：</td><td class="tablebody1" width="30%"><%=TopicInfo(4,0)%></td>
	<td class="tablebody2" width="20%" align="right"><b>浏览次数</b>：</td><td class="tablebody1" width="30%"><%=TopicInfo(5,0)%></td>
	</tr>
	<% If TopicInfo(11,0)>0 Then %>
		<tr><td class="tablebody2" height=23 align=right><b>帖子信息</b>：</td><td class="tablebody1" colspan=3><%=GetTopicMoneyInfo(TopicInfo(9,0),TopicInfo(11,0))%></td></tr>
		<tr><td class="tablebody2" valign=top height=23 align=right><b>详细信息</b>：</td>
		<td class=tablebody1 colspan=3>
		<%ShowBuyUser%>
		</td></tr>
	<% End If %>
	<%If TopicInfo(10,0)<>"" and DVbbs.Forum_Setting(90)="1" Then %>
		<tr><td class="tablebody2" height="23" align="right"><b>道具信息</b>：</td><td class="tablebody1" colspan=3><u><%=GetTopicToolsInfo(TopicInfo(10,0))%></u></td></tr>
	<% End If %>
	</table>
<%
End Sub
	
Sub FootInfo()
	Response.Write "<table border=""0"" cellpadding=4 cellspacing=0 align=center Style=""Width:99%"">"
	Response.Write "<tr><td align=center><input type=""button"" onclick=""window.close()"" value=""关闭""></td></tr>"
	Response.Write "</table>"
End Sub

Sub ShowBuyUser()
	Dim TempStr,i,BuyUser,n,m
	If BBsInfo(1,0)="" Or Instr(BBsInfo(1,0),"|||")=0 Then Exit Sub
	TempStr = Split(Server.htmlEncode(BBsInfo(1,0)),"|||")
	n = Ubound(TempStr)
	Select Case TopicInfo(11,0)
		Case 1,5
			Response.Write "目前总共悬赏的金币数为：<b>"&TempStr(0)
			Response.Write "</b>，悬赏次数为：<b>"&n-1&"</b>。<BR>"
			For i=2 to n
				BuyUser = Split(TempStr(i),",")
				Response.Write UserInfoUrl(BuyUser(0))
				Response.Write " 获得金币：<B>"&BuyUser(1)
				Response.Write "</B>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"
				If i mod 2 = 1 then Response.Write "<br>"
			Next
		Case 2
			Response.Write "目前作者共获得金币数为：<b>"&TempStr(0)
			Response.Write "</b>，悬赏人数为：<b>"&n-1&"</b>。<BR>"
			For i=2 to n
				BuyUser = Split(TempStr(i),",")
				Response.Write UserInfoUrl(BuyUser(0))
				Response.Write " 悬赏金币：<B>"&BuyUser(1)
				Response.Write "</B>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"
				If i mod 2 = 1 then Response.Write "<br>"
			Next
		Case 3
			Dim BuyMoneyInfo,GetMoney,BuyInfo
			BuyMoneyInfo = Split(TempStr(0),"@@@")
			If Ubound(BuyMoneyInfo)>0 Then
				GetMoney = BuyMoneyInfo(0)
				BuyInfo = "该帖购买限制数为："
				If BuyMoneyInfo(1)<>"-1" Then
					BuyInfo = BuyInfo & BuyMoneyInfo(1)&"。"
				Else
					BuyInfo = BuyInfo & "无限。"
				End If
				If BuyMoneyInfo(2)<>"0" Then
					BuyInfo = BuyInfo & "VIP用户需要支付购买。<br>"
				Else
					BuyInfo = BuyInfo & "VIP用户不需要支付购买。<br>"
				End If
				If BuyMoneyInfo(3)<>"" Then
					BuyInfo = BuyInfo & ("只允许以下用户购买：<br>" & BuyMoneyInfo(3))
				End If
				BuyInfo = BuyInfo&"<hr>"
			Else
				GetMoney = TempStr(0)
			End If
			Response.Write BuyInfo
			'Response.Write "<br>"
			Response.Write "目前作者共获得金币数为：<b>"&GetMoney
			Response.Write "</b>，购买人数为：<b>"&n-2&"</b>。<BR>"
			For i=2 to n
				Response.Write UserInfoUrl(TempStr(i))
				Response.Write "&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"
				If i mod 2 = 1 then Response.Write "<br>"
			Next
	End Select
End Sub

Function UserInfoUrl(Name)
	UserInfoUrl = "<a href=""dispuser.asp?name="&Name&""" target=_blank><u><b>"&Name&"</b></u></a>"
End Function

'读取道具名单列表
Function GetTopicToolsInfo(ToolsID)
	Dim Sql,Rs
	GetTopicToolsInfo = "没有使用道具！"
	If IsNull(ToolsID) Then Exit Function
	If Not IsNumeric(Replace(ToolsID,",","")) Then Exit Function
	If ToolsID="-1111" Then Exit Function
	Sql = "Select ToolsName From [Dv_Plus_Tools_Info] where ID in ("&Dvbbs.Checkstr(ToolsID)&")"
	Set Rs = Dvbbs.Plus_Execute(Sql)
	If Rs.Eof Then
		Exit Function
	Else
		Sql = Rs.GetString(,-1, "§§§", "&nbsp;&nbsp;&nbsp;&nbsp;", " </u>，<u> ")
		'Sql = Split(Sql,"§§§")
	End If
	GetTopicToolsInfo = Sql
End Function

Function GetTopicMoneyInfo(M,MoneyType)
	'帖子信息类型
	Dim TempStr
	Select Case MoneyType
		Case 1
			TempStr = Replace(Template.Strings(17),"{$SendMoney}",M)
			TempStr = Replace(TempStr,"{$Stats}","")
		Case 2
			TempStr = Replace(Template.Strings(18),"{$GetMoney}",M)
		Case 3
			TempStr = Replace(Template.Strings(19),"{$PayMoney}",M)
		Case 5
			TempStr = Replace(Template.Strings(17),"{$SendMoney}",M)
			TempStr = Replace(TempStr,"{$Stats}",Template.Strings(21))
		Case Else
			TempStr = ""
	End Select
	TempStr = Replace(TempStr,"{$ViewUrl}","#")
	TempStr = Replace(TempStr,"{$alertcolor}",Dvbbs.Mainsetting(1)) 
	GetTopicMoneyInfo = TempStr
End Function

'获取主题信息 TopicInfo:
'Title=0,PostUsername=1,PostUserid=2,DateAndTime=3,Child=4,Hits=5,LastPost=6,
'LastPostTime=7,PostTable=8,GetMoney=9,UseTools=10,GetMoneyType=11,TopicMode=12
Sub GetTopicInfo()
	Dim Sql,Rs
	Sql = "Select Title,PostUsername,PostUserid,DateAndTime,Child,Hits,LastPost,LastPostTime,PostTable,GetMoney,UseTools,GetMoneyType,TopicMode,HideName "
	Sql = Sql & "From Dv_Topic Where TopicID="&Rootid&" and boardid="&Dvbbs.boardid
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.eof and Rs.bof Then
		Dvbbs.AddErrCode(32)
		Exit Sub
	Else
		Sql = Rs.GetRows(1)
	End If
	Set Rs=Nothing
	TopicInfo = Sql
End Sub

'获取分表信息 BBsInfo
Sub GetBBsInfo()
	Dim Sql,Rs
	Sql = "Select isagree,PostBuyUser,GetMoney,UseTools,GetMoneyType,Announceid "
	Sql = Sql & "From "&TopicInfo(8,0)&" Where RootID="&Rootid&" and ParentID=0 and boardid="&Dvbbs.boardid
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.eof and Rs.bof Then
		Dvbbs.AddErrCode(32)
		Exit Sub
	Else
		Sql = Rs.GetRows(1)
	End If
	Set Rs=Nothing
	BBsInfo = Sql
End Sub

'获取分表信息 BBsInfo
Sub GetBBsReplyInfo()
	Dim Sql,Rs
	Sql = "Select UserName,PostUserID,DateAndTime,isagree,PostBuyUser,GetMoney,UseTools,GetMoneyType,signflag "
	Sql = Sql & "From "&TopicInfo(8,0)&" Where Announceid="&ReplyID
	Set Rs = Dvbbs.Execute(Sql)
	If Rs.eof and Rs.bof Then
		Dvbbs.AddErrCode(32)
		Exit Sub
	Else
		Sql = Rs.GetRows(1)
	End If
	Set Rs=Nothing
	BBsReplyInfo = Sql
End Sub

Function Checktable(Table)
	Table = Right(Trim(Table),2)
	If Not IsNumeric(Table) Then Table = Right(Trim(Table),1)
	If Not IsNumeric(Table) Then Dvbbs.AddErrCode(35)
	checktable = "Dv_bbs" & Table
End Function 
%>