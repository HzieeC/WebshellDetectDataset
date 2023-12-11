<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/dv_clsother.asp" -->
<%
Dim FA
Dvbbs.LoadTemplates("fmanage")
Dvbbs.Stats=template.Strings(0)
Dvbbs.Nav()
Dvbbs.Showerr()
If Dvbbs.BoardID=0 Then
	Dvbbs.AddErrCode(29)
	Dvbbs.showerr()
End If
Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
Set FA=New Dv_Forum_Admin
FA.main
Dvbbs.ActiveOnline()
Set Fa=Nothing
Dvbbs.Footer()
Dvbbs.PageEnd()
Class Dv_Forum_Admin
	Public IP,ID,ReplyID,ActionInfo,Topic,Content,AllMsg,TopicUserID,TopicUsername,TotalUseTable
	Public doGiveMoney,doWealth,douserCP,douserEP,UpdateBoardID,UpdateBoardID_1
	Public Rs,SQL,i
	Private LocalCanLockTopic,LocalCanDelTopic,LocalCanMoveTopic,LocalCanTopTopic,LocalCanBestTopic,LocalCanAwardTopic,LocalCanTopTopic_a,LocalCanTopTopic_m,LocalCanTopicMode
	Public title,sucmsg,LogType
	Public Lasttopic,Lastpost
	Public lastrootID,lastpostuser

	Private Sub Class_Initialize()
		Dim doGiveMoneyMsg,doWealthMsg,douserEPMsg,douserCPMsg
		IP			=	Dvbbs.UserTrueIP
		LocalCanLockTopic	=	False
		LocalCanDelTopic	=	False
		LocalCanMoveTopic	=	False
		LocalCanTopTopic	=	False
		LocalCanBestTopic	=	False
		LocalCanAwardTopic	=	False
		LocalCanTopTopic_a	=	False
		LocalCanTopTopic_m	=	False
		LocalCanTopicMode	=	False
		'本论坛和上级论坛ID
		UpdateBoardID	=Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@parentstr").text & "," & Dvbbs.BoardID
		doGiveMoney		=	0
		doWealth		=	0
		douserEP		=	0
		douserCP		=	0
		doWealthMsg		=	""
		allmsg			=	"没有对用户进行分值操作"
		If Dvbbs.UserID=0 Then Dvbbs.AddErrCode(6)
		ID=Request("ID")
		If ID="" or IsNumeric(ID)=0 Then
			Dvbbs.AddErrCode(30)
		Else
			ID=Clng(ID)
		End If
		If IsNumeric(Request("replyID")) and Request("replyID")<>"" Then ReplyID=Request("replyID")

		If IsNumeric(Request("GiveMoney")) And Not (Request("GiveMoney")="0" or Request("GiveMoney")="") Then
			doGiveMoney=Request("GiveMoney")
			doGiveMoneyMsg="金币" & Request("GiveMoney") & "，"
		End If

		If IsNumeric(Request("doWealth")) And Not (Request("doWealth")="0" or Request("doWealth")="") Then
			doWealth=Request("doWealth")
			doWealthMsg="金钱" & Request("doWealth") & "，"
		End If
		If IsNumeric(Request("douserEP")) And Not (Request("douserEP")="0" or Request("douserEP")="") Then
			douserEP=Request("douserEP")
			douserEPMsg="积分" & Request("douserEP") & "，"
		End If
		If IsNumeric(Request("douserCP")) And Not (Request("douserCP")="0" or Request("douserCP")="") Then
			douserCP=Request("douserCP")
			douserCPMsg="魅力" & Request("douserCP")
		End If
		If Not (doWealthMsg="" And douserEPMsg="" And douserCPMsg="") Then allmsg="用户操作：" & doGiveMoneyMsg & doWealthMsg & douserEPMsg & douserCPMsg

		If Dvbbs.ErrCodes<>"" Then Dvbbs.ShowErr

		Set Rs=Dvbbs.Execute("Select Title,Postusername,PostuserID,PostTable From Dv_Topic Where boardid="&dvbbs.boardid&" and TopicID="&ID)
		If Rs.Eof And Rs.Bof Then
			Dvbbs.AddErrCode(32)
		Else
			Topic=rs(0)
			Topicusername=rs(1)
			TopicuserID=Clng(rs(2))
			TotalUseTable=rs(3)
		End If
		Set Rs=Nothing			
		If Dvbbs.ErrCodes<>"" Then Dvbbs.ShowErr		
	End Sub
	'判断用户是否有专题管理操作权限
	Public Property Get CanTopicMod()
		If (dvbbs.master or dvbbs.superboardmaster or dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(65))=1 Then
			CanTopicMode=True
		End If
		If Cint(Dvbbs.GroupSetting(19))=1 and Dvbbs.UserGroupID>3 Then 
			LocalCanTopicMod=True
		End If
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(65))=1 and TopicUserID=Dvbbs.Userid Then
			LocalCanTopicMod=True
		Else
			LocalCanTopicMod=False
		End If
		CanTopicMod=LocalCanTopicMod
	End Property
	'判断用户是否有锁定/解除锁定权限
	Public Property Get CanLockTopic()
		If (dvbbs.master or dvbbs.superboardmaster or dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(20))=1 Then LocalCanLockTopic=True
		If Cint(Dvbbs.GroupSetting(20))=1 and Dvbbs.UserGroupID>3 Then LocalCanLockTopic=True
		If (Cint(Dvbbs.GroupSetting(13))=1 and TopicUsername=Dvbbs.MemberName) Then LocalCanLockTopic=True
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(13))=1 and TopicUsername=Dvbbs.MemberName Then
			LocalCanLockTopic=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(13))=0 and TopicUsername=Dvbbs.MemberName Then
			LocalCanLockTopic=False
		End If
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(20))=1 and TopicUsername<>Dvbbs.MemberName Then
			LocalCanLockTopic=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(20))=0 and TopicUsername<>Dvbbs.MemberName Then
			LocalCanLockTopic=False
		End If		
		CanLockTopic=LocalCanLockTopic
	End Property
	'判断用户是否有移动帖子权限
	Public Property Get CanMoveTopic()
		If (dvbbs.master or dvbbs.superboardmaster or dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(19))=1 Then LocalCanMoveTopic=True
		If Cint(Dvbbs.GroupSetting(19))=1 and Dvbbs.UserGroupID>3 Then LocalCanMoveTopic=True
		If (Cint(Dvbbs.GroupSetting(12))=1 and TopicUsername=Dvbbs.MemberName) Then LocalCanMoveTopic=True
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(12))=1 and TopicUsername=Dvbbs.MemberName Then
			LocalCanMoveTopic=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(12))=0 and TopicUsername=Dvbbs.MemberName Then
			LocalCanMoveTopic=False
		End If
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(19))=1 and TopicUsername<>Dvbbs.MemberName Then
			LocalCanMoveTopic=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(19))=0 and TopicUsername<>Dvbbs.MemberName Then
			LocalCanMoveTopic=False
		End If		
		CanMoveTopic=LocalCanMoveTopic
	End Property	
	'判断用户是否有删除帖子权限
	Public Property Get CanDelTopic()
		If (dvbbs.master or dvbbs.superboardmaster or dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(18))=1 Then LocalCanDelTopic=True
		If Cint(Dvbbs.GroupSetting(18))=1 and Dvbbs.UserGroupID>3 Then LocalCanDelTopic=True
		If (Cint(Dvbbs.GroupSetting(11))=1 and TopicUsername=Dvbbs.MemberName) Then LocalCanDelTopic=True
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(11))=1 and TopicUsername=Dvbbs.MemberName Then
			LocalCanDelTopic=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(11))=0 and TopicUsername=Dvbbs.MemberName Then
			LocalCanDelTopic=False
		End If
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(18))=1 and TopicUsername<>Dvbbs.MemberName Then
			LocalCanDelTopic=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(18))=0 and TopicUsername<>Dvbbs.MemberName Then
			LocalCanDelTopic=False
		End If
		CanDelTopic=LocalCanDelTopic
	End Property
	'判断用户是否有固顶/解除固顶帖子权限
	Public Property Get CanTopTopic()
		If (dvbbs.master or dvbbs.superboardmaster or dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(21))=1 Then LocalCanTopTopic=True
		If Cint(Dvbbs.GroupSetting(21))=1 and Dvbbs.UserGroupID>3 Then LocalCanTopTopic=True
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(21))=1 Then
			LocalCanTopTopic=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(21))=0 Then
			LocalCanTopTopic=False
		End If
		CanTopTopic=LocalCanTopTopic
	End Property	
	'判断用户是否有总固顶帖子权限
	Public Property Get CanTopTopic_a()
		If (dvbbs.master or dvbbs.superboardmaster or dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(38))=1 Then LocalCanTopTopic_a=True
		If Cint(Dvbbs.GroupSetting(38))=1 and Dvbbs.UserGroupID>3 Then LocalCanTopTopic_a=True
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(38))=1 Then
			LocalCanTopTopic_a=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(38))=0 Then
			LocalCanTopTopic_a=False
		End If
		CanTopTopic_a=LocalCanTopTopic_a
	End Property	
	'判断用户是否有区域固顶帖子权限
	Public Property Get CanTopTopic_m()
		If (dvbbs.master or dvbbs.superboardmaster or dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(54))=1 Then LocalCanTopTopic_m=True
		If Cint(Dvbbs.GroupSetting(54))=1 and Dvbbs.UserGroupID>3 Then LocalCanTopTopic_m=True
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(54))=1 Then
			LocalCanTopTopic_m=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(54))=0 Then
			LocalCanTopTopic_m=False
		End If
		CanTopTopic_m=LocalCanTopTopic_m
	End Property
	'判断用户是否有加入/解除精华帖子权限
	Public Property Get CanBestTopic()
		If (dvbbs.master or dvbbs.superboardmaster or dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(24))=1 Then LocalCanBestTopic=True
		If Cint(Dvbbs.GroupSetting(24))=1 and Dvbbs.UserGroupID>3 Then LocalCanBestTopic=True
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(24))=1 Then
			LocalCanBestTopic=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(24))=0 Then
			LocalCanBestTopic=False
		End If
		CanBestTopic=LocalCanBestTopic
	End Property
	'判断用户是否有奖励/惩罚帖子权限
	Public Property Get CanAwardTopic()
		If (dvbbs.master or dvbbs.superboardmaster or dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(22))=1 Then LocalCanAwardTopic=True
		If Cint(Dvbbs.GroupSetting(22))=1 and Dvbbs.UserGroupID>3 Then LocalCanAwardTopic=True
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(22))=1 Then
			LocalCanAwardTopic=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(22))=0 Then
			LocalCanAwardTopic=False
		End If
		CanAwardTopic=LocalCanAwardTopic
	End Property
	Public Function Main()
		If Not Dvbbs.ChkPost() Then Dvbbs.AddErrCode(42):Dvbbs.Showerr()
		Select Case Request("action")
		Case "修复"
			If Dvbbs.userid=0 Then
				Dvbbs.AddErrCode(6)
			Else
				ActionInfo="修复帖子"
				fixtopic()
			End If
		Case "lock"
			If Request.form("submit")="" Then Exit Function
			If not CanLockTopic Then
				Dvbbs.AddErrCode(28)
			Else
				ActionInfo="锁定帖子"
				lock()
			End If
		Case "unlock"
			If Request.form("submit")="" Then Exit Function
			If not CanLockTopic Then
				Dvbbs.AddErrCode(28)
			Else
				ActionInfo="解除锁定"
				unlock()
			End If
		Case "uptopic"
			If Request.form("submit")="" Then Exit Function
			If Not CanLockTopic Then
				Dvbbs.AddErrCode(28)
			Else
				ActionInfo="提升帖子"
				uptopic()
			End If
		Case "downtopic"
			If Request.form("submit")="" Then Exit Function
			If Not CanLockTopic Then
				Dvbbs.AddErrCode(28)
			Else
				ActionInfo="沉底帖子"
				downtopic()
			End If
		Case "move"
			If Request.form("submit")="" Then Exit Function
			If not CanMoveTopic Then
				Dvbbs.AddErrCode(28)
			Else
				ActionInfo="移动帖子"
				Tmove()
			End If
		Case "copy"
			If Request.form("submit")="" Then Exit Function
			ActionInfo="复制帖子"
			copy()
		Case "istop"
			If Request.form("submit")="" Then Exit Function
			If CanTopTopic Or CanTopTopic_a Or CanTopTopic_m Then
				ActionInfo="固顶帖子"
				Getistop()
			Else
				Dvbbs.AddErrCode(28)
			End If
		Case "delet"
			If Request.form("submit")="" Then Exit Function
			If not CanDelTopic Then
				Dvbbs.AddErrCode(28)
			Else
				ActionInfo="删除帖子"
				delete()
			End If
		Case "dele"
		If Request.form("submit")="" Then Exit Function
			ActionInfo="删除帖子"
			dele(1)
		Case "islockpage"
			If Request.form("submit")="" Then Exit Function
			If not CanBestTopic Then
				Dvbbs.AddErrCode(28)
			Else
				ActionInfo="单帖屏蔽"
				islockpage()
			End If
		Case "nolockpage"
			If Request.form("submit")="" Then Exit Function
			If not CanBestTopic Then
				Dvbbs.AddErrCode(28)
			Else
				ActionInfo="解除屏蔽"
				nolockpage()
			End If
		Case "isbest"
			If Request.form("submit")="" Then Exit Function
			If not CanBestTopic Then
				Dvbbs.AddErrCode(28)
			Else
				ActionInfo="精华帖子"
				isbest()
			End If
		Case "nobest"
			If Request.form("submit")="" Then Exit Function
			If not CanBestTopic Then
				Dvbbs.AddErrCode(28)
			Else
				ActionInfo="解除精华"
				nobest()
			End If
		Case "TopicMode"
			If Request.form("submit")="" Then Exit Function
			ActionInfo="专题管理"
			If not CanMoveTopic Then Dvbbs.AddErrCode(28)
			TopicMode()
		Case "delre"
			If Request.form("submit")="" Then Exit Function
			ActionInfo="批量删除跟贴"
			Call delre()
		Case "SaveRewardMoney"	'奖励金币操作
			If Request.form("submit")="" Then Exit Function
			ActionInfo="帖子评价"
			Call RewardMoney
		Case Else
			main_a()
		End Select
		If Dvbbs.ErrCodes<>"" Then Dvbbs.ShowErr()
	End Function
	'批量删除跟贴
	Private Sub delre()
		Check_topicInfo()
		If Dvbbs.ErrCodes<>"" Then Exit Sub
		'判断用户是否有删除帖子权限
		If Not CanDelTopic Then Dvbbs.AddErrCode(28)
		Dim DelID,j,i
		j=0
		DelID=Request("DelID")
		If delid="" Then
			Dvbbs.AddErrCode(35)
			Exit Sub
		End If
		delid=Split(delid,",")
		For i = 0 to UBound(delid)
			If Trim(delid(i))<>"" and IsNumeric(Trim(delid(i))) Then
				j=j+1
				replyID=Ccur(Trim(delid(i)))
				dele(0)
			End If
		Next
		If j>0 Then
			Dvbbs.Dvbbs_Suc(SucMsgInfo("批量删除"&j&"个跟贴,您的操作已经记录"))
		Else
			Dvbbs.AddErrCode(35)
		End If
	End Sub
	Public Sub main_a()
		Dim seldisable,reaction,Action
		Dim postusername,DelUpFile,Decrease
		DelUpFile=0
		Action=Request("action")
		Decrease=0
		Select Case Action
		Case "锁定"
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="lock"
			If not CanLockTopic Then Dvbbs.AddErrCode(28)
		Case "解锁"
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="unlock"
			If not CanLockTopic Then Dvbbs.AddErrCode(28)
		Case "提升"
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="uptopic"
			If not CanLockTopic Then Dvbbs.AddErrCode(28)
		Case "沉底"
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="downtopic"
			If not CanLockTopic Then Dvbbs.AddErrCode(28)
		Case "删除主题"
			doWealth=-Dvbbs.Forum_user(3)
			douserEP=-Dvbbs.Forum_user(8)
			douserCP=-Dvbbs.Forum_user(13)
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="delet"
			If not CanDelTopic Then Dvbbs.AddErrCode(28)
			If SysObjFso=True Then DelUpFile=1
			Decrease=1
		Case "dele_a"
			doWealth=-Dvbbs.Forum_user(3)
			douserEP=-Dvbbs.Forum_user(8)
			douserCP=-Dvbbs.Forum_user(13)
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="dele"
			Action="删除单贴"
			Check_AnnounceInfo()
			If Dvbbs.ErrCodes<>"" Then Exit Sub
			'判断用户是否有删除帖子权限
			If Not CanDelTopic Then Dvbbs.AddErrCode(28)
			If SysObjFso=True Then DelUpFile=1
			Decrease=2
		Case "islockpage_a"
			doWealth=-Dvbbs.Forum_user(15)
			douserEP=-Dvbbs.Forum_user(17)
			douserCP=-Dvbbs.Forum_user(16)
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="islockpage"
			Action="单贴屏蔽"
			Check_AnnounceInfo()
			If Dvbbs.ErrCodes<>"" Then Exit Sub
			If Not CanBestTopic Then Dvbbs.AddErrCode(28)
		Case "nolockpage_a"
			doWealth=Dvbbs.Forum_user(15)
			douserEP=Dvbbs.Forum_user(17)
			douserCP=Dvbbs.Forum_user(16)
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="nolockpage"
			Action="解除屏蔽"
			Check_AnnounceInfo()
			If Dvbbs.ErrCodes<>"" Then Exit Sub
			If Not CanBestTopic Then Dvbbs.AddErrCode(28)
		Case "isbest_a"
			doWealth=Dvbbs.Forum_user(15)
			douserEP=Dvbbs.Forum_user(17)
			douserCP=Dvbbs.Forum_user(16)
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="isbest"
			Action="加为精华"
			Check_AnnounceInfo()
			If Dvbbs.ErrCodes<>"" Then Exit Sub
			If Not CanBestTopic Then Dvbbs.AddErrCode(28)
		Case "nobest_a"
			doWealth=-Dvbbs.Forum_user(15)
			douserEP=-Dvbbs.Forum_user(17)
			douserCP=-Dvbbs.Forum_user(16)
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="nobest"
			Action="解除精华"
			Check_AnnounceInfo()
			If Dvbbs.ErrCodes<>"" Then Exit Sub
			If not CanBestTopic Then Dvbbs.AddErrCode(28)
		Case "copy_a"
			seldisable="disabled"
			reaction="copy"
			Action="复制贴子"
			Check_AnnounceInfo()
			If Dvbbs.ErrCodes<>"" Then Exit Sub
			'判断用户是否有移动帖子权限
			If Not CanMoveTopic Then Dvbbs.AddErrCode(28)
		Case "设置固顶"
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="istop"
			If CanTopTopic Or CanTopTopic_a Or CanTopTopic_m Then
			Else
				Dvbbs.AddErrCode(28)
			End If
		Case "编辑固顶"
			If Not CanAwardTopic Then seldisable="disabled"
			reaction="istop"
			If CanTopTopic Or CanTopTopic_a Or CanTopTopic_m Then
			Else
				Dvbbs.AddErrCode(28)
			End If
		Case "移动"
			seldisable="disabled"
			reaction="move"
			If Not CanMoveTopic Then Dvbbs.AddErrCode(28)
		Case "专题管理"
			If Not CanMoveTopic Then Dvbbs.AddErrCode(28)
			reaction="TopicMode"
		Case "跟帖管理"
			doWealth=-Dvbbs.Forum_user(3)
			douserEP=-Dvbbs.Forum_user(8)
			douserCP=-Dvbbs.Forum_user(13)
			Check_topicInfo()
			If Dvbbs.ErrCodes<>"" Then Exit Sub
			'判断用户是否有删除帖子权限
			If Not CanDelTopic Then Dvbbs.AddErrCode(28)
			Dim Star,i,j,treedata,tmpstr,blank
			Star=Request("Star")
			If Star="" Then Star=1
			If Not IsNumeric(Star) Then star=1
			Set Rs=Dvbbs.iCreateObject("adodb.recordset")
			sql="select AnnounceID,parentID,BoardID,UserName,PostUserid,Topic,DateAndTime,length,RootID,layer,orders,Expression,body from "&TotalUseTable&" where BoardID="&Dvbbs.BoardID&" and RootID="&ID&" and BoardID<>777 and BoardID<>444 order by RootID desc,orders"
			rs.open sql,conn,1,1
			j=0
			If Not(Rs.EOF And Rs.BOF) Then
				Rs.PageSize=Cint(Dvbbs.Board_Setting(27))
				Rs.AbsolutePage=Star
				Do while Not Rs.EOF
				treedata=template.html(6)
				For i=1 to Rs(9)
					blank=blank&"&nbsp;"
				Next
				If Rs("topic")="" or isnull(rs("topic")) Then 
					treedata=Replace(treedata,"{$topic}",cutStr(replace(reubbcode(Dvbbs.ChkBadWords(rs("body"))),chr(10),""),35))
				Else
					treedata=Replace(treedata,"{$topic}",cutStr(Dvbbs.ChkBadWords(rs("Topic")),35))
				End If		
				If j=0 Then
					If star=1 Then 
						treedata=Replace(treedata,"{$del}","")
						treedata=Replace(treedata,"{$alertcolor}",Dvbbs.mainsetting(1))
					Else
						treedata=Replace(treedata,"{$del}"," <input type=""checkbox"" name=""DelID"" value="""&Rs(0)&""">")
						treedata=Replace(treedata,"{$alertcolor}","")
					End If
					
				Else
					treedata=Replace(treedata,"{$del}"," <input type=""checkbox"" name=""DelID"" value="""&Rs(0)&""">")
					treedata=Replace(treedata,"{$alertcolor}","")
				End If 
				treedata=Replace(treedata,"{$announceid}",Rs(0))
				treedata=Replace(treedata,"{$boardid}",Rs(2))
				treedata=Replace(treedata,"{$username}",Rs(3))
				treedata=Replace(treedata,"{$DateAndTime}",Rs(6))
				If Rs(7)=0 Then 
					treedata=Replace(treedata,"{$length}","无内容")
				Else
					treedata=Replace(treedata,"{$length}",Rs(7)&"字节")
				End If
				treedata=Replace(treedata,"{$rootid}",Rs(8))
				treedata=Replace(treedata,"{$Expression}",Rs(11))
				treedata=Replace(treedata,"{$blank}",blank)
				treedata=Replace(treedata,"{$PicUrl}",Dvbbs.Forum_PicUrl)
				blank=""
				tmpstr=tmpstr&treedata
				Rs.MoveNext
				j=j+1
				If j=Cint(Dvbbs.Board_Setting(27)) Then Exit Do
				Loop
			End If
			Dim Tpl5
			Decrease=2
			Tpl5 = Replace(template.html(5),"{$id}",ID)
			Tpl5 = Replace(Tpl5,"{$boardid}",Dvbbs.boardid)
			Tpl5 = Replace(Tpl5,"{$reaction}",reaction)
			Tpl5 = Replace(Tpl5,"{$seldisable}",seldisable)
			Tpl5 = Replace(Tpl5,"{$doWealth}",doWealth)
			Tpl5 = Replace(Tpl5,"{$dousercp}",dousercp)
			Tpl5 = Replace(Tpl5,"{$douserep}",douserep)
			Tpl5 = Replace(Tpl5,"{$decrease}",Decrease)
			Tpl5 = Replace(Tpl5,"{$fileconfirm}",DelUpFile)
			Tpl5 = Replace(Tpl5,"{$action}",request("action"))
			Tpl5 = Replace(Tpl5,"{$treeloop}",tmpstr)
			Response.Write Tpl5
			Endpage=Rs.PageCount
			Response.Write "<table border=0 cellpadding=0 cellspacing=3 width="""&Dvbbs.mainsetting(0)&""" align=center>"
			Response.Write "<tr><td valign=middle nowrap>"
			Response.Write "页次：<b>"&Star&"</b>/<b>"&Endpage&"</b>页"
			Response.Write "每页<b>"& Dvbbs.Board_Setting(27) &"</b> 贴数<b>"& Rs.RecordCount &"</b></td>"
			Response.Write "<td valign=middle nowrap><div align=right><p>分页： <b>"
			Dim Endpage
			If Star > 4 Then
				Response.Write "<a href=""admin_postings.asp?action=跟帖管理&BoardID="&Dvbbs.BoardID&"&ID="&ID&"&star=1"">[1]</a> ..."
			End If
			
			If Endpage >Star+3 Then
				Endpage=Star+3
			End If
			For i=Star-3 to Endpage
				If Not i<1 Then
					If i = CLng(star) Then
						response.write " <font color="&dvbbs.mainsetting(1)&">["&i&"]</font>"
					Else
						Response.Write " <a href=""admin_postings.asp?action=跟帖管理&BoardID="&Dvbbs.BoardID&"&ID="&ID&"&star="&i&""">["&i&"]</a>"
					End If
				End If
			Next
			If star+3 < Rs.PageCount Then
				response.write "... <a href=""admin_postings.asp?action=跟帖管理&BoardID="&Dvbbs.BoardID&"&ID="&ID&"&star="&Rs.PageCount&""">["&Rs.PageCount&"]</a></b>"
			End If
			Response.Write "</p></div></td></tr></table>"
			Set Rs=Nothing
			Response.Write "<script language=""JavaScript"">"
			Response.Write Chr(10)
			Response.Write "<!--"
			Response.Write Chr(10)
			Response.Write "function CheckAll(form) {"
			Response.Write Chr(10)
			Response.Write "for (var i=0;i<form.elements.length;i++){"
			Response.Write Chr(10)
			Response.Write "var e = form.elements[i];"
			Response.Write Chr(10)
			Response.Write "if (e.name != 'chkall')  e.checked = form.chkall.checked;"
			Response.Write Chr(10)
			Response.Write "}"
			Response.Write Chr(10)
			Response.Write "}"
			Response.Write Chr(10)
			Response.Write "//-->"
			Response.Write Chr(10)
			Response.Write "</script>"
			Response.Write Chr(10)		
			Exit Sub
		Case "RewardMoney"
			If Not ChkRewardMoney Then Dvbbs.AddErrCode(28) : Exit Sub
			reaction = "SaveRewardMoney"
			Action = "帖子评价"
			Dim TempStr0
			If Not Dvbbs.Master Then
				TempStr0 = Replace(template.html(8),"{$UserTodyInfo}",Replace(template.Strings(3),"{$PayMoney}",(Clng(Dvbbs.Forum_Setting(97))-Clng(Dvbbs.UserToday(4)))))
			Else
				TempStr0 = Replace(template.html(8),"{$UserTodyInfo}","")
			End If
		Case Else
			Dvbbs.AddErrCode(35)
			Exit Sub
		End Select
		Dim TempStr
		TempStr = template.html(0)
		If reaction = "SaveRewardMoney" Then
			TempStr = Replace(TempStr,"{$ManageInfo}",TempStr0)
		Else
			TempStr = Replace(TempStr,"{$ManageInfo}",template.html(7))
		End If
		TempStr = Replace(TempStr,"{$reaction}",reaction)
		TempStr = Replace(TempStr,"{$action}",Action)
		TempStr = Replace(TempStr,"{$seldisable}",seldisable)
		TempStr = Replace(TempStr,"{$doWealth}",doWealth)
		TempStr = Replace(TempStr,"{$dousercp}",dousercp)
		TempStr = Replace(TempStr,"{$douserep}",douserep)
		TempStr = Replace(TempStr,"{$douserep}",douserep)
		TempStr = Replace(TempStr,"{$decrease}",Decrease)
		TempStr = Replace(TempStr,"{$boardid}",Dvbbs.BoardID)
		TempStr = Replace(TempStr,"{$id}",id)
		TempStr = Replace(TempStr,"{$replyid}",replyid)
		TempStr = Replace(TempStr,"{$fileconfirm}",DelUpFile)
		Dim TopicQuestion,iTopicQuestion
		TopicQuestion = Split(Dvbbs.Board_Setting(65),"|")
		For i = 0 To Ubound(TopicQuestion)
			iTopicQuestion = iTopicQuestion & "<option value="""&TopicQuestion(i)&""">"&TopicQuestion(i)&"</option>"
		Next
		TempStr = Replace(TempStr,"{$topicquestion}",iTopicQuestion)
		Response.Write TempStr
	End Sub

	Public Function Check_AnnounceInfo()
		Set Rs=Dvbbs.Execute("Select topic,username,postuserID From "&TotalUseTable&" Where boardid="&Dvbbs.boardid&" and AnnounceID="&ReplyID)
		If Rs.Eof And Rs.Bof Then
			Dvbbs.AddErrCode(32)
			Exit Function
		End If
		Topic=rs(0)
		TopicUsername=rs(1)
		TopicUserID=Clng(rs(2))
		Rs.close
	End Function
	Public Function Check_topicInfo()
		Set Rs=Dvbbs.Execute("Select topic,username,postuserID From "&TotalUseTable&" Where ParentID=0 and boardid="&dvbbs.boardid&" and RootID="&ID)
		If Rs.Eof And Rs.Bof Then
			Dvbbs.AddErrCode(32)
			Exit Function
		End If
		Topic=rs(0)
		TopicUsername=rs(1)
		TopicUserID=Clng(rs(2))
		Rs.close
	End Function
	Public Function Insert_Forum_Log()
		Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (" & ID & "," & Dvbbs.BoardID & ",'" & Dvbbs.CheckStr(TopicUsername) & "','" & Dvbbs.MemberName & "','" & Dvbbs.CheckStr(sucmsg) & "','" & IP & "',"&LogType&")")
	End Function

	Public Function Update_User_Point(SQLSTR)
		If allmsg<>"" Then
		Dvbbs.Execute("Update [Dv_user] Set userWealth=userWealth+"&doWealth&",userCP=userCP+"&douserCP&",userEP=userEP+"&douserEP&" "&SQLSTR&" Where UserID="&TopicUserID)
		End If
	End Function

	Public Function Topic_Manage_Sms()
		If Request("ismsg")="1" Then
		Dim msgcontent
		msgcontent="您发表的帖子《[url=dispbbs.asp?boardID="&Dvbbs.BoardID&"&ID="&ID&"]"&Topic&"[/url]》因"&replace(Content,"理由：","")&"而被"&ActionInfo&"，且进行了"&replace(Allmsg,"用户操作：","")&"的操作"
		If Request("msg")<>"" Then msgContent=msgContent & chr(10) & "以下为操作者给您的附言：" & Request("msg")

		Dvbbs.Execute("Insert Into Dv_Message(incept,sender,title,content,sendtime,flag,issend) values('"&Dvbbs.CheckStr(TopicUsername)&"','"&Dvbbs.MemberName&"','系统消息','"&Dvbbs.CheckStr(msgContent)&"',"&SqlNowString&",0,1)")
		Update_User_Msg(TopicUsername)
		End If
	End Function
	Public Function Update_User_Msg(username)
		Dim msginfo
		If newincept(username)>0 Then
			msginfo=newincept(username) & "||" & inceptid(1,username) & "||" & inceptid(2,username)
		Else
			msginfo="0||0||null"
		End If
		Dvbbs.Execute("Update [Dv_User] Set UserMsg='"&dvbbs.CheckStr(msginfo)&"' Where username='"&dvbbs.CheckStr(username)&"'")
	End Function
	'统计留言
	Public Function newincept(iusername)
		Dim rs
		Rs=Dvbbs.Execute("Select Count(id) From Dv_Message Where flag=0 and issend=1 and delR=0 And incept='"& iusername &"'")
		newincept=Rs(0)
		Set Rs=Nothing
		If IsNull(newincept) Then newincept=0
	End Function
	Public Function inceptid(stype,iusername)
		Dim ars
		set ars=Dvbbs.Execute("Select top 1 id,sender From Dv_Message Where flag=0 and issend=1 and delR=0 And incept ='"& iusername &"'")
		if stype=1 then
			inceptid=ars(0)
		else
			inceptid=ars(1)
		end if
		set ars=nothing
	End Function
	'判断是否为帖子最后回复
	Public Function isLastPost()
		Dim LastTopic,body,LastRootID,LastPostTime,LastPostUser
		Dim LastPost,uploadpic_n,LastPostUserID,LastID
		isLastPost=False
		'取得当前主题最后回复ID
		Set Rs=Dvbbs.Execute("select LastPost from Dv_topic where topicID="&ID)
		If not (rs.eof and rs.bof) Then
			If not isnull(rs(0)) and rs(0)<>"" Then
				If Clng(split(rs(0),"$")(1))=Clng(replyID) Then isLastPost=True
			End If
		End If
		If isLastPost Then
			Set Rs=Dvbbs.Execute("select top 1 topic,body,AnnounceID,dateandtime,username,PostUserID,rootID,boardID from "&TotalUseTable&" where BoardID="&Dvbbs.BoardID&" And rootID="&ID&" order by AnnounceID desc")
			If not(rs.eof and rs.bof) Then
				body=rs(1)
				LastRootID=rs(2)
				LastPostTime=rs(3)
				LastPostUser=replace(rs(4),"$","")
				LastTopic=left(replace(body,"$",""),20)
				LastPostUserID=rs(5)
				LastID=rs(6)
				Dvbbs.BoardID=rs(7)
			Else
				LastTopic="无"
				LastRootID=0
				LastPostTime=now()
				LastPostUser="无"
				LastPostUserID=0
				LastID=0
				Dvbbs.BoardID=0
			End If
			set rs=nothing
			LastPost=LastPostUser & "$" & LastRootID & "$" & LastPostTime & "$" & replace(left(Dvbbs.Replacehtml(LastTopic),20),"$","") & "$" & uploadpic_n & "$" & LastPostUserID & "$" & LastID & "$" & Dvbbs.BoardID
			Dvbbs.Execute("update Dv_topic set LastPost='"&Dvbbs.CheckStr(LastPost)&"' where topicID="&ID)
		End If
	End Function
	'更新帖子最后回复信息 2005-1-12 Dv.Yz
	Public Function FixLastPost()
		Dim LastTopic,body,LastRootID,LastPostTime,LastPostUser
		Dim LastPost,uploadpic_n,LastPostUserID,LastID
		Set Rs = Dvbbs.Execute("SELECT TOP 1 Topic, Body, AnnounceID, Dateandtime, Username, PostUserID, RootID, BoardID ,signflag FROM " & TotalUseTable & " WHERE BoardID = " & Dvbbs.BoardID & " AND RootID = " & ID & " ORDER BY AnnounceID DESC")
		If Not(Rs.Eof And Rs.Bof) Then
			Body = Rs(1)
			LastRootID = Rs(2)
			LastPostTime = Rs(3)
			If Rs(8)=2 Then
				LastPostUser = "匿名用户"
			Else
				LastPostUser = Replace(Rs(4),"$","")
			End If
			LastTopic = Left(Replace(Body,"$",""),20)
			LastPostUserID = Rs(5)
			LastID = Rs(6)
			Dvbbs.BoardID = Rs(7)
		Else
			LastTopic = "无"
			LastRootID = 0
			LastPostTime = Now()
			LastPostUser = "无"
			LastPostUserID = 0
			LastID = 0
			Dvbbs.BoardID = 0
		End If
		Set Rs = Nothing
		LastPost = LastPostUser & "$" & LastRootID & "$" & LastPostTime & "$" & Replace(left(Dvbbs.Replacehtml(LastTopic),20),"$","") & "$" & Uploadpic_n & "$" & LastPostUserID & "$" & LastID & "$" & Dvbbs.BoardID
		Dvbbs.Execute("UPDATE Dv_Topic SET LastPost = '" & Dvbbs.CheckStr(LastPost) & "' WHERE TopicID = " & ID)
	End Function
	'更新指定论坛信息
	Public Function LastCount(boardID)
		Dim LastTopic,body,LastRootID,LastPostTime,LastPostUser
		Dim LastPost,uploadpic_n,LastpostuserID,LastID
		set rs=Dvbbs.Execute("select top 1 T.title,b.AnnounceID,b.dateandtime,b.username,b.postuserID,b.rootID from "&dvbbs.NowUseBBS&" b inner join dv_Topic T on b.rootID=T.TopicID where b.boardID="&boardID&" order by b.announceID desc")
		If not(rs.eof and rs.bof) Then
			Lasttopic=replace(left(Dvbbs.Replacehtml(rs(0)),15),"$","")
			LastRootID=rs(1)
			LastPostTime=rs(2)
			LastPostUser=rs(3)
			LastPostUserID=rs(4)
			LastID=rs(5)
		Else
			LastTopic="无"
			LastRootID=0
			LastPostTime=now()
			LastPostUser="无"
			LastPostUserID=0
			LastID=0
		End If
		set rs=nothing

		LastPost=LastPostUser & "$" & LastRootID & "$" & LastPostTime & "$" & LastTopic & "$" & uploadpic_n & "$" & LastPostUserID & "$" & LastID & "$" & BoardID
		Dim SplitUpBoardID,SplitLastPost
		SplitUpBoardID=split(UpdateBoardID,",")
		For i=0 to ubound(SplitUpBoardID)
			set rs=Dvbbs.Execute("select LastPost from dv_board where boardID="&SplitUpBoardID(i))
			If not (rs.eof and rs.bof) Then
				SplitLastPost=split(rs(0),"$")
				If IsNumeric(LastRootID) and IsNumeric(SplitLastPost(1)) Then
					If ubound(SplitLastPost)=7 and clng(LastRootID)<>clng(SplitLastPost(1)) Then
						Dvbbs.Execute("update dv_board set LastPost='"&Dvbbs.CheckStr(LastPost)&"' where boardID="&SplitUpBoardID(i))
						If  IsObject(Application(Dvbbs.CacheName &"_information_" & SplitUpBoardID(i))) Then
							Application(Dvbbs.CacheName &"_information_" & SplitUpBoardID(i)).documentElement.selectSingleNode("information/@lastpost_0").text=LastPostUser
							Application(Dvbbs.CacheName &"_information_" & SplitUpBoardID(i)).documentElement.selectSingleNode("information/@lastpost_1").text=LastRootID
							Application(Dvbbs.CacheName &"_information_" & SplitUpBoardID(i)).documentElement.selectSingleNode("information/@lastpost_2").text=LastPostTime
							Application(Dvbbs.CacheName &"_information_" & SplitUpBoardID(i)).documentElement.selectSingleNode("information/@lastpost_3").text=LastTopic
							Application(Dvbbs.CacheName &"_information_" & SplitUpBoardID(i)).documentElement.selectSingleNode("information/@lastpost_4").text=uploadpic_n 
							Application(Dvbbs.CacheName &"_information_" & SplitUpBoardID(i)).documentElement.selectSingleNode("information/@lastpost_5").text=LastPostUserID
							Application(Dvbbs.CacheName &"_information_" & SplitUpBoardID(i)).documentElement.selectSingleNode("information/@lastpost_6").text=LastID
							Application(Dvbbs.CacheName &"_information_" & SplitUpBoardID(i)).documentElement.selectSingleNode("information/@lastpost_7").text=BoardID
						End If
					End If
				End If
			End If
		Next
		Set Rs=Nothing
	End Function

	'版面发帖数增加
	Public Sub BoardNumAdd(boardID,topicNum,postNum,todayNum)
		Dvbbs.Execute("update dv_board set postnum=postnum+"&postNum&",topicNum=topicNum+"&topicNum&",todayNum=todayNum+"&todayNum&" where boardID in ("&UpdateBoardID&")")
		'Dvbbs.ReloadBoardInfo(UpdateBoardID)
	End Sub
	
	'版面发帖数减少
	Public Sub BoardNumSub(boardID,topicNum,postNum,todayNum)
		Dvbbs.Execute("update dv_board set postnum=postnum-"&postNum&",topicNum=topicNum-"&topicNum&",todayNum=todayNum-"&todayNum&" where boardID in ("&UpdateBoardID&")")
		Dim trs,LastPostTime,LastpostuserID,Lastid,uploadpic_n
		Set trs=Dvbbs.Execute("select top 1 T.title,b.Announceid,b.dateandtime,b.username,b.postuserid,b.rootid from "&Dvbbs.NowUseBBS&" b inner join dv_Topic T on b.rootid=T.TopicID where b.boardid="&boardid&" order by b.Announceid desc")
		If not(trs.eof and trs.bof) Then
			Lasttopic=replace(left(Dvbbs.Replacehtml(trs(0)),15),"$","")
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
	End Sub
	
	'所有论坛发帖数增加
	Public Function AllboardNumAdd(todayNum,postNum,topicNum)
		Dvbbs.Execute("Update dv_Setup Set Forum_TodayNum=Forum_todayNum+"&todaynum&",Forum_PostNum=Forum_PostNum+"&postNum&",Forum_TopicNum=Forum_topicNum+"&TopicNum)
		Dvbbs.Name="setup"
		Dvbbs.RemoveCache()
	End Function

	'所有论坛发帖数减少
	Public Function AllboardNumSub(todayNum,postNum,topicNum)
		Dvbbs.Execute("Update dv_Setup Set Forum_TodayNum=Forum_TodayNum-"&todaynum&",Forum_PostNum=Forum_PostNum-"&postNum&",Forum_TopicNum=Forum_TopicNum-"&TopicNum)
		Dvbbs.Name="setup"
		Dvbbs.RemoveCache()
	End Function

	Public Sub Get_RequestInfo()
		sucmsg=""
		title=Dvbbs.htmlencode(Request.form("title"))
		content=Dvbbs.htmlencode(Request.form("content"))
		content="理由：" & title & content
		If Request.form("title")="" and Request.form("content")="" Then
			Dvbbs.AddErrCode(39)
			Dvbbs.ShowErr()
		End If
		sucmsg=ActionInfo&"《"&server.htmlencode(topic)&"》，"&server.htmlencode(content)& "，"&allmsg&""
	End Sub

	Private Function SucMsgInfo(GetMsg)
		SucMsgInfo="<li>"+GetMsg
		SucMsgInfo=SucMsgInfo+"<li>"+"<a href=index.asp?boardid="&Dvbbs.boardid&">返回论坛列表</a>"
		SucMsgInfo=SucMsgInfo+"<li>"+"<a href=dispbbs.asp?boardid="&Dvbbs.boardid&"&id="&ID&" >返回主题：《"&server.htmlencode(Topic)&"》</a>"
	End Function

	'专题管理操作
	Public Sub TopicMode()
	Dim ModeID
	ModeID=Request.Form("mode")
	If Request.form("title")="" and Request.form("content")="" Then
		Dvbbs.AddErrCode(39)
		Exit Sub
	End If

	If ModeID<>"" And IsNumeric(ModeID) Then
	LogType=5
	Get_RequestInfo
	ModeID=Cint(ModeID)
	DVbbs.Execute("Update Dv_Topic Set Mode="&ModeID&" Where BoardID="&Dvbbs.BoardID&" And TopicID=" & ID)
	Insert_Forum_Log()
	Update_User_Point("")
	Topic_Manage_Sms()
	Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	Else
		title=Dvbbs.htmlencode(Request.form("title"))
		content=Dvbbs.htmlencode(Request.form("content"))
		content=title & content
		Dim BoardTopic,SelectBoardTopic,TempStr
		BoardTopic=Split(Dvbbs.Board_Setting(48),"$$")
		If Ubound(BoardTopic)>0 Then
			For i=0 to Ubound(BoardTopic)-1
				SelectBoardTopic=SelectBoardTopic+"<option value="&(i+1)
				SelectBoardTopic=SelectBoardTopic+" >"&BoardTopic(i)&"</option>"
			Next
		End If
		TempStr = template.html(4)
		TempStr = Replace(TempStr,"{$reaction}",request("action"))
		TempStr = Replace(TempStr,"{$boardid}",Request("boardID"))
		TempStr = Replace(TempStr,"{$id}",Request("ID"))
		TempStr = Replace(TempStr,"{$title}",content)
		TempStr = Replace(TempStr,"{$doWealth}",doWealth)
		TempStr = Replace(TempStr,"{$dousercp}",dousercp)
		TempStr = Replace(TempStr,"{$douserep}",douserep)
		TempStr = Replace(TempStr,"{$msg}",Request.form("msg"))
		TempStr = Replace(TempStr,"{$ismsg}",Request.form("ismsg"))
		TempStr = Replace(TempStr,"{$TopicMode}",SelectBoardTopic)
		Response.Write TempStr
	End If
	End Sub

	'锁定帖子
	Public Sub lock()
		LogType=5
		Get_RequestInfo
		Dvbbs.Execute("Update Dv_topic Set locktopic=1 where boardID="&Dvbbs.boardID&" and topicID="&ID)
		Insert_Forum_Log()
		Update_User_Point("")
		Topic_Manage_Sms()
		Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	End Sub
	'解除锁定帖子
	Public Sub unlock()
		LogType=3
		Get_RequestInfo
		Dvbbs.Execute("Update Dv_topic Set locktopic=0 where boardID="&Dvbbs.boardID&" and topicID="&ID)
		Insert_Forum_Log()
		Update_User_Point("")
		Topic_Manage_Sms()
		Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	End Sub

	'提升帖子
	Public Sub uptopic()
		LogType=3
		Get_RequestInfo
		Dvbbs.Execute("Update dv_topic set LastPostTime="&SqlNowString&" where boardID="&Dvbbs.BoardID&" and IsTop=0 and topicID="&ID)
		Insert_Forum_Log()
		Update_User_Point("")
		Topic_Manage_Sms()
		Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	End Sub
	'沉底帖子
	Public Sub downtopic()
		LogType=3
		Get_RequestInfo
		If IsSqlDataBase=1 Then
			Dvbbs.Execute("Update dv_topic set LastPostTime=dateadd(day,-100,LastPostTime) where boardID="&Dvbbs.BoardID&" and IsTop=0 and topicID="&ID)
		Else
			Dvbbs.Execute("Update dv_topic set LastPostTime=dateadd('d',-100,LastPostTime) where boardID="&Dvbbs.BoardID&" and IsTop=0 and topicID="&ID)
		End If
		Insert_Forum_Log()
		Update_User_Point("")
		Topic_Manage_Sms()
		Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	End Sub
	'固顶帖子，包括总固顶、区固顶和固顶
	Public Sub Getistop()
		Dim IsTop
		Dim iForum_AllTopNum,mForum_AllTopNum
		Dim getBoard,BoardTopStr,iBoardTopStr,UpBoardID
		Dim Bn
		LogType=4
		Get_RequestInfo
		If Request("getboard")<>"" Then	'防止注入 by Dv.ADRX
			If Not IsNumeric(Replace(Request("getboard")," ","")) Then
				Dvbbs.AddErrCode(18)
				Exit Sub
			End If
		End If
		If Request("istopaction")="1" Then
			'------------------------------------------------------------------------------------
			'如果等级是斑主，需要判断是否有该版的管理权限 Fssunwin
			If Dvbbs.UserGroupID = 3 and Trim(Request("getboard"))<>"" Then
				Dim CanMsterBoardID
				CanMsterBoardID = GetBoardMsterID(Dvbbs.MemberName)
				If CanMsterBoardID="" Then
					Dvbbs.AddErrCode(28)
					Exit Sub
				End If
				GetBoard = Split(Replace(Request("getboard")," ",""),",")
				For i=0 To Ubound(GetBoard)
					If InStr("," & CanMsterBoardID & ",", "," & GetBoard(i) & ",")=0 Then
						Dvbbs.AddErrCode(28)
						Exit Sub
					End If
				Next
			End If
			'------------------------------------------------------------------------------------
			'如果原来是固顶、区域固顶或总固顶，判断其是否有需要清理数据
			Set Rs=Dvbbs.Execute("Select IsTop From Dv_Topic Where TopicID="& ID)
			IsTop = Rs(0)
			'如果有总固顶需要清理
			If IsTop = 3 And Request("alltop")="" And CanTopTopic_a Then
				ActionInfo = "清除总固顶"
				Dvbbs.Execute("update dv_topic set istop=0  where boardID="&Dvbbs.BoardID&" and topicID="&ID)
				IsTop = 0
				'将总固顶ID从总设置表去除
				Set Rs=Dvbbs.Execute("Select Forum_AllTopNum From Dv_Setup")
				iForum_AllTopNum = "," & Rs(0) & ","
				If Instr(iForum_AllTopNum,"," & ID & ",")>0 Then
					iForum_AllTopNum = Split(iForum_AllTopNum,",")
					For i=1 To Ubound(iForum_AllTopNum)-1
						If Cstr(Trim(iForum_AllTopNum(i)))<>Cstr(ID) Then
							If mForum_AllTopNum="" Then
								mForum_AllTopNum = iForum_AllTopNum(i)
							Else
								mForum_AllTopNum = mForum_AllTopNum & "," & iForum_AllTopNum(i)
							End If
						End If
					Next
					Dvbbs.Execute("Update Dv_Setup Set Forum_AllTopNum='"&mForum_AllTopNum&"'")
					'Dvbbs.ReloadSetupCache mForum_AllTopNum,28
				End If
				Set Rs=Nothing
			End If

			'如果有固顶需要清理
			If IsTop = 1 And CanTopTopic And Trim(Request("getboard"))="" Then
				ActionInfo = "解除固顶"
				Dvbbs.Execute("update dv_topic set istop=0 where boardID="&Dvbbs.BoardID&" and topicID="&ID)
				IsTop = 0
				'清理对应版面中的帖子ID
				Set Rs=Dvbbs.Execute("Select BoardID,BoardTopStr From Dv_Board Where BoardID="&Dvbbs.BoardID)
				If Not (Rs.Eof And Rs.Bof) Then
					If Rs(1)="" Or IsNull(Rs(1)) Then
						iBoardTopStr = ""
					Else
						If InStr(","&Rs(1)&",",","&ID&",")>0 Then
							BoardTopStr = "," & Rs(1) & ","
							BoardTopStr = Split(BoardTopStr,",")
							For i = 1 To Ubound(BoardTopStr)-1
								If Cstr(Trim(BoardTopStr(i)))<>Cstr(ID) Then
									If iBoardTopStr="" Then
										iBoardTopStr = BoardTopStr(i)
									Else
										iBoardTopStr = iBoardTopStr & "," & BoardTopStr(i)
									End If
								End If								
							Next
						Else
							iBoardTopStr = Rs(1)
						End If
					End If
					Dvbbs.Execute("Update Dv_Board Set BoardTopStr='"&iBoardTopStr&"' Where BoardID="&Rs(0))
					Dvbbs.LoadBoardinformation Rs(0)
					BoardTopStr = ""
				End If
			End If

			'如果有区域固顶需要清理
			UpBoardID = ""
			If IsTop = 2 And CanTopTopic_m Then
				'如果返回的getboard为空，则已经解除该贴的区域固顶，应清理所有含有该ID的版面
				If Trim(Request("getboard"))="" Then
					ActionInfo = "解除区域固顶"
					
						Dvbbs.Execute("update dv_topic set istop=0  where boardID="&Dvbbs.BoardID&" and topicID="&ID)
					
					IsTop = 0
					'查询得出原来该贴所固顶的版面
					Set Rs=Dvbbs.Execute("Select BoardID,BoardTopStr From Dv_Board Where BoardTopStr Like '%"&ID&"%'")
					Rem 以数组代替循环查询。 2004-5-7 Dvbbs.YangZheng
					If Not (Rs.Eof And Rs.Bof) Then
						Sql = Rs.GetRows(-1)
						Rs.Close:Set Rs = Nothing
						For Bn = 0 To Ubound(Sql,2)
							UpBoardID = UpBoardID & Sql(0,Bn) &","
							If Sql(1,Bn) = "" Or IsNull(Sql(1,Bn)) Then
								iBoardTopStr = ""
							Else
								If InStr("," & Sql(1,Bn) & ",", "," & ID & ",") > 0 Then
									BoardTopStr = "," & Sql(1,Bn) & ","
									BoardTopStr = Split(BoardTopStr,",")
									For i = 1 To Ubound(BoardTopStr)-1
										If Cstr(Trim(BoardTopStr(i))) <> Cstr(ID) Then
											If iBoardTopStr="" Then
												iBoardTopStr = BoardTopStr(i)
											Else
												iBoardTopStr = iBoardTopStr & "," & BoardTopStr(i)
											End If
										End If								
									Next
								Else
									iBoardTopStr = Sql(1,Bn)
								End If
							End If
							Dvbbs.Execute("Update Dv_Board Set BoardTopStr='" & iBoardTopStr & "' Where BoardID = " & Sql(0,Bn))
							BoardTopStr = ""
							iBoardTopStr = ""
						Next
						Dvbbs.ReloadBoardInfo(UpBoardID & Dvbbs.BoardID)
					End If
				'如果返回的getboard不为空，则应清理原来含有该ID且不属于返回的getboard的版面的该帖子ID
				'需同时判断，如果用户将原区域固顶设置升级为总固顶，且忘记取消列表中的版面，则应清理该ID对应的版面
				Else
					Dim ii
					ii = 0
					If Request("alltop")="1" Then
						
							Dvbbs.Execute("update dv_topic set istop=0 where boardID="&Dvbbs.BoardID&" and topicID="&ID)
					
					
						IsTop = 0
						'查询得出原来该贴所固顶的版面
						UpBoardID = ""
						Set Rs = Dvbbs.Execute("Select BoardID,BoardTopStr From Dv_Board Where BoardTopStr Like '%" & ID & "%'")
						If Not (Rs.Eof And Rs.Bof) Then
							Sql = Rs.GetRows(-1)
							Rs.Close:Set Rs = Nothing
							For Bn = 0 To Ubound(Sql,2)
								UpBoardID = UpBoardID & Sql(0,Bn) &","
								If Sql(1,Bn) = "" Or IsNull(Sql(1,Bn)) Then
									iBoardTopStr = ""
								Else
									If InStr("," & Sql(1,Bn) & ",", "," & ID & ",") > 0 Then
										BoardTopStr = "," & Sql(1,Bn) & ","
										BoardTopStr = Split(BoardTopStr,",")
										For i = 1 To Ubound(BoardTopStr)-1
											If Cstr(Trim(BoardTopStr(i))) <> Cstr(ID) Then
												If iBoardTopStr="" Then
													iBoardTopStr = BoardTopStr(i)
												Else
													iBoardTopStr = iBoardTopStr & "," & BoardTopStr(i)
												End If
											End If								
										Next
									Else
										iBoardTopStr = Sql(1,Bn)
									End If
								End If
								Dvbbs.Execute("UPDATE Dv_Board SET BoardTopStr = '" & iBoardTopStr & "' WHERE BoardID = " & Sql(0,Bn))	
								BoardTopStr = ""
								iBoardTopStr = ""
							Next
							Dvbbs.ReloadBoardInfo(UpBoardID & Dvbbs.BoardID)
						End If
						IsTop = 0
					Else
						UpBoardID = ""
						Set Rs = Dvbbs.Execute("SELECT BoardID, BoardTopStr FROM Dv_Board WHERE (NOT BoardID IN (" & Dvbbs.Checkstr(Request("getboard")) & ")) AND BoardTopStr LIKE '%" & ID & "%'")
						If Not (Rs.Eof And Rs.Bof) Then
							Sql = Rs.GetRows(-1)
							Rs.Close:Set Rs = Nothing
							For Bn = 0 To Ubound(Sql,2)
								UpBoardID = UpBoardID & Sql(0,Bn) &","
								If Sql(1,Bn) = "" Or IsNull(Sql(1,Bn)) Then
									iBoardTopStr = ""
								Else
									If InStr("," & Sql(1,Bn) & ",", "," & ID & ",") > 0 Then
										BoardTopStr = "," & Sql(1,Bn) & ","
										BoardTopStr = Split(BoardTopStr,",")
										For i = 1 To Ubound(BoardTopStr)-1
											If Cstr(Trim(BoardTopStr(i)))<>Cstr(ID) Then
												If iBoardTopStr="" Then
													iBoardTopStr = BoardTopStr(i)
												Else
													iBoardTopStr = iBoardTopStr & "," & BoardTopStr(i)
												End If
											End If								
										Next
										ii = ii + 1
									Else
										iBoardTopStr = Sql(1,Bn)
									End If
								End If
								Dvbbs.Execute("UPDATE Dv_Board SET BoardTopStr = '" & iBoardTopStr & "' WHERE BoardID = " & Sql(0,Bn))
								BoardTopStr = ""
								iBoardTopStr = ""
							Next
							Dvbbs.ReloadBoardInfo(UpBoardID & Dvbbs.BoardID)
						End If
						GetBoard = Split(Request("getboard"),",")
						'如果单选当前版面，则取消区域固顶，还原为版面固顶，如多选则不做处理
						If Ubound(getBoard)=0 And Clng(getBoard(0))=Dvbbs.BoardID And CanTopTopic Then
							'Select Case IsTop
							'	Case 0 : TimeAdd = 100
							'	Case 1 : TimeAdd = 0
							'	Case 2 : TimeAdd = -100
							'	Case 3 : TimeAdd = -200
							'	Case Else : TimeAdd = 0
							'End Select
						
							Dvbbs.Execute("UPDATE Dv_Topic SET Istop = 1 WHERE BoardID = " & Dvbbs.BoardID & " AND TopicID = " & ID)
						
							IsTop = 1
						End If
					End If 'End By AllTop
				End If
			End If

			'总固顶操作
			Dim TimeAdd
			TimeAdd = 0
			If Request("alltop")="1" And CanTopTopic_a Then
					Dvbbs.Execute("update dv_topic set istop=3  where boardID="&Dvbbs.BoardID&" and topicID="&ID)
			
				'将总固顶ID插入总设置表
				Set Rs=Dvbbs.Execute("Select Forum_AllTopNum From Dv_Setup")
				iForum_AllTopNum = "," & Rs(0) & ","
				If Instr(iForum_AllTopNum,"," & ID & ",")=0 Then
					If Trim(Rs(0))="" Then
						iForum_AllTopNum = ID
					Else
						iForum_AllTopNum = Rs(0) & "," & ID
					End If
					Dvbbs.Execute("Update Dv_Setup Set Forum_AllTopNum='"&iForum_AllTopNum&"'")
					Dvbbs.ReloadSetupCache iForum_AllTopNum,28
				End If
				Set Rs=Nothing
			Else
				If Request("getboard")<>"" Then
					getBoard = Split(Request("getBoard"),",")
					'单选且当前版面固顶
					i = 0
					If Ubound(getBoard)=0 And Clng(getBoard(0))=Dvbbs.BoardID And CanTopTopic Then
						Set Rs=Dvbbs.Execute("Select BoardID,BoardTopStr From Dv_Board Where BoardID="&Clng(getBoard(0)))
						If Not (Rs.Eof And Rs.Bof) Then
							If Rs(1)="" Or IsNull(Rs(1)) Then
								BoardTopStr = ID
								i = i + 1
							Else
								If InStr(","&Rs(1)&",",","&ID&",")>0 Then
									BoardTopStr = Rs(1)
								Else
									BoardTopStr = Rs(1) & "," & ID
									i = i + 1
								End If
							End If
							Dvbbs.Execute("Update Dv_Board Set BoardTopStr='"&BoardTopStr&"' Where BoardID="&Rs(0))
							Dvbbs.LoadBoardinformation Rs(0)
							BoardTopStr = ""
						End If
						Dvbbs.Execute("update dv_topic set istop=1 where boardID="&Dvbbs.BoardID&" and topicID="&ID)
					
					'多选区域固顶，包含在当前版面固顶操作中单选其它版面
					'在这里不需判断当前用户在其它版面的权限
					'因为只要在用户组或版面权限或用户权限中对当前版面有区域固顶权限，则默认为可添加固顶到其它版面
					Else
						Set Rs=Dvbbs.Execute("Select BoardID,BoardTopStr From Dv_Board Where BoardID In ("&Dvbbs.Checkstr(Request("getBoard"))&")")
						Rem 数组替换循环查询。 2004-5-7 Dvbbs.YangZheng
						If Not (Rs.Eof And Rs.Bof) Then
							Sql = Rs.GetRows(-1)
							Rs.Close:Set Rs = Nothing
							For Bn = 0 To Ubound(Sql,2)
								If Sql(0,Bn) = Dvbbs.BoardID And CanTopTopic Then
									If Sql(1,Bn) = "" Or IsNull(Sql(1,Bn)) Then
										BoardTopStr = ID
										i = i + 1
									Else
										If InStr("," & Sql(1,Bn) & ",", "," & ID & ",") > 0 Then
											BoardTopStr = Sql(1,Bn)
										Else
											BoardTopStr = Sql(1,Bn) & "," & ID
											i = i + 1
										End If
									End If
									Dvbbs.Execute("UPDATE Dv_Board SET BoardTopStr = '" & BoardTopStr & "' WHERE BoardID = " & Sql(0,Bn))
									Dvbbs.ReloadBoardInfo(Sql(0,Bn))
								ElseIf CanTopTopic_m Then
									If Sql(1,Bn) = "" Or IsNull(Sql(1,Bn)) Then
										BoardTopStr = ID
										i = i + 1
									Else
										If InStr("," & Sql(1,Bn) & ",", "," & ID & ",") > 0 Then
											BoardTopStr = Sql(1,Bn)
										Else
											BoardTopStr = Sql(1,Bn) & "," & ID
											i = i + 1
										End If
									End If
									Dvbbs.Execute("UPDATE Dv_Board SET BoardTopStr = '" & BoardTopStr & "' WHERE BoardID = " & Sql(0,Bn))
								End If
								BoardTopStr = ""
							Next
							Dvbbs.ReloadBoardInfo(Dvbbs.Checkstr(Request("getBoard")))
						End If
						If i > 0 And CanTopTopic_m Then
								Dvbbs.Execute("update dv_topic set istop=2  where boardID="&Dvbbs.BoardID&" and topicID="&ID)
					
						ElseIf i > 0 And CanTopTopic Then
									Dvbbs.Execute("update dv_topic set istop=1  where boardID="&Dvbbs.BoardID&" and topicID="&ID)
						End If
					End If
					Set Rs=Nothing
				End If
			End If
			sucmsg=ActionInfo&"《"&Server.htmlencode(topic)&"》，"&Server.htmlencode(content)& "，"&allmsg&""
			Insert_Forum_Log()
			Update_User_Point("")
			Topic_Manage_Sms()
			Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
		Else
			Dim Body,TempStr,BoardJump
			TempStr = template.html(3)
			Set Rs=Dvbbs.Execute("Select title,Istop From Dv_Topic Where Boardid="&Dvbbs.boardid&" and TopicID="&ID)
			IsTop = Rs(1)
			Set Rs=Dvbbs.Execute("Select Body From "&TotalUseTable&" Where Boardid="&Dvbbs.boardid&" and RootID="&ID&" And ParentID=0")
			Body = Left(Dvbbs.HtmlEncode(Rs(0)),"250") & "..."
			Set Rs=Nothing
			TempStr = Replace(TempStr,"{$boardid}",Dvbbs.BoardID)
			TempStr = Replace(TempStr,"{$id}",ID)
			TempStr = Replace(TempStr,"{$topic}",Dvbbs.HtmlEncode(Topic))
			TempStr = Replace(TempStr,"{$content}",Body)
			TempStr = Replace(TempStr,"{$reaction}",request("action"))
			'有总固顶和区域固顶权限则显示所有版面列表
			If CanTopTopic_a Or CanTopTopic_m Then
				Set Rs=Dvbbs.Execute("select boardid,boardtype,depth,BoardTopStr from dv_board order by rootid,orders")
			Else
				Set Rs=Dvbbs.Execute("select boardid,boardtype,depth,BoardTopStr from dv_board Where BoardID="&Dvbbs.BoardID)
			End If
			Do While Not Rs.Eof
			BoardJump = BoardJump & "<option "
			If rs(0)=dvbbs.boardid Then
				BoardJump = BoardJump & " selected"
			Else
				If Rs(3)<>"" And Not IsNull(Rs(3)) And IsTop>0 Then
					If Instr("," & Rs(3) & ",","," & ID & ",")>0 Then BoardJump = BoardJump & " selected"
				End If
			End If
			BoardJump = BoardJump & " value="&rs(0)&">"
			Select Case rs(2)
			Case 0
				BoardJump = BoardJump & "╋"
			Case 1
				BoardJump = BoardJump & "&nbsp;&nbsp;├"
			End Select
			If rs(2)>1 Then
				For ii=2 To rs(2)
					BoardJump = BoardJump & "&nbsp;&nbsp;│"
				Next
				BoardJump = BoardJump & "&nbsp;&nbsp;├"
			End If
			BoardJump = BoardJump & rs(1)
			BoardJump = BoardJump & "</option>"
			Rs.MoveNext
			Loop
			Set Rs=Nothing
			TempStr = Replace(TempStr,"{$boardselected}",BoardJump)
			If Not CanTopTopic_a Then TempStr = Replace(TempStr,"{$checkbox1}","disabled")
			If IsTop = 3 Then TempStr = Replace(TempStr,"{$checkbox1}","checked")
			TempStr = Replace(TempStr,"{$checkbox1}","")
			TempStr = Replace(TempStr,"{$title}",Dvbbs.htmlencode(Request.form("title")))
			TempStr = Replace(TempStr,"{$msgcontent}",Dvbbs.htmlencode(Request.form("content")))
			TempStr = Replace(TempStr,"{$doWealth}",doWealth)
			TempStr = Replace(TempStr,"{$dousercp}",dousercp)
			TempStr = Replace(TempStr,"{$msg}",Request.form("msg"))
			TempStr = Replace(TempStr,"{$ismsg}",Request.form("ismsg"))
			If Dvbbs.GroupSetting(21)="1" Then TempStr = Replace(TempStr,"{$boardtop}","√")
			TempStr = Replace(TempStr,"{$boardtop}","<font color=red>×</font>")
			If Dvbbs.GroupSetting(54)="1" Then TempStr = Replace(TempStr,"{$areatop}","√")
			TempStr = Replace(TempStr,"{$areatop}","<font color=red>×</font>")
			If Dvbbs.GroupSetting(38)="1" Then TempStr = Replace(TempStr,"{$alltop}","√")
			TempStr = Replace(TempStr,"{$alltop}","<font color=red>×</font>")
			Response.Write TempStr
		End If
	End Sub

	'单帖屏蔽帖子
	Public Sub islockpage()
		LogType=5
		Get_RequestInfo
		Dvbbs.Execute("Update "&TotalUseTable&" Set LockTopic=2 where boardID="&Dvbbs.BoardID&" and announceID="&replyID)
		GetUserID
		Insert_Forum_Log()
		Update_User_Point("")
		Topic_Manage_Sms()
		Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	End Sub
	Sub GetUserID()
		Dim Rs
		Set Rs=Dvbbs.Execute("Select PostUserid,UserName From "&TotalUseTable&" Where Boardid="&Dvbbs.boardid&" and announceID="&replyID&"")
		If Not Rs.EOF Then
			TopicUserID=Rs(0)
			TopicUsername=Rs(1)
		End If 
		Set Rs=Nothing	
	End Sub 
	'解除单帖屏蔽帖子
	Public Sub nolockpage()
		LogType=3
		Get_RequestInfo
		Dvbbs.Execute("Update "&TotalUseTable&" set LockTopic=0 Where boardID="&Dvbbs.BoardID&" and announceID="&replyID)
		GetUserID
		Insert_Forum_Log()
		Update_User_Point("")
		Topic_Manage_Sms()
		Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	End Sub

	Public Sub fixtopic()
		Dim UseTools
		Set Rs=dvbbs.Execute("select UseTools from Dv_topic where BoardID="&Dvbbs.BoardID&" And topicid="&ID)
		UseTools=Rs(0)
		LogType=3
		'Get_RequestInfo
		sucmsg="修复帖子"
		Set Rs = Dvbbs.Execute("SELECT COUNT(*), MAX(DateAndTime) FROM " & TotalUseTable & " WHERE BoardID = " & Dvbbs.BoardID & " AND RootID = " & ID)
		If Not IsNull(rs(0)) And Not IsNull(rs(1)) Then
			If  InStr("," & UseTools & ",",",13,")>0 Or InStr("," & UseTools & ",",",14,")>0 Then
				Dvbbs.Execute("update dv_topic set child="&Rs(0)-1&" where topicID="&ID)
			Else
				Dvbbs.Execute("update dv_topic set child="&Rs(0)-1&",LastPostTime='"&rs(1)&"' where topicID="&ID)
			End If
			Set Rs=Nothing
		End If
		FixLastPost
		Insert_Forum_Log()
		Update_User_Point("")
		Topic_Manage_Sms()
		Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	End Sub
	'精华帖子
	Public Sub isbest()
		LogType=3
		Dim datetimestr
		Get_RequestInfo
		Set Rs = Dvbbs.Execute("Select * From Dv_BestTopic Where boardid="&dvbbs.boardid&" and AnnounceID="&replyID)
		If Not (Rs.Eof or Rs.Bof) Then
			Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
			Exit Sub
		End If

		Set rs=Dvbbs.Execute("Select * From "&TotalUseTable&" Where boardid="&dvbbs.boardid&" and AnnounceID="&replyID)
		If rs.eof and rs.bof Then
			Dvbbs.AddErrCode(32)
			Exit Sub
		End If

		topic=rs("topic")
		topicusername=rs("username")
		topicuserID=rs("postuserID")
		If topic="" Then topic=left(replace(rs("body"),chr(10),","),26)
		datetimestr=replace(replace(rs("dateandtime"),"上午",""),"下午","")

		Dvbbs.Execute("Update "&TotalUseTable&" Set isbest=1 where boardID="&Dvbbs.BoardID&" and announceID="&replyID)
		Dvbbs.Execute("Update Dv_topic Set isbest=1 where boardID="&Dvbbs.BoardID&" and topicID="&ID)
		Dvbbs.Execute("Insert Into Dv_bestTopic (title,boardID,AnnounceID,rootID,postusername,postuserID,dateandtime,expression) values ('"&Dvbbs.CheckStr(topic)&"',"&rs("boardID")&","&rs("AnnounceID")&","&rs("rootID")&",'"&Dvbbs.CheckStr(topicusername)&"',"&rs("postuserID")&",'"&datetimestr&"','"&rs("expression")&"')")
		
		Set Rs=Nothing

		Insert_Forum_Log()
		Update_User_Point(",userIsBest=userisBest+1")
		Topic_Manage_Sms()
		Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	End Sub
	'解除精华帖子
	Public Sub nobest()
		LogType=3
		Dim datetimestr
		Get_RequestInfo
		Set Rs = Dvbbs.Execute("Select * From Dv_BestTopic Where boardid="&dvbbs.boardid&" and AnnounceID="&replyID)
		If Rs.Eof or Rs.Bof Then
			Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
			Exit Sub
		End If
		Set rs=Dvbbs.Execute("Select * From "&TotalUseTable&" Where boardid="&dvbbs.boardid&" and AnnounceID="&replyID)
		If rs.eof and rs.bof Then
			Dvbbs.AddErrCode(32)
			Exit Sub
		End If
		topic=rs("topic")
		topicusername=rs("username")
		topicuserID=rs("postuserID")
		If topic="" Then topic="本帖子为回复帖子"
		Set Rs=Nothing

		Dvbbs.Execute("Update "&TotalUseTable&" set isbest=0 Where boardID="&Dvbbs.BoardID&" and announceID="&replyID)
		Dvbbs.Execute("Update Dv_topic set isbest=0 Where boardID="&Dvbbs.BoardID&" and topicID="&ID)
		Dvbbs.Execute("Delete from Dv_besttopic Where AnnounceID="&replyID)

		Insert_Forum_Log()
		Update_User_Point(",userIsBest=userisBest-1")
		Topic_Manage_Sms()
		Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	End Sub

	'删除跟贴
	Public Sub dele(md)
		Dim todaynum
		Dim isbest,IsUpload
		todaynum=0
		Set rs=Dvbbs.Execute("select topic,username,postuserID,DateAndTime,isbest,IsUpload from "&TotalUseTable&" where boardid="&dvbbs.boardid&" and AnnounceID="&replyID)
		If Not rs.eof Then
			Topic=Dvbbs.CheckStr(rs(0))
			topicusername=rs(1)
			topicuserID=rs(2)
			isbest=rs(4)
			IsUpload=rs(5)
			If topic="" Then topic="本帖子为回复帖子"
			If datediff("d",rs(3),now())=0 Then
				todaynum=1
			Else
				todaynum=0
			End If
		Else
			If md=1 Then
				Dvbbs.AddErrCode(32)
				Exit Sub
			End If
		End If
		Set Rs=Nothing
		
		'判断用户是否有删除帖子权限
		If Not CanDelTopic Then
			Dvbbs.AddErrCode(28)
			Exit Sub
		End If
		LogType=3
		Get_RequestInfo
		Dim LastPostime,istop
		'删除时自动删除精华回复帖
		If IsBest=1 Then
			Dvbbs.Execute("update dv_topic set isbest=0 where boardid="&Dvbbs.BoardID&" and topicid="&ID)
			Dvbbs.Execute("delete from dv_besttopic where Announceid="&replyID)
		End If
		Set Rs=Dvbbs.Execute("select istop from dv_topic where boardID="&Dvbbs.BoardID&" and topicID="&ID)
		istop=Rs(0)
		Rs.close
		Dvbbs.Execute("Update "&TotalUseTable&" Set BoardID=444,locktopic="&Dvbbs.BoardID&" Where BoardID="&Dvbbs.BoardID&" And AnnounceID="&replyID)
		Set Rs=Dvbbs.Execute("select Max(dateandtime) from "&TotalUseTable&" where boardID="&Dvbbs.BoardID&" and rootID="&ID)
		LastPostime=rs(0)
		Set Rs=Nothing
		isLastPost
		call LastCount(dvbbs.boardID)
		call BoardNumSub(dvbbs.boardID,0,1,todaynum)
		call AllboardNumSub(todaynum,1,0)
		Dvbbs.ReloadBoardInfo(UpdateBoardID)
		If IsUpload=1 Then
			If Request.form("delupfile")<>"" and Request.form("delupfile")=1 Then
				Call Delupfiles(Dvbbs.BoardID,ID&"|"&replyID)
			Else
			'更新上传附件数据
			Dvbbs.Execute("update Dv_Upfile Set F_flag=4 Where F_BoardID="&Dvbbs.BoardID&" And F_AnnounceID LIKE  '"&ID&"|"&replyID&"' ")
			End If
		End IF

		If istop>0 Then
			sql="update dv_topic set child=child-1 where boardID="&Dvbbs.BoardID&" and topicID="&ID
		Else
			sql="update dv_topic set child=child-1,LastPostTime='"&LastPostime&"' where boardID="&Dvbbs.BoardID&" and topicID="&ID
		End If
		'Response.Write sql
		Dvbbs.Execute(sql)
		Insert_Forum_Log()
		If Dvbbs.CheckNumeric(Request.Form("decrease"))>1 Then
			Update_User_Point(",UserPost=UserPost-1")
		Else
			Update_User_Point(",UserPost=UserPost-1,userDel=userDel-1")
		End If
		Topic_Manage_Sms()
		If md=1 Then
			Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
		End If 
	End Sub
	'删除主贴
	Public Sub delete()
		Dim voteID,isvote,isbest,istop
		Dim UpBoardID
		set rs=Dvbbs.Execute("select title,postusername,postuserID,PollID,isvote,isbest,istop from dv_topic where boardid="&Dvbbs.boardid&" and topicID="&ID)
		If rs.eof and rs.bof Then
			Dvbbs.AddErrCode(32)
			Exit Sub
		Else
			Topic=rs(0)
			topicusername=rs(1)
			topicuserID=rs(2)
			voteID=rs(3)
			isvote=rs(4)
			isbest=rs(5)
			istop=rs(6)
			If topic="" Then topic="本帖子为回复帖子"
		End If
		Set Rs=Nothing
		LogType=3
		Get_RequestInfo

		Dim todaynum,postnum
		set rs=Dvbbs.Execute("select count(*) from "&TotalUseTable&" where rootID="&ID)
		postNum=rs(0)
		If IsSqlDataBase=1 Then
			sql="select count(*) from "&TotalUseTable&" where rootID="&ID&" and dateandtime>'"&date()&"'"
		else
			sql="select count(*) from "&TotalUseTable&" where rootID="&ID&" and dateandtime>#"&date()&"#"
		end if
		Set Rs=Dvbbs.Execute(sql)
		todayNum=rs(0)
	
		'放入回收站，回收站boardid为444，locktopic为原版面ID
		Dvbbs.Execute("update "&TotalUseTable&" set BoardID=444,locktopic="&Dvbbs.BoardID&" where rootID="&ID)
		If isvote=1 Then
			Dvbbs.Execute("update dv_topic set BoardID=444,locktopic="&Dvbbs.BoardID&",isvote=0,VoteTotal=0 where topicID="&ID)
			Dvbbs.Execute("delete from dv_vote where voteID="&voteID)
			Dvbbs.Execute("delete from dv_voteuser where voteID="&voteID)
		'删帖时自动解除精华帖子
		ElseIf isbest=1 Then
			Dvbbs.Execute("update dv_topic set BoardID=444,locktopic="&Dvbbs.BoardID&",isbest=0 where topicid="&id)
			Dvbbs.Execute("delete from dv_besttopic where rootid="&id)
		Else
			Dvbbs.Execute("update dv_topic set BoardID=444,locktopic="&Dvbbs.BoardID&" where topicID="&ID)
		End If
		If istop>0 Then
			Dvbbs.Execute("update dv_topic set istop=0,LastPostTime="&SqlNowString&" where topicid="&ID)
			If istop=3 Then
				'将总固顶ID从总设置表去除
				Set Rs=Dvbbs.Execute("Select Forum_AllTopNum From Dv_Setup")
				Dim iForum_AllTopNum,mForum_AllTopNum
				iForum_AllTopNum = "," & Rs(0) & ","
				If Instr(iForum_AllTopNum,"," & ID & ",")>0 Then
					iForum_AllTopNum = Split(iForum_AllTopNum,",")
					For i=1 To Ubound(iForum_AllTopNum)-1
						If Cstr(Trim(iForum_AllTopNum(i)))<>Cstr(ID) Then
							If mForum_AllTopNum="" Then
								mForum_AllTopNum = iForum_AllTopNum(i)
							Else
								mForum_AllTopNum = mForum_AllTopNum & "," & iForum_AllTopNum(i)
							End If
						End If
					Next
					Dvbbs.Execute("Update Dv_Setup Set Forum_AllTopNum='"&mForum_AllTopNum&"'")
					'Dvbbs.ReloadSetupCache mForum_AllTopNum,28
				End If
				Set Rs=Nothing
			Else
				'将固顶贴ID从版面表中去除
				'查询得出原来该贴所固顶的版面
				Dim BoardTopStr,iBoardTopStr
				Set Rs=Dvbbs.Execute("Select BoardID,BoardTopStr From Dv_Board Where BoardTopStr Like '%"&ID&"%'")
				Do While Not Rs.Eof
					UpBoardID = UpBoardID & Rs(0) &","
					If Rs(1)="" Or IsNull(Rs(1)) Then
						iBoardTopStr = ""
					Else
						If InStr(","&Rs(1)&",",","&ID&",")>0 Then
							BoardTopStr = "," & Rs(1) & ","
							BoardTopStr = Split(BoardTopStr,",")
							For i = 1 To Ubound(BoardTopStr)-1
								If Cstr(Trim(BoardTopStr(i)))<>Cstr(ID) Then
									If iBoardTopStr="" Then
										iBoardTopStr = BoardTopStr(i)
									Else
										iBoardTopStr = iBoardTopStr & "," & BoardTopStr(i)
									End If
								End If								
							Next
						Else
							iBoardTopStr = Rs(1)
						End If
					End If
					Dvbbs.Execute("Update Dv_Board Set BoardTopStr='"&iBoardTopStr&"' Where BoardID="&Rs(0))
					BoardTopStr = ""
					iBoardTopStr = ""
				Rs.Movenext
				Loop
				Set Rs=Nothing
				Dvbbs.ReloadBoardInfo(UpBoardID&Dvbbs.Boardid)
			End If
		End If
		If  Request.form("delupfile")="1" Then
			Call Delupfiles(Dvbbs.BoardID,ID&"|")
		Else
			'上传文件数据更新
			Dvbbs.Execute("update Dv_Upfile Set F_flag=4 Where F_BoardID="&Dvbbs.BoardID&" And F_AnnounceID LIKE  '"&ID&"|"&"%' ")
		End IF
		call LastCount(dvbbs.boardID)
		call BoardNumSub(dvbbs.boardID,1,postNum,todayNum)
		call AllboardNumSub(todayNum,postNum,1)
		Dvbbs.ReloadBoardInfo(UpdateBoardID)

		Insert_Forum_Log()
		If topicuserID<>0 Then Topic_Manage_Sms()
		Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
		If topicuserID<> 0 Then
			If AllMsg<>"" Then
				Dim PostDecrease,TopicDecrease
				Select Case Dvbbs.CheckNumeric(Request.Form("decrease"))
					Case 0
						TopicDecrease=",UserPost=UserPost-1,userDel=userDel-1"
						PostDecrease=",UserPost=UserPost-1,userDel=userDel-1"
					Case 1
						TopicDecrease=",UserPost=UserPost-1"
						PostDecrease=",UserPost=UserPost-1,userDel=userDel-1"
					Case 2
						TopicDecrease=",UserPost=UserPost-1,userDel=userDel-1"
						PostDecrease=",UserPost=UserPost-1"
					Case 3
						PostDecrease=",UserPost=UserPost-1"
						TopicDecrease=",UserPost=UserPost-1"
				End Select
				Set Rs=Dvbbs.Execute("select postuserID,ParentID from "&TotalUseTable&" where rootID="&ID)
				Do While Not Rs.Eof
					TopicUserID = Rs(0)
					If Rs(1)=0 Then
						Update_User_Point(TopicDecrease)
					Else
						Update_User_Point(PostDecrease)
					End If
					Rs.MoveNext
				Loop
			End If
		End If
		Set Rs=Nothing
		Dvbbs.Name="setup"
		Dvbbs.RemoveCache()
	End Sub
	'移动帖子
	Public Sub Tmove()
		LogType=3
		Get_RequestInfo
		Dim reBoard_Setting,newboardID,newParentID,nrs,newtopic
		Set Rs=Dvbbs.iCreateObject("ADODB.RecordSet")
		If Request("checked")="yes" Then
			If Request("boardID")=Request("newboardID") Then
				Dvbbs.AddErrCode(40)
				Exit Sub
			ElseIf not IsNumeric(Request("newboardID")) Or Request("newboardID") = "" Then
				Dvbbs.AddErrCode(29)
				Exit Sub
			Else
				newboardID=Request("newboardID")
			End If
			'目标论坛和其上级论坛ID
			set rs=Dvbbs.Execute("select ParentStr,Board_Setting from dv_board where boardID="&newboardID)
			UpdateBoardID_1=rs(0) & "," & newboardID
			reBoard_Setting=split(rs(1),",")
			If Cint(reBoard_Setting(43))=1 Then
				Dvbbs.AddErrCode(41)
				Exit Sub
			End If
			sql="select * from dv_topic where boardID="&Dvbbs.BoardID&" and topicID="&ID
			set rs=Dvbbs.Execute(sql)
			If rs.eof and rs.bof Then
				Dvbbs.AddErrCode(32)
				Exit Sub
			Else
				If Request.form("isdispmove")="yes" Then
					newtopic=Dvbbs.CheckStr(Request.form("topic")) & "-->" & Dvbbs.MemberName & "转移"
				Else
					newtopic=Dvbbs.CheckStr(Request.form("topic"))
				End If
				If Request("leavemessage")="yes" Then
					sql="insert into dv_topic (Title,BoardID,PostUsername,PostUserID,DateAndTime,Expression,LastPost,LastPostTime,child,hits,isvote,isbest,votetotal,PostTable,GetMoney,UseTools,GetMoneyType) values ('"&newtopic&"',"&newboardID&",'"&rs("postusername")&"',"&rs("postuserID")&",'"&rs("dateandtime")&"','"&Dvbbs.CheckStr(rs("Expression"))&"','"&Dvbbs.CheckStr(rs("LastPost"))&"','"&rs("LastPosttime")&"',"&rs("child")&","&rs("hits")&","&rs("isvote")&",0,"&rs("votetotal")&",'"&Dvbbs.NowUseBBS&"',"&Rs("GetMoney")&",'"&Rs("UseTools")&"',"&Rs("GetMoneyType")&")"
					Dvbbs.Execute(sql)
				End If
			End If
			'移动后，取消专题所属
			Dvbbs.Execute("update dv_topic set mode=0 where topicID="&ID)
			If Request("leavemessage")="yes" Then
				Dvbbs.Execute("update dv_topic set locktopic=1 where topicID="&ID)
				set rs=Dvbbs.Execute("select Max(topicID) from dv_topic where boardid="&newboardID)
				newparentID=rs(0)
				sql="select * from "&TotalUseTable&" where rootID="&ID&"  and boardid<>444 and boardID <>777 order by AnnounceID"
				set rs=Dvbbs.Execute(sql)
				do while not rs.eof
					Sql="insert into "&Dvbbs.NowUseBBS&"(BoardID,ParentID,username,topic,body,DateAndTime,length,rootID,layer,orders,ip,Expression,locktopic,signflag,emailflag,isbest,postuserID,UbbList,GetMoney,UseTools,GetMoneyType,PostBuyUser) values "&_
						"("&_
						newboardID&","&rs("parentID")&",'"&_
						rs("username")&"','"&_
						Dvbbs.CheckStr(rs("topic"))&"','"&_
						Dvbbs.CheckStr(rs("body"))&"','"&_
						rs("DateAndTime")&"','"&_
						rs("length")&"',"&newParentID&","&rs("layer")&","&rs("orders")&",'"&rs("ip")&"','"&_
						rs("Expression")&"',"&rs("locktopic")&","&rs("signflag")&","&rs("emailflag")&",0,"&rs("postuserID")&",'"&rs("UbbList")&"',"&Rs("GetMoney")&",'"&Rs("UseTools")&"',"&Rs("GetMoneyType")&",'"&Dvbbs.CheckStr(Rs("PostBuyUser"))&"')"
					'response.write sql
					Dvbbs.Execute(sql)
				rs.movenext
				loop
			ElseIf Request("leavemessage")="no" Then
				If Request.form("isdispmove")="yes" Then
					newtopic=Dvbbs.CheckStr(Request.form("topic")) & "-->" & Dvbbs.MemberName & "转移"
				Else
					newtopic=Dvbbs.CheckStr(Request.form("topic"))
				End If
				'移动且不保留时自动解除精华帖子
				if rs("isbest")=1 then
					Dvbbs.Execute("update dv_topic set title='"&newtopic&"',boardid="&newboardid&",isbest=0 where topicid="&id)
					Dvbbs.Execute("update "&TotalUseTable&" set topic='"&newtopic&"',isbest=0 where announceid="&replyid)
					Dvbbs.Execute("update "&TotalUseTable&" set boardid="&newboardid&",isbest=0 where rootid="&id&" And boardid<>444 and boardID <>777")
					Dvbbs.Execute("delete from dv_besttopic where rootid="&id)
					'更新回收站或审核帖中的跟贴的原版面编号
					Dvbbs.Execute("update "&TotalUseTable&" set locktopic="&newboardid&" where rootid="&id &" and (boardid=444 OR boardID=777)")
					
				else
					Dvbbs.Execute("update dv_topic set title='"&newtopic&"',boardid="&newboardid&" where topicid="&id)
					Dvbbs.Execute("update "&TotalUseTable&" set topic='"&newtopic&"' where announceid="&replyid)
					Dvbbs.Execute("update "&TotalUseTable&" set boardid="&newboardid&" where rootid="&id &" and boardid<>444 and boardID <>777")
					'更新回收站中的跟贴的原版面编号
					Dvbbs.Execute("update "&TotalUseTable&" set locktopic="&newboardid&" where rootid="&id &" and (boardid=444 OR boardID =777)")	
				end if
				'移动时判断是否固顶并作相关处理 2004-4-25 Dvbbs.YangZheng
				If Rs("istop") > 0 Then
					Dim Yrs, TopstrinfoN, TopstrinfoO
					'读取新旧版面的固顶信息
					Set Yrs = Dvbbs.Execute("SELECT BoardTopStr From Dv_Board Where Boardid = " & Dvbbs.Boardid)
					TopstrinfoO = Yrs(0)
					Set Yrs = Dvbbs.Execute("SELECT BoardTopStr From Dv_Board Where Boardid = " & Newboardid)
					TopstrinfoN = Yrs(0)
					Yrs.Close:Set Yrs = Nothing
					'删除原固顶主题ID
					TopstrinfoO = Replace(TopstrinfoO, Cstr(Rs("TopicID"))&",", "")
					TopstrinfoO = Replace(TopstrinfoO, ","&Cstr(Rs("TopicID")), "")
					TopstrinfoO = Replace(TopstrinfoO, Cstr(Rs("TopicID")), "")
					If TopstrinfoN = "" Or Isnull(TopstrinfoN) Then
						TopstrinfoN = Cstr(Rs("TopicID"))
					ElseIf TopstrinfoN = Cstr(Rs("TopicID")) Then
						TopstrinfoN = TopstrinfoN
					ElseIf Instr(TopstrinfoN, ","&Cstr(Rs("TopicID"))) > 0 Then
						TopstrinfoN = TopstrinfoN
					Else
						TopstrinfoN = TopstrinfoN & "," & Cstr(Rs("TopicID"))
					End If
					'更新当前版面固顶信息及缓存
					Sql = "UPDATE Dv_Board SET BoardTopStr = '" & TopstrinfoO & "' WHERE BoardID = " & Dvbbs.Boardid
					Dvbbs.Execute(Sql)
					'更新新版面固顶信息及缓存
					Sql = "UPDATE Dv_Board SET BoardTopStr = '" & TopstrinfoN & "' WHERE Boardid = " & Newboardid
					Dvbbs.Execute(Sql)
					Dvbbs.LoadBoardinformation  Dvbbs.Boardid
					Dvbbs.LoadBoardinformation Newboardid
				End If
				'批量移动上传文件数据
				dim F_announceID
				F_announceID=id & "|"
				Dvbbs.Execute("update DV_Upfile set F_readme='"&newtopic&"',F_boardid="&newboardid&" where F_announceID like '"& F_announceID&"%'")
			Else
				Dvbbs.AddErrmsg "请选择相应操作。"
				exit sub
			End If
			Dim postNum,todayNum
			'计算该帖子的回复数量，用来统计对应版面帖子数
			'老迷修正，查询跟贴数字排除被删除和待审核的(2004.8.6)
			set rs=Dvbbs.Execute("select count(*) from "&TotalUseTable&" where rootID="&ID&" And BoardID <> 444 And BoardID <> 777")
			'set rs=Dvbbs.Execute("select count(*) from "&TotalUseTable&" where rootID="&ID)
			postNum=rs(0)
			'计算该帖子中今日回复的数量,8.6加入非删除条件（boardid<>444 and boardID <>777）
	
			If IsSqlDataBase=1 Then
				sql="select count(*) from "&TotalUseTable&" where rootID="&ID&" and dateandtime>='"&date()&"' and boardid<>444 and boardID <>777"
			else
				sql="select count(*) from "&TotalUseTable&" where rootID="&ID&" and dateandtime>=#"&date()&"# and boardid<>444 and boardID <>777"
			end if
			Set Rs=Dvbbs.Execute(sql)
			todayNum=rs(0)
			set rs=nothing
			'更新论坛贴子数据
			call LastCount(dvbbs.boardID)
			call BoardNumSub(dvbbs.boardID,1,postNum,todayNum)
			Dvbbs.ReloadBoardInfo(UpdateBoardID)
			UpdateBoardID=UpdateBoardID_1
			call LastCount(newboardID)
			call BoardNumAdd(newboardID,1,postNum,todayNum)
			Dvbbs.ReloadBoardInfo(UpdateBoardID)
			'更新论坛数据结束
			Insert_Forum_Log()
			Update_User_Point("")
			Topic_Manage_Sms()
			Dvbbs.Boardid = Newboardid
			Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
		Else
			Dim TempStr
			TempStr = Replace(template.html(1),"{$boardid}",Request("boardID"))
			TempStr = Replace(TempStr,"{$replyID}",Request("replyID"))
			TempStr = Replace(TempStr,"{$ID}",Request("ID"))
			TempStr = Replace(TempStr,"{$title}",Dvbbs.htmlencode(Request.form("title")))
			TempStr = Replace(TempStr,"{$content}",Dvbbs.htmlencode(Request.form("content")))
			TempStr = Replace(TempStr,"{$doWealth}",doWealth)
			TempStr = Replace(TempStr,"{$dousercp}",dousercp)
			TempStr = Replace(TempStr,"{$douserep}",douserep)
			TempStr = Replace(TempStr,"{$msg}",Request.form("msg"))
			TempStr = Replace(TempStr,"{$ismsg}",Request.form("ismsg"))
			TempStr = Replace(TempStr,"{$topic}",Server.Htmlencode(Topic))
			Response.Write TempStr
		End If
	End Sub
	'复制帖子
	Public Sub copy()
		Dim reBoard_Setting
		set rs=Dvbbs.Execute("select topic,username,postuserID from "&TotalUseTable&" where boardid="&dvbbs.boardid&" and AnnounceID="&replyID)
		If rs.eof and rs.bof Then
			Dvbbs.AddErrCode(32)
			exit sub
		Else
			Topic=rs(0)
			topicusername=rs(1)
			topicuserID=rs(2)
			If topic="" Then topic="本帖子为回复帖子"
		End If
		Set Rs=Nothing

		'判断用户是否有移动帖子权限
		If Not CanMoveTopic Then
			Dvbbs.AddErrCode(28)
			exit sub
		End If

		LogType=3
		Get_RequestInfo
	
		If Request("checked")="yes" Then
			Dim newboardID
			Dim todaynum,postnum
			set rs=Dvbbs.Execute("select count(*) from "&TotalUseTable&" where rootID="&ID)
			postNum=rs(0)
			If IsSqlDataBase=1 Then
				sql="select count(*) from "&TotalUseTable&" where rootID="&ID&" and dateandtime>'"&date()&"'"
			else
				sql="select count(*) from "&TotalUseTable&" where rootID="&ID&" and dateandtime>#"&date()&"#"
			end if
			set rs=Dvbbs.Execute(sql)
			todayNum=rs(0)
			If Request("boardID")=Request("newboardID") Then
				Dvbbs.AddErrCode(40)
				exit sub
			ElseIf not IsNumeric(Request("newboardID")) Then
				Dvbbs.AddErrCode(29)
				exit sub
			Else
				newboardID=Request("newboardID")
			End If

			'目标论坛和其上级论坛ID
			set rs=Dvbbs.Execute("select ParentStr,Board_Setting from dv_board where boardID="&newboardID)
			UpdateBoardID=rs(0) & "," & newboardID
			reBoard_Setting=split(rs(1),",")
			If Cint(reBoard_Setting(43))=1 Then
				Dvbbs.AddErrCode(41)
				exit sub
			End If
			set rs=Dvbbs.Execute("select boardID from "&TotalUseTable&" where announceID="&replyID&" and boardID="&Dvbbs.BoardID)
			If rs.eof and rs.bof Then
				Dvbbs.AddErrCode(32)
				exit sub
			End If

			Dim newtopic,trs
			set rs=Dvbbs.iCreateObject("adodb.recordset")
			sql="select * from "&TotalUseTable&" where announceID="&replyID
			rs.open sql,conn,1,1
			If Request.form("isdispmove")="yes" Then
				newtopic=Dvbbs.CheckStr(Request.form("topic")) & "-->" & Dvbbs.MemberName & "添加"
			Else
				newtopic=Dvbbs.CheckStr(Request.form("topic"))
			End If
			sql="insert into dv_topic (Title,BoardID,PostUsername,PostUserID,DateAndTime,Expression,LastPost,LastPostTime,child,hits,isvote,isbest,votetotal,PostTable) values ('"&newtopic&"',"&newboardID&",'"&rs("username")&"',"&rs("postuserID")&","&SqlNowString&",'"&Dvbbs.CheckStr(rs("Expression"))&"','"&rs("username")&"$#$"&Now()&"$$$$',"&SqlNowString&",0,0,0,0,0,'"&Dvbbs.NowUseBBS&"')"
			Dvbbs.Execute(sql)
			set trs=Dvbbs.Execute("select Max(topicID) from dv_topic where boardid="&newboardID&" and postuserID="&rs("postuserID"))
			Sql="insert into "&Dvbbs.NowUseBBS&"(BoardID,ParentID,username,topic,body,DateAndTime,length,rootID,layer,orders,ip,Expression,locktopic,signflag,emailflag,isbest,postuserID,UbbList) values "&_
				"("&_
				newboardID&",0,'"&_
				rs("username")&"','"&_
				newtopic&"','"&_
				Dvbbs.CheckStr(rs("body"))&"','"&_
				rs("DateAndTime")&"','"&_
				rs("length")&"',"&trs(0)&",1,0,'"&rs("ip")&"','"&_
				rs("Expression")&"',"&rs("locktopic")&","&rs("signflag")&","&rs("emailflag")&",0,"&rs("postuserID")&",'"&rs("UbbList")&"')"
			Dvbbs.Execute(sql)
			rs.close
			set rs=nothing
			'移动上传文件数据
			Dim F_announceID
			F_announceID=ID & "|" &replyID
			Dvbbs.Execute("update DV_Upfile set F_readme='"&newtopic&"',F_boardid="&newboardid&" where F_announceID = '"& F_announceID&"'")
			'更新论坛贴子数据
			call LastCount(NewboardID)
			call BoardNumAdd(newboardID,1,postNum,todayNum)
			call AllboardNumAdd(todayNum,postNum,1)
			Dvbbs.ReloadBoardInfo(UpdateBoardID)
			Insert_Forum_Log()
			Update_User_Point("")
			Topic_Manage_Sms()
			Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
		Else
			Dim TempStr
			TempStr = Replace(template.html(2),"{$boardid}",Request("boardID"))
			TempStr = Replace(TempStr,"{$replyID}",Request("replyID"))
			TempStr = Replace(TempStr,"{$ID}",Request("ID"))
			TempStr = Replace(TempStr,"{$title}",Dvbbs.htmlencode(Request.form("title")))
			TempStr = Replace(TempStr,"{$content}",Dvbbs.htmlencode(Request.form("content")))
			TempStr = Replace(TempStr,"{$doWealth}",doWealth)
			TempStr = Replace(TempStr,"{$dousercp}",dousercp)
			TempStr = Replace(TempStr,"{$douserep}",douserep)
			TempStr = Replace(TempStr,"{$msg}",Request.form("msg"))
			TempStr = Replace(TempStr,"{$ismsg}",Request.form("ismsg"))
			TempStr = Replace(TempStr,"{$topic}",Server.Htmlencode(Topic))
			TempStr = Replace(TempStr,"{$BoardJumpList}","<select name=newboardID id=newboard size=1></select>")
			Response.Write TempStr
			Response.Write "<script language=""javascript"">BoardJumpListSelect("&Dvbbs.Boardid&",""newboard"","""",""移动帖子请选择"",1);</script>"
		End If
	End Sub

	Private Function SysObjFso()
		Dim xTestObj
		SysObjFso = False
		On Error Resume Next
		Set xTestObj = Dvbbs.iCreateObject("Scripting.FileSystemObject")
		If Err = 0 Then SysObjFso = True
		Set xTestObj = Nothing
		Err = 0
	End Function

	Private Sub Delupfiles(F_BoardID,F_announceID)
		Dim DelSql,DelRs,Filepath,ViewFilepath,objFSO,path
		If Dvbbs.Forum_Setting(76)="" Or Dvbbs.Forum_Setting(76)="0" Then Dvbbs.Forum_Setting(76)="UploadFile/"
		If right(Dvbbs.Forum_Setting(76),1)<>"/" Then Dvbbs.Forum_Setting(76)=Dvbbs.Forum_Setting(76)&"/"
		path=Dvbbs.Forum_Setting(76)
		Err=0
		On Error Resume Next
		Set objFSO = Dvbbs.iCreateObject("Scripting.FileSystemObject")
		DelSql="Select F_Filename,F_Viewname,F_ID From Dv_Upfile Where F_BoardID="&F_BoardID&" And F_AnnounceID LIKE '"&F_announceID&"%' And F_Flag=0"
		Set DelRs=Dvbbs.Execute(DelSql)
		Do While Not DelRs.Eof
		Filepath = path&DelRs(0)
		ViewFilepath = DelRs(1)
		If Err <> 0 Then
			Dvbbs.Execute("update Dv_Upfile Set F_flag=4 Where F_BoardID="&F_BoardID&" And F_AnnounceID LIKE '"&F_announceID&"%'")
			Exit Sub
		Else
			If objFSO.FileExists(Server.MapPath(Filepath)) Then
				objFSO.DeleteFile(Server.MapPath(Filepath))
			End If
			IF NOT IsNull(ViewFilepath) and ViewFilepath<>"" Then
				ViewFilepath=Replace(ViewFilepath,"..","")
				If objFSO.FileExists(Server.MapPath(ViewFilepath)) Then
					objFSO.DeleteFile(Server.MapPath(ViewFilepath))
				End If
			End IF
			Dvbbs.Execute("Delete from Dv_Upfile Where F_ID="&DelRs(2))
		End If
		DelRs.MoveNext
		Loop
		DelRs.close:Set DelRs=Nothing
		Set objFSO=Nothing
	End Sub

	'---------------------------------------------------
	'斑主奖惩帖子
	'---------------------------------------------------
	Private Function ChkRewardMoney()
		Dim CanRewardMoney
		CanRewardMoney = False
		If (Dvbbs.Master or Dvbbs.superboardmaster or Dvbbs.boardmaster) and Cint(Dvbbs.GroupSetting(22))=1 Then CanRewardMoney=True
		If Cint(Dvbbs.GroupSetting(22))=1 and Dvbbs.UserGroupID>3 Then CanRewardMoney=True
		If Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(22))=1 Then
			CanRewardMoney=True
		ElseIf Dvbbs.FoundUserPer and Cint(Dvbbs.GroupSetting(22))=0 Then
			CanRewardMoney=False
		End If
		If Not Dvbbs.Master Then
			If Not (Clng(Dvbbs.UserToday(4)) < Clng(Dvbbs.Forum_Setting(97))) Then CanRewardMoney = False
		End If
		ChkRewardMoney = CanRewardMoney
		If Not CanRewardMoney Then Dvbbs.AddErrCode(28) : Dvbbs.ShowErr()
	End Function

	Private Sub RewardMoney
		Dim CanRewardMoney,GiveMoney
		Dim ReAct,UpIsagree,UpGetMoney,TempString
		GiveMoney = Request.FORM("GiveMoney")
		LogType = 5
		Get_RequestInfo
			'If GiveMoney = 0 Then Dvbbs.AddErrCode(35) : Exit Sub
			If Not ChkRewardMoney Then Dvbbs.AddErrCode(28) : Exit Sub
			If Not IsNumeric(GiveMoney) Then
				GiveMoney = 0
			Else
				GiveMoney = Clng(GiveMoney)
			End If
			
			If GiveMoney<0 Then
				ReAct=1
				GiveMoney = Abs(GiveMoney)
			Else
				ReAct=0
				
			End If
			If not Dvbbs.Master and Clng(Dvbbs.UserToday(4))>Clng(Dvbbs.Forum_Setting(97)) Then Dvbbs.AddErrCode(28) : Exit Sub
			Set Rs = Dvbbs.Execute("Select topic,username,postuserID,Isagree,GetMoney From "&TotalUseTable&" Where boardid="&Dvbbs.boardid&" and AnnounceID="&ReplyID)
			If Rs.Eof And Rs.Bof Then
				Dvbbs.AddErrCode(32)
				Exit Sub
			End If
			Topic = Rs(0)
			TopicUsername = Rs(1)
			TopicUserID = Clng(Rs(2))
			TempString = Rs(3)	'Isagree
			UpGetMoney = Clng(Rs(4))
			Rs.close
			If TopicUserID=Dvbbs.UserID Then Dvbbs.AddErrCode(38) : Exit Sub
			'更新斑主每日奖励金币数
			If not Dvbbs.Master and ReAct = 0 Then
				Dvbbs.UserToday(4) = Clng(Dvbbs.UserToday(4)) + GiveMoney
				Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usertoday").text = Clng(Dvbbs.UserToday(0)) &"|"& Clng(Dvbbs.UserToday(1)) &"|"& Clng(Dvbbs.UserToday(2)) &"|"& Clng(Dvbbs.UserToday(3)) &"|"& Clng(Dvbbs.UserToday(4))
				Sql = "Update Dv_user Set UserToday='"&Dvbbs.CheckStr(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usertoday").text)&"' where UserID="&Dvbbs.UserID
				Dvbbs.Execute Sql
			End If

			'Isagree字段，定义为：扣金币数|加金币数 ,GetMoney中为得到的总数  
			'Isagree : 扣金币数|加金币数|惩罚原因|好评原因|中立人数|支持人数|反对人数
			If TempString="" or Instr(TempString&"","|")=0 Then
				If ReAct = 1 Then
					UpIsagree = GiveMoney&"|0|"&Replace(content,"|","")&"||0|0|0|"
				Else
					UpIsagree = "0|"&GiveMoney&"||"&Replace(content,"|","")&"|0|0|0"
				End If
			Else
				If Ubound(Split(TempString,"|"))<6 Then
					TempString = TempString &"|||0|0|0"
				End If
				TempString = Split(TempString,"|")
				'修正斑竹奖励金币计算,dv.linzi'
				Dim temmoney'定义实际值
				If TempString(0)>0 Then '数据为扣除的
					If ReAct = 1 Then'当前扣除
						temmoney=TempString(0) + GiveMoney
						TempString(0) = temmoney
						TempString(2) = Replace(content,"|","")
					Else'当前奖励
						temmoney = GiveMoney-TempString(0)
						If temmoney<0 Then'数据库继续显示扣除
							TempString(0) = abs(temmoney)
							TempString(2) = Replace(content,"|","")
						Else '数据库显示奖励
							TempString(0) = 0
							TempString(1) = temmoney
							TempString(2) =""'删除原说明
							TempString(3) = Replace(content,"|","")
						End If 
						TempString(3) = Replace(content,"|","")
					End If
				Else '数据库为奖励
					If ReAct = 1 Then'当前是处罚
						temmoney = TempString(1) - GiveMoney
						If temmoney<0 Then'当前为惩罚
							TempString(0)=abs(temmoney):TempString(1)=0
							TempString(2) = Replace(content,"|","")
							TempString(3) = ""'清除
						Else'继续奖励
							TempString(0) = 0
							TempString(1) = temmoney
							TempString(2) = ""
							TempString(3) = Replace(content,"|","")	
						End If 
					Else'继续奖励
						temmoney=GiveMoney+TempString(0)
						TempString(1) =temmoney
						TempString(3) = Replace(content,"|","")
					End If
				End If 

				UpIsagree = join(TempString,"|")
			End If
			If ReAct = 1 Then
				UpGetMoney = UpGetMoney - GiveMoney
				Sql = "Update Dv_user Set UserMoney=UserMoney-"&GiveMoney&" where UserID="&TopicUserID
				sucmsg = sucmsg&",对用户："&TopicUsername&"扣除"&GiveMoney&"个金币!"
			Else
				UpGetMoney = UpGetMoney + GiveMoney
				Sql = "Update Dv_user Set UserMoney=UserMoney+"&GiveMoney&" where UserID="&TopicUserID
				sucmsg = sucmsg&",对用户："&TopicUsername&"奖励"&GiveMoney&"个金币!"
			End If
			Dvbbs.Execute Sql
			Sql = "Update "&TotalUseTable&" Set Isagree='"&UpIsagree&"' Where boardid="&Dvbbs.boardid&" and AnnounceID="&ReplyID
			Dvbbs.Execute Sql
			Update_User_Point("")
			Topic_Manage_Sms()
			Insert_Forum_Log()
			Dvbbs.Dvbbs_Suc(SucMsgInfo(sucmsg))
	End Sub

	Function reUBBCode(strContent)
		Dim re
		Set re=new RegExp
		re.IgnoreCase =True
		re.Global=True
		strContent=replace(strContent,"&nbsp;"," ")
		re.Pattern="(\[QUOTE\])(.*)(\[\/QUOTE\])"
		strContent=re.Replace(strContent,"$2")
		re.Pattern="(\[point=*([0-9]*)\])(.*)(\[\/point\])"
		strContent=re.Replace(strContent,"&nbsp;")
		re.Pattern="(\[post=*([0-9]*)\])(.*)(\[\/post\])"
		strContent=re.Replace(strContent,"&nbsp;")
		re.Pattern="(\[power=*([0-9]*)\])(.*)(\[\/power\])"
		strContent=re.Replace(strContent,"&nbsp;")
		re.Pattern="(\[usercp=*([0-9]*)\])(.*)(\[\/usercp\])"
		strContent=re.Replace(strContent,"&nbsp;")
		re.Pattern="(\[money=*([0-9]*)\])(.*)(\[\/money\])"
		strContent=re.Replace(strContent,"&nbsp;")
		re.Pattern="(\[replyview\])(.*)(\[\/replyview\])"
		strContent=re.Replace(strContent,"&nbsp;")
		re.Pattern="(\[usemoney=*([0-9]*)\])(.*)(\[\/usemoney\])"
		strContent=re.Replace(strContent,"&nbsp;")
		re.Pattern="\[username=(.[^\[]*)\](.[^\[]*)\[\/username\]"
		strContent=re.Replace(strContent,"&nbsp;")
		strContent=replace(strContent,"<I></I>","")
		set re=Nothing
		reUBBCode=strContent
	End Function
	'截取指定字符
	Function cutStr(str,strlen)
		'去掉所有HTML标记
		Dim re
		Set re=new RegExp
		re.IgnoreCase =True
		re.Global=True
		re.Pattern="<(.[^>]*)>"
		str=re.Replace(str,"")	
		set re=Nothing
		Dim l,t,c,i
		l=Len(str)
		t=0
		For i=1 to l
			c=Abs(Asc(Mid(str,i,1)))
			If c>255 Then
				t=t+2
			Else
				t=t+1
			End If
			If t>=strlen Then
				cutStr=left(str,i)&"..."
				Exit For
			Else
				cutStr=str
			End If
		Next
		str = dvHTMLEncode(str)
		cutStr=Replace(cutStr,chr(10),"")
	End Function
	Function dvHTMLEncode(fString)
		If Not IsNull(fString) Then
			fString = replace(fString, ">", "&gt;")
			fString = replace(fString, "<", "&lt;")
			fString = replace(fString, "&#", "<I>&#</I>")
			fString = Replace(fString, CHR(32), "<I></I>&nbsp;")
			fString = Replace(fString, CHR(9), "&nbsp;")
			fString = Replace(fString, CHR(34), "&quot;")
			fString = Replace(fString, CHR(39), "&#39;")
			fString = Replace(fString, CHR(13), "")
			fString = Replace(fString, CHR(10) & CHR(10), "</P><P> ")
			fString = Replace(fString, CHR(10), "<br> ")
			fString=Dvbbs.ChkBadWords(fString)
			dvHTMLEncode = fString
		End If
	End Function

	'返回该用户名的可管理的版块ID 以“,”分隔。
	Private Function GetBoardMsterID(username)
		Dim Srs,Board_Data,i,TempData
		TempData = ""
		Set Srs=Dvbbs.Execute("Select Boardid,Boardmaster,Child From Dv_Board Where Boardmaster<>'' Order By Rootid,Orders")
		If not Srs.Eof Then
			Board_Data=Srs.GetRows(-1)
		End if
		Srs.Close:Set Srs=Nothing
		If IsArray(Board_Data) Then
			For i=0 to Ubound(Board_Data,2)
				If Instr(","&TempData&",",","&Board_Data(0,i)&",")=0 Then
					If instr("|" & Trim(Board_Data(1,i)) & "|","|" & username & "|")>0 Then
						TempData = TempData & Board_Data(0,i) &","
						If Cint(Board_Data(2,i))>0 Then
							TempData = TempData & GetBoardID(Board_Data(0,i))
						End If
					End If
				End If
			Next
			If Right(TempData,1)="," Then
				TempData = Left(TempData,Len(TempData)-1)
			End If
		End If
		GetBoardMsterID=TempData
	End Function
	'获取下属版块ID
	Private Function GetBoardID(BoardIDVal)
		Dim TempData,Nodelist,Node
		Set Nodelist = Application(Dvbbs.CacheName&"_boardlist").cloneNode(True).documentElement.getElementsByTagName("board")
		For Each Node in Nodelist
			If Instr(","&Node.attributes.getNamedItem("parentstr").text&",",","&BoardIDVal&",")>0 Then
				TempData = TempData & Node.attributes.getNamedItem("boardid").text &","
			End If
		Next
		GetBoardID = TempData
	End Function
End Class
%>