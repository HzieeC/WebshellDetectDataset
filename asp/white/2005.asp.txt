<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/dv_clsother.asp" -->
<%
Dim AdminLockTopic,XMLDom,TableList,paramnode,AccessSetting
Rem 审核权限设置,如果不希望版主可以审核全部贴,请屏蔽下面第一行
If Dvbbs.UserGroupID=3 Then  Dvbbs.Boardmaster=True
Rem =======================================================
AdminLockTopic=False 
If (Dvbbs.master or Dvbbs.superboardmaster or Dvbbs.boardmaster) And Cint(Dvbbs.GroupSetting(36))=1 Then
	AdminLockTopic=True 
Else 
	AdminLockTopic=False 
End If
If Cint(Dvbbs.GroupSetting(36))=1 And Dvbbs.UserGroupID>3 Then
	AdminLockTopic=True 
End If 
If Dvbbs.FoundUserPer And Cint(Dvbbs.GroupSetting(36))=1 Then
	AdminLockTopic=true
ElseIf Dvbbs.FoundUserPer And Cint(Dvbbs.GroupSetting(36))=0 Then
	AdminLockTopic=False 
End If 
If Not AdminLockTopic Then Response.redirect "showerr.asp?ErrCodes=<li>您没有在本版面审核帖子的权限。&action=OtherErr"
If Not Dvbbs.Master and (Request("action")="modify" Or Request("action")="save") Then Response.redirect "showerr.asp?ErrCodes=<li>您没有修改审核设置的权限。&action=OtherErr"
LoadTableList()
Set paramnode=XMLDom.documentElement.appendChild(XMLDom.createNode(1,"param",""))
paramnode.attributes.setNamedItem(XMLDom.createNode(2,"boardid","")).text=Dvbbs.Boardid
paramnode.attributes.setNamedItem(XMLDom.createNode(2,"action","")).text=Request("action")
If Dvbbs.Master Then paramnode.attributes.setNamedItem(XMLDom.createNode(2,"master","")).text=1
Dvbbs.LoadTemplates("query")
If Not Dvbbs.ChkPost() Then  Response.Redirect "index.asp"
 Select Case Request("action")
 	Case "manage"'批量审核
 		Dvbbs.stats="批量审核"
 		Dvbbs.Nav
 		manage()
 		If Dvbbs.BoardID=0 Then
			Dvbbs.Head_var 2,0,"",""
		Else
			Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
		End If
 	Case "view"'查看单个审核贴
 		Dvbbs.stats="审核贴子"
 		View()
 		Dvbbs.Head()
 	Case "modify"'审核设置管理
 		Dvbbs.stats="帖子审核设置管理"
 		Dvbbs.Nav
 		If Dvbbs.BoardID=0 Then
			Dvbbs.Head_var 2,0,"",""
		Else
			Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
		End If
		LoadAccessSetting
 	Case "info"'查看当前审核规则
 		Dvbbs.stats="审核设置信息"
 		Dvbbs.Nav
 		If Dvbbs.BoardID=0 Then
			Dvbbs.Head_var 2,0,"",""
		Else
			Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
		End If
		LoadAccessSetting
	Case "save"
		Dvbbs.stats="保存审核设置"
		Dvbbs.Nav
		Dvbbs.Head_var 2,0,"",""
		SaveAccessSetting()
	Case "addnocheck"
		Dvbbs.stats="豁免审核用户"
		Dvbbs.Nav()
		Dvbbs.Head_var 2,0,"",""
		addnocheck
	Case "unlock"
		Dvbbs.stats="解除固封"
		Dvbbs.Nav()
		Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
		unlockPost()
 	Case Else
 		Dvbbs.stats="帖子审核"
 		Dvbbs.Nav
 		If Dvbbs.BoardID=0 Then
			Dvbbs.Head_var 2,0,"",""
		Else
			Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
		End If
		LoadAccessCount()
 		accesslist()
 End Select
XMLDom.documentElement.appendChild(Application(Dvbbs.CacheName&"_boardlist").documentElement.cloneNode(True))
ShowHTML()
If Request("action")="view" Then
	Response.Write "</body></html>"
Else
	Dvbbs.activeonline()
	Dvbbs.footer()
End If
Dvbbs.PageEnd()

