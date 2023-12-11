<!--#include file="conn.asp"-->
<!--#Include File="inc/Const.asp"-->
<%
'//*****************************************************************************//
''@	动网论坛首页调用
''@ 程序适用版本：DVBBS v8.3.0 
''@ 官方论坛地址：http://bbs.dvbbs.net http://www.aspsky.net
''@ 程序作者：	Fssunwin
''@ 更新日期：	2005-04-16
'//*****************************************************************************//
Dim Temp_Dv_ForumNews
'//*****************************************************************************//
''@ 设置外部调用限制
Const LockUrl = ""
''@ 说明：只允许调用网址,要以"HTTP://"开头,为空则不限制所有外部调用.(可允许多网址限制，要以","分隔。)
''@ 使用：例如只允许此两个网址调用: lockurl="http://www.artistsky.net/,http://www.artbbs.net/"
'//*****************************************************************************//
'//*****************************************************************************//
''@ 设置临时文件名
Temp_Dv_ForumNews = "Dv_ForumNews/Temp_Dv_ForumNews.config"	'临时文件名可自行修改。该文件可以随时删除清理。
'//*****************************************************************************//

If CheckServer(Lockurl)=False then
	OutPut "数据被保护,禁止被其他站点调用!"
	Response.End	
End If
Dim NewsConfigFile
Dim XmlDoc,Node,NewsMainStr
Dim BbsUrl
BbsUrl = Dvbbs.Get_ScriptNameUrl
NewsConfigFile = "Dv_ForumNews/Dv_NewsSetting.config"
'Dvbbs.ShowSQL = 1

Call LoadXml()
Call Page_Main()
Call CloseFile()
'Dvbbs.PageEnd()
'Response.Write "<br>页面执行时间 "&FormatNumber((Timer()-Startime)*1000,5)&" 毫秒"
Sub Page_Main()
	Dim GetName
	GetName = Lcase(Request.QueryString("GetName"))
	If GetName = "" Then
		OutPut "参数错误，调用已中止！"
		Exit Sub
	End If
	Set Node = XmlDoc.DocumentElement.selectSingleNode("NewsCode[@NewsName='"&GetName&"']")
	If (Node is nothing) Then
		OutPut "设置数据不存在，调用已中止！"
		Exit Sub
	End If
	Dim Updatetime,LastTime
	Updatetime = Dvbbs.CheckNumeric(Node.getAttribute("Updatetime"))
	LastTime = Node.getAttribute("LastTime")
	If Updatetime>0 and IsDate(LastTime) Then
		If Datediff("s",LastTime,now()) > Updatetime Then
			'更新
			Call UpNewsData()
			Call SaveData()
		Else
			Call LoadNewsData()
		End If
	Else
		Call UpNewsData()
	End If
	Call ShowData()
End Sub

Sub LoadXml()
	NewsConfigFile = Server.MapPath(NewsConfigFile)
	Temp_Dv_ForumNews = Server.MapPath(Temp_Dv_ForumNews)
	Set XmlDoc = Dvbbs.iCreateObject("MSXML.DOMDocument")
	XmlDoc.Async = False
	If Not XmlDoc.load(NewsConfigFile) Then
		XmlDoc.loadxml "<?xml version=""1.0"" encoding=""gb2312""?><NewscodeInfo/>"
	End If
End Sub

Sub CloseFile()
	Set XmlDoc = Nothing
End Sub

Sub ShowData()
	If NewsMainStr <>"" Then
		OutPut NewsMainStr
	End If
End Sub

