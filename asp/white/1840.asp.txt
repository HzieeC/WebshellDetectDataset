<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/dv_clsother.asp"-->
<!--#include file="inc/dv_ubbcode.asp"-->
<!--#include file="inc/md5.asp"-->
<%
Const PageSize=50
Dim XMLDOM,EmotPath,UserName,PostBuyUser,replyid_a,RootID_a,AnnounceID_a
Dim dv_ubb
Dim T_GetMoneyType,T_GetMoney,T_UseTools
If  IpInList Then
 Select Case Request("action") 
 	Case "","0"
 		GetPostData()
 	Case "1"
 		GetBestTopic()
 	Case "2"
 		hotTopic()
	Case "3"
		GetUserData()
	Case "4"
		GetForumInfo()
	Case "5"
		GetForumPic()
 End Select
End If
Dvbbs.PageEnd()
Sub hotTopic()
	Dim Rs,SQL,SQL1,Rs1,page,PageCount,node,node1,blist,TopicNum
	blist=boardlists()
	page=Request("tid")
	If Not IsNumeric(Page) Then Page="1"
	If Page="" Then Page="1"
	Page=CLng(Page)
	Set Rs=Dvbbs.iCreateObject("Adodb.RecordSet")
	If IsSqlDataBase = 1 Then
		SQL="Select TopicID,Title,PostUsername,hidename,boardid,child,hits,dateandtime,lastposttime,istop,isvote,isbest From Dv_topic Where Boardid in ("& blist &") and Datediff(d,LastPostTime, " & SqlNowString & ") < 5 order by hits Desc"
		'SQL1="Select count(*) From Dv_topic Where Boardid in("& blist &") and Datediff(d,LastPostTime, " & SqlNowString & ") < 15 And hits > 20"
	Else
		SQL="Select TopicID,Title,PostUsername,hidename,boardid,child,hits,dateandtime,lastposttime,istop,isvote,isbest From Dv_topic Where Boardid in("& blist &") and Datediff('d',LastPostTime, " & SqlNowString & ") < 5 order by hits Desc"
		'SQL1="Select count(*) From Dv_Topic Where Boardid in ("& blist &") and Datediff('d',LastPostTime, " & SqlNowString & ") < 15 And hits > 20"
	End If
	'Set Rs1=Dvbbs.Execute(SQL1)
	'计算一下当前Page参数是否合法。如果超出范围，强制为最后一页
	If Not IsObject(Conn) Then ConnectionDatabase
	Rs.Open Sql,Conn,1,1
	If Rs.Eof And Rs.Bof Then Exit Sub
	TopicNum = Rs.Recordcount
	If TopicNum mod Pagesize =0 then
			PageCount= TopicNum \ Pagesize
	Else
			PageCount= TopicNum \ Pagesize+1
	End If
	If Page > PageCount Then Page=PageCount
	Set XMLDOM=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XMLDOM.appendChild(XMLDOM.createElement("hotlist"))
	Set node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"listinfo",""))
	Node.attributes.setNamedItem(XMLDom.createNode(2,"count","")).text =TopicNum
	Node.attributes.setNamedItem(XMLDom.createNode(2,"page","")).text=Page
	Node.attributes.setNamedItem(XMLDom.createNode(2,"pagesize","")).text=pagesize
	Node.attributes.setNamedItem(XMLDom.createNode(2,"lastpage","")).text=PageCount
	If TopicNum <> 0  and Not IsNull(TopicNum)Then
		'Set Rs=Dvbbs.Execute(SQL)
		If Not page=1 Then 	Rs.Move(pagesize*(page-1))
		SQL=RS.GetRows(Pagesize)
		Set Node=Dvbbs.ArrayToxml(SQL,rs,"row","datarows")
		For each node1 in node.documentElement.selectNodes("row")
			If node1.selectSingleNode("@hidename").text="1" Then
				node1.selectSingleNode("@postusername").text="匿名用户"
			End If
		Next
		XMLDom.documentElement.appendChild(node.documentElement)
		Rs.Close:Set Rs=Nothing
	End If
	'Rs1.Close:Set Rs1=Nothing
	Response.Clear
	Select Case Session.CodePage
			 	Case 65001
			 		Response.CharSet="utf-8" 
			 		Response.Write "<?xml version=""1.0"" encoding=""utf-8""?>"&vbNewLine
			 	Case 936
			 		Response.CharSet="gb2312"
			 		Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"&vbNewLine 
			 	Case 950
			 		Response.CharSet="big5" 
			 		Response.Write "<?xml version=""1.0"" encoding=""big5""?>"&vbNewLine
			End Select
	Response.ContentType="text/xml"
	
	Response.Write XMLDom.documentElement.XML