Sub unlockPost()
	Dim id,replyid,rs,posttable
	id=Request("id")
	replyid=Request("replyid")
	If Not IsNumeric(id) Or id="" Then
		Response.redirect "showerr.asp?ErrCodes=<li>请指定所需参数。&action=OtherErr"
	End If
	Id =CLng(id)
	If Not IsNumeric(replyid) Or replyid="" Then
		Response.redirect "showerr.asp?ErrCodes=<li>请指定所需参数。&action=OtherErr"
	End If
	Set Rs=Dvbbs.Execute("select posttable From Dv_topic Where topicid="&id)
	If Rs.EOF Then
		Response.redirect "showerr.asp?ErrCodes=<li>记录不存在。&action=OtherErr"
	End If
	posttable=Rs(0)
	Dvbbs.Execute("update " & Dvbbs.Checkstr(posttable) &" Set locktopic=0 where announceid="& replyid &" and rootid="&id&" and locktopic=3")
	Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,"&Dvbbs.BoardID&",'解除固封','" & Dvbbs.MemberName & "','主题编号："& ID & ", 贴子编号："& replyid &" ','" & Dvbbs.userTrueIP & "',3)")
End Sub

Function Getusergroupid(username)
	If Username<>"" Then
		Dim Rs,SQL
		SQL="select usergroupid From Dv_user Where username='"& Dvbbs.Checkstr(username) &"'"
		Set Rs=Dvbbs.Execute(SQL)
		If Rs.EOF Then
			Getusergroupid=0
		Else
			Getusergroupid=Rs(0)
		End If
	Else
		Getusergroupid=0
	End If
End Function

Sub addnocheck()
	Dim usergroupid,Dom,node
	Set Dom=Application(Dvbbs.CacheName & "_accesstopic").cloneNode(True)
	usergroupid=CInt(Getusergroupid(Request("username")))
	If usergroupid<>0 Then
		Set Node=Dom.documentElement.selectSingleNode("setting/checkuser[@usergroupid="&usergroupid&"]")
		If Not Node is Nothing Then
			Set Node=Dom.documentElement.selectSingleNode("setting/nocheck[username='"&Dvbbs.Checkstr(Request("username"))&"']")
			If  Node is Nothing Then
				For Each node in Dom.documentElement.selectNodes("setting")
					Node.selectSingleNode("nocheck").appendChild(Dom.createNode(1,"username","")).text=Request("username")
				Next
			End If
		End If
	End If	
	'删除已经不在审核组的用户的免审核数据
	For Each Node in Dom.documentElement.selectNodes("setting/nocheck/username")
		usergroupid=CInt(Getusergroupid(node.text))
		If usergroupid=0 Then
			node.parentNode.removeChild(node)
		Else
			If Dom.documentElement.selectSingleNode("setting/checkuser[@usergroupid="&usergroupid&"]") is Nothing Then
				node.parentNode.removeChild(node)
			End If
		End If
	Next
	Dvbbs.Execute("update Dv_setup Set Forum_BoardXML='"&Dvbbs.Checkstr(Replace(Replace(Dom.xml,Chr(13),""),Chr(10),""))&"'")
	Set Application(Dvbbs.CacheName & "_accesstopic")=Dom.cloneNode(True)
	Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,"&Dvbbs.BoardID&",'豁免审核','" & Dvbbs.MemberName & "','豁免：" & Dvbbs.CheckStr(Request("username")) & "','" & Dvbbs.userTrueIP & "',3)")
End Sub

