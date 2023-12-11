<!-- #include file="conn.asp" -->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/dv_clsother.asp"-->
<%
	Dvbbs.LoadTemplates("dispbbs")
	Dim Rootid,PostTable,Action,RootID_a
	Dim AnnounceID,Rs,SQL,i
	Dim topic,boardid
	Action = Dvbbs.CheckStr(Request("action"))
	PostTable=Dvbbs.CheckStr(Request("PostTable"))
	PostTable=Checktable(PostTable)
	Rootid=Dvbbs.Checknumeric(Request("ID"))
	RootID_a=Dvbbs.Checknumeric(Request("rootid"))
	AnnounceID=Dvbbs.Checknumeric(Request("ReplyID"))
	topic=Dvbbs.CheckStr(Request("topic"))
	boardid=Dvbbs.Checknumeric(Request("boardid"))
	dvbbs.boardid=boardid
	If dvbbs.boardid=0 Then Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>版块ID号错误。&action=OtherErr"
	Select Case Action
		Case "view" : Dvbbs.stats="查看购买贴子的用户"
		Case "buy" : Dvbbs.stats="金币购买帖子"
		Case "Send" : Dvbbs.stats="悬赏金币"
		Case "Close" : Dvbbs.stats="结帖操作"
		Case "Cancel" : Dvbbs.stats="转成普通帖" 'add by reoaiq by 090927
		Case Else
		Dvbbs.stats="购买帖子"
	End Select
	Dvbbs.Nav()
	Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
	If Rootid="" Or Not IsNumeric(Rootid) Then Dvbbs.AddErrCode(35)
	If AnnounceID="" or Not IsNumeric(AnnounceID) Then Dvbbs.AddErrCode(35)
	If Dvbbs.UserID=0 Then Dvbbs.AddErrCode(6)
	Dvbbs.ShowErr()
	Select Case Action
		Case "view" : view()
		Case "buy" : Buy()
		Case "Send" : SendMoney()
		Case "Close" : Close()
        Case "Cancel" : Cancel() 'add by reoaiq by 090927
		Case Else
		main()
	End Select
	Dvbbs.ShowErr()
	Dvbbs.Activeonline()
	Dvbbs.Footer
	Dvbbs.PageEnd()
	'转成普通帖 'add by reoaiq by 090927
	Sub Cancel()