End Sub
Function boardlists()
	Dim xml,node,blist
	Set XML=Application(Dvbbs.CacheName&"_boardlist").cloneNode(True)
	If Dvbbs.GroupSetting(37)="0"  Then
		For each node in XML.documentElement.selectNodes("board[@hidden=1 or @checkout=1]")
				XML.documentElement.removeChild(node)
		Next
	Else
		For each node in XML.documentElement.selectNodes("board[@checkout=1]")
				XML.documentElement.removeChild(node)
		Next
	End If
	blist=""
	For each node in XML.documentElement.selectNodes("board")
		If blist="" Then
			blist=Node.selectSingleNode("@boardid").text
		Else
			blist=blist&","&Node.selectSingleNode("@boardid").text
		End If
	Next
	boardlists=blist
End Function
Sub GetBestTopic()
Dim Rs,SQL,SQL1,Rs1,page,PageCount,node,node1
	page=Request("tid")
	If Not IsNumeric(Page) Then Page="1"
	If Page="" Then Page="1"
	Page=CLng(Page)
	SQL="Select TopicID,Title,PostUsername,hidename,boardid,child,hits,dateandtime,lastposttime,istop,isvote,isbest From Dv_topic Where isbest=1  order by topicid Desc"
	SQL1="Select count(*) From Dv_topic Where isbest=1"
	Set Rs1=Dvbbs.Execute(SQL1)
		'计算一下当前Page参数是否合法。如果超出范围，强制为最后一页
	If Rs1(0) mod Pagesize =0 then
			PageCount= Rs1(0) \ Pagesize
	Else
			PageCount= Rs1(0) \ Pagesize+1
	End If
	If Page > PageCount Then Page=PageCount
	Set XMLDOM=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XMLDOM.appendChild(XMLDOM.createElement("bestlist"))
	Set node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"listinfo",""))
	Node.attributes.setNamedItem(XMLDom.createNode(2,"count","")).text =Rs1(0)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"page","")).text=Page
	Node.attributes.setNamedItem(XMLDom.createNode(2,"pagesize","")).text=pagesize
	Node.attributes.setNamedItem(XMLDom.createNode(2,"lastpage","")).text=PageCount
	If Rs1(0) <> 0  and Not IsNull(Rs1(0))Then
		Set Rs=Dvbbs.Execute(SQL)
		If Not page=1 Then 	Rs.Move(pagesize*(page-1))
		SQL=RS.GetRows(Pagesize)
		Set Node=Dvbbs.ArrayToxml(SQL,rs,"row","datarows")
		For each node1 in node.documentElement.selectNodes("row")
			If node1.selectSingleNode("@hidename").text="1" Then
				node1.selectSingleNode("@postusername").text="匿名用户"
			End If
		Next
		XMLDom.documentElement.appendChild(node.documentElement)
		Rs.Close:Set Rs=Nothing
	End If
	Rs1.Close:Set Rs1=Nothing
	Response.Clear
	Select Case Session.CodePage
			 	Case 65001
			 		Response.CharSet="utf-8" 
			 		Response.Write "<?xml version=""1.0"" encoding=""utf-8""?>"&vbNewLine
			 	Case 936
			 		Response.CharSet="gb2312"
			 		Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"&vbNewLine 
			 	Case 950
			 		Response.CharSet="big5" 
			 		Response.Write "<?xml version=""1.0"" encoding=""big5""?>"&vbNewLine
			End Select
	Response.ContentType="text/xml"
	Response.Write XMLDom.documentElement.XML