Sub SaveAccessSetting()
	If Request.form("action")="" Then Exit Sub
	Dim id,node,Dom,node1,queststr,checkuser,node2
	Set Dom=Dvbbs.CreateXmlDoc("Msxml2.FreeThreadedDOMDocument" & MsxmlVersion)
	Dom.appendChild(Dom.createElement("accesspost"))
	For Each id in Request("id")
		Set Node=Dom.documentElement.appendChild(Dom.createNode(1,"setting",""))
		Node.attributes.setNamedItem(Dom.createNode(2,"type","")).text=Request("setting_"&id&"_type")
		queststr=Request("setting_"&id&"_use")
		If queststr="" Then queststr="0"
		Node.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
		Set Node1=Node.appendChild(Dom.createNode(1,"check",""))
		queststr=Request("setting_"&id&"_check_new")
		If queststr="" Then queststr="0"
		Node1.attributes.setNamedItem(Dom.createNode(2,"new","")).text=queststr
		queststr=Request("setting_"&id&"_check_re")
		If queststr="" Then queststr="0"
		Node1.attributes.setNamedItem(Dom.createNode(2,"re","")).text=queststr
		queststr=Request("setting_"&id&"_check_edit")
		If queststr="" Then queststr="0"
		Node1.attributes.setNamedItem(Dom.createNode(2,"edit","")).text=queststr
		For Each checkuser in Request("setting_"&id&"_checkuser")
			Set Node1=Node.appendChild(Dom.createNode(1,"checkuser",""))
			Node1.attributes.setNamedItem(Dom.createNode(2,"usergroupid","")).text=Request("setting_"&id&"_checkuser_"&checkuser&"_usergroupid")
			queststr=Request("setting_"&id&"_checkuser_"&checkuser&"_use")
			If queststr="" Then queststr="0"
			Node1.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
			Set node2=Node1.appendChild(Dom.createNode(1,"usertopic",""))
			Node2.attributes.setNamedItem(Dom.createNode(2,"value","")).text=Request("setting_"&id&"_checkuser_"&checkuser&"_usertopic")
			queststr=Request("setting_"&id&"_checkuser_"&checkuser&"_usertopic_use")
			If queststr="" Then queststr="0"
			Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
			Set node2=Node1.appendChild(Dom.createNode(1,"userpost",""))
			Node2.attributes.setNamedItem(Dom.createNode(2,"value","")).text=Request("setting_"&id&"_checkuser_"&checkuser&"_userpost")
			queststr=Request("setting_"&id&"_checkuser_"&checkuser&"_userpost_use")
			If queststr="" Then queststr="0"
			Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
			Set node2=Node1.appendChild(Dom.createNode(1,"regdate",""))
			Node2.attributes.setNamedItem(Dom.createNode(2,"value","")).text=Request("setting_"&id&"_checkuser_"&checkuser&"_regdate")
			queststr=Request("setting_"&id&"_checkuser_"&checkuser&"_regdate_use")
			If queststr="" Then queststr="0"
			Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
			Set node2=Node1.appendChild(Dom.createNode(1,"userdel",""))
			Node2.attributes.setNamedItem(Dom.createNode(2,"value","")).text=Request("setting_"&id&"_checkuser_"&checkuser&"_userdel")
			queststr=Request("setting_"&id&"_checkuser_"&checkuser&"_userdel_use")
			If queststr="" Then queststr="0"
			Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
			Set node2=Node1.appendChild(Dom.createNode(1,"lockuser",""))
			queststr=Request("setting_"&id&"_checkuser_"&checkuser&"_lockuser_use")
			If queststr="" Then queststr="0"
			Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
			Set node2=Node1.appendChild(Dom.createNode(1,"checkcontent",""))
			queststr=Request("setting_"&id&"_checkuser_"&checkuser&"_checkcontent_use")
			If queststr="" Then queststr="0"
			Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
		Next
		Set Node1=Node.appendChild(Dom.createNode(1,"checkcontent",""))
		Set node2=Node1.appendChild(Dom.createNode(1,"checkpic",""))
		queststr=Request("setting_"&id&"_checkcontent_checkpic_use")
		If queststr="" Then queststr="0"
		Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
		Set node2=Node1.appendChild(Dom.createNode(1,"checklink",""))
		queststr=Request("setting_"&id&"_checkcontent_checklink_use")
		If queststr="" Then queststr="0"
		Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
		Set node2=Node1.appendChild(Dom.createNode(1,"checkflash",""))
		queststr=Request("setting_"&id&"_checkcontent_checkflash_use")
		If queststr="" Then queststr="0"
		Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
		Set node2=Node1.appendChild(Dom.createNode(1,"checkmp",""))
		queststr=Request("setting_"&id&"_checkcontent_checkmp_use")
		If queststr="" Then queststr="0"
		Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
		Set node2=Node1.appendChild(Dom.createNode(1,"checkrm",""))
		queststr=Request("setting_"&id&"_checkcontent_checkrm_use")
		If queststr="" Then queststr="0"
		Node2.attributes.setNamedItem(Dom.createNode(2,"use","")).text=queststr
		queststr=Request("setting_"&id&"_checkcontent_checkword")
		If queststr <>"" Then
			queststr=split(queststr,vbnewline)
			For Each Node2 in queststr
				If node2<>"" Then
					Node1.appendChild(Dom.createNode(1,"checkword","")).attributes.setNamedItem(Dom.createNode(2,"content","")).text=Node2
				End If
			Next
		End If
		Set Node1=Node.appendChild(Dom.createNode(1,"nocheck",""))
		queststr=Request("setting_"&id&"_nocheck")
		If queststr <>"" Then
			queststr=split(queststr,vbnewline)
			For Each Node2 in queststr
				If node2<>"" Then
					Node1.appendChild(Dom.createNode(1,"username","")).text=Node2
				End If
			Next
		End If
	Next
	Dvbbs.Execute("update Dv_setup Set Forum_BoardXML='"&Dvbbs.Checkstr(Replace(Replace(Dom.xml,Chr(13),""),Chr(10),""))&"'")
	Dvbbs.LoadSetup()
	Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,"&Dvbbs.BoardID&",'审核设置','" & Dvbbs.MemberName & "','更新','" & Dvbbs.userTrueIP & "',3)")
