<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<%
If Not Dvbbs.master  and Not Dvbbs.Superboardmaster Then Response.redirect "showerr.asp?ErrCodes=<li>您没有权限浏览本页。&action=OtherErr"
Server.ScriptTimeOut=999999
Dim maxcount
Dvbbs.LoadTemplates("query")
Dim XMLDom,TableList
Dim bid,keyword,tableid,posttable
Dvbbs.stats="回收站管理"
Dvbbs.nav()
Dvbbs.Head_var 2,0,"",""
LoadTableList()
If Not Dvbbs.ChkPost() Then Dvbbs.AddErrCode(16)
Dvbbs.Showerr
If Request("action")="manage" Then
	Manage_Main()
ElseIf Request("action")="view" Then
	View()
ElseIf Request("action")="delpic" Then
	If Not Dvbbs.Master Then
		Response.redirect "showerr.asp?ErrCodes=<li>只有管理员才可以在回收站中删除贴子.</li>&action=OtherErr"
		Response.End
	Else
		delpic()
	End If
ElseIf Request("action")="alldel" Then
	If session("flag")<>"" Or Not Dvbbs.Master Then 
		alldel
	Else
		Response.redirect "showerr.asp?ErrCodes=<li>本操作要管理员身份并登录了后台放可完成。</li>&action=OtherErr"
		Response.End
	End If
Else
	Main()
End If
ShowHTML()
If Request.Form("maxcount")<>"" and Request("action")="alldel" Then
	DoDel()
End If
Dvbbs.footer()
Dvbbs.PageEnd()
Sub Dodel()
	If Request.form("dv_submit")="" Then Exit Sub
	Dim i,Rs,SQL,k,isvote,PollID
	Response.Flush
	Response.Write "<script language=""javascript"" type=""text/javascript"">document.getElementById('dv_submit').disabled=true;document.getElementById('dv_submit').value='继 续';plantext('正在清空回收站，请耐心等待，中途不要离开本页面。');</script>"
	Response.Flush
	Set Rs=Dvbbs.Execute("SELECT Top " & Maxcount & " TopicId, PostTable,isvote,PollID From Dv_Topic Where BoardId = 444 ORDER BY TopicId")
	If Rs.Eof Then
		DelAllRepost()
		Response.Write "<script language=""javascript"" type=""text/javascript"">plantext('回收站中已经清空');</script>"
		Response.Flush
	Else
		SQL=Rs.GetRows(-1)
		maxcount=UBound(SQL,2)+1
		k = maxcount \ 100
		For i=0 to maxcount-1
			isvote=SQL(2,i)
			PollID=SQL(3,i)
			Dvbbs.Execute("DELETE FROM " & Sql(1,i) & " WHERE RootId = " & Sql(0,i))
			Dvbbs.Execute("Delete From Dv_Appraise Where TopicID=" & Sql(0,i))
			Dvbbs.Execute("DELETE From Dv_Topic WHERE TopicId = " & Sql(0,i))
			If isvote=1 Then
					Dvbbs.Execute("delete From Dv_vote  Where voteid="& PollID &"")
			End If
			Response.Write "<script language=""javascript"" type=""text/javascript"">plantext('删除第"&i+1&"个主题');</script>"
			Response.Flush
			If k > 0 Then 
				If (i+1) mod k = 0 Or (i+1)= maxcount Then
					Response.Write "<script language=""javascript"" type=""text/javascript"">planwidth("&Int(((i+1)/ maxcount )*100)&");</script>"
					Response.Flush
				End If
			Else
				If (i+1)= maxcount Then
					Response.Write "<script language=""javascript"" type=""text/javascript"">planwidth("&Int(((i+1)/ maxcount )*100)&");</script>"
					Response.Flush
				End If
			End If
		Next
		Set Rs=Dvbbs.Execute("SELECT Count(*) From Dv_Topic Where BoardId = 444")
		If Not IsNull(Rs(0)) and Rs(0) <> 0 Then
			Response.Write "<script language=""javascript"" type=""text/javascript"">plantext('本次共删除"&i&"个主题，检测到回收站中还有 "&Rs(0)&" 个主题。');</script>"
			Tolog("删除"&i&"个主题")
		Else
			Tolog("删除"&i&"个主题，主题回收站被清空。")
			DelAllRepost()
			Response.Write "<script language=""javascript"" type=""text/javascript"">plantext('本次共删除"&i&"个主题，回收站已经清空！');</script>"
			Response.Flush
		
		End If
		Response.Write "<script language=""javascript"" type=""text/javascript"">document.getElementById('dv_submit').disabled=false;</script>"
		Response.Flush
	End If