End Sub
Sub GetPostData()
	Set XMLDOM=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XMLDOM.Load Server.MapPath(MyDbPath &"inc\templates\bbsinfo.xml")
	XMLDOM.documentElement.selectSingleNode("LastTid").text=GetLastPostID()
	XMLDOM.documentElement.selectSingleNode("Version").text=fVersion
	XMLDOM.documentElement.selectSingleNode("BBSName").text=Server.HtmlEncode(Dvbbs.Forum_info(0) & "")
	XMLDOM.documentElement.selectSingleNode("BBSURL").text=Dvbbs.Get_ScriptNameUrl()
	Dim tid,rs,tRs,node,boardid,RootID,posttable,SQL,node1,getmoneytype,body,Node2
	tid=Request("tid")
	If IsNumeric(tid) and tid<>"" Then
		Set Rs=Dvbbs.Execute("select * From dv_topic Where topicid="& tid)
		If Not Rs.EOF Then
			Set node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"ChannelInfo",""))
			If rs("boardid")<>444 and Rs("boardid")<> 777 Then
				boardid=Rs("boardid")
			Else
				boardid=rs("locktopic")
			End If
			Node.appendChild(XMLDOM.createNode(1,"ForumName","")).text=Server.HtmlEncode(Getbbsname(boardid) & "")
			Node.appendChild(XMLDOM.createNode(1,"ForumUrl","")).text=Dvbbs.Get_ScriptNameUrl()&"index.asp?boardid="&boardid
			Node.appendChild(XMLDOM.createNode(1,"ForumType","")).text="科技"
			 Select Case Session.CodePage
			 	Case 65001
			 		Node.appendChild(XMLDOM.createNode(1,"Language","")).text="utf-8"
			 	Case 936
			 		Node.appendChild(XMLDOM.createNode(1,"Language","")).text="GBK"
			 	Case 950
			 		Node.appendChild(XMLDOM.createNode(1,"Language","")).text="Big5"
			End Select
			Node.appendChild(XMLDOM.createNode(1,"ForumID","")).text=BoardID
			Set node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"StatusElement",""))
			Node.appendChild(XMLDOM.createNode(1,"Link","")).text=Dvbbs.Get_ScriptNameUrl()&"dispbbs.asp?boardid="&boardid&"&id="&Rs("topicid")
			Node.appendChild(XMLDOM.createNode(1,"ReplyTime","")).text=DateDiff("s","1970-01-01",Rs("LastPostTime"))
			Node.appendChild(XMLDOM.createNode(1,"PostTime","")).text=DateDiff("s","1970-01-01",Rs("DateAndTime"))
			Node.appendChild(XMLDOM.createNode(1,"ReplyNum","")).text=Rs("Child")
			Node.appendChild(XMLDOM.createNode(1,"ViewNum","")).text=Rs("hits")
			If Rs("istop")=0 Then
				Node.appendChild(XMLDOM.createNode(1,"Sticky","")).text="false"
			Else
				Node.appendChild(XMLDOM.createNode(1,"Sticky","")).text="true"
			End If
			If Rs("isbest")=1 Then
				Node.appendChild(XMLDOM.createNode(1,"Digest","")).text="true"
			Else
				Node.appendChild(XMLDOM.createNode(1,"Digest","")).text="false"
			End If
			If Rs("boardid")=444 Then
				Node.appendChild(XMLDOM.createNode(1,"Deleted","")).text="true"
			Else
				Node.appendChild(XMLDOM.createNode(1,"Deleted","")).text="false"
			End If
			If Rs("locktopic") > 2 and rs("boardid")<>444 and Rs("boardid")<> 777 Then
				Node.appendChild(XMLDOM.createNode(1,"Locked","")).text="true"
			Else
				Node.appendChild(XMLDOM.createNode(1,"Locked","")).text="false"
			End If
			If (Dvbbs.GroupSetting(2)="1"  and GetBoardhidden(boardid)=0 and checkoutbaord(Boardid)=0 and Rs("getmoneytype") <> 3) Then
				Node.appendChild(XMLDOM.createNode(1,"Access","")).text="true"
			Else
				Node.appendChild(XMLDOM.createNode(1,"Access","")).text="false"
			End If
			RootID=Rs("topicid")
			posttable=Rs("posttable")
			getmoneytype=Rs("getmoneytype")
			Dvbbs.LoadTemplates("")
			EmotPath=Split(Dvbbs.Forum_emot,"|||")(0)		'em心情路径
			Set dv_ubb=new Dvbbs_UbbCode
			dv_ubb.PostType=1
			SQL="Select top 50 AnnounceID,UserName,Topic,body,signflag,isbest,PostUserid,GetMoneyType,rootid,Ubblist,LockTopic,GetMoney,UseTools,PostBuyUser,ParentID,IsUpload From "& posttable &" where  RootID="& RootId &" and Boardid="& Boardid&" Order By ParentID,dateandtime"
			Set Rs=Dvbbs.Execute(SQL)
			If Not Rs.EOF Then
				Do while Not Rs.EOF
				If Rs("ParentID")=0 Then
					Set node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"PageInfo",""))
				Else
					Set node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"ReplyInfo",""))
				End If
				Set node1=node.appendChild(XMLDOM.createNode(1,"ContentElement",""))
				Node1.appendChild(XMLDOM.createNode(1,"Title","")).text=Server.HtmlEncode(Dvbbs.ChkBadWords(Rs("topic")))
				'If Rs("isbest")=0 or Dvbbs.GroupSetting(41)="1" Then
					If Rs("LockTopic")=0 Then
						body=Dvbbs.ChkBadWords(Rs("body"))
						UserName=Rs("username")
						PostBuyUser=rs("postbuyuser")
						ReplyID_a=rs("announceid")
						RootID_a=rs("rootid")
						AnnounceID_a=ReplyID_a
						Ubblists=Rs("ubblist")&""
						If InStr(Ubblists,",39,") > 0  Then
									body = dv_ubb.Dv_UbbCode(body,10,1,0)
						Else
									body = dv_ubb.Dv_UbbCode(body,10,1,1) 
						End If
					Else
						body="内容被限制或禁止"
					End If
				'Else
				'	body="精华贴,需要权限方可查看"
				'End If
				body=replace(body,"]]>","]]&gt;")
				Node1.appendChild(XMLDOM.createNode(1,"Content","")).appendChild(XMLDOM.createCDATASection(body))
				If Rs("signflag")=2 Then
					Node1.appendChild(XMLDOM.createNode(1,"Author","")).text=""
				Else
					Node1.appendChild(XMLDOM.createNode(1,"Author","")).text=Server.HtmlEncode(Dvbbs.ChkBadWords(Rs("username")))
				End If
				If Rs("IsUpload") = 1 Then
					Set tRs = Dvbbs.Execute("Select F_FileName,F_FileType,F_FileSize From Dv_Upfile Where F_AnnounceID='"&Rs("RootID")&"|"&Rs("AnnounceID")&"'")
					Do While Not tRs.Eof
						Set node2=node1.appendChild(XMLDOM.createNode(1,"Attachment",""))
						node2.appendChild(XMLDOM.createNode(1,"URL","")).text=Dvbbs.Get_ScriptNameUrl() & Dvbbs.Forum_Setting(76) & tRs(0)
						Select Case tRs(1)
						Case "asf","asr","asx"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="video/x-ms-" & tRs(1)
						Case "bmp","gif","png"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="image/" & tRs(1)
						Case "tif","tiff"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="image/tiff"
						Case "jpe","jpeg","jpg"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="image/jpeg"
						Case "mpe","mpeg","mpg"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="video/mpeg"
						Case "css"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="text/css"
						Case "txt"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="text/plain"
						Case "js"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="text/javascript"
						Case "avi"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="video/x-msvideo"
						Case "zip","rar"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="application/" & tRs(1)
						Case "ppt","pps","pot"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="application/vnd.ms-powerpoint"
						Case "ra","ram","rm","rmvb"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="audio/x-pn-realaudio"
						Case "swf","fla"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="application/x-shockwave-flash"
						Case "doc","dot"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="application/msword"
						Case "xla","xlc","xlm","xls","xlw"
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="application/vnd.ms-excel"
						Case Else
							node2.appendChild(XMLDOM.createNode(1,"MIMEType","")).text="application/" & tRs(1)
						End Select
						node2.appendChild(XMLDOM.createNode(1,"FileName","")).text=Split(tRs(0),"/")(Ubound(Split(tRs(0),"/")))
						node2.appendChild(XMLDOM.createNode(1,"Size","")).text=tRs(2)
						node2.appendChild(XMLDOM.createNode(1,"Description","")).text=Server.HtmlEncode(Dvbbs.ChkBadWords(Rs("topic"))&"")
					tRs.MoveNext
					Loop
					tRs.Close:Set tRs=Nothing
				End If
				Rs.MoveNext
				Loop	
			End If
		End If
		Rs.Close:Set Rs=Nothing
	End If
	Response.Clear
	Select Case Session.CodePage
			 	Case 65001
			 		Response.CharSet="utf-8" 
			 		Response.Write "<?xml version=""1.0"" encoding=""utf-8""?>"&vbNewLine
			 	Case 936
			 		Response.CharSet="gb2312"
			 		Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"&vbNewLine 
			 	Case 950
			 		Response.CharSet="big5" 
			 		Response.Write "<?xml version=""1.0"" encoding=""big5""?>"&vbNewLine
			End Select
	Response.ContentType="text/xml"
	Response.Write XMLDom.documentElement.XML