End Sub

Sub LoadAccessSetting()
	Dim dom,Node,i,position,position1,node1
	Set Dom=Application(Dvbbs.CacheName & "_accesstopic").cloneNode(True)
	If Request("addnew")="1"  and Request("action")="modify" Then
		Set Node=Dom.documentElement.appendChild(Dom.createNode(1,"setting",""))
		Node.attributes.setNamedItem(Dom.createNode(2,"type","")).text="新建设置"
		Node.appendChild(Dom.createNode(1,"checkuser",""))
	ElseIf Request("delsetting")="1" Then
		Set Node=Dom.documentElement.selectNodes("setting")
		position=CLng(Request("position"))
		If position < Node.length +1 Then
			Dom.documentElement.removeChild(Node(position-1))
		End If
	ElseIf Request("delusergroup")="1" Then
		Set Node=Dom.documentElement.selectNodes("setting")
		position=CLng(Request("position"))
		position1=CLng(Request("position1"))
		If position < Node.length +1 Then
			Set Node1=Node(position-1).selectNodes("checkuser")
			If position1 < Node1.length +1 Then
				Node(position-1).removeChild(Node1(position1-1))
			End If
		End If
	ElseIf Request("addusergroup")="1" Then
		Set Node=Dom.documentElement.selectNodes("setting")
		position=CLng(Request("position"))
		If position < Node.length +1 Then
			Node(position-1).appendChild(Dom.createNode(1,"checkuser",""))
		End If
	End If
	XMLDom.documentElement.appendChild(dom.documentElement)
	XMLDom.documentElement.appendChild(Application(Dvbbs.CacheName &"_grouppic").documentElement.cloneNode(True))
	'Response.Write Server.HtmlEnCode(XMLDom.xml)
End Sub