End Sub
Sub DelAllRepost()
	Dim i,tmp
	For i= 0 to UBound(TableList,2)
		Conn.Execute "Delete From "& TableList(1,i) &" where BoardID=444",tmp
		Response.Write "<script language=""javascript"" type=""text/javascript"">plantext('彻底删除表"& TableList(1,i) &" 中的被删回复"&tmp&"个');</script>"
		Response.Flush
		Tolog("彻底删除表"& TableList(1,i) &" 中的被删回复"&tmp&"个")
	Next
End Sub
Sub LoadTableList()
	Dim Rs
	Set Rs=Dvbbs.Execute("select * from [Dv_TableList]")
	TableList=Rs.GetRows(-1)
	Set XMLDom=Dvbbs.ArrayToxml(TableList,Rs,"posttable","xml")
End Sub
Sub alldel()
	Dim Node
	Set Node = XMLDom.documentElement.appendChild(XMLDom.createNode(1,"param",""))
	Node.attributes.setNamedItem(XMLDom.createNode(2,"action","")).text=Request("action")
	maxcount=Request.Form("maxcount")
	If maxcount="" Then maxcount=500
	If Not IsNumeric(maxcount) Then maxcount=500
	If CLng(maxcount) < 500 Then maxcount=500
	Node.attributes.setNamedItem(XMLDom.createNode(2,"maxcount","")).text=maxcount
End Sub
Sub delpic()
	Dim Node,id,replyid,rs,posttable,Boardid,sql,title,isvote,PollID
	Set Node = XMLDom.documentElement.appendChild(XMLDom.createNode(1,"param",""))
	Node.attributes.setNamedItem(XMLDom.createNode(2,"action","")).text=Request("action")
	id=Request("id")
	replyid=Request("replyid")
	If Not IsNumeric(id) Then
			Response.redirect "showerr.asp?ErrCodes=<li>参数错误</li>&action=OtherErr"
			Response.End
	End If
	If 	replyid <>"" Then
		If Not IsNumeric(replyid) Then
				Response.redirect "showerr.asp?ErrCodes=<li>参数错误</li>&action=OtherErr"
				Response.End
		End If
	End If
	Set rs=Dvbbs.Execute("Select posttable,boardid,title,isvote,PollID From Dv_topic Where topicid="&Id)
	If Rs.EOF Then
		Response.redirect "showerr.asp?ErrCodes=<li>记录不存在</li>&action=OtherErr"
		Exit Sub
		Response.End
	End If
	Boardid=Rs("Boardid")
	posttable=Rs("posttable")
	title=Rs("title")
	isvote=Rs("isvote")
	PollID=Rs("PollID")
	If replyid="" Then
		If Boardid<>444 Then
			Response.redirect "showerr.asp?ErrCodes=<li>此主题不在回收站中，不能删除!</li>&action=OtherErr"
			Exit Sub
			Response.End
		End If
		Dvbbs.Execute("delete From "&posttable&" Where RootID ="&ID)
		Dvbbs.Execute("delete From dv_topic where topicid ="&ID)
		Tolog("删除主题："&title)
	Else
		Set Rs=Dvbbs.Execute("select ParentID From " &posttable &" where AnnounceID=" &CLng(replyid))
		If Rs.EOF Then
			Response.redirect "showerr.asp?ErrCodes=<li>记录不存在</li>&action=OtherErr"
			Exit Sub
			Response.End
		Else
			If Rs(0)=0 Then
					If Boardid<>444 Then
						Response.redirect "showerr.asp?ErrCodes=<li>此主题不在回收站中，不能删除!</li>&action=OtherErr"
						Exit Sub
						Response.End
					End If
					If isvote=1 Then
						Dvbbs.Execute("delete From Dv_vote  Where voteid="& PollID &"")
					End If
					Dvbbs.Execute("delete From "&posttable&" Where RootID ="&ID)
					Dvbbs.Execute("delete From dv_topic where topicid ="&ID)
					Tolog("删除主题："&title)
			Else
				Dvbbs.Execute("delete From "&posttable&" Where RootID ="&ID &" and AnnounceID=" &CLng(replyid))
				Tolog("删除主题："&title&"的回复一条")
			End If	
		End If
	End If