Sub SaveData()
	If NewsMainStr = "" Then Exit Sub
	Node.Attributes.getNamedItem("LastTime").Text = Now()
	XmlDoc.save NewsConfigFile
	Dim TempXmlDoc,TempXml_Nodes,attributes,ChildNode,createCDATASection
	Set TempXmlDoc = Dvbbs.iCreateObject("MSXML.DOMDocument")
	TempXmlDoc.Async = False
	If Not TempXmlDoc.load(Temp_Dv_ForumNews) Then
		TempXmlDoc.loadxml "<?xml version=""1.0"" encoding=""gb2312""?><TempNewsData/>"
	End If
	Set TempXml_Nodes = TempXmlDoc.DocumentElement.selectSingleNode("NewsData[@NewsName='"&Node.getAttribute("NewsName")&"']")
	If Not (TempXml_Nodes is nothing) Then
		TempXmlDoc.DocumentElement.RemoveChild(TempXml_Nodes)
	End If
	'创建调用数据
	Set TempXml_Nodes = XmlDoc.createNode(1,"NewsData","")
	Set attributes = TempXmlDoc.createAttribute("NewsName")
	attributes.text = Node.getAttribute("NewsName")
	TempXml_Nodes.attributes.setNamedItem(attributes)
	Set ChildNode = TempXmlDoc.createNode(1,"Temp_Data","")
	Set createCDATASection=TempXmlDoc.createCDATASection(NewsMainStr)
	ChildNode.appendChild(createCDATASection)
	TempXml_Nodes.appendChild(ChildNode)
	TempXmlDoc.documentElement.appendChild(TempXml_Nodes)
	TempXmlDoc.save Temp_Dv_ForumNews
	Set TempXmlDoc = Nothing
End Sub

Sub LoadNewsData()
	Dim TempXmlDoc,TempXml_Nodes
	Set TempXmlDoc = Dvbbs.iCreateObject("MSXML.DOMDocument")
	TempXmlDoc.Async = False
	If Not TempXmlDoc.load(Temp_Dv_ForumNews) Then
		Call UpNewsData()
		Call SaveData()
		Exit Sub
	End If
	Set TempXml_Nodes = TempXmlDoc.DocumentElement.selectSingleNode("NewsData[@NewsName='"&Node.getAttribute("NewsName")&"']")
	If Not (TempXml_Nodes is nothing) Then
		NewsMainStr = TempXml_Nodes.selectSingleNode("Temp_Data").text
	Else
		Call UpNewsData()
		Call SaveData()
	End If
	Set TempXmlDoc = Nothing
End Sub

Sub OutPut(Strings)
	Response.Write "document.write('"
	Response.Write Strings
	Response.Write "');"
	Response.Write vbNewline
End Sub

Sub UpNewsData()
	Select Case Node.getAttribute("NewsType")
		Case "1" : Call NewsType_1() '帖子调用
		Case "2" : Call NewsType_2() '信息调用
		Case "3" : Call NewsType_3() '版块调用
		Case "4" : Call NewsType_4() '会员调用
		Case "5" : Call NewsType_5() '公告调用
		Case "6" : Call NewsType_6() '展区调用
		Case "7" : Call NewsType_7() '圈子调用
		Case "8" : Call NewsType_8() '登陆框调用
		Case Else
			OutPut "调用类型不存在，调用已中止！"
			Exit Sub
	End Select
	NewsMainStr = Fixjs(NewsMainStr)
End Sub

