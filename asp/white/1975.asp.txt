<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/dv_clsother.asp" -->
<%
If Dvbbs.BoardID < 0 Then
	Response.Write "参数错误"
	Response.End
End If
Dim action,ReportText
action=Request("action")
If (Not Dvbbs.Master) And Action<>"isWeb" Then action=""
If Dvbbs.Forum_Setting(3)="0" Then
	Dvbbs.Forum_Setting(3)="120"
End If
Dim XMLDOM
Dvbbs.LoadTemplates("query")
If Cint(Dvbbs.GroupSetting(14))=0 And Request("isWeb")<>"2" Then Dvbbs.AddErrCode(60):Dvbbs.ShowErr()
If request("stype")="" Then
	If action="" Then
		Dvbbs.stats=template.Strings(0)
		Dvbbs.nav()
		If DVbbs.BoardID=0 then
			Dvbbs.Head_var 0,0,template.Strings(0),"query.asp"
		Else
			Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
		End If
		Dvbbs.ShowErr()
		Queryform()
	ElseIf action="batch" Then 
			Dvbbs.stats=template.Strings(22)
			Dvbbs.nav()
			If DVbbs.BoardID=0 then
				Dvbbs.Head_var 0,0,template.Strings(0),"query.asp"
			Else
				Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
			End If
			Dobatch()
	ElseIf action="isWeb" Then
		isWeb_Query()
	End If
Else
	Dvbbs.Stats=template.Strings(1)
	Dim stype,pSearch,nSearch,keyword,stable,page,searchday,searchboard,page_count,Pcount,isWeb,isMoreInfo
	Dim totalrec,endpage,ordername
	Dim SqlColumn,SelSearch
	Dim SearchMaxPageList
	isWeb = 0
	isMoreInfo = 0
	'搜索返回结果数控制
	If Dvbbs.Forum_Setting(12)<>"0" Then
		If IsNumeric(Dvbbs.Forum_Setting(12)) Then
		If Clng(Dvbbs.Forum_Setting(12)) Mod Cint(Dvbbs.Forum_Setting(11))=0 Then
			SearchMaxPageList = Clng(Dvbbs.Forum_Setting(12)) \ Cint(Dvbbs.Forum_Setting(11))
		Else
	     	SearchMaxPageList = Clng(Dvbbs.Forum_Setting(12)) \ Cint(Dvbbs.Forum_Setting(11))+1
  		End If
		Else
			SearchMaxPageList = 50
		End If
	Else
		SearchMaxPageList = 50
	End If
	CheckRequestInfo()
	Dvbbs.ShowErr()
	SearchResult()
	Dvbbs.ShowErr()
End If
Set XMLDOM=Nothing
Dvbbs.ActiveOnline
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub Queryform()
	If Not IsObject(Application(Dvbbs.CacheName&"_Searchform")) Then
		Set XMLDOM=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument")
		XMLDOM.appendChild(XMLDOM.createElement("xml"))
		Dim keywordlimited
		keywordlimited = Split(Dvbbs.Forum_Setting(4),"|")
		If UBound(keywordlimited)<1 Then
			ReDim keywordlimited(1)
			keywordlimited(0)=2
			keywordlimited(1)=20
		End If
		XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"keywordminlen","")).text=keywordlimited(0)		
		XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"keywordmaxlen","")).text=keywordlimited(1)
		XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"timelimited","")).text=Dvbbs.Forum_Setting(3)
		XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"sqlsearch","")).text=Dvbbs.Forum_Setting(16) Rem 全文索引开关
		Dim Rs,Node
		Set Rs=Dvbbs.Execute("select * from Dv_TableList")
		Do while Not Rs.Eof
			Set Node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"posttable",""))
			Node.text=Rs("tablename")&""
			Node.attributes.setNamedItem(XMLDOM.createNode(2,"type","")).text=""&Rs("tabletype")
			Rs.MoveNext
		Loop
		Set Rs=Nothing
		Set Application(Dvbbs.CacheName&"_Searchform")=XMLDOM.cloneNode(True)
	Else
		Set XMLDOM=Application(Dvbbs.CacheName&"_Searchform").cloneNode(True)		
	End If
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"boardid","")).text=Dvbbs.BoardID
	Call DoShowHTML()
End Sub
Sub DoShowHTML()
	Dim XSLTemplate,stylesheet,proc,node,cnode
	If Not IsObject(Application(Dvbbs.CacheName&"_querytemplate_"&Dvbbs.SkinID)) Then
		Set stylesheet=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
		'stylesheet.load server.MapPath("query.xslt")
		stylesheet.loadxml template.html(0)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="ztopic"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainpic(0)
		stylesheet.documentElement.appendChild(node)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="istopic"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainpic(1)
		stylesheet.documentElement.appendChild(node)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="opentopic"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainpic(2)
		stylesheet.documentElement.appendChild(node)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="hottopic"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainpic(3)
		stylesheet.documentElement.appendChild(node)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="ilocktopic"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainpic(4)
		stylesheet.documentElement.appendChild(node)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="besttopic"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainpic(5)
		stylesheet.documentElement.appendChild(node)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="votetopic"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainpic(6)
		stylesheet.documentElement.appendChild(node)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="pic_toptopic1"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainpic(19)
		stylesheet.documentElement.appendChild(node)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="tablewidth"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainsetting(0)
		stylesheet.documentElement.appendChild(node)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="bordercolor"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainsetting(12)
		stylesheet.documentElement.appendChild(node)
		Set Node=stylesheet.createNode(1,"xsl:variable","http://www.w3.org/1999/XSL/Transform")
		Set CNode=stylesheet.createNode(2,"name","")
		CNode.text="alertcolor"
		Node.attributes.setNamedItem(CNode)
		node.text=Dvbbs.mainsetting(1)
		stylesheet.documentElement.appendChild(node)
		Set XSLTemplate=Dvbbs.iCreateObject("Msxml2.XSLTemplate" &MsxmlVersion )
		XSLTemplate.stylesheet=stylesheet
		Set Application(Dvbbs.CacheName&"_querytemplate_"&Dvbbs.SkinID)=XSLTemplate
	Else
		Set XSLTemplate=Application(Dvbbs.CacheName&"_querytemplate_"&Dvbbs.SkinID)
	End If
	'XMLDOM.save Server.MapPath(MyDbPath &"query.xml")
	Set proc = XSLTemplate.createProcessor()
	proc.input = XMLDOM
	proc.transform()
	Response.Write  proc.output