Dim LogMsg
If Dvbbs.BoardMaster Then 
Dvbbs.Execute("update dv_topic set getmoney=0,getmoneytype=0 where topicid="&Rootid&"")
Dvbbs.Execute("update "&PostTable&" set getmoney=0,getmoneytype=0 where Rootid="&Rootid&"")
LogMsg = "金币帖《<a href=""Dispbbs.asp?BoardID="&Dvbbs.BoardID&"&ID="&Rootid&"&ReplyID="&Announceid&"&Skin=1"" target=_blank><b>"&Dvbbs.strCut(Topic,20)&"</b></a>》转换为普通成功"
Dvbbs.Dvbbs_Suc(LogMsg)
Else 
Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>您不是管理员等级，不能转换。&action=OtherErr"
Exit Sub
End If 
End Sub 
	'结帖操作
	Sub Close()
		Dim PostBuyUser,ToUserName,PostUserID,GetMoney,Topic,TopAnnounceID,LogMsg
		Dim TempStr
		Sql = "Select Top 1 PostBuyUser,GetMoney,Topic,AnnounceID From "&PostTable&" where RootID="&Rootid&" and ParentID=0 and GetMoneyType=1 and PostUserID="&Dvbbs.UserID
		Set Rs=Dvbbs.Execute(Sql)
		If Rs.eof and Rs.bof Then
			Dvbbs.AddErrCode(32)
			Exit Sub
		Else
			PostBuyUser = Rs(0)
			GetMoney = Rs(1)
			Topic = Rs(2)
			TopAnnounceID = Rs(3)
		End If
		Rs.Close
		TempStr = Split(PostBuyUser,"|||",2)
		TempStr(0) = cCur(TempStr(0))
		If Request.Form("ReAct")="SaveClose" Then
			Dim SendMoney
			If Not Dvbbs.ChkPost Then
				Dvbbs.AddErrCode(16)
				Exit Sub
			End If
			SendMoney = GetMoney-TempStr(0)
			If SendMoney<0 Then SendMoney = 0
			'更新用户，返还金币
			If SendMoney>0 Then
				Dvbbs.Execute("update [Dv_user] set UserMoney=UserMoney+"&SendMoney&" where userid="&Dvbbs.UserID)
				Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text  = cCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text )+SendMoney	'用户金币数量
			End If
			'更新帖子类型
			Dvbbs.Execute("update Dv_Topic set GetMoneyType=5 where TopicID="&Rootid)
			Dvbbs.Execute("update "&PostTable&" set GetMoneyType=5 where AnnounceID="&TopAnnounceID)

			LogMsg = "<b>结帖操作</b>：悬赏金币帖主题《<a href=""Dispbbs.asp?boardid="&Dvbbs.BoardID&"&id="&Rootid&""" target=_blank><b>"&Topic&"</b></a>》结帖成功，还返金币数为：<b>"&SendMoney&"</b>"
			Dim Dv_LogMsg
			Dv_LogMsg = "结帖操作：悬赏金币帖主题《"&Topic&"》结帖成功，还返金币数为："&SendMoney
			Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (" & Rootid & "," & Dvbbs.BoardID & ",'" & Dvbbs.MemberName & "','" & Dvbbs.MemberName & "','" & Dvbbs.CheckStr(Dv_LogMsg) & "','" & Dvbbs.UserTrueIP & "',5)")
			Dvbbs.Dvbbs_Suc(LogMsg)
		Else
	%>
	<FORM METHOD=POST ACTION="buypost.asp?action=Close">
	<table cellpadding=3 cellspacing=1 align=center class=tableborder1>
	<tr><th colspan=2>《 <%=Topic%> 》 悬赏金币结帖操作</th></tr>
	<tr>
	<td class=tablebody1 colspan=2><li>执行结帖后，帖子关闭，不允许其他会员回复。</td>
	</tr>
	<tr>
	<td class=tablebody2 align=right width="30%">悬赏金币总数：</td>
	<td class=tablebody1 width="70%"><%=GetMoney%></td>
	</tr>
	<tr>
	<td class=tablebody2 align=right>已悬赏金币总数：</td>
	<td class=tablebody1><%=TempStr(0)%></td>
	</tr>
	<tr>
	<td class=tablebody2 align=right>返还用户金币数：</td>
	<td class=tablebody1><%=GetMoney-TempStr(0)%></td>
	</tr>
	<tr><td class=tablebody2 colspan=2 align=center>
	<INPUT TYPE="submit" value="确定结帖"> <INPUT TYPE="button" value="取消" onclick="history.go(-1)">
	</td></tr>
	<INPUT TYPE="hidden" NAME="react" value="SaveClose">
	<INPUT TYPE="hidden" NAME="PostTable" value="<%=PostTable%>">
	<INPUT TYPE="hidden" NAME="ID" value="<%=Rootid%>">
	<INPUT TYPE="hidden" NAME="ReplyID" value="<%=AnnounceID%>">
	<INPUT TYPE="hidden" NAME="BoardID" value="<%=Dvbbs.BoardID%>">
	</table>
	<%
		End If
	End Sub

	'悬赏金币帖
	Sub SendMoney()
		Dim PostBuyUser,ToUserName,PostUserID,GetMoney,Topic,TopAnnounceID,LogMsg
		Dim TempStr,IsSendUser
		Sql = "Select Top 1 PostBuyUser,GetMoney,Topic,AnnounceID From "&PostTable&" where RootID="&Rootid&" and ParentID=0 and GetMoneyType=1 and PostUserID="&Dvbbs.UserID
		Set Rs=Dvbbs.Execute(Sql)
		If Rs.eof and Rs.bof Then
			Dvbbs.AddErrCode(32)
			Exit Sub
		Else
			PostBuyUser = Rs(0)
			GetMoney = Rs(1)
			Topic = Rs(2)
			TopAnnounceID = Rs(3) 
		End If
		Rs.Close
		ToUserName = Request("UserName")
		TempStr = Split(PostBuyUser,"|||",2)
		TempStr(0) = cCur(TempStr(0))
		If Instr(PostBuyUser,"|||"&ToUserName&",")>0 Then
			IsSendUser = "<font class=Redfont>[已悬赏]</font>"
		Else
			IsSendUser = "<font color=gray>[未悬赏]</font>"
		End If
		If Request.Form("ReAct")="SaveMoney" Then
			If Not Dvbbs.ChkPost Then
				Dvbbs.AddErrCode(16)
				Exit Sub
			End If
			Dim SendMoney
			SendMoney = Request.Form("SendMoney")
			If Not Isnumeric(SendMoney) Then 
				Dvbbs.AddErrCode(35)
				Exit Sub
			Else
				SendMoney = cCur(SendMoney)
			End If
			If TempStr(0) < 0 Then Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>悬赏的金币数太少或已超出了剩余金币数。&action=OtherErr"
			TempStr(0) = TempStr(0)+SendMoney

			If SendMoney<1 or TempStr(0)>GetMoney  Then
				Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>悬赏的金币数太少或已超出了剩余金币数。&action=OtherErr"
				Exit Sub
			End If

			'读取回复用户信息，更新GetMoney数值
			Sql = "Select username,PostUserID,GetMoney From "&PostTable&" where AnnounceID="&AnnounceID
			Dvbbs.SqlQueryNum=Dvbbs.SqlQueryNum+1
			set Rs=Dvbbs.iCreateObject("adodb.recordset")
			Rs.open sql,conn,1,3
			If Rs.eof and Rs.bof Then
				Dvbbs.AddErrCode(32)
				Dvbbs.ShowErr()
			Else
				ToUserName = Rs(0)
				PostUserID = Rs(1)
				If PostUserID=Dvbbs.UserID Then
					Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>悬赏金币帖不能对自已悬赏金币。&action=OtherErr"
					Exit Sub
				End If
				Rs(2) = Rs(2)+SendMoney
				Rs.Update
			End If
			Rs.close
			TempStr(1) = TempStr(1) & "|||" &ToUserName&","&SendMoney
			PostBuyUser = TempStr(0) & "|||" & TempStr(1)
			'更新目标用户，增加金币
			Dvbbs.Execute("update [Dv_user] set UserMoney=UserMoney+"&SendMoney&" where userid="&PostUserID)
			'更新分表中主题行PostBuyUser数据
			Dvbbs.Execute("update "&PostTable&" set PostBuyUser = '"&PostBuyUser&"' where AnnounceID="&TopAnnounceID)
			LogMsg = "关于回复主题《<a href=""Dispbbs.asp?BoardID="&Dvbbs.BoardID&"&ID="&Rootid&"&ReplyID="&Announceid&"&Skin=1"" target=_blank><b>"&Topic&"</b></a>》的帖子悬赏金币成功，<b>"&ToUserName&"</b>获得金币数为：<b>"&SendMoney&"</b>，剩余可悬赏金币数为：<b>"& GetMoney-TempStr(0) &"</b>。"
			Dvbbs.Dvbbs_Suc(LogMsg)
		Else
	%>
	<FORM METHOD=POST ACTION="buypost.asp?action=Send">
	<table cellpadding=3 cellspacing=1 align=center class=tableborder1>
	<tr><th colspan=2>《 <%=Topic%> 》 悬赏金币操作</th></tr>
	<tr>
	<td class=tablebody1 align=right width="30%">悬赏金币总数：</td>
	<td class=tablebody1 width="70%"><%=GetMoney%></td>
	</tr>
	<tr>
	<td class=tablebody1 align=right>已悬赏金币总数：</td>
	<td class=tablebody1><%=TempStr(0)%></td>
	</tr>
	<tr>
	<td class=tablebody1 align=right>悬赏目标用户：</td>
	<td class=tablebody1><%=Server.HtmlEncode(ToUserName)%> <%=IsSendUser%></td>
	</tr>
	<tr>
	<td class=tablebody1 align=right>设置悬赏金币个数：</td>
	<td class=tablebody1><INPUT TYPE="text" NAME="SendMoney" value=""> 剩余<b><font class="Redfont"><%=(GetMoney-TempStr(0))%></font></b>金币。</td>
	</tr>
	<tr><td class=tablebody2 colspan=2 align=center>
	<INPUT TYPE="submit" value="确定"> <INPUT TYPE="button" value="取消" onclick="history.go(-1)">
	</td></tr>
	<INPUT TYPE="hidden" NAME="react" value="SaveMoney">
	<INPUT TYPE="hidden" NAME="PostTable" value="<%=PostTable%>">
	<INPUT TYPE="hidden" NAME="ID" value="<%=Rootid%>">
	<INPUT TYPE="hidden" NAME="ReplyID" value="<%=AnnounceID%>">
	<INPUT TYPE="hidden" NAME="BoardID" value="<%=Dvbbs.BoardID%>">
	</table>
	<%
		End If
	End Sub

	'金币帖子购买
	Sub Buy()
		Dim PostBuyUser,ToUserName,PostUserID,GetMoney,GetMoneyType,IsUpdate,LogMsg,Topic,TempStr
		IsUpdate = False
		Sql = "Select PostBuyUser,username,PostUserID,GetMoney,GetMoneyType,Topic From "&PostTable&" where RootID="&Rootid&" and ParentID=0 and GetMoneyType=3"
		If Not IsObject(Conn) Then ConnectionDatabase
		Dvbbs.SqlQueryNum=Dvbbs.SqlQueryNum+1
		Set Rs = Dvbbs.iCreateObject("adodb.recordset")
		Rs.open Sql,conn,1,3
		If Rs.eof and Rs.bof Then
			Dvbbs.AddErrCode(32)
			Dvbbs.ShowErr()
		Else
			PostBuyUser = Rs(0)
			ToUserName = Rs(1)
			PostUserID = Rs(2)
			GetMoney = Rs(3)
			GetMoneyType = Rs(4)
			Topic = Rs(5)
			If Not IsNumeric(GetMoney) Then GetMoney=0
			If GetMoney < 0 Then Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>由于此贴金币设置数据错误，购买失败。&action=OtherErr"
			If Instr(PostBuyUser,"|||$PayMoney|||") AND Dvbbs.UserID<>PostUserID AND GetMoney<>0 and InStr(PostBuyUser,"|||"&Dvbbs.Membername&"|||")=0 Then
				TempStr = Split(Rs(0),"|||",2)
				Dim BuyMoneyInfo
				BuyMoneyInfo = Split(TempStr(0),"@@@")
				BuyMoneyInfo(1) = cCur(BuyMoneyInfo(1))
				BuyMoneyInfo(2) = Clng(BuyMoneyInfo(2))
				'购买数量限制(设置为“-1”则不限制)
				If BuyMoneyInfo(1)=0 Then
					Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>本帖子已售完。&action=OtherErr"
					Exit Sub
				ElseIf BuyMoneyInfo(1)>0 Then
					BuyMoneyInfo(1) = BuyMoneyInfo(1) - 1 
				End If
				'当VIP不需要付费时将GetMoney清为0
				'If BuyMoneyInfo(2)=0 and Dvbbs.VipGroupUser Then
					'GetMoney = 0
				'End If
				'可购买用户名单限制(每个用户名用英文逗号“,”分隔符分开，注意区分大小写)
				If BuyMoneyInfo(3)<>"" Then
					If Instr(","&BuyMoneyInfo(3)&",",","&Dvbbs.Membername&",")=0 Then
						Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>购买失败，非作者指定的用户不能购买该帖。&action=OtherErr"
						Exit Sub
					End If
				End If
				If GetMoney>CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)  Then 
					Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>你的用户金币不足，购买该帖失败。&action=OtherErr"
					Exit Sub
				End If
				BuyMoneyInfo(0) = cCur(BuyMoneyInfo(0)) + GetMoney '*ToolsSetting(4)
				TempStr(0) = BuyMoneyInfo(0) & "@@@" & BuyMoneyInfo(1) & "@@@" & BuyMoneyInfo(2) & "@@@" & BuyMoneyInfo(3)
				Rs(0) = TempStr(0) & "|||" & TempStr(1) & Dvbbs.Membername & "|||"
				Rs.Update

				Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text  = Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text -GetMoney
				Dvbbs.Execute("update [Dv_user] set UserMoney="&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text &" where userid="&Dvbbs.userid)
				Dvbbs.Execute("update [Dv_user] set UserMoney=UserMoney+"&GetMoney&" where userid="&PostUserID)
				IsUpdate = True
			Else
				Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>你不能重复购买或者不能购买自已的金币帖子。&action=OtherErr"
				Exit Sub
			End If
		End If
		Rs.Close : Set Rs=Nothing
		If IsUpdate Then
			LogMsg = "购买金币帖《<a href=""Dispbbs.asp?boardid="&Dvbbs.BoardID&"&id="&Rootid&""" target=_blank><b>"&Topic&"</b></a>》成功，支付金币数为：<b>"&GetMoney&"</b>，<b>"&ToUserName&"</b>得到金币为："&GetMoney
			Dvbbs.Dvbbs_Suc(LogMsg)
		End If
	End Sub

	Sub Main()
		dim re
		dim po,ii
		dim reContent
		dim strContent
		dim PostBuyUser
		po=0
		ii=0
		dim usermoney
		If Rootid_a="" Or Not IsNumeric(Rootid_a) Then Dvbbs.AddErrCode(35)
		set rs=Dvbbs.Execute("select userWealth from [Dv_user] where userid="&Dvbbs.Userid)
		usermoney=rs(0)
		Dvbbs.SqlQueryNum=Dvbbs.SqlQueryNum+1
		set rs=Dvbbs.iCreateObject("adodb.recordset")
		sql="select body,PostBuyUser,username,PostUserID,GetMoneyType From "&PostTable&" where Announceid="&Announceid
		rs.open sql,conn,1,3
		If rs.eof and rs.bof Then
			Dvbbs.AddErrCode(32)
			Dvbbs.ShowErr()
		Else
			If rs(4)>0 Then
				Response.redirect "showerr.asp?ErrCodes=<Br>"+"<li>由于帖子使用了特殊类型，所以不能采用金钱购买帖。&action=OtherErr"
				Exit Sub
			End If
			strContent=Dvbbs.HTMLEncode(rs(0))
			PostBuyUser=Trim(rs(1))
			'Response.Write PostBuyUser
			'Response.End
			Set re=new RegExp
			re.IgnoreCase =true
			re.Global=True
			re.Pattern="(^.*)(\[UseMoney=*([0-9]*)\])(.*)(\[\/UseMoney\])(.*)"
			po=re.Replace(strContent,"$3")
			If IsNumeric(po) Then 
				ii=int(po) 
			Else
				ii=0
			End If
			Set re=Nothing
					
			If Dvbbs.membername=rs(2) Then
				response.write "<script>alert('呵呵，您要花钱购买自己发布的帖子吗？');</script>"
			ElseIf  usermoney >ii then
				If (not isnull(PostBuyUser)) Or  PostBuyUser<>"" Then
					If InStr("|"&PostBuyUser&"|","|"&Dvbbs.membername&"|")>0 Then
						response.write "<script>alert('呵呵，您已经购买过了呀？');</script>"
					Else
						Dvbbs.Execute("update [Dv_user] set userWealth=userWealth-"&ii&" where userid="&Dvbbs.userid)
						Dvbbs.Execute("update [Dv_user] set userWealth=userWealth+"&ii&" where userid="&rs(3))
						If IsNull(Rs(1)) or  Rs(1)="" Then 
							rs(1)=Dvbbs.membername
						Else
							rs(1)=rs(1) & "|" & Dvbbs.membername
						End If
						Rs.Update 
						response.write "<script>alert('购买成功！');</script>"
					End If
				Else 
					Dvbbs.Execute("update [Dv_user] set userWealth=userWealth-"&ii&" where userid="&Dvbbs.userid)
					Dvbbs.Execute("update [Dv_user] set userWealth=userWealth+"&ii&" where userid="&rs(3))
					rs(1)=Dvbbs.membername
					Rs.Update
					response.write "<script>alert('购买成功！');</script>"
				End If
			Else
				response.write "<script>alert('您都没有钱呀？');</script>"
			End If
			
		End If
		Rs.Close 
		Set  Rs=Nothing
		Response.Write "<script language=""javascript"">"
		Response.Write "parent.location.href='"
		Response.Write "dispbbs.asp?boardid="&request("boardid")&"&ID="&RootID_a&"&replyID="&AnnounceID&"&star=1&skin=1#"&AnnounceID
		Response.Write "';"
		Response.Write "</script>"
	End Sub
	Sub view()
		Dim PostBuyUser
		sql="select PostBuyUser from "&PostTable&" where Announceid="&Announceid
		Set rs=Dvbbs.Execute(sql)
		PostBuyUser=Trim(rs(0))
		Response.Write "<table cellpadding=3 cellspacing=1 align=center class=tableborder1>"
		Response.Write "<TBODY><TR>"
		Response.Write "<Th height=24 colspan=1>查看购买贴子的用户</Th>"
		Response.Write "</TR>"
		Response.Write "<tr><TD class=tablebody2>"
		If (not isnull(PostBuyUser)) Or  PostBuyUser<>"" Then
			PostBuyUser=Replace(PostBuyUser,"|","<li>")
			Response.Write "<li>"&PostBuyUser		
		Else
			Response.Write "<br><li>还未有人购买！"
		End If
		Response.Write "</td></tr>"
		Response.Write "</table>"
		Set rs=Nothing
	End Sub
	Function checktable(Table)
		Table=Right(Trim(Table),2)
		If Not IsNumeric(table) Then Table=Right(Trim(Table),1)
		If Not IsNumeric(table) Then Dvbbs.AddErrCode(35)
		checktable="Dv_bbs"&table
	End Function 
%>