'帖子调用
Sub NewsType_1()
	Dim Skin_Main
	Dim SQL,Rs,i
	SET Rs = Dvbbs.Execute(Node.selectSingleNode("Search").text)
	If Not Rs.eof Then
		SQL=Rs.GetRows(-1)
	Else
		OutPut "暂未有新帖子！"
		Exit Sub
	End If
	Rs.close:Set Rs = Nothing
	Dim Topic,Topiclen
	Dim Expression,LastInfo,LastPostName,LastPostTime
	Topiclen = Node.getAttribute("Topiclen")
	If Not Isnumeric(Topiclen) or Topiclen = "" Then
		Topiclen = 20
	Else
		Topiclen = Cint(Topiclen)
	End If
	Dim BoardNode,Nodes
	Set BoardNode = Application(Dvbbs.CacheName&"_boardlist").cloneNode(True).documentElement.getElementsByTagName("board")
	For i=0 To Ubound(SQL,2)
		Skin_Main = Node.selectSingleNode("Skin_Main").text
		Expression = ""
		LastPostName = ""
		LastPostTime = ""
		Topic = SQL(1,i)
		If Topic = "" and Node.getAttribute("TopicType") = "2" then
			Topic = SQL(6,i)
		End If
		If Len(Topic)>Topiclen then
			Topic = Left(Stringhtml(Topic),Topiclen)&"..."
		End if
		'Topic = Stringhtml(topic)
		Expression = SQL(7,i)
		If Instr(SQL(7,i)&"","|") Then
			Expression = Split(SQL(7,i),"|")(1)
		End If
		If Node.getAttribute("TopicType")="0" then
			If SQL(8,i)<>"" then
				LastInfo=split(SQL(8,i),"$")
				LastPostName = LastInfo(0)
				LastPostTime = LastInfo(2)
			End If
		End If
		If Instr(Skin_Main,"{$BoardName}") Then
			For Each Nodes in BoardNode
				If Nodes.getAttribute("boardid") = Cstr(SQL(3,i)) Then
						Skin_Main = Replace(Skin_Main,"{$BoardName}",Nodes.getAttribute("boardtype"))
						Skin_Main = Replace(Skin_Main,"{$BoardInfo}",Nodes.getAttribute("readme")&"")
					Exit For
				End If
			Next
		End If
		Skin_Main = Replace(Skin_Main,"{$ID}",SQL(2,i))
		Skin_Main = Replace(Skin_Main,"{$ReplyID}",SQL(5,i))
		Skin_Main = Replace(Skin_Main,"{$Boardid}",SQL(3,i))
		Skin_Main = Replace(Skin_Main,"{$ID}",SQL(2,i))
		Skin_Main = Replace(Skin_Main,"{$Topic}",Topic)
		Skin_Main = Replace(Skin_Main,"{$Face}",Expression)
		Skin_Main = Replace(Skin_Main,"{$ReplyName}",LastPostName)
		Skin_Main = Replace(Skin_Main,"{$ReplyTime}",FormatTime(LastPostTime,Node.getAttribute("FormatTime")))
		Skin_Main = Replace(Skin_Main,"{$UserName}",SQL(0,i))
		Skin_Main = Replace(Skin_Main,"{$PostTime}",FormatTime(SQL(4,i),Node.getAttribute("FormatTime")))
		NewsMainStr = NewsMainStr & Skin_Main
	Next
	NewsMainStr = Node.selectSingleNode("Skin_Head").text & NewsMainStr & Node.selectSingleNode("Skin_Footer").text
End Sub

'信息调用
Sub NewsType_2()
	Set MyBoardOnline=New Cls_UserOnlne 
	Dvbbs.GetForum_Setting
	Dim Skin_Main
	Skin_Main = Node.selectSingleNode("Skin_Main").text
	Skin_Main = Replace(Skin_Main,"{$TopicNum}",Dvbbs.CacheData(7,0))
	Skin_Main = Replace(Skin_Main,"{$PostNum}",Dvbbs.CacheData(8,0))
	Skin_Main = Replace(Skin_Main,"{$JoinMembers}",Dvbbs.CacheData(10,0))
	Skin_Main = Replace(Skin_Main,"{$AllOnline}",MyBoardOnline.Forum_Online)
	Skin_Main = Replace(Skin_Main,"{$LastUser}",Dvbbs.CacheData(14,0))
	Skin_Main = Replace(Skin_Main,"{$TodayPostNum}",Dvbbs.CacheData(9,0))
	Skin_Main = Replace(Skin_Main,"{$YesterdayPostNum}",Dvbbs.CacheData(11,0))
	Skin_Main = Replace(Skin_Main,"{$TopPostNum}",Dvbbs.CacheData(12,0))
	Skin_Main = Replace(Skin_Main,"{$TopOnline}",Dvbbs.Maxonline)
	Skin_Main = Replace(Skin_Main,"{$BuildDay}",FormatTime(Dvbbs.Forum_Setting(74),Node.getAttribute("FormatTime")))
	NewsMainStr = Node.selectSingleNode("Skin_Head").text & Skin_Main & Node.selectSingleNode("Skin_Footer").text