Sub manage()
	Dim id,passed,replyid,i,node,posttable,LockTopic,boardid,rs,today,isvote,PollID
	i=1
	For Each id in Request.form("id")
		Set Node = XMLDom.documentElement.appendChild(XMLDom.createNode(1,"result",""))
		If IsNumeric(id) and id<>"" Then
			replyid=Request("replyid")(i)
			passed = Request("pass_"&id&"_"& replyid)
			If replyid="" Then replyid=id
			replyid=Dvbbs.CheckNumeric(replyid)
			Node.attributes.setNamedItem(XMLDom.createNode(2,"rootid","")).text=id
			Node.attributes.setNamedItem(XMLDom.createNode(2,"announceid","")).text=replyid
			Rem 检查主题表是否有记录，并且取得其主贴状态
			Set Rs=Dvbbs.Execute("select Boardid,LockTopic,PostTable,isvote,PollID From Dv_topic Where topicid="& id &"")
			If Not Rs.EOF Then
				posttable=Rs("posttable")
				LockTopic=Rs("boardid")
				isvote=Rs("isvote")
				PollID=Rs("PollID")
				today=0
				If Passed="1" Then
					Rem 通过审核
					If replyid=id Then
						Set Rs=Dvbbs.Execute("select dateandtime,PostUserid,ParentID,LockTopic,UserName,Body,IsUpLoad From "& posttable &" Where RootID="& id &" and Boardid=777")
						If Not Rs.EOF Then 
							boardid=rs("LockTopic")
							If datediff("d",rs(0),Now()) =0 Then today=1
							Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=1
							Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
							Node.attributes.setNamedItem(XMLDom.createNode(2,"today","")).text=today
							Node.attributes.setNamedItem(XMLDom.createNode(2,"boardid","")).text=boardid
							Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="通过审核成功。"
							Dvbbs.Execute("update " & posttable &" set boardid="&boardid&",LockTopic=0 Where RootID="& id &" and ParentID=0 and Boardid=777")
							Dvbbs.Execute("update dv_topic Set boardid="&boardid&",LockTopic=0,Child=0  Where topicid="& id &" and Boardid=777")
							UpdatepostUser  rs(1),boardid,1
							UpdateLastPost1 rs(4),replyid,rs(0),rs(5),"",rs(1),id,rs(3)
							UpdateLastPost2 boardid,id,replyid
							If Rs(1)<>0 Then Dvbbs.Sendmessanger Rs(1),"系统[审核]","您发表的贴子已经通过审核，请<a href=""dispbbs.asp?boardid="& boardid&"&amp;id="&id&""" target=""_blank"">点此查看</a>"
						Else
								Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
								Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
								Node.attributes.setNamedItem(XMLDom.createNode(2,"today","")).text=0
								Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="失败，原因：找不到相关记录，数据可能已经被别的管理人员处理了。"
						End If
					Else
						Set Rs=Dvbbs.Execute("select dateandtime,PostUserid,ParentID,LockTopic,UserName,Body,IsUpLoad From "& posttable &" Where RootID="& id &" and AnnounceID="&replyid&" and  Boardid=777")
						If Not Rs.EOF Then
							If datediff("d",rs(0),Now())=0 Then today=1
							boardid=rs("LockTopic")
							Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
							Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=1
							Node.attributes.setNamedItem(XMLDom.createNode(2,"today","")).text=today
							Node.attributes.setNamedItem(XMLDom.createNode(2,"boardid","")).text=boardid
							Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="通过审核成功。"
							If Rs("ParentID")=0 Then
								Dvbbs.Execute("update " & posttable &" set boardid="&boardid&",LockTopic=0 Where RootID="& id &" and ParentID=0")
								Dvbbs.Execute("update dv_topic Set boardid="&boardid&",LockTopic=0,Child=0  Where topicid="& id)
								UpdatepostUser  rs(1),boardid,1
								UpdateLastPost1 rs(4),replyid,rs(0),rs(5),"",rs(1),id,rs(3)
								UpdateLastPost2 boardid,id,replyid
								If Rs(1)<>0 Then Dvbbs.Sendmessanger Rs(1),"系统[审核]","您发表的贴子已经通过审核，请<a href=""dispbbs.asp?boardid="& boardid&"&amp;id="&id&""" target=""_blank"">点此查看</a>"
							Else
								Dvbbs.Execute("update " & posttable &" set boardid="&boardid&",LockTopic=0 Where RootID="& id &" and AnnounceID="&replyid )
								Dvbbs.Execute("update dv_topic Set boardid="&boardid&",LockTopic=0,Child=Child+1  Where topicid="& id)
								UpdatepostUser  rs(1),boardid,0
								UpdateLastPost1 rs(4),replyid,rs(0),rs(5),"",rs(1),id,rs(3)
								UpdateLastPost2 boardid,id,replyid
								If Rs(1)<>0 Then Dvbbs.Sendmessanger Rs(1),"系统[审核]","您发表的贴子已经通过审核，请<a href=""dispbbs.asp?boardid="& boardid&"&amp;id="&id&"&amp;skin=1&amp;replyid="&replyid&""" target=""_blank"">点此查看</a>"
							End If
						Else
								Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
								Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
								Node.attributes.setNamedItem(XMLDom.createNode(2,"today","")).text=0
								Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="失败，原因：找不到相关记录,数据可能已经被别的管理人员处理了。"
						End If
					End If
				ElseIf Passed="0" Then
					Rem 删除
					If replyid=id Then
						Set Rs=Dvbbs.Execute("select PostUserid From "& posttable &" Where RootID="& id &"")
						If Not Rs.EOF Then
							If Rs(0)<>0 Then Dvbbs.Sendmessanger Rs(0),"系统[审核]","您发表的贴子未能通过审核，请注意您发表的内容。"
						End If
						If isvote=1 Then
							Dvbbs.Execute("delete From Dv_vote  Where voteid="& PollID &"")
						End If
						Dvbbs.Execute("delete From " & posttable &"  Where RootID="& id &"")
						Dvbbs.Execute("delete From dv_topic Where topicid="& id)
					Else
						Set Rs=Dvbbs.Execute("select ParentID,PostUserid From "& posttable &" Where RootID="& id &" and AnnounceID="&replyid )
						If Not Rs.EOF Then
							If Rs(1)<>0 Then Dvbbs.Sendmessanger Rs(1),"系统[审核]","您发表的贴子未能通过审核，请注意您发表的内容。"
							If  Rs(0) <> 0 Then 
								Dvbbs.Execute("delete From " & posttable &"  Where RootID="& id &" and AnnounceID="&replyid)
							Else
								Dvbbs.Execute("delete From " & posttable &"  Where RootID="& id &"")
								Dvbbs.Execute("delete From dv_topic Where topicid="& id)
								If isvote=1 Then
									Dvbbs.Execute("delete From Dv_vote  Where voteid="& PollID &"")
								End If
							End If
						End If
					End If
					'清除上传附件 2005-12-5 Dv.Yz
					Dvbbs.Execute("UPDATE Dv_Upfile SET F_Flag = 4 WHERE F_AnnounceID = '" & Id & "|" & Replyid & "'")
					Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
					Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
					Node.attributes.setNamedItem(XMLDom.createNode(2,"today","")).text=0
					Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="删除待审核贴成功。"
				Else
					Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
					Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
					Node.attributes.setNamedItem(XMLDom.createNode(2,"today","")).text=0
					Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="待审，您没有对该贴进行处理。"
				End If
			Else
				Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
				Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
				Node.attributes.setNamedItem(XMLDom.createNode(2,"today","")).text=0
				Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="失败，原因：找不到相关记录。"
			End If
		Else
			Node.attributes.setNamedItem(XMLDom.createNode(2,"topic","")).text=0
			Node.attributes.setNamedItem(XMLDom.createNode(2,"child","")).text=0
			Node.attributes.setNamedItem(XMLDom.createNode(2,"today","")).text=0
			Node.attributes.setNamedItem(XMLDom.createNode(2,"stats","")).text="失败，原因：参数错误。"
		End If
		i=i+1
	Next
		Dim allpost,alltopic,alltoday,topic,Child,TmpID
	allpost=0
	alltopic=0
	alltoday=0
	'统计一下更新情况
	For each boardid in Application(Dvbbs.CacheName&"_boardlist").documentElement.selectNodes("board/@boardid")
		Set Node =XMLDom.documentElement.selectNodes("result[@boardid="& boardid.text &"]")
		If Node.length > 0 Then
			topic=0
			Child=0
			today=0
			For Each TmpID in node
				topic=topic+CLng(tmpid.selectSingleNode("@topic").text)
				Child=Child+CLng(tmpid.selectSingleNode("@child").text)
				today=today+CLng(tmpid.selectSingleNode("@today").text)
			Next
			If topic+Child >0 Then
				alltopic=alltopic+topic
				allpost=allpost+topic+Child
				alltoday=alltoday+today
				UpDate_BoardInfoAndCache BoardID.text,topic,Child,today
			End If
		End If
	Next
	If allpost >0 Or alltopic >0 or alltoday >0  Then
		Dvbbs.Execute("update dv_setup Set forum_postNum=forum_postNum+"& allpost &",forum_TopicNum=forum_topicNum +"& alltopic &",Forum_TodayNum=Forum_TodayNum +"& alltoday )
		Dvbbs.loadSetup
	End If
	Tolog("批量审核")
End Sub

Sub View()
	Dim Node,id,replyid,Rs,posttable,SQL
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
		If replyid=0 Then
			SQL="Select * From "&posttable & " where rootid="&ID&" and ParentID=0 and Boardid=777"
		Else
			SQL="Select * From "&posttable & " where rootid="&ID&" and AnnounceID="&replyid&"and Boardid=777"
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
	Set XMLStyle=Dvbbs.CreateXmlDoc("Msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XMLStyle.loadxml template.html(2)
	'XMLStyle.load Server.MapPath("inc/AccessTopic.xslt")
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

Sub accesslist()
	Dim Tableid,posttable
	tableid=Request("tableid")
	If Not IsNumeric(tableid) Then tableid="0"
	If Trim(tableid)="" Then tableid="0"
	Dim i,SQL,node,keyword,tmpsql,Rs,SQL1,Pagesize,Page,pagecount
	keyword=Trim(Request("keyword"))
	If Request("tableid") <> "0" Then
		posttable=LCase(Dvbbs.NowUseBBS)
	Else
		posttable="dv_topic"
	End If
	If tableid <> "0" Then
		For i= 0 to UBound(TableList,2)
			If CStr(TableList(0,i))=tableid Then
				posttable=TableList(1,i)
				Exit For
			End If
		Next
	Else
		For i= 0 to UBound(TableList,2)
			If posttable=LCase(TableList(1,i))  Then
				tableid=TableList(0,i)
				Exit For
			End If
		Next
	End If
	Page=Request("Page")
	If Not IsNumeric(Page) Then Page="1"
	If Page="" Then Page="1"
	Page=CLng(Page)
		'传送参数到xml
	Pagesize=30'手工设置每页最大显示30条
	paramnode.attributes.setNamedItem(XMLDom.createNode(2,"tableid","")).text=tableid
	paramnode.attributes.setNamedItem(XMLDom.createNode(2,"keyword","")).text=keyword
	paramnode.attributes.setNamedItem(XMLDom.createNode(2,"pagesize","")).text=Pagesize
	paramnode.attributes.setNamedItem(XMLDom.createNode(2,"posttable","")).text=posttable
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
	SQL= SQL &" Where Boardid=777 "
	SQL1= SQL1 &" Where Boardid=777 "
	If Dvbbs.boardid <> 0 Then
		SQL= SQL &"and LockTopic="& Dvbbs.boardid &" "
		SQL1= SQL1 &"and LockTopic="& Dvbbs.boardid &" "
	End If
	If keyword<>"" Then
		SQL= SQL & tmpsql
		SQL1= SQL1 & tmpsql
	End If
	If tableid="0" Then
		SQL= SQL &" order by topicid"
	Else
		SQL= SQL &" order by AnnounceID"
	End If
	Set Rs=Dvbbs.Execute(SQL1)
	paramnode.attributes.setNamedItem(XMLDom.createNode(2,"count","")).text =Rs(0)
	'计算一下当前Page参数是否合法。如果超出范围，强制为最后一页
	If Rs(0) mod Pagesize =0 then
			PageCount= Rs(0) \ Pagesize
	Else
			PageCount= Rs(0) \ Pagesize+1
	End If
	If Page > PageCount Then Page=PageCount
	paramnode.attributes.setNamedItem(XMLDom.createNode(2,"page","")).text=Page
	If Rs(0) <> 0  and Not IsNull(Rs(0))Then
		Set Rs=Dvbbs.Execute(SQL)
		If Not page=1 Then 	Rs.Move(pagesize*(page-1))
		SQL=RS.GetRows(Pagesize)
		Set Node=Dvbbs.ArrayToxml(SQL,rs,"row","datarows")
		XMLDom.documentElement.appendChild(node.documentElement)
	End If
End Sub

Sub LoadTableList()
	Dim Rs
	Set Rs=Dvbbs.Execute("select * from [Dv_TableList]")
	TableList=Rs.GetRows(-1)
	Set XMLDom=Dvbbs.ArrayToxml(TableList,Rs,"posttable","xml")
End Sub

Sub LoadAccessCount()
	Dim Node,SQL
	If Dvbbs.Boardid > 0 Then
		SQL =" and locktopic="& Dvbbs.Boardid
	End If
	XMLDom.documentElement.attributes.setNamedItem(XMLDom.createNode(2,"count","")).text=Dvbbs.Execute("select Count(*) From Dv_topic Where boardid=777 "&SQL)(0)
	For Each Node In XMLDom.documentElement.selectNodes("posttable")
		Node.attributes.setNamedItem(XMLDom.createNode(2,"count","")).text=Dvbbs.Execute("select Count(*) From "& Node.selectSingleNode("@tablename").text &" Where boardid=777 "&SQL)(0)
	Next
End Sub
'更新用户发贴数,积分
Sub UpdatepostUser( UserID,postboardid,istopic)
	Dim Forum_user
	If Not IsObject(Application(dvbbs.CacheName &"_boarddata_" & postboardid)) Then Dvbbs.LoadBoardData postboardid
	Forum_user = Split(Application(Dvbbs.CacheName &"_boarddata_" & postboardid).documentElement.selectSingleNode("boarddata/@board_user").text,",")
	If istopic=1 Then
		Dvbbs.Execute("update [Dv_user] set UserPost=UserPost+1,UserTopic=UserTopic+1,userWealth=userWealth+"&CLng(Forum_user(1))&",userEP=userEP+"&CLng(Forum_user(6))&",userCP=userCP+"&CLng(Forum_user(11))&"  Where UserID="&userID)
	Else
		Dvbbs.Execute("update [Dv_user] set UserPost=UserPost+1,userWealth=userWealth+"&CLng(Forum_user(2))&",userEP=userEP+"&CLng(Forum_user(7))&",userCP=userCP+"&CLng(Forum_user(12))&" Where UserID="&userID)
	End If
End Sub

'更新主题最后回复人
Sub UpdateLastPost1(LastPost_1,LastPost_2,LastPost_3,LastPost_4,LastPost_5,LastPost_6,LastPost_7,LastPost_8)
Dim LastPost,LastPostArr
LastPost_4=Dvbbs.Replacehtml(Dvbbs.strCut(LastPost_4,20))
LastPost=LastPost_1&"$"&LastPost_2&"$"&LastPost_3&"$"&LastPost_4&"$"&LastPost_5&"$"&LastPost_6&"$"&LastPost_7&"$"&LastPost_8
Dvbbs.Execute("Update Dv_Topic Set lastpost='"&LastPost&"' ,LastPostTime='"&LastPost_3&"' Where Topicid="&LastPost_7&"")
End Sub 

'更新版块最新回复
Sub UpdateLastPost2(BoardId,topicid,announceid)
Dim LastPost,rs
Dim LastPost_1,LastPost_2,LastPost_3,LastPost_4,LastPost_5,LastPost_6,LastPost_7,LastPost_8
Set Rs=Dvbbs.Execute("Select * from dv_topic where topicid="&topicid&"")
If Not rs.eof Then 
LastPost_1=rs("postusername")
LastPost_2=announceid
LastPost_3=rs("dateandtime")
LastPost_4=Dvbbs.Replacehtml(Dvbbs.strCut(rs("title"),20))
LastPost_5=""
LastPost_6=rs("postuserid")
LastPost_7=topicid
LastPost_8=BoardId
LastPost=LastPost_1&"$"&LastPost_2&"$"&LastPost_3&"$"&LastPost_4&"$"&LastPost_5&"$"&LastPost_6&"$"&LastPost_7&"$"&LastPost_8
Else 
Exit Sub 
End If 
Dvbbs.Execute("Update Dv_Board set lastpost='"&LastPost&"' where boardid="&Boardid&"")
rs.close:Set rs=Nothing 
End Sub 

Sub UpDate_BoardInfoAndCache(BoardID,topic,Child,today)
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
			Application(Dvbbs.CacheName &"_information_" & updateboard).documentElement.selectSingleNode("information/@todaynum").text=CLng(Application(Dvbbs.CacheName &"_information_" & updateboard).documentElement.selectSingleNode("information/@todaynum").text)+today
		End If
	Next
End Sub

Sub Tolog(Info)
	Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,"&Dvbbs.BoardID&",'审核贴子','" & Dvbbs.MemberName & "','" & Dvbbs.CheckStr(Info) & "','" & Dvbbs.userTrueIP & "',3)")
End Sub
%>