End Sub
Sub Manage_Main()
	Dim Node,Rs,posttable,Child,boardid,LockTopic,topic
	Set Node = XMLDom.documentElement.appendChild(XMLDom.createNode(1,"param",""))
	Node.attributes.setNamedItem(XMLDom.createNode(2,"action","")).text=Request("action")
	Dim id,rootid,AnnounceID,TmpID
	TmpID=replace(Replace(Request("id"),"_","")," ","")
	tmpid=split(TmpID,",")
	For each id in tmpid
		If Not IsNumeric(id) Then
			Response.redirect "showerr.asp?ErrCodes=<li>参数错误</li>&action=OtherErr"
			Exit For
			Response.End
		End If
	Next
	For each tmpid in Request("id")
			Set Node = XMLDom.documentElement.appendChild(XMLDom.createNode(1,"result",""))
			id=Split(tmpid,"_")
			rootid=CLng(id(0))
			AnnounceID=id(1)
			If AnnounceID="" Then AnnounceID=rootid
			Node.attributes.setNamedItem(XMLDom.createNode(2,"rootid","")).text=rootid
			Node.attributes.setNamedItem(XMLDom.createNode(2,"announceid","")).text=AnnounceID
			Rem 检查主题表是否有记录，并且取得其主贴状态
			Set Rs=Dvbbs.Execute("select Boardid,LockTopic,PostTable From Dv_topic Where topicid="& rootid &" and boardid<> 777")
			If Rs.EOF Then
				Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
				Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
				Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="失败，原因：找不到相关记录。"
			Else
				posttable=Rs("posttable")
				LockTopic=Rs("boardid")
				boardid=rs("LockTopic")
				If AnnounceID=rootid Then 'AnnounceID=rootid说明要还原的是主题
					If LockTopic=444  Then
						Set Rs=Dvbbs.Execute("Select Count(*) From " & posttable &" where RootID="& Rootid &" and boardid<> 777")
						Child=Rs(0)
						If IsNull(Child) Then Child=1
						Child=Child-1
						Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=1
						Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=Child
						Node.attributes.setNamedItem(XMLDom.createNode(2,"boardid","")).text=boardid
						Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="成功。"
						Dvbbs.Execute("update " & posttable &" set boardid="&boardid&",LockTopic=0 Where RootID="& Rootid &" and boardid<> 777")
						Dvbbs.Execute("update dv_topic Set boardid="&boardid&",LockTopic=0,Child="&Child&"  Where topicid="& rootid)
					Else
						Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
						Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
						Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="失败，原因：主题不在回收站中。"
					End If
				Else
					Set Rs=Dvbbs.Execute("Select ParentID  From " & posttable &" where RootID="& Rootid &" and AnnounceID="& AnnounceID &" and boardid=444")
					If Rs.EOF Then
						Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
						Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
						Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="失败，原因：此贴不在回收站中。"
					Else
						If Rs(0)=0 Then '主题贴，还原单个主题贴。跟贴不还原。
							Child=0
							If LockTopic <> 444  Then
								boardid=LockTopic
							End If
							Dvbbs.Execute("update dv_topic Set boardid="&boardid&",LockTopic=0,Child="&Child&"  Where topicid="& rootid)
							Dvbbs.Execute("update " & posttable &" set boardid="&boardid&",LockTopic=0 Where RootID="& Rootid &" and AnnounceID="& AnnounceID)
							Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=1
							Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=Child
							Node.attributes.setNamedItem(XMLDom.createNode(2,"boardid","")).text=boardid
							Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="成功。"
						Else
							If LockTopic <> 444  Then
								boardid=LockTopic
								Dvbbs.Execute("update " & posttable &" set boardid="&boardid&",LockTopic=0 Where RootID="& Rootid &" and AnnounceID="& AnnounceID)
								Dvbbs.Execute("update dv_topic Set boardid="&boardid&",LockTopic=0,Child=Child+1  Where topicid="& rootid)
								Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
								Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=1
								Node.attributes.setNamedItem(XMLDom.createNode(2,"boardid","")).text=boardid
								Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="成功。"
							Else
								Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
								Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
								Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="失败，原因：主题贴还在回收站中，请先还原主题。"
							End If
						End If
					End If
				End If
			End If
	Next
	Dim allpost,alltopic
	allpost=0
	alltopic=0
	'统计一下更新情况
	For each boardid in Application(Dvbbs.CacheName&"_boardlist").documentElement.selectNodes("board/@boardid")
		Set Node =XMLDom.documentElement.selectNodes("result[@boardid="& boardid.text &"]")
		If Node.length > 0 Then
			topic=0
			Child=0
			For Each TmpID in node
				topic=topic+CLng(tmpid.selectSingleNode("@topic").text)
				Child=Child+CLng(tmpid.selectSingleNode("@child").text)
			Next
			If topic+Child >0 Then
				alltopic=alltopic+topic
				allpost=allpost+topic+Child
				UpDate_BoardInfoAndCache BoardID.text,topic,Child
			End If
		End If
	Next
	If allpost >0 Or alltopic >0 Then
		Dvbbs.Execute("update dv_setup Set forum_postNum=forum_postNum+"& allpost &",forum_TopicNum=forum_topicNum +"& alltopic )
		Dvbbs.loadSetup
	End If
	Tolog("还原主题："&alltopic&"个，贴子"& allpost &"条")