End Sub

'版块调用
Sub NewsType_3()
	Dim Skin_Main,Mode,Depth
	Dim BoardNode,Nodes
	Dim ParentID,BoardTab
	Dim ii,i
	Depth = Node.getAttribute("Depth")
	Mode = Node.getAttribute("Orders")
	BoardTab = Node.getAttribute("BoardTab")
	If Depth = "" Then Depth = "20"
	If Mode = "1" and Depth > "1" Then
		Depth = "1"
		If BoardTab="" Then
			BoardTab = "5"
		End If
		BoardTab = Cint(BoardTab)
	End If
	Set BoardNode = Application(Dvbbs.CacheName&"_boardlist").cloneNode(True).documentElement.getElementsByTagName("board")
	
	ParentID = "0"
	For Each Nodes in BoardNode
		If Nodes.attributes.getNamedItem("depth").text <= Depth Then
		
			If Node.getAttribute("Stats") = "1" and Nodes.getAttribute("hidden")="1" Then
				
			Else
				Skin_Main = Node.selectSingleNode("Skin_Main").text
				Skin_Main = Replace(Skin_Main,"{$BoardID}",Nodes.attributes.getNamedItem("boardid").text)
				Skin_Main = Replace(Skin_Main,"{$BoardName}",Nodes.getAttribute("boardtype"))
				Skin_Main = Replace(Skin_Main,"{$BoardInfo}",Nodes.getAttribute("readme")&"")
				Skin_Main = Replace(Skin_Main,"{$BoardChild}",Nodes.getAttribute("child"))
				If Not IsObject(Application(Dvbbs.CacheName &"_information_" & Nodes.attributes.getNamedItem("boardid").text)) Then Dvbbs.LoadBoardinformation Nodes.attributes.getNamedItem("boardid").text
				Skin_Main = Replace(Skin_Main,"{$PostNum}",Application(Dvbbs.CacheName &"_information_" & Nodes.attributes.getNamedItem("boardid").text).documentElement.selectSingleNode("information/@postnum").text)
				Skin_Main = Replace(Skin_Main,"{$TopicNum}",Application(Dvbbs.CacheName &"_information_" & Nodes.attributes.getNamedItem("boardid").text).documentElement.selectSingleNode("information/@topicnum").text)
				Skin_Main = Replace(Skin_Main,"{$TodayNum}",Application(Dvbbs.CacheName &"_information_" & Nodes.attributes.getNamedItem("boardid").text).documentElement.selectSingleNode("information/@todaynum").text)
				Skin_Main = Replace(Skin_Main,"{$Rules}",Nodes.getAttribute("rules")&"")
				If Mode = "1" Then
					If Nodes.getAttribute("parentid") > "0" Then
						If ParentID <> Nodes.getAttribute("parentid") Then
							NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input3").text
							For i=0 to Nodes.getAttribute("depth")
								NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input2").text
							Next
							ParentID = Nodes.getAttribute("parentid")
						Else
							If ii >= BoardTab Then
								NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input3").text
								For i=0 to Nodes.getAttribute("depth")
									NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input2").text
								Next
								ii = 1
							Else
								ii = ii + 1
								NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input4").text
							End If
						End If
					Else
						NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input3").text
					End If
					If Nodes.getAttribute("depth") = "0" Then
						ii = 1
						NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input0").text
					Else
						NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input1").text
					End If
					NewsMainStr = NewsMainStr & Skin_Main
				Else
					For i=0 to Nodes.getAttribute("depth")
						NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input2").text
					Next
					If Nodes.getAttribute("child") >"0" or Nodes.getAttribute("depth") = "0" Then
						NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input0").text
					Else
						NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input1").text
					End If
					NewsMainStr = NewsMainStr & Skin_Main & Node.selectSingleNode("Board_Input3").text
				End If
			End If
		End If
	Next
	NewsMainStr = Node.selectSingleNode("Skin_Head").text & NewsMainStr & Node.selectSingleNode("Skin_Footer").text