End Sub
Sub CheckRequestInfo()
	Dim i
	stype=Trim(Request("stype"))
	pSearch=Trim(Request("pSearch"))
	nSearch=Trim(Request("nSearch"))
	keyword=Trim(Dvbbs.checkStr(Request("keyword")))
	stable=Replace(Request("stable"),"'","")
	SelSearch = Request("selsearch")
	If not IsNumeric(pSearch) Then pSearch=1
	If not IsNumeric(nSearch) Then nSearch=1
	If stable="" Or Len(stable)>10 Then stable=Dvbbs.NowUseBbs
	If request("page")<>"" and IsNumeric(request("page")) Then
		page=Clng(request("page"))
	Else
		page=1
	End If
	If SelSearch = "" Then SelSearch = "Page"
	If Cint(Dvbbs.GroupSetting(14))=0 And Request("isWeb")<>"2" Then Dvbbs.AddErrCode(60)
	If Len(stable)>8 Then Dvbbs.AddErrCode(35)
	If sType = 1 Or sType = 2 Or sType = 7 Then
		If keyword="" Then Dvbbs.AddErrCode(61)
		If keyword<>"" Then
			Dim Foundmykeyword
			Foundmykeyword = False
			'搜索不受长度限制的词
			If Dvbbs.Forum_Setting(9)<>"0" Then
				Dim mykeyword
				mykeyword = Split(Dvbbs.Forum_Setting(9),"|")
				For i = 0 To Ubound(mykeyword)
					If Instr(Lcase(keyword),Lcase(mykeyword(i)))>0 Then
						Foundmykeyword = True
						Exit For
					End If
				Next
			End If
			'搜索字串最小和最大长度
			If Dvbbs.Forum_Setting(4)<>"0" And Not Foundmykeyword And Not (Dvbbs.Master Or Dvbbs.BoardMaster Or Dvbbs.SuperBoardMaster) Then
				Dim keywordlimited
				keywordlimited = Split(Dvbbs.Forum_Setting(4),"|")
				If Ubound(keywordlimited)=1 Then
					If IsNumeric(keywordlimited(0)) Then
						If Len(keyword)<Clng(keywordlimited(0)) Then Response.redirect "showerr.asp?ErrCodes=<li>"&Replace(template.Strings(17),"{$minlength}",keywordlimited(0))&"&action=OtherErr"
					End If
					If IsNumeric(keywordlimited(1)) Then
						If Len(keyword)>Clng(keywordlimited(1)) Then Response.redirect "showerr.asp?ErrCodes=<li>"&Replace(template.Strings(18),"{$maxlength}",keywordlimited(1))&"&action=OtherErr"
					End If
				End If
			End If
		End If
		
		'搜索多少天内帖子
		'If Lcase(request("SearchDate"))="all" Then
		'	searchday=" "
		'Else
		'	If request("SearchDate")<>"" And IsNumeric(Request("SearchDate"))  Then
		'		If IsSqlDataBase=1 Then
		'			searchday=" datediff(d,DateAndTime,"&SqlNowString&") < "&Dvbbs.checkStr(request("SearchDate"))&" and "
		'		Else
		'			searchday=" datediff('d',DateAndTime,"&SqlNowString&") < "&Dvbbs.checkStr(request("SearchDate"))&" and "
		'		End If
		'	Else
		'		Dvbbs.AddErrCode(62)
		'	End If
		'End If
	End If
	'isWeb=0包含站外搜索,isWeb=1仅站内,isWeb=2仅站外
	If sType = 3 Or sType = 5 Then
		isWeb = 1
	Else
		isWeb = Request("isWeb")
		If isWeb = "" Or Not IsNumeric(isWeb) Then isWeb = 1
		If Request("submit")="网页搜索" And Not isWeb=0 Then
			isWeb=2
			sType=8
		End If
		isWeb = Cint(isWeb)
		If isWeb < 0 Or isWeb > 2 Then isWeb = 1
	End If
	SearchBoard = " "
	If Dvbbs.BoardID>0 Then SearchBoard=" BoardID="&Dvbbs.BoardID&" and "
	Dim FobWords
	'搜索过滤字
	FobWords = Array(91,92,304,305,430,431,437,438,12460,12461,12462,12463,12464,12465,12466,12467,12468,12469,12470,12471,12472,12473,12474,12475,12476,12477,12478,12479,12480,12481,12482,12483,12485,12486,12487,12488,12489,12490,12496,12497,12498,12499,12500,12501,12502,12503,12504,12505,12506,12507,12508,12509,12510,12532,12533,65339,65340)
	For i = 1 to Ubound(FobWords,1)
		If InStr(keyword,ChrW(FobWords(i))) > 0 Then
			Dvbbs.AddErrCode(61)
			Exit For
		End If
	Next
	FobWords = Array("~","!","@","#","$","%","^","&","*","(",")","_","+","=","`","[","]","{","}",";",":","""","'",",","<",">",".","/","\","|","?","_","about","1","2","3","4","5","6","7","8","9","0","a","b","c","d","e","f","g","h","i","j","k","l","m","n","o","p","q","r","s","t","u","v","w","x","y","z","after","all","also","an","and","another","any","are","as","at","be","because","been","before","being","between","both","but","by","came","can","come","could","did","do","each","for","from","get","got","had","has","have","he","her","here","him","himself","his","how","if","in","into","is","it","like","make","many","me","might","more","most","much","must","my","never","now","of","on","only","or","other","our","out","over","said","same","see","should","since","some","still","such","take","than","that","the","their","them","then","there","these","they","this","those","through","to","too","under","up","very","was","way","we","well","were","what","where","which","while","who","with","would","you","your","的","一","不","在","人","有","是","为","以","于","上","他","而","后","之","来","及","了","因","下","可","到","由","这","与","也","此","但","并","个","其","已","无","小","我","们","起","最","再","今","去","好","只","又","或","很","亦","某","把","那","你","乃","它")
	keyword = Left(keyword,100)
	keyword = Replace(keyword,"!"," ")
	keyword = Replace(keyword,"]"," ")
	keyword = Replace(keyword,"["," ")
	keyword = Replace(keyword,")"," ")
	keyword = Replace(keyword,"("," ")
	keyword = Replace(keyword,"　"," ")
	'keyword = Replace(keyword,"-"," ")
	keyword = Replace(keyword,"/"," ")
	keyword = Replace(keyword,"+"," ")
	keyword = Replace(keyword,"="," ")
	keyword = Replace(keyword,","," ")
	keyword = Replace(keyword,"'"," ")
	For i = 0 To Ubound(FobWords,1)
		If keyword=FobWords(i) Then
			Dvbbs.AddErrCode(61)
			Exit for
		End If
	Next
	If sType = 8 And (Request("submit")="站内搜索") Then sType = 2
	If sType = 8 And (Request("submit")="") Then isWeb = 2
	'Response.Write stype&Request("submit")&isweb
	'response.end
End Sub

Sub SQLQueryStr()
	Dim SearchUserID,Rs
	SqlColumn = "Select Top " & Cint(Dvbbs.Forum_Setting(11))*SearchMaxPageList
	If stype=1 And (nSearch=2 or nSearch=3) Then
		SqlColumn = SqlColumn & LCase(" BoardID,RootID,Topic,Expression,UserName,PostUserID,DateAndTime,IsBest,LockTopic,Body,AnnounceID")
	ElseIf stype=7 Then
		If IsSqlDataBase Then
		SqlColumn = SqlColumn & LCase(" T1.BoardID,T1.RootID,T1.Topic,T1.Expression,T1.UserName,T1.PostUserID,T1.DateAndTime,T1.IsBest,T1.LockTopic,T1.Body,T1.AnnounceID,T1.signflag")
		Else
		SqlColumn = SqlColumn & LCase(" BoardID,RootID,Topic,Expression,UserName,PostUserID,DateAndTime,IsBest,LockTopic,Body,AnnounceID,signflag")
		End If
	ElseIf stype=3 Then
		SqlColumn = LCase("Select Top 50 BoardID,rootid,topic,Expression,username,postuserid,dateandtime,IsBest,LockTopic,Body,Announceid")
	Else
		SqlColumn = SqlColumn & LCase(" BoardID,TopicID as RootID,Title as topic,Expression,PostUserName as UserName,PostUserID,DateAndtime,IsBest,LockTopic,Child,Hits")
		isMoreInfo = 1
	End If
	Dvbbs.Stats = template.Strings(4)
	
	'If Trim(searchday)<>"" Then
	'	Dvbbs.Stats = Dvbbs.Stats & Replace(template.Strings(5),"{$searchday}",request("SearchDate"))
	'Else
	'	Dvbbs.Stats = Dvbbs.Stats & template.Strings(6)
	'End If
	Select Case stype
	Case 1
		Set Rs=Dvbbs.Execute("Select UserID From Dv_User Where UserName='"&keyword&"'")
		If Rs.Eof And Rs.Bof Then
			Set Rs=Nothing
			Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(21)&"&action=OtherErr"
		Else
			SearchUserID = Rs(0)
		End If
		Select Case nSearch
		'主题作者
		Case 1
			If Not Dvbbs.master Then SearchUserID =SearchUserID&" And HideName=0"
			SqlColumn = SqlColumn & " From dv_Topic Where "&searchboard&" PostUserID="&SearchUserID&" Order By TopicID Desc"
			Dvbbs.Stats = Dvbbs.Stats & template.Strings(7)
		'回复作者
		Case 2
			If Not Dvbbs.master Then SearchUserID =SearchUserID&" And signflag<2"
			SqlColumn = SqlColumn & " From " & stable & " Where "&searchboard&" ParentID>0 And PostUserID="&SearchUserID&" Order By AnnounceID Desc"
			Dvbbs.Stats = Dvbbs.Stats & template.Strings(8)
		'主题和回复作者
		Case 3
			If Not Dvbbs.master Then SearchUserID =SearchUserID&" And signflag<2"
			SqlColumn = SqlColumn & " From " & stable & " Where "&searchboard&" PostUserID="&SearchUserID&" Order By AnnounceID Desc"
			Dvbbs.Stats = Dvbbs.Stats & template.Strings(9)
		End Select
	Case 2
		SearchUserID="'%"&keyword&"%'"
		If IsSqlDataBase=1 Then
			SqlColumn = SqlColumn &",HideName From dv_Topic Where "&searchboard&" Title like "&SearchUserID&" Order By TopicID Desc"
		Else
			SqlColumn = SqlColumn &",HideName From dv_Topic Where "&searchboard&" InStr(1,LCase(Title),LCase('"&keyword&"'),0)<>0 Order By TopicID Desc" 
		End if
		'Response.write SqlColumn
		Dvbbs.Stats = Dvbbs.Stats & template.Strings(10)
	Case 3
		'最新50贴
		If Dvbbs.BoardID > 0 then
			SqlColumn = SqlColumn & ",signflag From "&stable&" where BoardID="&Dvbbs.BoardID&" ORDER BY announceID desc"
		Else
			SqlColumn = SqlColumn &",signflag From "&stable&" ORDER BY announceID desc"
		End if
		Dvbbs.Stats = template.Strings(12)
	Case 4
		'用户热贴
		If keyword<>"" Then
			Set Rs=Dvbbs.Execute("Select UserID From Dv_User Where UserName='"&keyword&"'")
			If Rs.Eof And Rs.Bof Then
				Set Rs=Nothing
				Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(21)&"&action=OtherErr"
			Else
				SearchUserID = Rs(0)
			End If
		End If
		Dim HotTopicDay,HotTopicView,MyHotTopic
		If Dvbbs.Forum_Setting(13)<>"0" Then
			MyHotTopic = Split(Dvbbs.Forum_Setting(13),"|")
			If Ubound(MyHotTopic)=1 Then
				HotTopicDay = MyHotTopic(0)
				HotTopicView = MyHotTopic(1)
			Else
				HotTopicDay = 10
				HotTopicView = 200
			End If
		Else
			HotTopicDay = 10
			HotTopicView = 200
		End If
		Dvbbs.Stats = Replace(Replace(template.Strings(13),"{$daylimited}",HotTopicDay),"{$viewlimited}",HotTopicView)
		If IsSqlDataBase=1 Then
			searchday=" datediff(d,DateAndTime,"&SqlNowString&") < "&HotTopicDay&" and "
		Else
			searchday=" datediff('d',DateAndTime,"&SqlNowString&") < "&HotTopicDay&" and "
		End If
		If keyword<>"" Then
			keyword = " And PostUserID="&SearchUserID
			If Not (Dvbbs.master Or Dvbbs.SuperBoardmaster) Then keyword =keyword&" And HideName=0"
		End If
		SqlColumn = SqlColumn & ",HideName From dv_Topic Where "&searchday&" hits>"&HotTopicView&" "&keyword&" Order By TopicID Desc"
		'Response.write SqlColumn
		keyword = Dvbbs.CheckStr(Request("keyword"))
	Case 5
		'我的主题和被回复的主题
		If Dvbbs.UserID=0 Then
			Dvbbs.AddErrCode(61)
			Exit Sub
		End If
		Dim s
		s=request("s")
		If s="" Or Not IsNumerIc(s) Then s=1
		s=clng(s)
		If s=1 Then
			SqlColumn=LCase("select BoardID,TopicID as rootid,Title as topic,Expression,PostUserName as UserName,PostUserID,DateAndtime,IsBest,LockTopic,Child,Hits from Dv_Topic where Boardid<>444 And topicid in (select top 200 rootid from "&stable&" where ParentID>0 And PostUserID="&Dvbbs.UserID&" order by AnnounceID desc) order by topicid desc")
			Dvbbs.Stats = template.Strings(14)
		Else
			SqlColumn=LCase("select BoardID,TopicID as rootid ,Title as topic,Expression,PostUserName as UserName,PostUserID,DateAndtime,IsBest,LockTopic,Child,Hits from dv_topic where Boardid<>444 And postUserID="&Dvbbs.UserID&" ORDER BY topicid desc")
			Dvbbs.Stats = template.Strings(15)
		End If
		isMoreInfo = 1
	Case 6
		'版面或用户精华贴
		If keyword<>"" Then
			Set Rs=Dvbbs.Execute("Select UserID From Dv_User Where UserName='"&keyword&"'")
			If Rs.Eof And Rs.Bof Then
				Set Rs=Nothing
				Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(21)&"&action=OtherErr"
			Else
				SearchUserID = Rs(0)
			End If
		End If
		If Trim(searchboard)="" Then
			If keyword<>"" Then
				keyword = " Where PostUserID="&SearchUserID
			Else
				isWeb = 0
			End If
			SqlColumn = LCase("select BoardID,RootID,Title as topic ,Expression,PostUserName as username,PostUserID,DateAndtime,PostUserID As IsBest,PostUserID As LockTopic From dv_BestTopic ")& keyword&" Order By ID Desc"
			Else
			If keyword<>"" Then
				keyword = " And PostUserID="&SearchUserID
			End If
			SqlColumn = LCase("select BoardID,RootID,Title as topic,Expression,PostUserName as username ,PostUserID,DateAndtime,PostUserID As IsBest,PostUserID As LockTopic From dv_BestTopic Where ") & Replace(searchboard,"and","")&" "&keyword&" Order By ID Desc"
		End If
		keyword = Dvbbs.CheckStr(Request("keyword"))
		Dvbbs.Stats = template.Strings(16)
	Case 7
		'内容或全文索引
		If Dvbbs.Forum_Setting(16)<>"0" Then
			If IsSqlDataBase Then
				If Trim(searchboard)="" Then
					SqlColumn = SqlColumn & " From " & stable & " T1 Inner Join ContainsTable("&stable&",body,'""" & keyword & """'," & Dvbbs.Forum_Setting(11)*SearchMaxPageList & ") As T2 ON T1.AnnounceID = T2.[KEY] Order By T1.AnnounceID Desc"
				Else
					SqlColumn = SqlColumn & " From " & stable & " T1 Inner Join ContainsTable("&stable&",body,'""" & keyword & """'," & Dvbbs.Forum_Setting(11)*SearchMaxPageList & ") As T2 ON T1.AnnounceID = T2.[KEY] Where "&Replace(Replace(searchboard,"BoardID","T1.BoardID"),"and","")&" Order By T1.AnnounceID Desc"
				End If
			Else
				If IsSqlDataBase=1 Then
					SqlColumn = SqlColumn & " From " & stable & " Where "&searchboard&" body like '%"&keyword&"%' Order By AnnounceID Desc"
				Else
					SqlColumn = SqlColumn & " From " & stable & " Where "&searchboard&" InStr(1,LCase(body),LCase('"&keyword&"'),0)<>0 Order By AnnounceID Desc"
				End if
			End If
			Dvbbs.Stats = Dvbbs.Stats & template.Strings(11)
		Else
			 Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(19)&"&action=OtherErr"
		End If
	Case 8
		SqlColumn = "Select Top 1 id as BoardID,id as RootID,Forum_lastUser as topic,Forum_lastUser as Expression,Forum_lastUser as UserName,id as PostUserID,Forum_MaxPostDate as DateAndtime,id as IsBest,id as LockTopic,id as Child,id as Hits From Dv_setup"
		Dvbbs.Stats = Dvbbs.Stats & "搜索引擎搜索结果"
	Case Else
		Dvbbs.AddErrCode(61)
		Exit Sub
	End Select

	Dvbbs.Nav()
	If Dvbbs.BoardID=0 then
		Dvbbs.Head_var 0,0,template.Strings(0),"query.asp"
	Else
		Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
	End If
	If IsEmpty(Session("QueryLimited")) Then
		Session("QueryLimited") = keyword & "|" & stype & "|" & Now()
	Else
		If IsWeb <> 2 Then
		Dim QueryLimited
		QueryLimited = Split(Session("QueryLimited"),"|")
		If Ubound(QueryLimited) = 2 Then
			If Cstr(Trim(QueryLimited(0))) = Cstr(keyword) And Cstr(Trim(QueryLimited(1))) = Cstr(stype) Then
				Session("QueryLimited") = keyword & "|" & stype & "|" & Now()
			Else
				If DateDiff("s",QueryLimited(2),Now()) < Clng(Dvbbs.Forum_Setting(3)) And Not(Dvbbs.Master Or Dvbbs.BoardMaster Or Dvbbs.SuperBoardMaster) Then
					Response.redirect "showerr.asp?ErrCodes=<li>"&Replace(template.Strings(20),"{$timelimited}",Dvbbs.Forum_Setting(3))&"&action=OtherErr"
				Else
					Session("QueryLimited") = keyword & "|" & stype & "|" & Now()
				End If
			End If
		Else
			Session("QueryLimited") = keyword & "|" & stype & "|" & Now()
		End If
		End If
	End If
End Sub
Sub SearchResult()
	Dim Rs
	Dim Record_Count,n,sql
	SQLQueryStr()
	If Dvbbs.ErrCodes<>"" Then Exit Sub
	If Not IsObject(Conn) Then ConnectionDatabase
	Dvbbs.SqlQueryNum = Dvbbs.SqlQueryNum + 1
	Set Rs=Dvbbs.iCreateObject("adodb.recordset")
	'Response.Write SqlColumn
	Rs.Open SqlColumn,Conn,1,1
	If Err Then
		Dvbbs.AddErrCode(61)
		Exit Sub
	Else
		If Not Rs.Eof Then
			Record_Count = Rs.RecordCount
			If Record_Count Mod Cint(Dvbbs.Forum_Setting(11))=0 Then
				n = Record_Count \ Cint(Dvbbs.Forum_Setting(11))
			Else
				n = Record_Count \ Cint(Dvbbs.Forum_Setting(11))+1
			End If
			Rs.MoveFirst
			If page > n Then page = n
			If page < 1 Then page = 1
			If page > 1 Then 				
				Rs.Move (page-1) * Clng(Dvbbs.Forum_Setting(11))
			End if
			Sql = Rs.GetRows(Clng(Dvbbs.Forum_Setting(11)))
			Set XMLDOM=QueryArrayToxml(sql,rs,"","")
		ElseIf sType=5 Then
			Set XMLDOM=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
			XMLDOM.appendChild(XMLDOM.createElement("xml"))
		Else
			Dvbbs.AddErrCode(32)
			Exit Sub
		End If
	End If
	Rs.Close:Set Rs=Nothing
	Dim SearchStr
	SearchStr = "stype="& Request("stype") &"&pSearch="& Request("pSearch")&"&nSearch=" & Request("nSearch") &"&boardid="&Request("boardid")&"&SearchDate="& Request("SearchDate")&"&keyword=" &Server.urlencode(Request("keyword"))&"&s="&Request("s")&"&isWeb="&isWeb&"&stable="&stable

	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"pagecount","")).text=Record_Count
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"loginhidden","")).text=Dvbbs.GroupSetting(37)
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"page","")).text=page
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"action","")).text=action
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"pagesize","")).text=Dvbbs.Forum_Setting(11)
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"PageStr","")).text=SearchStr
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"boardid","")).text=Dvbbs.boardid
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"keyword","")).text=KeyWord
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"isWeb","")).text=isWeb
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"sType","")).text=sType
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"sqlsearch","")).text=Dvbbs.Forum_Setting(16) Rem 全文索引开关
	If sType=5 Then
		XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"s","")).text=Request("s")
	End If
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"isMoreInfo","")).text=isMoreInfo
	XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"SelSearch","")).text=SelSearch
	If Dvbbs.Master Or Dvbbs.SuperBoardMaster Then
		XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"Master","")).text=1
	Else
		XMLDOM.documentElement.attributes.setNamedItem(XMLDOM.createNode(2,"Master","")).text=0
	End If
	Dim Node,ChildNode
	Set Node=XMLDOM.createNode(1,"TableList","")
	Set Rs=Dvbbs.Execute("Select * From Dv_TableList")
	Do While Not Rs.Eof
		Set ChildNode=XMLDOM.createNode(1,"posttable","")
		ChildNode.text=Rs("tablename")&""
		ChildNode.attributes.setNamedItem(XMLDOM.createNode(2,"type","")).text=""&Rs("tabletype")
		Node.appendChild(ChildNode)
	Rs.MoveNext
	Loop
	XMLDOM.documentElement.appendChild(Node)
	Rs.Close:Set Rs=Nothing

	'Response.Write XMLDOM.xml
	'Response.End

	Dim BoardXML
	Set  BoardXML=Application(Dvbbs.CacheName&"_boardlist").cloneNode(True)
	XMLDOM.documentElement.appendChild(BoardXML.documentElement)
	Set BoardXML=Nothing
	DoShowHTML()
End Sub
Function QueryArrayToxml(DataArray,Recordset,row,xmlroot)
	Dim i,node,rs,j
	If xmlroot="" Then xmlroot="xml"
	Set QueryArrayToxml=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	QueryArrayToxml.appendChild(QueryArrayToxml.createElement(xmlroot))
	If row="" Then row="row"
	For i=0 To UBound(DataArray,2)
		Set Node=QueryArrayToxml.createNode(1,row,"")
		j=0
		'node.attributes.setNamedItem(QueryArrayToxml.createNode(2,"querynum","")).text = (i + 1) + ((Page - 1) * Dvbbs.Forum_Setting(11))

		node.setAttribute "querynum",(i + 1) + ((Page - 1) * Dvbbs.Forum_Setting(11))
		For Each rs in Recordset.Fields
			'当TOPIC为空，取BODY内容前30个字符，去除<*>标记
			If LCase(rs.name)="body" Then
				If node.attributes.getNamedItem("topic").text<>"" Then
					node.setAttribute LCase(rs.name),""
				Else
					node.setAttribute LCase(rs.name),Dvbbs.ChkBadWords(Left(Dvbbs.Replacehtml(DataArray(j,i)),30))
				End If
			Else
				If LCase(rs.name)="topic" Then
					node.setAttribute LCase(rs.name),Dvbbs.ChkBadWords(Dvbbs.Replacehtml(DataArray(j,i)&""))
				Else
					node.setAttribute LCase(rs.name),DataArray(j,i)&""
				End If
			End If
			j=j+1
		Next
		QueryArrayToxml.documentElement.appendChild(Node)
	Next
End Function

'进行批量操作
Sub Dobatch()
	If Not Dvbbs.Master Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(27)&"&action=OtherErr"
		Exit Sub
	End If
	Dim announceid
	announceid=Request("announceid")
	If announceid="" Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(23)&"&action=OtherErr"
		Exit Sub
	ElseIf Not IsNumeric(replace(replace(replace(announceid,",","")," ",""),"_","")) Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(24)&"&action=OtherErr"
		Exit Sub
	End If
	If Request("maction")="" Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(25)&"&action=OtherErr"
		Exit Sub
	ElseIf Request("maction")="move"Then
		If Request("newboard")="" Or Not IsNumeric(Request("newboard")) Then
			Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(26)&"&action=OtherErr"
			Exit Sub
		Else
			Set BoardNode=Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid="& Request("newboard") & "]")
			If BoardNode.attributes.getNamedItem("nopost").text="1" Then
				Response.redirect "showerr.asp?ErrCodes=<li>目标版面不允许发贴.无法移动&action=OtherErr"
				Exit Sub
			End If
		End If
	End If
	Dim Rs,SQL,RootID,postid,i,j,posttable,SQL1,k
	j=0
	k=0
	Dim topic,topicusername,topicuserID,datetimestr
	Dim Forum_user,BoardNode,UpdateCount,boardid,Setupreload,BordidList
	Setupreload=False
	announceid=split(announceid,",")
	ReportText="批量操作信息：<br>"
	BordidList=","
	For i=0 to UBound(announceid)
			RootID=split(announceid(i),"_")(0)
			postid=split(announceid(i),"_")(1)
			SQL="select posttable From Dv_topic Where topicID ="&RootID&""
			Set Rs=Dvbbs.execute(SQL)
			If Not Rs.EOF Then
				posttable=Rs(0)
				Set rs=Nothing
				If PostID="" Or IsNull(PostID) Then
					SQL="select * From ["&posttable&"] where RootID="&RootID&" And ParentID=0"
				Else
					SQL="select * From ["&posttable&"] where announceid="&postid
				End If
				Set Rs=Dvbbs.execute(SQL)
				If Not Rs.EOF Then
					postid = Rs("announceID")
					If Rs("BoardID")<>444 And Rs("BoardID")<>777 Then
						boardid=Rs("BoardID")
						If Not IsObject(Application(dvbbs.CacheName &"_boarddata_" & boardid)) Then Dvbbs.LoadBoardData boardid
						Forum_user = Split(Application(Dvbbs.CacheName &"_boarddata_" & boardid).documentElement.selectSingleNode("boarddata/@board_user").text,",")		
						Select Case Request("maction")
							Case "isbest"
								If Rs("ParentID")=0 Then
									ReportText=ReportText&"精华主题《"& rs("Topic") &"》(ID"& Rs(0)&").<br>"
								Else
									ReportText=ReportText&"精华跟贴(ID"& Rs(0)&").<br>"
								End If
								Dvbbs.Execute("Update "& posttable &" Set isbest=1 where announceID="&postid)
								Dvbbs.Execute("Update Dv_topic Set isbest=1 where topicID="&RootID)
								topic=rs("topic")
								topicusername=rs("username")
								topicuserID=rs("postuserID")
								If topic="" Then topic=left(replace(Dvbbs.Replacehtml(rs("body")),chr(10),","),26)
								datetimestr=replace(replace(rs("dateandtime"),"上午",""),"下午","")
								Dvbbs.Execute("Insert Into Dv_bestTopic (title,boardID,AnnounceID,rootID,postusername,postuserID,dateandtime,expression) values ('"&Dvbbs.CheckStr(topic)&"',"&rs("boardID")&","&rs("AnnounceID")&","&rs("rootID")&",'"&Dvbbs.CheckStr(topicusername)&"',"&rs("postuserID")&",'"&datetimestr&"','"&Dvbbs.CheckStr(rs("expression"))&"')")
								Dvbbs.Execute("update [Dv_user] set userWealth=userWealth+"& Forum_user(15) &",userCP=userCP+"& Forum_user(16) &",userEP=userEP+"& Forum_user(17) &",userIsBest=userisBest+1 where userid="& topicuserID)
							Case "dele"	
								If Rs("ParentID")=0 Then
										ReportText=ReportText&"删除主题《"& rs("Topic") &"》(ID"& Rs(0)&").<br>"
										Set Rs=Dvbbs.Execute("Select istop From Dv_Topic Where topicid="&RootID)
										If Rs(0)=0 Then	
											Set Rs = Dvbbs.iCreateObject("adodb.recordset")
											SQL="select * From ["&posttable&"] where RootID="&RootID&" Order by ParentID"
											rs.open sql,conn,1,3
											UpdateCount=0
											Do While not rs.eof
												UpdateCount=UpdateCount+1
												Rs("BoardID")=444
												Rs("locktopic")=BoardID
												If Rs("isbest")=1 Then
													Rs("isbest")=0
													Dvbbs.Execute("Delete From [Dv_BestTopic] Where Announceid="&Rs("Announceid"))
													Dvbbs.Execute("update [Dv_user] set userWealth=userWealth-"& Forum_user(15) &",userCP=userCP-"& Forum_user(16) &",userEP=userEP-"& Forum_user(17) &",userIsBest=userisBest-1 where userid="& rs("postuserID"))
												End If
												If Rs("ParentID")=0 Then
													Dvbbs.Execute("update [Dv_user] set userWealth=userWealth-"& Forum_user(3) &",userCP=userCP-"& Forum_user(8) &",userEP=userEP-"& Forum_user(13) &",UserTopic=UserTopic-1 where userid="& rs("postuserID"))
												Else
														Dvbbs.Execute("update [Dv_user] set userWealth=userWealth-"& Forum_user(3) &",userCP=userCP-"& Forum_user(8) &",userEP=userEP-"& Forum_user(13) &",UserPost=UserPost-1 where userid="& rs("postuserID"))
												End If
												Rs.Update
												Rs.MoveNext
												Loop
												Rs.close
												Dvbbs.Execute("Update Dv_topic Set isbest=0,BoardID=444,LockTopic="&BoardID&" where topicID="&RootID)
												Dvbbs.Execute("Update Dv_Board Set TopicNum=TopicNum-1,PostNum=PostNum-"&UpdateCount&" where BoardID="&boardID)
												Dvbbs.Execute("Update Dv_setup Set Forum_TopicNum=Forum_TopicNum-1,Forum_PostNum=Forum_PostNum-"&UpdateCount)
												Setupreload=True
												If InStr(BordidList,","&BoardId &",")=0 Then
													BordidList=BordidList&BoardID&","
												End If
										Else
											ReportText=ReportText&"该主题为固顶主题，请解除固顶后操作.<br>"
										End If
								Else
									If Rs("isbest")=1 Then
												Dvbbs.Execute("Delete [Dv_BestTopic] Where Announceid="&Rs("Announceid"))
												Dvbbs.Execute("update [Dv_user] set userWealth=userWealth-"& Forum_user(15) &",userCP=userCP-"& Forum_user(16) &",userEP=userEP-"& Forum_user(17) &",userIsBest=userisBest-1 where userid="& rs("postuserID"))
									End If
									Dvbbs.Execute("Update "& posttable &" Set isbest=0,boardid=444,LockTopic="&BoardID&" where announceID="&postid)
									UpdateCount=1
									Dvbbs.Execute("update [Dv_user] set userWealth=userWealth-"& Forum_user(3) &",userCP=userCP-"& Forum_user(8) &",userEP=userEP-"& Forum_user(13) &",UserPost=UserPost-1 where userid="& rs("postuserID"))
									Dvbbs.Execute("Update Dv_topic Set Child=Child-1 where topicID="&RootID)
									Dvbbs.Execute("Update Dv_Board Set TopicNum=TopicNum-1,PostNum=PostNum-"&UpdateCount&" where BoardID="&boardID)
									Dvbbs.Execute("Update Dv_setup Set Forum_TopicNum=Forum_TopicNum-1,Forum_PostNum=Forum_PostNum-"&UpdateCount)
									Setupreload=True
									ReportText=ReportText&"删除跟贴(ID"& Rs(0)&").<br>"
									If InStr(BordidList,","&BoardId &",")=0 Then
											BordidList=BordidList&BoardID&","
									End If
								End If
							Case "move"
								If boardid<>Clng(Request("newboard")) Then
									If Rs("ParentID")=0 Then
										ReportText=ReportText&"成功移动主题《"& rs("Topic") &"》(ID"& Rs(0)&").<br>"		
										SQL="update ["&posttable&"] Set BoardID="&Request("newboard")&" where RootID="&RootID&" and BoardID<>444 and BoardID<>777"
										Dvbbs.Execute(SQL)
										SQL="select Count(*) From ["&posttable&"]  where RootID="&RootID&" and BoardID<>444 and BoardID<>777"
										Set Rs=Dvbbs.Execute(SQL)
										UpdateCount=Rs(0)
										Dvbbs.Execute("Update Dv_topic Set BoardID="&Request("newboard")&",mode=0 where topicID="&RootID)
										Dvbbs.Execute("Update Dv_Board Set TopicNum=TopicNum-1,PostNum=PostNum-"&UpdateCount&" where BoardID="&boardID)
										Dvbbs.Execute("Update Dv_Board Set TopicNum=TopicNum+1,PostNum=PostNum+"&UpdateCount&" where BoardID="&Request("newboard"))
										If InStr(BordidList,","&BoardId &",")=0 Then
												BordidList=BordidList&BoardID&","
										End If
										If InStr(BordidList,","&Request("newboard") &",")=0 Then
												BordidList=BordidList&Request("newboard")&","
										End If
									Else
										ReportText=ReportText&"该贴(ID"& Rs(0)&")不是主题,无法移动,操作:跳过.<br>"
									End If
								Else
									ReportText=ReportText&"该贴(ID"& Rs(0)&")已经在目标版面了,无须移动,操作:跳过.<br>"
								End If
							Case "lock"
								If Rs("ParentID")=0 Then
									ReportText=ReportText&"成功锁定主题《"& rs("Topic") &"》(ID"& Rs(0)&").<br>"
									Dvbbs.Execute("update dv_Topic Set LockTopic=1 Where TopicID=" & RootID)
								Else
									ReportText=ReportText&"该贴(ID"& Rs(0)&")不是主题,无法锁定,操作:跳过.<br>"
								End If
						End Select
					Else
							ReportText=ReportText&"贴子被删除或在待审核中，跳过操作"
					End If
				End If
			End If
	Next
	SQL="insert into Dv_Log (L_AnnounceID,L_BoardID,L_ToUser,L_UserName,L_Content,L_IP,l_type) Values (0,0,'More','"&Dvbbs.Membername&"','"&Dvbbs.Checkstr(Request("maction"))&"','"&Dvbbs.UserTrueIP&"',3)"
	Dvbbs.Execute(SQL)
	Dvbbs.Dvbbs_suc(ReportText)
	If Setupreload Then Dvbbs.loadSetup
	If Len(BordidList)>1 Then 
		BordidList=Left(BordidList,Len(BordidList)-1)
		BordidList=Right(BordidList,Len(BordidList)-1)
	End If
	If Len(BordidList)>1 Then 
		Dvbbs.ReloadBoardInfo(BordidList)
	End If
End Sub

Sub isWeb_Query()
	Dvbbs.Head()
	Dim S,keyword,SelSearch
	S = Request.QueryString("s")
	keyword = Request("keyword")
	SelSearch = Request("SelSearch")
	If SelSearch <> "" Then SelSearch = Lcase(SelSearch)
	If SelSearch = "page" Then SelSearch = "massist"
	If SelSearch = "" Then SelSearch = "massist"
	If keyword = "" Then keyword = "dvbbs"

	Response.Write "<div id=""Seardata"" style=""height:500px;"">"
	Select Case Request.QueryString("t")
	Case 0
		'Response.Write "搜索引擎框架,关键字：" & request("keyword")
		Response.Write "<iframe name=""isWeb"" id=""isWebQuery"" frameborder=""0"" width=""100%"" height=""100%"" scrolling=""auto"" src=""http://so.dvbbs.net/search.asp?k="&keyword&"&s="&SelSearch&"""></iframe>"
	Case 1
		'Response.Write "搜索引擎框架,关键字：" & request("keyword")
		Response.Write "<iframe name=""isWeb"" id=""isWebQuery"" frameborder=""0"" width=""100%"" height=""100%"" scrolling=""auto"" src=""http://so.dvbbs.net/search.asp?k="&keyword&"&s="&SelSearch&"""></iframe>"
	Case 2
		Response.Write ""
	Case 3
		Response.Write ""
	Case 4
		Response.Write ""
	End Select
	Response.Write "</div>"
%>
<script language="JavaScript">
<!--
var obj=parent.document.getElementById("<%=s%>");
var SearchData = document.getElementById("Seardata");
if (obj){
	if (obj.style.display=='none'){
		obj.style.display='';
		//alert(document.getElementById("SearchMain1").offsetHeight);
	}
	//obj.style.height=(parent.document.getElementById("SearchMain1").offsetHeight)+'px';
	if (!document.all){obj.style.height="700px";}
	obj.innerHTML = SearchData.innerHTML;
}

//-->
</script>
<%
End Sub
%>
