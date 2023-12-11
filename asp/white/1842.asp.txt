<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/dv_clsother.asp"-->
<!--#include file="inc/dv_ubbcode.asp"-->
<!--#include file="inc/ubblist.asp"-->
<%
Dim RssDataMode,rsbody

RssDataMode = "0"'0为不取帖子内容，1为取帖子内容，取帖子内容较为消耗资源
'用参数控制

Dim XMLDOM,node,Cnode,Cnode1,msginfo
Set XMLDOM=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument")
XMLDOM.appendChild(XMLDOM.createElement("rss"))
XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"version","")).text="2.0"
Set node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"channel",""))
RssDataMode=Request("RssDataMode")
If RssDataMode="" Then RssDataMode="0"
Dim dv_ubb,replyid_a
Dim EmotPath,board_Setting
If RssDataMode<>"0" Then 
	Dvbbs.LoadTemplates("")
	Set dv_ubb=new Dvbbs_UbbCode
	dv_ubb.posttype=1	
	EmotPath=Dvbbs.Get_ScriptNameUrl & Split(Dvbbs.Forum_emot,"|||")(0)		
End If
Dim Rs,Sql,i,RssTitle,RssID
Dim RssHomePageUrl
RssHomePageUrl = Dvbbs.Get_ScriptNameUrl
RssID = Request("RssID")
If RssID="" Or Not IsNumeric(RssID) Then RssID = 0
RssID = Clng(RssID)

Sql = "Select Top 20 TopicID,Title,PostUserName,PostUserID,DateAndTime,BoardID,PostTable,GetMoneyType,HideName "
Select Case RssID
Case 1
	RssTitle = "最新20篇论坛主题"
	Sql = Sql & " From Dv_Topic Where Boardid <> 444 and BoardID <> 777 Order By DateAndTime Desc"
Case 2
	RssTitle = "最新20篇论坛精华"
	If Dvbbs.BoardID=0 Then 
		Sql = Sql & " From Dv_Topic Where IsBest=1 And Boardid <> 444 and BoardID <> 777 Order By DateAndTime Desc"
	Else
		Sql = Sql & " From Dv_Topic Where BoardID="&Dvbbs.BoardID&" and IsBest=1 And Boardid <> 444 and BoardID <> 777 Order By DateAndTime Desc"
	End If
Case 3
	RssTitle = "今日热门主题"
	If IsSqlDataBase = 1 Then
		Sql = Sql & " From Dv_Topic Where DateDiff(d,DateAndTime,"&SqlNowString&")=0 and Boardid <> 444 and BoardID <> 777 Order By Hits Desc"
	Else
		Sql = Sql & " From Dv_Topic Where DateDiff('d',DateAndTime,"&SqlNowString&")=0 And Boardid <> 444 and BoardID <> 777 Order By Hits Desc"
	End If
Case 4
	If Dvbbs.BoardID = 0 Then
		RssTitle = "最新20篇论坛主题"
		Sql = Sql & " From Dv_Topic where Boardid <> 444 and BoardID <> 777 Order By DateAndTime Desc"
	Else
		RssTitle = Dvbbs.BoardType & "最新20篇论坛主题"
		Sql = Sql & " From Dv_Topic Where BoardID="&Dvbbs.BoardID&" Order By DateAndTime Desc"
	End If
Case 5
	RssTitle = "最新20篇论坛精华"
	If Dvbbs.BoardID=0 Then 
		Sql = Sql & " From Dv_Topic Where IsBest=1 And Boardid <> 444 and BoardID <> 777 Order By DateAndTime Desc"
	Else
		Sql = Sql & " From Dv_Topic Where BoardID="&Dvbbs.BoardID&" and IsBest=1 And Boardid <> 444 and BoardID <> 777 Order By DateAndTime Desc"
	End If