End Sub
Sub Main()
	bid=Request("bid")
	If Not IsNumeric(BID) Then bid="0"
	If bid="" Then bid="0"
	tableid=Request("tableid")
	If Not IsNumeric(tableid) Then tableid="0"
	If Trim(tableid)="" Then tableid="0"
	Dim i,SQL,node,keyword,tmpsql,Rs,SQL1,Pagesize,Page,pagecount
	keyword=Trim(Request("keyword"))
	posttable="dv_topic"
	If tableid <> "0" Then
		For i= 0 to UBound(TableList,2)
			If CStr(TableList(0,i))=tableid Then
				posttable=TableList(1,i)
				Exit For
			End If
		Next
	End If
	If posttable="dv_topic" Then tableid="0"
	Page=Request("Page")
	If Not IsNumeric(Page) Then Page="1"
	If Page="" Then Page="1"
	Page=CLng(Page)
	'传送参数到xml
	Pagesize=30'手工设置每页最大显示30条
	Set Node = XMLDom.documentElement.appendChild(XMLDom.createNode(1,"param",""))
	Node.attributes.setNamedItem(XMLDom.createNode(2,"bid","")).text=bid
	Node.attributes.setNamedItem(XMLDom.createNode(2,"tableid","")).text=tableid
	Node.attributes.setNamedItem(XMLDom.createNode(2,"keyword","")).text=keyword
	Node.attributes.setNamedItem(XMLDom.createNode(2,"pagesize","")).text=Pagesize
	Node.attributes.setNamedItem(XMLDom.createNode(2,"posttable","")).text=posttable
	Node.attributes.setNamedItem(XMLDom.createNode(2,"action","")).text=Request("action")
	If session("flag")<>"" Then
		Node.attributes.setNamedItem(XMLDom.createNode(2,"admin","")).text=1
	End If
	'根据页面参数产生查询代码
	keyword=Dvbbs.Checkstr(keyword)
	SQl ="Select "
	SQl1 ="Select Count(*) as length From "& posttable
	If tableid="0" Then
		SQL= SQL &"topicid as id,Title as topic,LockTopic as bid,PostUsername as username,PostUserid as userid,PostTable,DateAndTime From "& posttable
		tmpsql="and (title like '%"&keyword&"%' or PostUsername='"&keyword&"') "
	Else
		SQL= SQL &"rootid as id,topic,body,LockTopic as bid,username,PostUserid as userid,AnnounceID as replyID,DateAndTime,ParentID From "& posttable
		tmpsql="and (topic like '%"&keyword&"%' or Username='"&keyword&"') "
	End If
	SQL= SQL &" Where Boardid=444 "
	SQL1= SQL1 &" Where Boardid=444 "
	If Bid <> "0" Then
		SQL= SQL &"and LockTopic="&bid&" "
		SQL1= SQL1 &"and LockTopic="&bid&" "
	End If
	If keyword<>"" Then
		SQL= SQL & tmpsql
		SQL1= SQL1 & tmpsql
	End If
	If tableid="0" Then
		SQL= SQL &" order by topicid desc"
	Else
		SQL= SQL &" order by AnnounceID desc"
	End If
	Set Rs=Dvbbs.Execute(SQL1)
	Node.attributes.setNamedItem(XMLDom.createNode(2,"count","")).text =Rs(0)
	'计算一下当前Page参数是否合法。如果超出范围，强制为最后一页
	If Rs(0) mod Pagesize =0 then
			PageCount= Rs(0) \ Pagesize
	Else
			PageCount= Rs(0) \ Pagesize+1
	End If
	If Page > PageCount Then Page=PageCount
	Node.attributes.setNamedItem(XMLDom.createNode(2,"page","")).text=Page
	If Rs(0) <> 0  and Not IsNull(Rs(0))Then
		Set Rs=Dvbbs.Execute(SQL)
		If Not page=1 Then 	Rs.Move(pagesize*(page-1))
		SQL=RS.GetRows(Pagesize)
		Set Node=Dvbbs.ArrayToxml(SQL,rs,"row","datarows")
		XMLDom.documentElement.appendChild(node.documentElement)
	End If
	XMLDom.documentElement.appendChild(Application(Dvbbs.CacheName&"_boardlist").documentElement.cloneNode(True))