End Sub

'会员调用
''UserID,UserName,UserTopic,UserPost,UserBest,UserWealth,UserCP,UserEP,UserDel,UserSex,JoinDate 
Sub NewsType_4()
	Dim Skin_Main
	Dim SQL,Rs,i
	SQL = Node.selectSingleNode("Search").text
	SET Rs = Dvbbs.Execute(SQL)
	If Not Rs.eof Then
		SQL=Rs.GetRows(-1)
	Else
		OutPut "暂未有会员数据！"
		Exit Sub
	End If
	Rs.close:Set Rs = Nothing
	For i=0 To Ubound(SQL,2)
		Skin_Main = Node.selectSingleNode("Skin_Main").text
		Skin_Main = Replace(Skin_Main,"{$UserID}",SQL(0,i))
		Skin_Main = Replace(Skin_Main,"{$UserName}",Stringhtml(SQL(1,i)))
		Skin_Main = Replace(Skin_Main,"{$UserTopic}",SQL(2,i))
		Skin_Main = Replace(Skin_Main,"{$UserPost}",SQL(3,i))
		Skin_Main = Replace(Skin_Main,"{$UserBest}",SQL(4,i))
		Skin_Main = Replace(Skin_Main,"{$UserWealth}",SQL(5,i))
		Skin_Main = Replace(Skin_Main,"{$UserCP}",SQL(6,i))
		Skin_Main = Replace(Skin_Main,"{$UserEP}",SQL(7,i))
		Skin_Main = Replace(Skin_Main,"{$UserDel}",SQL(8,i))
		Skin_Main = Replace(Skin_Main,"{$UserSex}",UserSex(Cstr(SQL(9,i))))
		Skin_Main = Replace(Skin_Main,"{$JoinDate}",FormatTime(SQL(10,i),Node.getAttribute("FormatTime")))
		Skin_Main = Replace(Skin_Main,"{$UserLogins}",SQL(11,i))
		NewsMainStr = NewsMainStr & Skin_Main
	Next
	NewsMainStr = Node.selectSingleNode("Skin_Head").text & NewsMainStr & Node.selectSingleNode("Skin_Footer").text
End Sub

'公告调用
Sub NewsType_5()
	Dim Skin_Main
	Dim SQL,Rs,i
	SET Rs = Dvbbs.Execute(Node.selectSingleNode("Search").text)
	If Not Rs.eof Then
		SQL=Rs.GetRows(-1)
	Else
		OutPut "暂未有新公告！"
		Exit Sub
	End If
	Rs.close:Set Rs = Nothing
	Dim Topic,Topiclen
	Topiclen = Node.getAttribute("Topiclen")
	If Not Isnumeric(Topiclen) or Topiclen = "" Then
		Topiclen = 20
	Else
		Topiclen = Cint(Topiclen)
	End If
	'ID,Boardid,Title,UserName,AddTime
	Dim BoardNode,Nodes
	Set BoardNode = Application(Dvbbs.CacheName&"_boardlist").cloneNode(True).documentElement.getElementsByTagName("board")
	For i=0 To Ubound(SQL,2)
		Topic = SQL(2,i)
		If Len(Topic)>Topiclen then
			Topic = Left(Topic,Topiclen)&"..."
		End if
		Skin_Main = Node.selectSingleNode("Skin_Main").text
		If Instr(Skin_Main,"{$BoardName}") Then
			If Cstr(SQL(1,i)) >"0" Then
				For Each Nodes in BoardNode
					If Nodes.getAttribute("boardid") = Cstr(SQL(1,i)) Then
							Skin_Main = Replace(Skin_Main,"{$BoardName}",Nodes.getAttribute("boardtype"))
						Exit For
					End If
				Next
			Else
				Skin_Main = Replace(Skin_Main,"{$BoardName}","")
			End If
		End If
		Skin_Main = Replace(Skin_Main,"{$ID}",SQL(0,i))
		Skin_Main = Replace(Skin_Main,"{$Boardid}",SQL(1,i))
		Skin_Main = Replace(Skin_Main,"{$Topic}",Topic)
		Skin_Main = Replace(Skin_Main,"{$UserName}",SQL(3,i))
		Skin_Main = Replace(Skin_Main,"{$PostTime}",FormatTime(SQL(4,i),Node.getAttribute("FormatTime")))
		NewsMainStr = NewsMainStr & Skin_Main
	Next
	NewsMainStr = Node.selectSingleNode("Skin_Head").text & NewsMainStr & Node.selectSingleNode("Skin_Footer").text