Case 6
	If Dvbbs.BoardID = 0 Then
		RssTitle = "今日热门主题"
		If IsSqlDataBase = 1 Then
			Sql = Sql & " From Dv_Topic Where DateDiff(d,DateAndTime,"&SqlNowString&")=0  and Boardid <> 444 and BoardID <> 777 Order By Hits Desc"
		Else
			Sql = Sql & " From Dv_Topic Where DateDiff('d',DateAndTime,"&SqlNowString&")=0 and Boardid <> 444 and BoardID <> 777 Order By Hits Desc"
		End If
	Else
		RssTitle = Dvbbs.BoardType & "今日热门主题"
		If IsSqlDataBase = 1 Then
			Sql = Sql & " From Dv_Topic Where BoardID="&Dvbbs.BoardID&" And  DateDiff(d,DateAndTime,"&SqlNowString&")=0  and Boardid <> 444 and BoardID <> 777 Order By Hits Desc"
		Else
			Sql = Sql & " From Dv_Topic Where BoardID="&Dvbbs.BoardID&" And  DateDiff('d',DateAndTime,"&SqlNowString&")=0  and Boardid <> 444 and BoardID <> 777 Order By Hits Desc"
		End If
	End If
Case 7
	If Dvbbs.UserID = 0 Then
		RssTitle = "错误信息"
	Else
		RssTitle = "收取论坛短信"
	End If
Case 8
Case 9
Case Else
	RssTitle = "获取频道列表"
