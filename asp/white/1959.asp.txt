<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<%
Const IsReLoad = False '设为TRUE，强制更新
Const updatePeri = 12 '文档更新周期，单位小时
Const UrlSet = "http://www.baidu.com/search/bbs_sitemap.xsd" '文档名称空间
Const SaveCachePath = "data/"
Const MaxRows = 1000
Dim XmlDom
SetupXmlDoc()
ShowXml()
Dvbbs.PageEnd()
Sub ShowXml()
	Response.Clear
	Response.CharSet="gb2312"  
	Response.ContentType="text/xml"
	Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"&vbNewLine
	Response.Write "<?xml-stylesheet type=""text/xsl"" href=""inc/Templates/sitema_baidu.xslt"" ?>"&vbNewLine
	Response.Write XmlDom.documentElement.xml
	Set XmlDom = Nothing
End Sub
'<?xml-stylesheet type=""text/xsl"" href=""inc/rss.xsl"" ?>
'<?xml-stylesheet type="text/xsl" href="simple.xsl" ?>

Sub SetupXmlDoc()
	Dim Node
	Dim UpTime
	Set XmlDom = Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	If Not XmlDom.Load(Server.MapPath(MyDbPath &SaveCachePath&"sitemap_cache.xml")) Then
		CreatXmlDoc()
		LoadData()
		SaveCache()
	Else
		Set Node = XmlDom.documentElement.selectSingleNode("updatetime")
		If Node Is Nothing Then
			CreatXmlDoc()
			LoadData()
			SaveCache()
		Else
			UpTime = cdate(Node.Text)
			If Datediff("h",UpTime,Now())>updatePeri or IsReLoad Then
				CreatXmlDoc()
				LoadData()
				SaveCache()
			End If
		End If
	End If
		
End Sub

Sub SaveCache()
	XmlDom.save Server.MapPath(MyDbPath &SaveCachePath&"sitemap_cache.xml")
End Sub

Sub CreatXmlDoc()
	Dim Node
	Dim ArrNode
	'XmlDom.LoadXml("<?xml version=""1.0"" encoding=""gb2312""?><document/>")

	Dim pre
	Set pre = XmlDom.createProcessingInstruction("xml", "version='1.0' encoding='gb2312'")
	XmlDom.insertBefore pre, XmlDom.childNodes.item(0) 'createProcessingInstruction写在了最前面
	XmlDom.documentElement = XmlDom.createElement("document")
	Set Node = XmlDom.documentElement
	Node.appendChild(XMLDom.createNode(1,"webSite","")).text=Dvbbs.Forum_Info(0) '"www.baidu.com"'
	Node.appendChild(XMLDom.createNode(1,"webMaster","")).text=Dvbbs.Forum_Info(5) '"webmaster@baidu.com"
	'Node.appendChild(XMLDom.createNode(1,"urlset","")).setAttribute "xmlns:bbs",UrlSet
	Node.appendChild(XMLDom.createNode(1,"updatePeri","")).text=UpdatePeri
	Node.appendChild(XMLDom.createNode(1,"version","")).text="Dvbbs Version "& FVersion
	Node.appendChild(XMLDom.createNode(1,"updatetime","")).text=Now
	Node.setAttribute "xmlns:bbs",UrlSet
End Sub

Sub LoadData()
	Dim Node,i,Forum_Url,DispUrl
	Dim TopicData
	Dim SNode
	TopicData = GetTopicList
	Forum_Url = Dvbbs.Get_ScriptNameUrl
	If Not IsArray(TopicData) Then
		Exit Sub
	End If
	For i = 0 To Ubound(TopicData,2)
		DispUrl =  Forum_Url & "dispbbs.asp?boardID="&TopicData(2,i)&"&ID="&TopicData(0,i)
		Set Node = XmlDom.documentElement.appendChild(XMLDom.createNode(1,"item",""))
		Node.appendChild(XMLDom.createNode(1,"link","")).text = DispUrl
		Node.appendChild(XMLDom.createNode(1,"title","")).text = TopicData(1,i)
		Node.appendChild(XMLDom.createNode(1,"pubDate","")).text = TopicData(7,i)
		Set SNode = XmlDom.createNode(1,"bbs:lastDate",UrlSet)
		SNode.text = TopicData(12,i)
		Node.appendChild(SNode)
		Set SNode = XmlDom.createNode(1,"bbs:reply",UrlSet)
		SNode.text = TopicData(4,i)
		Node.appendChild(SNode)
		Set SNode = XmlDom.createNode(1,"bbs:hit",UrlSet)
		SNode.text = TopicData(8,i)
		Node.appendChild(SNode)
		Set SNode = XmlDom.createNode(1,"bbs:mainLen",UrlSet)
		SNode.text = ""
		Node.appendChild(SNode)
		Set SNode = XmlDom.createNode(1,"bbs:boardid",UrlSet)
		SNode.text = TopicData(2,i)
		Node.appendChild(SNode)
		Set SNode = XmlDom.createNode(1,"bbs:pick",UrlSet)
		SNode.text = TopicData(15,i)
		Node.appendChild(SNode)
	Next
End Sub


Function GetTopicList()
	GetTopicList = ""
	Dim Rs,Sql	'TopicID=0,Title=1,Boardid=2,LockTopic=3,Child=4,PostUsername=5,PostUserid=6,DateAndTime=7,hits=8,Expression=9,VoteTotal=10,LastPost=11,LastPostTime=12,istop=13,isvote=14,isbest=15,HideName=16
	Sql = "Select top "&MaxRows&" TopicID,Title,Boardid,LockTopic,Child,PostUsername,PostUserid,DateAndTime,hits,Expression,VoteTotal,LastPost  ,LastPostTime,istop,isvote,isbest,HideName From Dv_Topic where Not Boardid in ("&LockBoards&") "
	If IsSqlDataBase=1 Then
		Sql = Sql & " and datediff(hour,LastPostTime,"&SqlNowString&")<"&updatePeri
	Else
		Sql = Sql & " and datediff('h',LastPostTime,"&SqlNowString&")<"&updatePeri
	End If
	Sql = Sql & " order by TopicID desc"
	Set Rs = Dvbbs.Execute(Sql)
	If Not Rs.Eof Then
		GetTopicList = Rs.GetRows(-1)
	End If
	Rs.Close
	Set Rs = Nothing
End Function


'限制访问的版块ID列表
Function LockBoards()
	Dim Nodes,ChildNode
	Dim BoardList,i
	Set Nodes = Application(Dvbbs.CacheName&"_boardlist").documentElement.childNodes
	i = 0
	For Each ChildNode in Nodes
		i = i+1
		If ChildNode.getAttribute("checkout")="1" or ChildNode.getAttribute("hidden")=1 or ChildNode.getAttribute("checklock")=1 Then
			BoardList = BoardList & ChildNode.getAttribute("boardid")
			If i<Nodes.length Then BoardList =  BoardList & ","
		End If
	Next
	If BoardList<>"" Then
		BoardList = "444,777,"&BoardList
	Else
		BoardList= "444,777"
	End If
	If Right(BoardList,1)="," Then BoardList = Left(BoardList,Len(BoardList)-1)
	LockBoards = BoardList
End Function
%>