End Sub
Sub GetUserData()
	Dim iUserID,Rs,Sql,node,node1,blist
	iUserID = Request("tid")
	If iUserID = "" Or Not IsNumeric(iUserID) Then Exit Sub
	Set Rs=Dvbbs.Execute("Select UserID,UserName,UserEmail,UserPost,UserTopic,UserSex,UserFace,UserWidth,UserHeight,JoinDate,LastLogin,UserLogins,Lockuser,Userclass,UserGroupID,UserGroup,userWealth,userEP,userCP,UserPower,UserBirthday,UserLastIP,UserDel,UserIsBest,IsChallenge,UserMobile,TitlePic,UserTitle,UserAnswer From Dv_User Where UserID = " & iUserID)
	If Rs.Eof And Rs.Bof Then
		Rs.Close:Set Rs=Nothing
		Exit Sub
	Else
		Set XMLDOM=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
		XMLDOM.appendChild(XMLDOM.createElement("userinfo"))
		Set node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"userencode",""))
		Node.attributes.setNamedItem(XMLDom.createNode(2,"encodestr","")).text = MD5(Rs("UserAnswer") & ":" & FormatDateTime(Rs("JoinDate"),2),32)
		SQL=RS.GetRows(1)
		Set Node=Dvbbs.ArrayToxml(SQL,rs,"row","datarows")
		For each node1 in node.documentElement.selectNodes("row")
			node1.selectSingleNode("@useranswer").text="加密字段"
		Next
		XMLDom.documentElement.appendChild(node.documentElement)
	End If
	Rs.Close:Set Rs=Nothing
	Response.Clear
	Select Case Session.CodePage
		Case 65001
			Response.CharSet="utf-8" 
			Response.Write "<?xml version=""1.0"" encoding=""utf-8""?>"&vbNewLine
		Case 936
			Response.CharSet="gb2312"
			Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"&vbNewLine 
		Case 950
			Response.CharSet="big5" 
			Response.Write "<?xml version=""1.0"" encoding=""big5""?>"&vbNewLine
	End Select
	Response.ContentType="text/xml"
	Response.Write XMLDom.documentElement.XML
