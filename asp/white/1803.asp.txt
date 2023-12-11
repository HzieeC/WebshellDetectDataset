<!--#include FILE="conn.asp"-->
<!--#include file="inc/const.asp" -->
<!--#include file="inc/dv_clsother.asp" -->
<%
Dvbbs.Loadtemplates("show")
Dvbbs.stats=template.Strings(0)
Dvbbs.Nav
Dim BoardlistNode
Set BoardlistNode = Application(Dvbbs.CacheName&"_boardlist")

If Dvbbs.BoardID=0 then
	Dvbbs.Head_var 2,0,"",""
Else
	Dvbbs.Head_var 1,BoardlistNode.documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
End If
Dvbbs.ShowErr()
Dim TopStr
Main()
Dvbbs.ActiveOnline()
Dvbbs.NewPassword()
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub Main()
Dim TopicCount
Dim Pcount,endpage,star
Dim toback,tonext,uploadpath
Dim rsearch
Dim maxpage
Dim Tab
Dim Rs,SQl,i

tab=Request.cookies("tab")
If Dvbbs.Forum_Setting(76)="" Or Dvbbs.Forum_Setting(76)="0" Then Dvbbs.Forum_Setting(76)="UploadFile/"
If right(Dvbbs.Forum_Setting(76),1)<>"/" Then Dvbbs.Forum_Setting(76)=Dvbbs.Forum_Setting(76)&"/"
uploadpath=Trim(Dvbbs.Forum_Setting(76))
Dim bbsurl
Dim TempStr
bbsurl=Dvbbs.Get_ScriptNameUrl()
If Request("star")="" or Not IsNumeric(Request("star"))  then
	star=1
Else
	star=clng(Request("star"))
	toback=star-1
	tonext=star+1
End If 

If tab="" or not isnumeric(tab) Then tab=4

If Request("tab")="" or not isnumeric(Request("tab")) or Request("tab")="0" Then
	Tab=tab
Else
	Tab=clng(Request("tab"))
	Response.Cookies("tab").Expires=Date+365
	response.cookies("tab")=tab
End If
maxpage=clng(tab*3)	'每页显示文件的个数
If Request("username")="" or Request("filetype")="" or Request("boardid")="" then rsearch=""

If Request("boardid")<>"" and isnumeric(Request("boardid")) Then
	If clng(Request("boardid"))<>0 Then rsearch=rsearch&"and  F_BoardID="&clng(Request("boardid"))&" "