End Sub

'展区调用
Sub NewsType_6()
	Dim Skin_Main
	Dim SQL,Rs,i
	Set MyBoardOnline=New Cls_UserOnlne 
	Dvbbs.GetForum_Setting
	SET Rs = Dvbbs.Execute(Node.selectSingleNode("Search").text)
	If Not Rs.eof Then
		SQL=Rs.GetRows(-1)
	Else
		OutPut "暂未有新展区文件！"
		Exit Sub
	End If
	Rs.close:Set Rs = Nothing
	Dim Topic,Topiclen
	Topiclen = Node.getAttribute("Topiclen")
	If Not Isnumeric(Topiclen) or Topiclen = "" Then
		Topiclen = 10
	Else
		Topiclen = Cint(Topiclen)
	End If	'F_ID,F_AnnounceID,F_BoardID,F_Username,F_Filename,F_Readme,F_Type,F_FileType,F_AddTime,F_Viewname,F_ViewNum,F_DownNum,F_FileSize 
	'F_Typ : 1=图片集,2=FLASH集,3=音乐集,4=电影集,0=文件集
	Dim FileArray,Filename,Picheight,Picwidth
	Dim RootID,ReplyID,F_AnnounceID
	Dim BoardNode,Nodes,t,tab
	Dim TColor,TColor1,TColor2
	FileArray = "文件集||图片集||FLASH集||音乐集||电影集"
	FileArray = Split(FileArray,"||")
	Picheight = Node.getAttribute("PicHeight")
	Picwidth  = Node.getAttribute("PicWidth")
	Tab = Node.getAttribute("Tab")
	TColor1 = Node.getAttribute("TColor1")
	TColor2 = Node.getAttribute("TColor2")
	t=0
	Set BoardNode = Application(Dvbbs.CacheName&"_boardlist").cloneNode(True).documentElement.getElementsByTagName("board")
	For i=0 To Ubound(SQL,2)
		Topic = SQL(5,i)
		If Len(Topic)>Topiclen then
			Topic = Left(Topic,Topiclen)&"..."
		End if
		If TColor=TColor2 Then 
			TColor=TColor1
		Else
			TColor=TColor2
		End If
		Skin_Main = Node.selectSingleNode("Skin_Main").text
		Skin_Main = Replace(Skin_Main,"{$ID}",SQL(0,i))
		Skin_Main = Replace(Skin_Main,"{$Boardid}",SQL(2,i))
		Skin_Main = Replace(Skin_Main,"{$UserName}",SQL(3,i))
		Skin_Main = Replace(Skin_Main,"{$Readme}",Topic&"")
		Skin_Main = Replace(Skin_Main,"{$AddTime}",FormatTime(SQL(8,i),Node.getAttribute("FormatTime")))
		Skin_Main = Replace(Skin_Main,"{$ViewFilename}",SQL(9,i)&"")
		Skin_Main = Replace(Skin_Main,"{$ViewNum}",SQL(10,i))
		Skin_Main = Replace(Skin_Main,"{$DownNum}",SQL(11,i))
		Skin_Main = Replace(Skin_Main,"{$FileSize}",SQL(12,i))
		Skin_Main = Replace(Skin_Main,"{$FileType}",FileArray(SQL(6,i)))
		Skin_Main = Replace(Skin_Main,"{$TColor}",TColor)
		
		If Instr(SQL(1,i)&"","|") Then
			F_AnnounceID=Split(SQL(1,i),"|")
			RootID = F_AnnounceID(0)
			ReplyID = F_AnnounceID(1)
		Else
			RootID = ""
			ReplyID = ""
		End If
		Skin_Main = Replace(Skin_Main,"{$ReplyID}",ReplyID)
		Skin_Main = Replace(Skin_Main,"{$RootID}",RootID)
		If Instr(Skin_Main,"{$BoardName}") Then
			If Cstr(SQL(1,i)) >"0" Then
				For Each Nodes in BoardNode
					If Nodes.getAttribute("boardid") = Cstr(SQL(2,i)) Then
							Skin_Main = Replace(Skin_Main,"{$BoardName}",Nodes.getAttribute("boardtype"))
						Exit For
					End If
				Next
			Else
				Skin_Main = Replace(Skin_Main,"{$BoardName}","")
			End If
		End If
		If SQL(9,i)<>"" Then
			Filename = Bbsurl & SQL(9,i)
		Else
			Filename = SQL(4,i)
			If InStr(Filename,":") = 0 Or InStr(Filename,"//") = 0 Then
				Filename = Bbsurl & Dvbbs.Forum_Setting(76) & Filename
			End If
		End If
		
		If SQL(6,i)=1 Then
			Filename = "<IMG SRC="""&Filename&""" style=""border: 1 solid #000000"" width="&Picwidth&" height="&Picheight&" >"
		Else
			Filename = SQL(7,i) & " 类文件"
		End If
		Skin_Main = Replace(Skin_Main,"{$Filename}",Filename)
		NewsMainStr = NewsMainStr & Skin_Main
		If Tab<>"" Then
			If t=Tab-1 or Tab=1 Then 
				NewsMainStr = NewsMainStr & Node.selectSingleNode("Board_Input0").text
			end if
			If t>Tab-1 Then 
				t=1
			Else
				t=t+1
			End If
		End If
	Next
	NewsMainStr = Node.selectSingleNode("Skin_Head").text & NewsMainStr & Node.selectSingleNode("Skin_Footer").text
End Sub

Sub NewsType_7()
	Dim Skin_Main
	Dim SQL,Rs,i
	SQL = Node.selectSingleNode("Search").text
	If Not IsObject(Dv_IndivGroup_Conn) Then Dv_IndivGroup_ConnectionDatabase
	SET Rs = Dv_IndivGroup_Conn.Execute(SQL)
	If Not Rs.eof Then
		SQL=Rs.GetRows(-1)
	Else
		OutPut "暂未有会员数据！"
		Exit Sub
	End If
	Rs.close:Set Rs = Nothing
	For i=0 To Ubound(SQL,2)
		Skin_Main = Node.selectSingleNode("Skin_Main").text
		Skin_Main = Replace(Skin_Main,"{$IGID}",Dvbbs.CheckNumeric(SQL(0,i)))
		Skin_Main = Replace(Skin_Main,"{$IGName}",Stringhtml(SQL(1,i)))
		Skin_Main = Replace(Skin_Main,"{$GrouInfo}",Stringhtml(SQL(2,i)))
		Skin_Main = Replace(Skin_Main,"{$AppUserID}",Dvbbs.CheckNumeric(SQL(3,i)))
		Skin_Main = Replace(Skin_Main,"{$AppUserName}",SQL(4,i))
		Skin_Main = Replace(Skin_Main,"{$IGUserNum}",Dvbbs.CheckNumeric(SQL(5,i)))
		Skin_Main = Replace(Skin_Main,"{$Stats}",IGStatsStr(SQL(6,i)))
		Skin_Main = Replace(Skin_Main,"{$IGPostNum}",Dvbbs.CheckNumeric(SQL(7,i)))
		Skin_Main = Replace(Skin_Main,"{$IGTopicNum}",Dvbbs.CheckNumeric(SQL(8,i)))
		Skin_Main = Replace(Skin_Main,"{$IGTodayNum}",Dvbbs.CheckNumeric(SQL(9,i)))
		Skin_Main = Replace(Skin_Main,"{$IGYesterdayNum}",Dvbbs.CheckNumeric(SQL(10,i)))
		Skin_Main = Replace(Skin_Main,"{$LimitUser}",Dvbbs.CheckNumeric(SQL(11,i)))
		Skin_Main = Replace(Skin_Main,"{$PassDate}",FormatTime(SQL(12,i)&"",Node.getAttribute("FormatTime")))
		NewsMainStr = NewsMainStr & Skin_Main
	Next
	NewsMainStr = Node.selectSingleNode("Skin_Head").text & NewsMainStr & Node.selectSingleNode("Skin_Footer").text
End Sub

Sub NewsType_8()
	Dim TempStr
	Dvbbs.GetForum_Setting
	NewsMainStr = Node.selectSingleNode("Skin_Main").text
	Dvbbs.loadTemplates("")
	If Dvbbs.Forum_Setting(79)=1 Then
		NewsMainStr = Replace(NewsMainStr,"{$CheckCode}","验证码："&Dvbbs.mainhtml(15))
	Else
		NewsMainStr = Replace(NewsMainStr,"{$CheckCode}","")
	End If
	NewsMainStr = Node.selectSingleNode("Skin_Head").text & NewsMainStr & Node.selectSingleNode("Skin_Footer").text
	NewsMainStr = "<form action="""&Dvbbs.Get_ScriptNameUrl&"login.asp?action=chk"" method=""post"">"&NewsMainStr&"</form>"