End Sub
Sub GetForumInfo()
	Dim Rs,Sql,node,node1,blist,iUserID
	blist=boardlists()
	iUserID = Request("tid")
	If iUserID = "" Or Not IsNumeric(iUserID) Then iUserID = 0
	iUserID = cCur(iUserID)
	Set XMLDOM=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XMLDOM.appendChild(XMLDOM.createElement("foruminfo"))
	Set node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"baseforuminfo",""))
	Node.attributes.setNamedItem(XMLDom.createNode(2,"forumname","")).text = Dvbbs.Forum_Info(0)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"forumurl","")).text = Dvbbs.Forum_Info(1)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"homename","")).text = Dvbbs.Forum_Info(2)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"homeurl","")).text = Dvbbs.Forum_Info(3)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"systememail","")).text = Dvbbs.Forum_Info(5)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"topicnum","")).text = Dvbbs.CacheData(7,0)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"postnum","")).text = Dvbbs.CacheData(8,0)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"todaynum","")).text = Dvbbs.CacheData(9,0)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"yestodaynum","")).text = Dvbbs.CacheData(11,0)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"usernum","")).text = Dvbbs.CacheData(10,0)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"maxonline","")).text = Dvbbs.CacheData(5,0)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"maxpost","")).text = Dvbbs.CacheData(12,0)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"uploadpath","")).text = Dvbbs.Forum_Setting(76)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"forumversion","")).text = "8.3.0"

	If iUserID > 0 Then
		Set Rs=Dvbbs.Execute("Select UserID,UserName,UserEmail,UserPost,UserTopic,UserSex,UserFace,UserWidth,UserHeight,JoinDate,LastLogin,UserLogins,Lockuser,Userclass,UserGroupID,UserGroup,userWealth,userEP,userCP,UserPower,UserBirthday,UserLastIP,UserDel,UserIsBest,IsChallenge,UserMobile,TitlePic,UserTitle,UserAnswer From Dv_User Where UserID = " & iUserID)
		If Not (Rs.Eof And Rs.Bof) Then
			Set node=XMLDOM.documentElement.appendChild(XMLDOM.createNode(1,"userencode",""))
			Node.attributes.setNamedItem(XMLDom.createNode(2,"encodestr","")).text = MD5(Rs("UserAnswer") & ":" & FormatDateTime(Rs("JoinDate"),2),32)
			SQL=RS.GetRows(1)
			Set Node=Dvbbs.ArrayToxml(SQL,rs,"row","userdatarows")
			For each node1 in node.documentElement.selectNodes("row")
				node1.selectSingleNode("@useranswer").text="加密字段"
			Next
			XMLDom.documentElement.appendChild(node.documentElement)
		End If
	End If

	SQL="Select Top 12 TopicID,Title,PostUsername,hidename,boardid,child,hits,dateandtime,lastposttime,istop,isvote,isbest From Dv_topic Where isbest=1 order by topicid Desc"
	Set Rs=Dvbbs.Execute(SQL)
	SQL=RS.GetRows(-1)
	Set Node=Dvbbs.ArrayToxml(SQL,rs,"row","bestdatarows")
	For each node1 in node.documentElement.selectNodes("row")
		If node1.selectSingleNode("@hidename").text="1" Then
			node1.selectSingleNode("@postusername").text="匿名用户"
		End If
	Next
	XMLDom.documentElement.appendChild(node.documentElement)
	If IsSqlDataBase = 1 Then
		SQL="Select top 12 TopicID,Title,PostUsername,hidename,boardid,child,hits,dateandtime,lastposttime,istop,isvote,isbest From Dv_topic Where Boardid in ("& blist &") and Datediff(d,LastPostTime, " & SqlNowString & ") < 5 order by hits Desc"
	Else
		SQL="Select top 12 TopicID,Title,PostUsername,hidename,boardid,child,hits,dateandtime,lastposttime,istop,isvote,isbest From Dv_topic Where Boardid in("& blist &") and Datediff('d',LastPostTime, " & SqlNowString & ") < 5 order by hits Desc"
	End If
	Set Rs=Dvbbs.Execute(SQL)
	SQL=RS.GetRows(-1)
	Set Node=Dvbbs.ArrayToxml(SQL,rs,"row","hotdatarows")
	For each node1 in node.documentElement.selectNodes("row")
		If node1.selectSingleNode("@hidename").text="1" Then
			node1.selectSingleNode("@postusername").text="匿名用户"
		End If
	Next
	XMLDom.documentElement.appendChild(node.documentElement)
	SQL="Select Top 12 TopicID,Title,PostUsername,hidename,boardid,child,hits,dateandtime,lastposttime,istop,isvote,isbest From Dv_topic Where Not BoardID In (444,777) order by topicid Desc"
	Set Rs=Dvbbs.Execute(SQL)
	SQL=RS.GetRows(-1)
	Set Node=Dvbbs.ArrayToxml(SQL,rs,"row","newdatarows")
	For each node1 in node.documentElement.selectNodes("row")
		If node1.selectSingleNode("@hidename").text="1" Then
			node1.selectSingleNode("@postusername").text="匿名用户"
		End If
	Next
	XMLDom.documentElement.appendChild(node.documentElement)
	Rs.Close
	Set Rs=Nothing

	Response.Clear
	Select Case Session.CodePage
		Case 65001
			Response.CharSet="utf-8" 
			Response.Write "<?xml version=""1.0"" encoding=""utf-8""?>"&vbNewLine
		Case 936
			Response.CharSet="gb2312"
			Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"&vbNewLine 
		Case 950
			Response.CharSet="big5" 
			Response.Write "<?xml version=""1.0"" encoding=""big5""?>"&vbNewLine
	End Select
	Response.ContentType="text/xml"
	Response.Write XMLDom.documentElement.XML