End If
If Request("filetype")<>"" And IsNumeric(Request("filetype")) Then rsearch=rsearch&"and  F_Type="&cint(Request("filetype"))&"  "
If Request("username")<>"" Then rsearch=rsearch&"and F_Username='"&Dvbbs.checkStr(Request("username"))&"'"
If Cint(Dvbbs.GroupSetting(49))=0 then Dvbbs.AddErrCode(54)
Dvbbs.ShowErr()
TempStr = Template.html(0)
TempStr = Replace(TempStr,"{$stats}",Dvbbs.Stats)

	Dim filetype,username
	Dim TempStr1,TempStr2,TempStr3,TempArray,TempArray1,FileArray
	TempArray = Split(template.html(4),"||")
	TempArray1 = Split(template.html(6),"||")
	FileArray = Split(Dvbbs.lanstr(5),"||")
	TempStr1 = template.html(1)
	filetype=Request("filetype")
	username=Request("username")
	'If filetype="" or Not IsNumeric(filetype) Then filetype = 0
	'取出总数

	'定义变量
	Dim t
	Dim F_ID,F_AnnounceID,F_BoardID,F_Filename,F_FileType,F_Type,F_Readme,F_DownNum,F_ViewNum,F_Flag
	Dim F_typename,showfile,golist
	Dim Edit
	Edit=False 
	t=0
	Dim LockinBid
	LockinBid = "0,444,777"
	If Not Dvbbs.master or Dvbbs.GroupSetting(37)="0" Then
		If HiddenBID<>"" Then
			LockinBid = LockinBid & "," & HiddenBID
		End If
	End If
	
	Set Rs=Dvbbs.Execute("select count(F_id) from DV_Upfile where not F_BoardID in("&LockinBid&") and F_Flag<>4 "&rsearch&" ")
	TopicCount=rs(0)

	Sql = "select F_ID,F_BoardID,F_AnnounceID,F_Filename,F_Readme,F_Type,F_Flag,F_Viewname,F_Username,F_DownNum,F_ViewNum,F_FileType from [DV_Upfile] where not F_BoardID in("&LockinBid&") and F_Flag<>4 "&rsearch&" order by F_ID desc  "
	Set rs=Dvbbs.Execute(sql)
	If Not (rs.eof and rs.bof) Then
		'TopicCount = Rs.RecordCount
		If TopicCount Mod Cint(maxpage)=0 Then
			Pcount= TopicCount \ Cint(maxpage)
  		Else
			Pcount= TopicCount \ Cint(maxpage)+1
	  	End If
		RS.MoveFirst
		If star > Pcount Then star = Pcount
		If star < 1 Then star = 1
		If star >1 Then 
			RS.Move (star-1) * maxpage
		End If

		SQL=Rs.GetRows(maxpage)
	Else
		TempStr = Replace(TempStr,"{$showloop}",template.html(3))
	End If
	Rs.Close
	Set Rs=Nothing

	If IsArray(Sql) Then
		For i=0 To Ubound(SQL,2)
			TempStr2 = TempStr1
			F_AnnounceID=Sql(2,i)
			F_Type=Sql(5,i)
			F_Readme=Sql(4,i)
			F_Flag=Sql(6,i)
			F_Filename=Sql(7,i)
			If F_Filename="" or Isnull(F_Filename) Then
				F_Filename=Sql(3,i)
					'判断文件是否本论坛，若不是则采用表中的记录 2004-9-12 Dv.Yz．
					If InStr(F_Filename,":") = 0 Or InStr(F_Filename,"//") = 0 Then
						If Dvbbs.Forum_Setting(75)="0" Then
						F_Filename = Bbsurl & Dvbbs.Forum_Setting(76) & F_Filename
						Else
						F_Filename = "showimg.asp?Boardid=" & Sql(1,i) & "&filename=" & F_Filename
						End If
					End If
			End If
			If F_Flag<>1 Then
				If isshow(Sql(1,i))=0 Then
					F_Filename=Dvbbs.Forum_Info(6)
				End If	
			End If
			If Instr(F_AnnounceID,"|") Then
				F_AnnounceID=split(F_AnnounceID,"|")
				TempStr2 = Replace(TempStr2,"{$golist}",TempArray(2))
				TempStr2 = Replace(TempStr2,"{$announceid}",F_AnnounceID(0))
				TempStr2 = Replace(TempStr2,"{$replyid}",F_AnnounceID(1))
			Else
				TempStr2 = Replace(TempStr2,"{$golist}",template.Strings(26))
			End If
			Select case F_Type
			Case 1
				F_typename=FileArray(1)
				showfile=TempArray(3)
				showfile=Replace(showfile,"{$imgurl}",Dvbbs.HTMLEncode(Dv_FilterJS(F_Filename)))
			Case 2
				F_typename=FileArray(2)
				showfile=TempArray(4)
				showfile=Replace(showfile,"{$fileurl}",Dvbbs.HTMLEncode(F_Filename))
			Case 3
				F_typename=FileArray(3)
				showfile=TempArray(4)
				showfile=Replace(showfile,"{$fileurl}",Dvbbs.HTMLEncode(F_Filename))
			Case 4
				F_typename=FileArray(4)
				showfile=TempArray(4)
				showfile=Replace(showfile,"{$fileurl}",Dvbbs.HTMLEncode(F_Filename))
			Case Else
				F_typename=FileArray(0)
				showfile=TempArray(5)
			End Select

			If Dvbbs.GroupSetting(48)=1 Then
				If Dvbbs.master or Dvbbs.superboardmaster or Dvbbs.boardmaster Then
					edit=True
				ElseIf Sql(8,i)=Dvbbs.membername Then
					edit=True
				Else
					edit=False
				End If
			End If

			If Dvbbs.UserID > 0 Then
				TempStr2 = Replace(TempStr2,"{$useraction}",TempArray1(0))
				If edit Then TempStr2 = Replace(TempStr2,"{$usereditinfo}",TempArray1(1))
				TempStr2 = Replace(TempStr2,"{$usereditinfo}","")
			End If
			TempStr2 = Replace(TempStr2,"{$useraction}","")
			If F_Type = 0 Then
				TempStr2 = Replace(TempStr2,"{$typenum}",Sql(9,i))
			Else
				TempStr2 = Replace(TempStr2,"{$typenum}",Sql(10,i))
			End If
			If Trim(F_Readme) <> "" Then
				If Dvbbs.strLength(F_Readme)>16 Then
					F_Readme = Dvbbs.HtmlEncode(Dvbbs.strCut(F_Readme,16)) & ".."
				Else
					F_Readme = Dvbbs.HtmlEncode(F_Readme)
				End If
			Else
				F_Readme = template.Strings(1)
			End If
			TempStr2 = Replace(TempStr2,"{$filereadme}",F_Readme)
			TempStr2 = Replace(TempStr2,"{$showfile}",showfile)
			TempStr2 = Replace(TempStr2,"{$filetype}",F_Type)
			TempStr2 = Replace(TempStr2,"{$refiletype}",Sql(11,i))
			TempStr2 = Replace(TempStr2,"{$typename}",F_typename)
			TempStr2 = Replace(TempStr2,"{$username}",Dvbbs.HtmlEncode(Sql(8,i)))
			TempStr2 = Replace(TempStr2,"{$fid}",Sql(0,i))
			TempStr2 = Replace(TempStr2,"{$boardid}",Sql(1,i))
			If t = (tab-1) Then
				t = 0
				TempStr2 = Replace(TempStr2,"{$tableinfo}",TempArray(1))
			Else
				TempStr2 = Replace(TempStr2,"{$tableinfo}","")
				t = t + 1
			End If
			TempStr3 = TempStr3 & TempStr2
		Next
	TempStr2 = template.html(5)
	TempStr2 = Replace(TempStr2,"{$Pcount}",Pcount)
	TempStr2 = Replace(TempStr2,"{$boardid}",Dvbbs.boardid)
	TempStr2 = Replace(TempStr2,"{$star}",star)
	TempStr2 = Replace(TempStr2,"{$pagelistnum}",TopicCount)
	TempStr2 = Replace(TempStr2,"{$limitednum}",maxpage)
	End If
	TempStr = Replace(TempStr,"{$pagelist}",TempStr2)
	TempStr = Replace(TempStr,"{$showloop}",TempStr3)
	top 10,"F_ViewNum"
	TempStr = Replace(TempStr,"{$toplist1}",TopStr)
	top 10,"F_ID"
	TempStr = Replace(TempStr,"{$toplist2}",TopStr)
	If UserName<>"" Then TempStr = Replace(TempStr,"{$userinfo}",TempArray(0))
	'If UserName="{$username}" Then UserName=""'o 08.01.22 17:07
	Dim FileJump
	For t = 0 To Ubound(FileArray)
		FileJump = FileJump & "<option value=" & t & "> " & FileArray(t) & "</option>"
	Next
	TempStr = Replace(TempStr,"{$filetypejump}",FileJump)
	TempStr = Replace(TempStr,"{$filetype}",filetype)
	TempStr = Replace(TempStr,"{$tab}",tab)
	TempStr = Replace(TempStr,"{$username}",UserName)
	TempStr = Replace(TempStr,"{$userinfo}","")
	TempStr = Replace(TempStr,"{$boardid}",Dvbbs.boardid)
	TempStr = Replace(TempStr,"{$alertcolor}",Dvbbs.mainsetting(1))
	TempStr = Replace(TempStr,"{$forum_picurl}",Dvbbs.Forum_PicUrl)
	Response.Write TempStr