End Sub
Sub View()
	Dim Node,id,replyid,Rs,posttable,SQL
	Set Node = XMLDom.documentElement.appendChild(XMLDom.createNode(1,"param",""))
	Node.attributes.setNamedItem(XMLDom.createNode(2,"action","")).text=Request("action")
	If Dvbbs.Master Then 
		Node.attributes.setNamedItem(XMLDom.createNode(2,"master","")).text=1
	End If
	id=Request("id")
	replyid=Request("replyid")
	If Not IsNumeric(replyid) Or replyid="" Then replyid=0
	If Not IsNumeric(id) Or id="" Then
		Response.redirect "showerr.asp?ErrCodes=<li>请指定所需参数。&action=OtherErr"
	End If
	Id =CLng(id)
	Set rs=Dvbbs.Execute("Select posttable,boardid From Dv_topic Where topicid="&Id)
	If Rs.EOF Then
			Response.redirect "showerr.asp?ErrCodes=<li>记录不存在！&action=OtherErr"
	Else
		posttable=Rs(0)
		Node.attributes.setNamedItem(XMLDom.createNode(2,"boardid","")).text=Rs(1)
		If replyid=0 Then
			SQL="Select * From "&posttable & " where rootid="&ID&" and ParentID=0 and Boardid=444"
		Else
			SQL="Select * From "&posttable & " where rootid="&ID&" and AnnounceID="&replyid&"and Boardid=444"
		End If
		Set Rs=Dvbbs.Execute(SQL)
		If Rs.EOF Then
			Response.redirect "showerr.asp?ErrCodes=<li>找不到匹配记录！&action=OtherErr"
		Else
			XMLDom.documentElement.appendChild(Dvbbs.RecordsetToxml(rs,"row","").documentElement.firstChild)
		End If
	End If
End Sub

Sub ShowHTML()
	Dim xslt,proc,XMLStyle
	Set XMLStyle=Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XMLStyle.loadxml template.html(1)
	Set XSLT=Dvbbs.iCreateObject("Msxml2.XSLTemplate" & MsxmlVersion)
	XSLT.stylesheet=XMLStyle
	Set proc = XSLT.createProcessor()
	proc.input = XMLDom
  proc.transform()
  Response.Write  proc.output
	Set XMLDOM=Nothing
	Set XSLt=Nothing
	Set proc=Nothing	
End Sub
Sub UpDate_BoardInfoAndCache(BoardID,topic,Child)
		Dim UpdateBoardID,parentstr,SQL
		parentstr =Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&BoardID&"']/@parentstr").text
		If parentstr <> "0" Then 
			UpdateBoardID= parentstr & "," & BoardID	
		Else
			UpdateBoardID=BoardID
		End If
		Dim updateboard,i
		SQL="update Dv_board set PostNum=PostNum+"&topic+Child&",TopicNum=TopicNum+"&topic&" where boardid in ("&UpdateBoardID&")"
		Dvbbs.Execute(sql)
		UpdateBoardID=Split(UpdateBoardID,",")
		For Each updateboard in UpdateBoardID
			If IsObject(Application(Dvbbs.CacheName &"_information_" & updateboard)) Then
				Application(Dvbbs.CacheName &"_information_" & updateboard).documentElement.selectSingleNode("information/@postnum").text=CLng(Application(Dvbbs.CacheName &"_information_" & updateboard).documentElement.selectSingleNode("information/@postnum").text)+topic+Child
				Application(Dvbbs.CacheName &"_information_" & updateboard).documentElement.selectSingleNode("information/@topicnum").text=CLng(Application(Dvbbs.CacheName &"_information_" & updateboard).documentElement.selectSingleNode("information/@topicnum").text)+topic
			End If
		Next
	End Sub
	Sub Tolog(Info)
		Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,0,'回收站','" & Dvbbs.MemberName & "','" & Dvbbs.CheckStr(Info) & "','" & Dvbbs.userTrueIP & "',0)")
	End Sub
%>