End Sub
Sub GetForumPic()
	Dim Rs,Sql,node,node1,blist,ForumID
	blist=boardlists()
	Set XMLDOM=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XMLDOM.appendChild(XMLDOM.createElement("forumpic"))
	SQL="Select Top 50 * From Dv_upfile Where f_BoardID in ("&blist&") and f_announceid<>'0' order by f_id Desc"
	Set Rs=Dvbbs.Execute(SQL)
	SQL=RS.GetRows(-1)
	Set Node=Dvbbs.ArrayToxml(SQL,rs,"row","datarows")
	XMLDom.documentElement.appendChild(node.documentElement)

	Rs.Close
	Set Rs=Nothing

	Response.Clear
	Select Case Session.CodePage
		Case 65001
			Response.CharSet="utf-8" 
			Response.Write "<?xml version=""1.0"" encoding=""utf-8""?>"&vbNewLine
		Case 936
			Response.CharSet="gb2312"
			Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"&vbNewLine 
		Case 950
			Response.CharSet="big5" 
			Response.Write "<?xml version=""1.0"" encoding=""big5""?>"&vbNewLine
	End Select
	Response.ContentType="text/xml"
	Response.Write XMLDom.documentElement.XML
End Sub
Function GetparentBoard(bid)
	Dim Node
	Set Node=Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid="&bid&"]/@parentid")
	If Not Node is Nothing Then
		If Node.text<>"0" Then
			If Not Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid="&Node.text&"]/@boardtype") Is Nothing Then
				GetparentBoard=Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid="&Node.text&"]/@boardtype").text
			End If
		End If
	End If