End sub

'top 排行调用
Sub top(num,Str)
	Dim Rs
	TopStr = ""
	Dim TempStr1,TempStr2,TempStr3,tmptitle
	Dim k
	k=0
	TempStr1 = template.html(2)
	Set Rs=Dvbbs.Execute("select top "&num&" F_ID,F_BoardID,F_Filename,F_Readme from Dv_Upfile Where F_BoardID<>0 and F_BoardID<>444 and F_BoardID<>777 order by "&str&" desc,F_addTime desc")
	Do While Not Rs.Eof
		TempStr2 = TempStr1
		TempStr2 = Replace(TempStr2,"{$fid}",Rs(0))
		TempStr2 = Replace(TempStr2,"{$boardid}",Rs(1))
		If isshow(Rs("F_BoardID"))=1 Then
			If Rs(3)<>""  Then
				tmptitle=Rs(3) 
			Else
				tmptitle=Rs(2) 
			End If
			tmptitle=cutStr(tmptitle,16)
		Else
			tmptitle="==隐藏或认证版内容=="
		End If
		TempStr2 = Replace(TempStr2,"{$title}",tmptitle)
		TempStr3 = TempStr3 & TempStr2
		K=K+1
		
	Rs.MoveNext
	Loop
	Set Rs=Nothing
	TopStr = TempStr3
	TempStr3 = ""
	TempStr2 = ""
	TempStr1 = ""
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
	reUBBCode=Dvbbs.ChkBadWords(strContent)