End Sub

Function IGStatsStr(Stats)
	Select Case Stats
		Case 1
			IGStatsStr="正常"
		Case 2
			IGStatsStr="锁定"
		Case 3
			IGStatsStr="关闭"
		Case 0
			IGStatsStr="审核"
		Case Else
			IGStatsStr="未知"
	End Select
End Function

Function UserSex(Val)
	If Val = "1" Then
		UserSex = "先生"
	Else
		UserSex = "女士"
	End If
End Function

Function Stringhtml(str)
	Dim re
	Set re=new RegExp
	re.IgnoreCase =True
	re.Global=True
	re.Pattern="<(.[^>]*)>"
	str=re.replace(str, "")
	re.Pattern="\[(.[^\[]*)\]"
	str=re.replace(str, "")
	str = replace(str, ">", "&gt;")
	str = replace(str, "<", "&lt;")
	If str="" Then str="..."
	Stringhtml=str
End Function

Function Fixjs(Strings)
	Dim Str
	Str = Strings
	str = Replace(str, CHR(39), "\'")
	str = Replace(str, CHR(13), "")
	str = Replace(str, CHR(10), "")
	str = Replace(str, "]]>","]]&gt;")
	Fixjs = str
End Function

Function FormatTime(Strings,val)
	If IsDate(Strings) and val<>"" Then
		Strings = FormatdateTime(Strings,val)
	End If
	FormatTime = Strings
End Function

Function CheckServer(str)
	Dim i,servername
	If str="" Then
		CheckServer = True
		Exit Function
	Else
		CheckServer = False
	End If
	str=split(Cstr(str),",")
	servername=Request.ServerVariables("HTTP_REFERER")
	For i=0 to Ubound(str)
	If Right(str(i),1)="/" Then str(i)=left(Trim(str(i)),Len(str(i))-1)
		If Lcase(left(servername,Len(str(i))))=Lcase(str(i)) then
			checkserver = True
			Exit For
		Else
			checkserver = False
		End if
	Next
End Function
%>