End Function
Function checkoutbaord(bid)
Dim Node
	Set Node=Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid="&bid&"]")
	If Not Node is Nothing Then
		If Node.selectSingleNode("@checkout").text="0" Then
			checkoutbaord=0
		End If
	End If
End Function
Function GetBoardhidden(bid)
	Dim Node
	Set Node=Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid="&bid&"]")
	If Not Node is Nothing Then
		If Node.selectSingleNode("@hidden").text="0" Then
			GetBoardhidden=0
		End If
	End If
End Function
Function Getbbsname(bid)
	Dim Node
	Set Node=Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid="&bid&"]/@boardtype")
	If Not Node is Nothing Then
		Getbbsname=Node.text
	End If
End Function
Function GetLastPostID()
	Dim Rs
	Set Rs=Dvbbs.Execute("Select Max(topicID) From Dv_topic")
	If IsNull(rs(0)) Then
		GetLastPostID=0
	Else
		GetLastPostID=Rs(0)
	End If
End Function
Function IpInList()
	Ipinlist=False
	If not IsObject(Application(Dvbbs.CacheName & "_iplist")) Then
		SendData()
	ElseIf DateDiff("D",Application(Dvbbs.CacheName & "_iplist").documentElement.selectSingleNode("@date").text,Date())<> 0 Then
		SendData()
	End If
	Dim ip,iparray
	ip=Request.ServerVariables("REMOTE_ADDR")
	iparray=split(ip,".")
	If UBound(iparray)=3 Then
		If Not Application(Dvbbs.CacheName & "_iplist").documentElement.selectSingleNode("ip[.='"&iparray(0)&".*.*.*']") Is Nothing Then
			Ipinlist=True
		ElseIf Not Application(Dvbbs.CacheName & "_iplist").documentElement.selectSingleNode("ip[.='"&iparray(0)&"."&iparray(1)&".*.*']") Is Nothing Then
			Ipinlist=True
		ElseIf Not Application(Dvbbs.CacheName & "_iplist").documentElement.selectSingleNode("ip[.='"&iparray(0)&"."&iparray(1)&"."&iparray(2)&".*']") Is Nothing Then
			Ipinlist=True
		ElseIf Not Application(Dvbbs.CacheName & "_iplist").documentElement.selectSingleNode("ip[.='"&iparray(0)&"."&iparray(1)&"."&iparray(2)&"."&iparray(3)&"']") Is Nothing Then
			Ipinlist=True	
		End If
	End If