End Select
If RssDataMode<>"0" Then RssTitle =RssTitle &"-全文"
node.appendChild(XMLDOM.createNode(1,"title","")).text=Dvbbs.Forum_Info(0)&"--"&RssTitle
node.appendChild(XMLDOM.createNode(1,"link","")).text=Dvbbs.Forum_info(1)
node.appendChild(XMLDOM.createNode(1,"language","")).text="zh-cn"
node.appendChild(XMLDOM.createNode(1,"description","")).text=Dvbbs.Forum_Info(0)
node.appendChild(XMLDOM.createNode(1,"copyright","")).text=Dvbbs.Forum_info(3)
node.appendChild(XMLDOM.createNode(1,"generator","")).text="Rss Generator By Dvbbs.Net"
node.appendChild(XMLDOM.createNode(1,"webMaster","")).text=Dvbbs.Forum_info(5)
Set Cnode = node.appendChild(XMLDOM.createNode(1,"image",""))
Cnode.appendChild(XMLDOM.createNode(1,"url","")).text = Dvbbs.Forum_Info(6)
Cnode.appendChild(XMLDOM.createNode(1,"title","")).text = Dvbbs.Forum_Info(0)
Select Case RssID
Case 0
	Set Cnode=node.appendChild(XMLDOM.createNode(1,"item",""))
	Cnode.appendChild(XMLDOM.createNode(1,"title","")).text=Dvbbs.Forum_Info(0)&"--频道列表"
	Cnode.appendChild(XMLDOM.createNode(1,"link","")).text=Dvbbs.Forum_info(1)
	Cnode.appendChild(XMLDOM.createNode(1,"author","")).text=Dvbbs.Forum_info(0)
	Cnode.appendChild(XMLDOM.createNode(1,"pubDate","")).text=Now()
	Set Cnode1=Cnode.appendChild(XMLDOM.createNode(1,"description",""))
	msginfo= "<b>最新20篇论坛主题</b>：<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=1"">"&RssHomePageUrl&"RssFeed.asp?RssID=1</a>"
	msginfo=msginfo& "<br />"
	msginfo=msginfo& "<b>最新20篇论坛主题-全文</b>：<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=1&RssDataMode=1"">"&RssHomePageUrl&"RssFeed.asp?RssID=1&RssDataMode=1</a>"
	msginfo=msginfo& "<br />"
	msginfo=msginfo& "<b>最新20篇论坛精华</b>：<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=2"">"&RssHomePageUrl&"RssFeed.asp?RssID=2</a>"
	msginfo=msginfo& "<br />"
	msginfo=msginfo& "<b>最新20篇论坛精华-全文</b>：<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=2&RssDataMode=1"">"&RssHomePageUrl&"RssFeed.asp?RssID=2&RssDataMode=1</a>"
	msginfo=msginfo& "<br />"
	msginfo=msginfo& "<b>今日热门主题</b>：<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=3"">"&RssHomePageUrl&"RssFeed.asp?RssID=3</a>"
	msginfo=msginfo& "<br />"
	msginfo=msginfo& "<b>今日热门主题-全文</b>：<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=3&RssDataMode=1"">"&RssHomePageUrl&"RssFeed.asp?RssID=3&RssDataMode=1</a>"
	msginfo=msginfo& "<br />"
	msginfo=msginfo& "<b>版面信息订阅，点击相关字样查看连接</b>："
	msginfo=msginfo& "<br />"
	Dim bnode
	For each bnode in Application(Dvbbs.CacheName&"_boardlist").documentElement.selectNodes("board")
		msginfo=msginfo& BNode.attributes.getNamedItem("boardtype").text & "的 <a href="""&RssHomePageUrl&"RssFeed.asp?RssID=4&BoardID="&BNode.attributes.getNamedItem("boardid").text&""">最新主题</a>、<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=6&BoardID="&BNode.attributes.getNamedItem("boardid").text&""">今日热门</a>、<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=2&BoardID="&BNode.attributes.getNamedItem("boardid").text&""">最新精华</a>"
		msginfo=msginfo& "<br />"
		msginfo=msginfo& BNode.attributes.getNamedItem("boardtype").text & "的 <a href="""&RssHomePageUrl&"RssFeed.asp?RssID=4&RssDataMode=1&BoardID="&BNode.attributes.getNamedItem("boardid").text&""">最新主题-全文</a>、<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=6&RssDataMode=1&BoardID="&BNode.attributes.getNamedItem("boardid").text&""">今日热门-全文</a>、<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=2&RssDataMode=1&BoardID="&BNode.attributes.getNamedItem("boardid").text&""">最新精华-全文</a>"
		msginfo=msginfo& "<br />"
	Next
	msginfo=msginfo& "<b>收取论坛短信</b>：<a href="""&RssHomePageUrl&"RssFeed.asp?RssID=7"">"&RssHomePageUrl&"RssFeed.asp?RssID=7</a>"
	msginfo=msginfo& "<br />"
	Cnode1.appendChild(XMLDOM.createCDATASection(replace(msginfo,"]]>","]]&gt;")))