End Function
'截取指定字符
Function cutStr(str,strlen)
	Str=reUBBCode(Str)
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
End Function
Function Dv_FilterJS(v)
	If Not Isnull(V) Then
		Dim t
		Dim re
		Dim reContent
		Set re=new RegExp
		re.IgnoreCase =True
		re.Global=True
		re.Pattern="(&#)"
		t=re.Replace(v,"<I>&#</I>")
		re.Pattern="(script)"
		t=re.Replace(t,"<I>script</I>")
		re.Pattern="(js:)"
		t=re.Replace(t,"<I>js:</I>")
		re.Pattern="(value)"
		t=re.Replace(t,"<I>value</I>")
		re.Pattern="(about:)"
		t=re.Replace(t,"<I>about:</I>")
		re.Pattern="(file:)"
		t=re.Replace(t,"<I>file:</I>")
		re.Pattern="(Document.cookie)"
		t=re.Replace(t,"<I>Documents.cookie</I>")
		re.Pattern="(vbs:)"
		t=re.Replace(t,"<I>vbs:</I>")
		re.Pattern="(on(mouse|Exit|error|click|key))"
		t=re.Replace(t,"<I>on$2</I>")
		Dv_FilterJS=t
		Set Re=Nothing
	End If 
End Function
Function isshow(BID)
	Dim node
	isshow=0
	If Not BoardlistNode.documentElement.selectSingleNode("board[@boardid='"&Bid&"']") Is Nothing Then
		If not BoardlistNode.documentElement.selectSingleNode("board[@boardid='"&Bid&"']/@checkout").text="1"   or Dvbbs.GroupSetting(37)="1" Then
			isshow=1
		End If
	End If
End Function

'/所以有隐藏版面ID，以逗号分隔
Function HiddenBID()
	Dim Nodes,Node,Boardids,i
	i = 0
	Set Nodes = BoardlistNode.documentElement.selectNodes("board[@hidden=1]/@boardid")
	For Each Node in  Nodes
		Boardids =  Boardids & Node.text
		i = i+1
		If i<>Nodes.length Then Boardids =  Boardids & ","
	Next
	HiddenBID = Boardids
End Function

'/所以有认证版面ID，以逗号分隔
Function CheckoutBID()
	Dim Nodes,Node,Boardids,i
	i = 0
	Set Nodes = BoardlistNode.documentElement.selectNodes("board[@checkout=1]/@boardid")
	For Each Node in  Nodes
		Boardids =  Boardids & Node.text
		i = i+1
		If i<>Nodes.length Then Boardids =  Boardids & ","
	Next
	CheckoutBID = Boardids
End Function
'/所以有锁定版面ID，以逗号分隔
Function ChecklockBID()
	Dim Nodes,Node,Boardids,i
	i = 0
	Set Nodes = BoardlistNode.documentElement.selectNodes("board[@checklock=1]/@boardid")
	For Each Node in  Nodes
		Boardids =  Boardids & Node.text
		i = i+1
		If i<>Nodes.length Then Boardids =  Boardids & ","
	Next
	ChecklockBID = Boardids
End Function
%>