End Function
Function strAnsi2Unicode(asContents)
	Dim len1,i,varchar,varasc
	strAnsi2Unicode = ""
	len1=LenB(asContents)
	If len1=0 Then Exit Function
	  For i=1 to len1
	  	varchar=MidB(asContents,i,1)
	  	varasc=AscB(varchar)
	  	If varasc > 127  Then
	  		If MidB(asContents,i+1,1)<>"" Then
	  			strAnsi2Unicode = strAnsi2Unicode & chr(ascw(midb(asContents,i+1,1) & varchar))
	  		End If
	  		i=i+1
	     Else
	     	strAnsi2Unicode = strAnsi2Unicode & Chr(varasc)
	     End If	
	  Next
End Function
Sub SendData()
	Dim xmlhttp,xml,DataToSend,xmlserverurl
  On Error Resume Next
  Set xmlhttp = Dvbbs.iCreateObject("MSXML2.ServerXMLHTTP"&MsxmlVersion)
	xmlserverurl="http://server.dvbbs.net/dvbbs/iplist.asp"
	xmlhttp.setTimeouts 65000, 65000, 65000, 65000
  xmlhttp.Open "POST",xmlserverurl,false
  xmlhttp.setRequestHeader "Content-Type", "application/x-www-form-urlencoded"
  xmlhttp.send
  Set XML=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
  If XML.loadxml(strAnsi2Unicode(xmlhttp.responseBody)) Then
  	Xml.documentElement.selectSingleNode("@date").text=Date()
		Set Application(Dvbbs.CacheName & "_iplist")=Xml.cloneNode(true)
	End If
	Set xmlhttp = Nothing
End Sub
%>