Case 7
Case Else
	Set Rs=Dvbbs.Execute(Sql)
	If Rs.Eof And Rs.Bof Then
		Set Cnode=node.appendChild(XMLDOM.createNode(1,"item",""))
		Cnode.appendChild(XMLDOM.createNode(1,"title","")).text="今日没有更新信息"
		Cnode.appendChild(XMLDOM.createNode(1,"link","")).text=Dvbbs.Forum_info(1)
		Cnode.appendChild(XMLDOM.createNode(1,"author","")).text=Dvbbs.Forum_info(0)
		Cnode.appendChild(XMLDOM.createNode(1,"pubDate","")).text=Now()
		Set Cnode1=Cnode.appendChild(XMLDOM.createNode(1,"description",""))
		msginfo= "今日没有更新信息！"
		Cnode1.appendChild(XMLDOM.createCDATASection(replace(msginfo,"]]>","]]&gt;")))
	Else
		Do While Not Rs.Eof
			Set Cnode=node.appendChild(XMLDOM.createNode(1,"item",""))
			Cnode.appendChild(XMLDOM.createNode(1,"title","")).text=Rs(1)&""
			If RssID = 5 Then
				Cnode.appendChild(XMLDOM.createNode(1,"link","")).text=RssHomePageUrl&"dispbbs.asp?BoardID="&Rs(5)&"&ID="&Rs(0)&"&Page=1&replyID="&Rs(6)&"&skin=1"
			Else
				Cnode.appendChild(XMLDOM.createNode(1,"link","")).text=RssHomePageUrl&"dispbbs.asp?BoardID="&Rs(5)&"&ID="&Rs(0)&"&Page=1"
			End If
			If Dvbbs.Boardid <>0 Then
				If Rs(8)=1 And Dvbbs.Board_Setting(68)="1" And Not Dvbbs.Boardmaster Then
					Cnode.appendChild(XMLDOM.createNode(1,"author","")).text="匿名用户"
				Else
					Cnode.appendChild(XMLDOM.createNode(1,"author","")).text=Rs(2)&""
				End If
			Else
				If Rs(8)=1 and Not(Dvbbs.Master Or Dvbbs.Superboardmaster) Then
					If Board_Setting68(Rs(5))=1 Then
							Cnode.appendChild(XMLDOM.createNode(1,"author","")).text="匿名用户"
					Else
							Cnode.appendChild(XMLDOM.createNode(1,"author","")).text=Rs(2)&""
					End If
				Else
					Cnode.appendChild(XMLDOM.createNode(1,"author","")).text=Rs(2)&""	
				End If
			End If
			Cnode.appendChild(XMLDOM.createNode(1,"pubDate","")).text=Rs(4)&""
			Set Cnode1=Cnode.appendChild(XMLDOM.createNode(1,"description",""))
			
			If RssDataMode="0" Then 
				msginfo=  "要浏览本条信息请点击标题。"
			Else
				If Rs("GetMoneyType")=3 Then
					msginfo = "本贴子内容经过特殊加密，请到论坛直接查看"
				Else
					Set rsbody=Dvbbs.Execute("Select top 1 t.body,t.ubblist,u.LockUser,U.UserGroupID,t.isbest,t.BoardID From "&Rs("posttable")&" t Inner Join [dv_user] U On T.postuserid=u.userid Where  RootID="&Rs(0)&"  And t.BoardID<>444 and t.BoardID <>777 Order by AnnounceID asc")
					If RsBody.EOF Then
						msginfo = "数据错误或丢失。"
					Else
						If Dvbbs.BoardID<>0 Then 
							If Rsbody(2)=0 Then
								If Rsbody(4)=0 Or Dvbbs.GroupSetting(41)="1" Then 
								Ubblists=RSbody(1)&""
									msginfo= dv_ubb.Dv_UbbCode(Rsbody(0),Rsbody(3),2,0)
								Else
									msginfo = "精华贴内容需要有权限才可以浏览"		
								End If
							Else
								msginfo = "此用户已经被锁定,或屏蔽,不显示发言内容"		
							End If
						Else
							If GetSetting(Rsbody(5)) Then 
								If Rsbody(2)=0 Then
									If Rsbody(4)=0 Or Dvbbs.GroupSetting(41)="1" Then 
										Ubblists=RSbody(1)&""
										msginfo = dv_ubb.Dv_UbbCode(Rsbody(0),Rsbody(3),2,0)
									Else
										msginfo = "精华贴内容需要有权限才可以浏览"		
									End If
								Else
									msginfo = "此用户已经被锁定,或屏蔽,不显示发言内容"		
								End If
							Else
								msginfo = "您没有查看内容的权限。"		
							End If
						
						End If			
					End If	
				End If
			End If
			Cnode1.appendChild(XMLDOM.createCDATASection(replace(msginfo,"]]>","]]&gt;")))
		Rs.MoveNext
		Loop
		
	End If
	Rs.Close
	Set Rs=Nothing
End Select
Dvbbs.PageEnd()
Function Board_Setting68(bid)
	Dim board_Setting
	board_Setting = Split(Application(CacheName &"_boarddata_" & bid).documentElement.selectSingleNode("boarddata/@board_setting").text,",")
	Board_Setting68=board_Setting(68)
End Function

Sub TransNode(XmlDoc)
	'XSLT模板转换开始
	Dim Xmlskin,Proc,XmlStyle
	Set Xmlskin = Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	If Not (Xmlskin.load(Server.MapPath("inc/Templates/rss.xslt"))) Then
		Response.Write "模板数据出错，请与管理员联系！"
		Response.End
	End If
	Set XMLStyle=Dvbbs.iCreateObject("msxml2.XSLTemplate" & MsxmlVersion)
	XMLStyle.stylesheet=Xmlskin
	Set Proc=XMLStyle.createProcessor()
	Proc.input = XmlDoc
  	proc.transform()
  	Response.Write proc.output
	Set XmlStyle = Nothing
	Set Xmlskin = Nothing
End Sub


Sub ShowXML()
	Response.Clear
	Response.CharSet="gb2312"  '数据集
	Response.ContentType="text/xml"  '数据流格式定义
	Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"&vbNewLine
	Response.Write "<?xml-stylesheet type=""text/xsl"" href=""inc/Templates/rss.xslt"" ?>"&vbNewLine
	Response.Write XMLDOM.xml
	Set XMLDOM=Nothing
End Sub

Sub ShowHtml()
	Response.Clear
	Response.CharSet="gb2312"  '数据集
	TransNode(XMLDOM)
	Set XMLDOM=Nothing
End Sub

If RssDataMode<>"0" Then
	Set dv_ubb=Nothing
End If
If Request.QueryString("html")="1" Then
	ShowHtml()
Else
	ShowXML()
End If
Function GetSetting(BoardID)
	GetSetting=True
	Dim Node
	Dim Rs,IsGroupSetting
	If Not IsObject(Application(dvbbs.CacheName &"_boarddata_" & boardid)) Then Dvbbs.LoadBoardData boardid
	board_Setting=split(Application(Dvbbs.CacheName &"_boarddata_" & boardid).documentElement.selectSingleNode("boarddata/@board_setting").text,",")
	IsGroupSetting=Application(Dvbbs.CacheName &"_boarddata_" & boardid).documentElement.selectSingleNode("boarddata/@isgroupsetting").text
	BoardUser=split(Application(Dvbbs.CacheName &"_boarddata_" & boardid).documentElement.selectSingleNode("boarddata/@boarduser").text,",")
	If IsGroupSetting<>""  Then
		IsGroupSetting = "," & IsGroupSetting & ","
		If InStr(IsGroupSetting,"," & Dvbbs.UserGroupID & ",")>0 Then
			Set Rs=Dvbbs.Execute("Select PSetting From Dv_BoardPermission Where Boardid="&Dvbbs.Boardid&" And GroupID="&Dvbbs.UserGroupID)
			If Not (Rs.Eof And Rs.Bof) Then
				GroupSetting = Split(Rs(0),",")
			End If
			Set Rs=Nothing
		End If
		If Dvbbs.UserID>0 And InStr(IsGroupSetting,",0,")>0 Then
			Set Rs=Dvbbs.execute("Select Uc_Setting From Dv_UserAccess Where Uc_Boardid="&Dvbbs.BoardID&" And uc_UserID="&Dvbbs.Userid)
			If Not(Rs.Eof And Rs.Bof) Then
				Dvbbs.UserPermission=Split(Rs(0),",")
				Dvbbs.GroupSetting = Split(Rs(0),",")
				Dvbbs.FoundUserPer=True
			End If
			Set Rs=Nothing
		End If
	End If
	If Board_Setting(1)="1" And Dvbbs.GroupSetting(37)="0" Then
		GetSetting=False
		Exit Function
	End If
	If Dvbbs.GroupSetting(0)="0"  Then Dvbbs.AddErrCode(27)
	'访问论坛限制(包括文章、积分、金钱、魅力、威望、精华、被删数、注册时间)
	Dim BoardUserLimited
	BoardUserLimited = Split(Board_Setting(54),"|")
	If Ubound(BoardUserLimited)=8 Then
		'文章
		If Trim(BoardUserLimited(0))<>"0" And IsNumeric(BoardUserLimited(0)) Then
			If Dvbbs.UserID = 0 Then 
				GetSetting=False
				Exit Function
			End If
			If Clng(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userpost").text)<Clng(BoardUserLimited(0)) Then
				GetSetting=False
				Exit Function
			End If
		End If
		'积分
		If Trim(BoardUserLimited(1))<>"0" And IsNumeric(BoardUserLimited(1)) Then
			If Dvbbs.UserID = 0 Then
				GetSetting=False
				Exit Function
			End If
			If Clng(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userep").text)<Clng(BoardUserLimited(1)) Then
				GetSetting=False
				Exit Function
			End If
		End If
		'金钱
		If Trim(BoardUserLimited(2))<>"0" And IsNumeric(BoardUserLimited(2)) Then
			If Dvbbs.UserID = 0 Then 
				GetSetting=False
				Exit Function
			End If
			If Clng(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userwealth").text)<Clng(BoardUserLimited(2)) Then
				GetSetting=False
				Exit Function
			End If
		End If
		'魅力
		If Trim(BoardUserLimited(3))<>"0" And IsNumeric(BoardUserLimited(3)) Then
			If Dvbbs.UserID = 0 Then
				GetSetting=False
				Exit Function
			End If
			If Clng(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usercp").text)<Clng(BoardUserLimited(3)) Then
				GetSetting=False
				Exit Function
			End If
		End If
		'威望
		If Trim(BoardUserLimited(4))<>"0" And IsNumeric(BoardUserLimited(4)) Then
			If Dvbbs.UserID = 0 Then 
				GetSetting=False
				Exit Function
			End If
			If Clng(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userpower").text)<Clng(BoardUserLimited(4)) Then
				GetSetting=False
				Exit Function
			End If
		End If
		'精华
		If Trim(BoardUserLimited(5))<>"0" And IsNumeric(BoardUserLimited(5)) Then
			If Dvbbs.UserID = 0 Then
				GetSetting=False
				Exit Function
			End If
			If Clng(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userisbest").text)<Clng(BoardUserLimited(5)) Then
				GetSetting=False
				Exit Function
			End If
		End If
		'删贴
		If Trim(BoardUserLimited(6))<>"0" And IsNumeric(BoardUserLimited(6)) Then
			If Dvbbs.UserID = 0 Then
				GetSetting=False
				Exit Function
			End If
			If Clng(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userdel").text)>Clng(BoardUserLimited(6)) Then
				GetSetting=False
				Exit Function
			End If
		End If
		'注册时间
		If Trim(BoardUserLimited(7))<>"0" And IsNumeric(BoardUserLimited(7)) Then
			If Dvbbs.UserID = 0 Then 	
				GetSetting=False
				Exit Function
			End If
			If DateDiff("s",Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@joindate").text,Now)<Clng(BoardUserLimited(7))*60 Then 
				GetSetting=False
				Exit Function
			End If
		End If
		
	End If
	'认证版块判断Board_Setting(2)
	If Board_Setting(2)="1" Then
		If Dvbbs.UserID=0 Then
			GetSetting=False
			Exit Function
		Else
			Dim Boarduser,Canlogin,i
			Canlogin = False
			If Ubound(Boarduser)=-1 Then	'为空时值等于-1
				GetSetting=False
				Exit Function
			Else
				For i = 0 To Ubound(Boarduser)
					If Trim(Lcase(Boarduser(i))) = Trim(Lcase(Dvbbs.MemberName)) Then
						GetSetting = True
						Exit For
					End If				
				Next
			End If
		End If
	End If
End Function 
%>