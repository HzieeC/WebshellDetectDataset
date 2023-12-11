<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/dv_clsother.asp" -->
<!--#include file="inc/md5.asp"-->
<%
Dvbbs.LoadTemplates("paper_even_toplist")

Dim Rs,Sql,i,TempStr,isshow,cansmallpaper

Select Case Request("t")
Case "toplist"
	Show_Toplist()
Case "even"
	Show_Even()
Case "paper"
	Show_Paper()
Case "smallpaper"
	Show_SmallPaper()
Case Else
	Show_Toplist()
End Select

Dvbbs.ActiveOnline()
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub Show_Toplist()
	Dvbbs.stats=template.Strings(6)

	If Dvbbs.GroupSetting(1)="0" Then Dvbbs.AddErrCode(64)
	Dvbbs.ShowErr()

	Dim Page,Orders,ordername,Rs,SQL,keyword
	Dim select1,select2,select3,select4,select5,select6,select7,select8,select9
	Dim TempStr,TempStr1,TempStr2,TempStr3,TempArray,TopTempArray
	Dim TotalRec,i,Pcount

	TotalRec=0
	Page=request("page")
	If Page="" Or Not IsNumerIc(Page) Then Page=1
	Page=Clng(Page)
	If Not IsNumerIc(request("orders")) Or request("orders")="" Then
		Orders=1
	Else
		Orders=Cint(request("orders"))
	End If
	keyword=Request("keyword")
	If keyword<>"" Then keyword = Dvbbs.CheckStr(keyword)
	If Dvbbs.Forum_Setting(17)="0" Then keyword = ""

	TempStr = template.html(7)
	TopTempArray = Split(template.html(9),"||")
	If Dvbbs.Forum_Setting(17)="1" Then
		TempStr = Replace(TempStr,"{$isusersearch}",TopTempArray(4))
		TempStr = Replace(TempStr,"{$keyword}",keyword)
	Else
		TempStr = Replace(TempStr,"{$isusersearch}","")
	End If
	SQL="username,useremail,userclass,UserIM,UserPost,JoinDate,userwealth,userid,RLActTimeT"
	Select Case orders
	Case 1
		orders=1
		ordername=Replace(template.Strings(7),"{$toplistnum}",Dvbbs.Forum_Setting(68))
		select1="selected"
		If keyword<>"" Then keyword = " Where UserName='"&keyword&"'"
		SQL="select top "&Dvbbs.Forum_Setting(68)&" "&SQL&" from [dv_user] "&keyword&" order by UserPost desc"
		If Dvbbs.Forum_Setting(31)="0" Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(12)&"&action=OtherErr"
	Case 2
		orders=2
		ordername=template.Strings(8)
		select2="selected"
		If keyword<>"" Then keyword = " Where UserName='"&keyword&"'"
		SQL="select top "&Dvbbs.Forum_Setting(68)&" "&SQL&" from [dv_user] "&keyword&" order by JoinDate desc"
		If Dvbbs.Forum_Setting(31)="0" Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(12)&"&action=OtherErr"
	Case 3
		orders=3
		ordername=Replace(template.Strings(9),"{$toplistnum}",Dvbbs.Forum_Setting(68))
		select3="selected"
		If keyword<>"" Then keyword = " Where UserName='"&keyword&"'"
		SQL="select top "&Dvbbs.Forum_Setting(68)&" "&SQL&" from [dv_user] "&keyword&" order by userwealth desc"
		If Dvbbs.Forum_Setting(31)="0" Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(12)&"&action=OtherErr"
	Case 7
		orders=7
		ordername=template.Strings(10)
		select7="selected"
		If keyword<>"" Then keyword = " Where UserName='"&keyword&"'"
		SQL="select "&SQL&" from [dv_user]  "&keyword&" order by userid desc"
		If Dvbbs.Forum_Setting(27)="0" Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(14)&"&action=OtherErr"
	Case 8
		orders=8
		ordername=template.Strings(11)
		select8="selected"
		If keyword<>"" Then keyword = " And UserName='"&keyword&"'"
		SQL="select "&SQL&" from [dv_user] where usergroupid<=3 "&keyword&" order by usergroupid,UserPost desc"
		If Dvbbs.Forum_Setting(18)="0" Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(13)&"&action=OtherErr"
	Case 9
		
	Case Else
		orders=1
		ordername=Replace(template.Strings(7),"{$toplistnum}",Dvbbs.Forum_Setting(68))
		select1="selected"
		If keyword<>"" Then keyword = " Where UserName='"&keyword&"'"
		SQL="select top "&Dvbbs.Forum_Setting(68)&" "&SQL&" from [dv_user] "&keyword&" order by UserPost desc"
		If Dvbbs.Forum_Setting(31)="0" Then Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(12)&"&action=OtherErr"
	End Select

	Dvbbs.Stats = ordername
	Dvbbs.Nav()
	Dvbbs.ShowErr()
	Dvbbs.Head_var 0,0,template.Strings(6),"InfoList.asp?t=toplist"

	Set Rs=Dvbbs.Execute("Select Forum_PostNum,Forum_UserNum From Dv_Setup")
	TempStr = Replace(TempStr,"{$postnum}",Rs(0))
	TempStr = Replace(TempStr,"{$usernum}",Rs(1))

	If Orders=7 and keyword="" Then
		TotalRec=Rs(1)
		If IsSqlDataBase=1 And IsBuss=1 Then
			Dim Cmd
			Set cmd = Dvbbs.iCreateObject("ADODB.Command")
			Set cmd.ActiveConnection=conn
			cmd.CommandText="dv_toplist"
			cmd.CommandType=4
			cmd.Parameters.Append cmd.CreateParameter("@pagenow",3)
			cmd.Parameters.Append cmd.CreateParameter("@pagesize",3)
			cmd.Parameters.Append cmd.CreateParameter("@reture_value",3,2)
			cmd.Parameters.Append cmd.CreateParameter("@intUserRecordCount",3,2)
			cmd("@pagenow")=Page
			cmd("@pagesize")=Cint(Dvbbs.Forum_Setting(11))
			If Not IsObject(Conn) Then ConnectionDatabase
			Set Rs=Cmd.Execute
		Else
			Set Rs=Dvbbs.iCreateObject("ADODB.RecordSet")
			If Not IsObject(Conn) Then ConnectionDatabase
			Rs.Open SQL,Conn,1,1
			If Not Rs.Eof Then TotalRec=Rs.RecordCount
		End If
	Else
		Set Rs=Dvbbs.iCreateObject("ADODB.RecordSet")
		If Not IsObject(Conn) Then ConnectionDatabase
		Rs.Open SQL,Conn,1,1
		If Not Rs.Eof Then TotalRec=Rs.RecordCount
	End If
	Dvbbs.SqlQueryNum = Dvbbs.SqlQueryNum + 1
	If Rs.Eof And Rs.Bof Then
		TempStr = Replace(TempStr,"{$toplistloop}",TopTempArray(0))
		TempStr = Replace(TempStr,"{$pagelist}","")
	Else
		If TotalRec Mod Dvbbs.Forum_Setting(11)=0 Then
			Pcount= TotalRec \ Dvbbs.Forum_Setting(11)
		Else
			Pcount= TotalRec \ Dvbbs.Forum_Setting(11)+1
		End If

		If Not (IsSqlDataBase=1 And Orders=7 And IsBuss=1) Then
			RS.MoveFirst
			if Page > Pcount then Page = Pcount
   			if Page < 1 then Page=1
			RS.Move (Page-1) * Dvbbs.Forum_Setting(11)
			SQL=Rs.GetRows(Dvbbs.Forum_Setting(11))
		Else
			SQL=Rs.GetRows(-1)
		End If
		Set Rs=Nothing
		'username=0,useremail=1,userclass=2,UserIM=3,UserPost=4,JoinDate=5,userwealth=6,userid=7
		TempStr1 = template.html(8)
		For i = 0 To Ubound(SQL,2)
			TempStr2 = TempStr1
			TempArray = Split(Dvbbs.HtmlEncode(Replace(SQL(3,i)&"","'","\'")),"|||")
			TempStr2 = Replace(TempStr2,"{$userid}",SQL(7,i))
			TempStr2 = Replace(TempStr2,"{$username}",Dvbbs.HtmlEncode(SQL(0,i)))
			TempStr2 = Replace(TempStr2,"{$adddate}",SQL(5,i)&"")
			TempStr2 = Replace(TempStr2,"{$userclass}",SQL(2,i)&"")
			REM 修正文章数NULL值出错问题 2004-5-21 Dv.Yz
			TempStr2 = Replace(TempStr2,"{$article}",SQL(4,i)&"")
			TempStr2 = Replace(TempStr2,"{$wealth}",SQL(6,i))
			
			If Ubound(TempArray)>1 Then
				TempStr2 = Replace(TempStr2,"{$homepage}",TempArray(0))
				TempStr2 = Replace(TempStr2,"{$oicq}",TempArray(1))
				TempStr2 = Replace(TempStr2,"{$site}",Dvbbs.Forum_Info(0))
			Else
				TempStr2 = Replace(TempStr2,"{$homepage}","")
				TempStr2 = Replace(TempStr2,"{$oicq}","")
				TempStr2 = Replace(TempStr2,"{$site}","")
			End If
			TempStr3 = TempStr3 & TempStr2
		Next

		If IsSqlDataBase=1 And Orders=7 And keyword="" And IsBuss=1 Then
			TotalRec=cmd("@intUserRecordCount")
			If TotalRec Mod Dvbbs.Forum_Setting(11)=0 Then
				Pcount= TotalRec \ Dvbbs.Forum_Setting(11)
			Else
				Pcount= TotalRec \ Dvbbs.Forum_Setting(11)+1
			End If
			Set Cmd = Nothing
		End If

		TempStr = Replace(TempStr,"{$toplistloop}",TempStr3)
		TempStr = Replace(TempStr,"{$pagelist}",template.html(3))
		TempStr = Replace(TempStr,"{$page}",page)
		TempStr = Replace(TempStr,"{$Pcount}",Pcount)
		TempStr = Replace(TempStr,"{$action}","t=toplist&")
		TempStr = Replace(TempStr,"{$keyword}",Request("keyword"))
		TempStr = Replace(TempStr,"{$width}",Dvbbs.mainsetting(0))
		TempStr = Replace(TempStr,"{$alertcolor}",Dvbbs.mainsetting(1))
		TempStr = Replace(TempStr,"{$pagelimited}",Dvbbs.Forum_Setting(11))
		TempStr = Replace(TempStr,"{$listnum}",totalrec)
		TempStr = Replace(TempStr,"{$boardid}","0&orders="&orders)
		TempStr = Replace(TempStr,"{$oicqpic}",template.pic(1))
		TempStr = Replace(TempStr,"{$homepagepic}",template.pic(2))
		TempStr = Replace(TempStr,"{$msgpic}",template.pic(3))

		'管理团队
		If Dvbbs.Forum_Setting(18)<>"0" Then
			TempStr = Replace(TempStr,"{$myselect3}",TopTempArray(3))
		Else
			TempStr = Replace(TempStr,"{$myselect3}","")
		End If
		'用户排行
		If Dvbbs.Forum_Setting(31)<>"0" Then
			TempStr = Replace(TempStr,"{$myselect1}",TopTempArray(1))
		Else
			TempStr = Replace(TempStr,"{$myselect1}","")
		End If
		'所有用户
		If Dvbbs.Forum_Setting(27)<>"0" Then
			TempStr = Replace(TempStr,"{$myselect2}",TopTempArray(2))
		Else
			TempStr = Replace(TempStr,"{$myselect2}","")
		End If

		TempStr = Replace(TempStr,"{$ordername}",ordername)
		TempStr = Replace(TempStr,"{$pagelistnum}",Dvbbs.Forum_Setting(11))
		TempStr = Replace(TempStr,"{$select1}",select1)
		TempStr = Replace(TempStr,"{$select2}",select2)
		TempStr = Replace(TempStr,"{$select3}",select3)
		TempStr = Replace(TempStr,"{$select7}",select7)
		Response.Write TempStr
	End If
End Sub

Sub Show_Paper()
	If dvbbs.boardid=0 Then
		dvbbs.stats=template.Strings(0)
		Dvbbs.Nav()
		Dvbbs.Head_var 2,0,"",""
	Else
		dvbbs.stats=template.Strings(1)
		Dvbbs.Nav()
		Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
	End If

	If Not(Dvbbs.boardmaster or Dvbbs.master or Dvbbs.superboardmaster) Then Response.redirect "showerr.asp?ErrCodes=<li>只有管理员才能登录。&action=OtherErr"
	'If Dvbbs.Forum_Setting(56)=0 Then Dvbbs.AddErrCode(52)
	Dvbbs.ShowErr()
	If request("action")="delpaper" Then
		call batch()
	Else
		call boardpaper()
	End If
	Dvbbs.ShowErr()
End Sub

Sub Show_Even()
	isshow=False
	If Dvbbs.BoardID=0 then
		Dvbbs.stats=template.Strings(4)
		Dvbbs.nav()
		Dvbbs.Head_var 2,0,"",""
	Else
		Dvbbs.stats=template.Strings(5)
		Dvbbs.nav()
		Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
	End If
	If Cint(Dvbbs.GroupSetting(39))=0  And Not Dvbbs.master Then Dvbbs.AddErrCode(55)
	Dvbbs.ShowErr
	boardeven()
End Sub

Sub Show_SmallPaper()
	cansmallpaper=false
	Dvbbs.stats=Template.Strings(16)
	GetBoardPermission
	Dvbbs.Nav
	Dvbbs.ShowErr()
	If Cint(Dvbbs.GroupSetting(17))=0 then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(18)&"&action=OtherErr"
	Else
		If Dvbbs.userid=0 then
			Dvbbs.membername=Template.Strings(19)
		End If
		cansmallpaper=True 
	End If
	Dvbbs.ShowErr()
	If Request("action")="savepaper" then
		SavePaper
	Else
		SmallPaper_Main
	End If
End Sub

Sub boardpaper()
	Dim totalrec
	Dim n
	Dim currentpage,page_count,Pcount
	Pcount=0
	totalrec=0
	currentPage=request("page")
	If currentpage="" Or not IsNumeric(currentpage) Then
		currentpage=1
	Else
		currentpage=clng(currentpage)
	End If
	Dim TempArray,TempStr1,TempStr2,TempStr3
	TempStr = template.html(0)
	TempArray = Split(template.html(1),"||")
	TempStr2 = template.html(2)
	If Dvbbs.GroupSetting(27)="1" Then TempStr = Replace(TempStr,"{$manageinfo}",TempArray(2))
	TempStr = Replace(TempStr,"{$manageinfo}","")

	set rs=Dvbbs.iCreateObject("adodb.recordset")
	If dvbbs.boardid=0 Then
	sql="select * from dv_smallpaper order by s_addtime desc"
	Else
	sql="select * from dv_smallpaper where s_boardid="&dvbbs.boardid&" order by s_addtime desc"
	End If
	If Not IsObject(Conn) Then ConnectionDatabase
	rs.open sql,conn,1,1
	If rs.bof And rs.eof Then
		TempStr1 = TempArray(0)
		TempStr = Replace(TempStr,"{$pagelist}","")
	Else
		rs.PageSize = Dvbbs.Forum_Setting(11)
		rs.AbsolutePage=currentpage
		page_count=0
		totalrec=rs.recordcount
		while (not rs.eof) And (not page_count = rs.PageSize)
			TempStr3 = TempStr2
			TempStr3 = Replace(TempStr3,"{$username}",Dvbbs.HtmlEncode(rs("s_username")))
			TempStr3 = Replace(TempStr3,"{$addtime}",rs("s_addtime"))
			TempStr3 = Replace(TempStr3,"{$title}",Dvbbs.HtmlEncode(rs("s_title")))
			TempStr3 = Replace(TempStr3,"{$boardid}",rs("s_boardid"))
			If Dvbbs.GroupSetting(27)="1" Then
				TempStr3 = Replace(TempStr3,"{$manageinfo1}",TempArray(1) & rs("s_hits"))
			Else
				TempStr3 = Replace(TempStr3,"{$manageinfo1}",rs("s_hits"))
			End If
			TempStr3 = Replace(TempStr3,"{$sid}",rs("s_id"))
			TempStr1 = TempStr1 & TempStr3
			page_count = page_count + 1
		rs.movenext
		wend
		Pcount=rs.PageCount
	rs.close
	set rs=nothing	
	End If
	
	TempStr = Replace(TempStr,"{$paperloop}",TempStr1)
	TempStr = Replace(TempStr,"{$pagelist}",template.html(3))
	TempStr = Replace(TempStr,"{$page}",currentpage)
	TempStr = Replace(TempStr,"{$keyword}",Request.QueryString("keyword"))
	TempStr = Replace(TempStr,"{$Pcount}",Pcount)
	TempStr = Replace(TempStr,"{$action}","t=paper&")
	TempStr = Replace(TempStr,"{$width}",Dvbbs.mainsetting(0))
	TempStr = Replace(TempStr,"{$alertcolor}",Dvbbs.mainsetting(1))
	TempStr = Replace(TempStr,"{$pagelimited}",Dvbbs.Forum_Setting(11))
	TempStr = Replace(TempStr,"{$listnum}",totalrec)
	Response.Write TempStr
	%>
	<SCRIPT LANGUAGE="JavaScript">
	<!--
		//复选表单全选事件 form：表单名
	function CheckAll(form)  {
	for (var i=0;i<form.elements.length;i++)
	{
		var e = form.elements[i];
		if (e.name != 'chkall'&&e.type=="checkbox")
		{
			e.checked = form.chkall.checked;
		}
	}
	}
	//-->
	</SCRIPT>
	
	<%
End Sub

Sub batch()
	Dim sid,fixid
	Dim adminpaper
	adminpaper=False
	If dvbbs.userid=0 Then
		Dvbbs.AddErrCode(34)
	End If
	If (dvbbs.master Or dvbbs.superboardmaster Or dvbbs.boardmaster) And Cint(dvbbs.GroupSetting(27))=1 Then
		adminpaper=True
	Else
		adminpaper=False
	End If
	If dvbbs.UserGroupID>3 And Cint(dvbbs.GroupSetting(27))=1 Then
		adminpaper=True
	End If
	If dvbbs.FoundUserPer And Cint(dvbbs.GroupSetting(27))=1 Then
		adminpaper=True
	ElseIf dvbbs.FoundUserPer And Cint(dvbbs.GroupSetting(27))=0 Then
		adminpaper=False
	End If
	If not adminpaper Then
		Dvbbs.AddErrCode(28)
	End If
	If request.form("sid")="" Then
		Dvbbs.AddErrCode(35)
	Else
		sid=replace(request.Form("sid"),"'","")
		sid=replace(sid,";","")
		sid=replace(sid,"--","")
		sid=replace(sid,")","")
		fixid=replace(sid," ","")
		fixid=replace(fixid,",","")
		If Not IsNumeric(fixid) Then
			Dvbbs.AddErrCode(35)
			Exit Sub
		End If
	End If 	
	If dvbbs.ErrCodes<>"" Then exit Sub
	Dvbbs.Execute("delete from dv_smallpaper where s_id in ("&sid&")")
	LoadBoardNews_Paper()
	Dvbbs.Dvbbs_Suc(template.Strings(2))
	Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,"&Dvbbs.BoardID&",'小字报','" & Dvbbs.MemberName & "','在 "&Dvbbs.boardtype&"删除小字报','" & Dvbbs.userTrueIP & "',3)")
End Sub

Sub boardeven()
	Dim currentpage,page_count,Pcount
	Dim endpage
	Dim totalrec
	totalrec=0
	currentPage=request("page")
	If currentpage="" Or Not IsNumeric(currentpage) Then
		currentpage=1
	Else
		currentpage=clng(currentpage)
	End If
	Dim TempStr,TempStr1,TempStr2,TempStr3
	Dim TempArray
	TempStr = template.html(5)
	TempArray = Split(template.html(6),"||")
	TempStr2 = TempArray(1)
	Dim keyword,addstr
	If Dvbbs.Master Or Dvbbs.Superboardmaster Then
		keyword=Dvbbs.Checkstr(Request("keyword"))
		If keyword<>"" Then
			addstr="and (l_touser like '%"&keyword&"%' Or l_content like '%"&keyword&"%' Or l_username like '%"&keyword&"%')"
		End If
	End If
	Set Rs=Dvbbs.iCreateObject("ADODB.RecordSet")
	If Dvbbs.BoardID>0 Then
		sql="select * from dv_log where l_boardid="&DVbbs.BoardID&" and l_type >2 "&addstr&" order by l_addtime desc"
	Else
		sql="select * from dv_log where l_type > 2  "&addstr&" order by l_addtime desc"
	End If
	If Not IsObject(Conn) Then ConnectionDatabase
	Rs.Open sql,conn,1,1
	If rs.bof And rs.eof Then
		TempStr1 = TempArray(0)
	Else
		chkshow()
		rs.PageSize = Dvbbs.Forum_Setting(11)
		rs.AbsolutePage=currentpage
		page_count=0
		totalrec=rs.recordcount
		While (Not rs.eof) And (Not page_count = rs.PageSize)
			TempArray = rs("l_touser") & "||" & rs("l_content") & "||" & rs("l_username")
			TempArray = Dvbbs.HtmlEncode(TempArray)
			TempArray = Split(TempArray,"||")
			TempStr3 = TempStr2
			TempStr3 = Replace(TempStr3,"{$username}",TempArray(0))
			TempStr3 = Replace(TempStr3,"{$content}",TempArray(1))
			TempStr3 = Replace(TempStr3,"{$addtime}",rs("l_addtime"))
			If isshow or Dvbbs.MemberName=rs("l_username") Then
				TempStr3 = Replace(TempStr3,"{$postuser}","<a href=dispuser.asp?name="&TempArray(2)&" target=_blank>"&TempArray(2)&"</a>")
			Else
				TempStr3 = Replace(TempStr3,"{$postuser}","保密")
			End If
			TempStr1 = TempStr1 & TempStr3
			page_count = page_count + 1
		Rs.Movenext
		Wend
	End If

  	If totalrec Mod Dvbbs.Forum_Setting(11)=0 Then
     		Pcount= totalrec \ Dvbbs.Forum_Setting(11)
  	Else
     		Pcount= totalrec \ Dvbbs.Forum_Setting(11)+1
  	End If
	TempStr = Replace(TempStr,"{$evenloop}",TempStr1)
	TempStr = Replace(TempStr,"{$pagelist}",template.html(3))
	TempStr = Replace(TempStr,"{$page}",currentpage)
	TempStr = Replace(TempStr,"{$Pcount}",Pcount)
	TempStr = Replace(TempStr,"{$action}","t=even&")
	TempStr = Replace(TempStr,"{$width}",Dvbbs.mainsetting(0))
	TempStr = Replace(TempStr,"{$alertcolor}",Dvbbs.mainsetting(1))
	TempStr = Replace(TempStr,"{$pagelimited}",Dvbbs.Forum_Setting(11))
	TempStr = Replace(TempStr,"{$listnum}",totalrec)
	TempStr = Replace(TempStr,"{$boardid}",Dvbbs.BoardID)
	Dim Searchstr
	If Dvbbs.Master Or Dvbbs.Superboardmaster Then
		Searchstr=Replace(template.html(11),"{$boardid}",Dvbbs.BoardID)
		Searchstr=Replace(Searchstr,"{$keyword}",Request("keyword"))
		Response.Write Searchstr
		TempStr = Replace(TempStr,"{$keyword}",Request("keyword"))
	Else
		TempStr = Replace(TempStr,"{$keyword}","")
	End If
	Response.Write TempStr
	Rs.Close
	Set Rs=Nothing
	
End Sub
Sub chkshow()
	If Dvbbs.master or Dvbbs.superboardmaster  Then
		isshow=True
	ElseIf Dvbbs.BoardID<>0 Then 
		If Dvbbs.Board_Setting(36)<>"" and IsNumeric(Dvbbs.Board_Setting(36)) Then
			If Cint(Dvbbs.Board_Setting(36))=1  Then
				isshow=True
			Else
				isshow=False 
			End If
		End If
	Else
		isshow=False 
	End If
End Sub

Sub SmallPaper_Main()
	Dim redcolor,ispass1,ispass2
	Dim Tempwrite,SQL
	redcolor=Dvbbs.Mainsetting(1)
	If Dvbbs.Forum_Setting(35) then
		ispass1=Template.Strings(21)
	Else
		ispass1=Template.Strings(20)
	End if
	If Dvbbs.Forum_Setting(34) then
		ispass2=Template.Strings(21)
	Else
		ispass2=Template.Strings(20)
	End If
	If Not IsObject(Conn) Then ConnectionDatabase
	Dim tmp
	If IsSqlDataBase=1 Then
		SQL="delete from Dv_smallpaper where datediff(d,s_addtime,"&SqlNowString&")>1"
	Else
		SQL="delete from Dv_smallpaper where datediff('d',s_addtime,"&SqlNowString&")>1"
	End If
	Conn.Execute SQL,tmp
	If TMP >0 Or Not IsObject(Application(Dvbbs.CacheName & "_smallpaper")) Then
		LoadBoardNews_Paper()
	End If
	Dvbbs.head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
	Tempwrite=Template.html(10)
	Tempwrite=Replace(Tempwrite,"{$username}",Dvbbs.HtmlEnCode(Dvbbs.Membername))
	Tempwrite=Replace(Tempwrite,"{$password}",Dvbbs.Memberword)
	Tempwrite=Replace(Tempwrite,"{$redcolor}",redcolor)
	Tempwrite=Replace(Tempwrite,"{$paymoney}",Dvbbs.GroupSetting(46))
	Tempwrite=Replace(Tempwrite,"{$ispass1}",ispass1)
	Tempwrite=Replace(Tempwrite,"{$ispass2}",ispass2)
	Tempwrite=Replace(Tempwrite,"{$boardid}",Dvbbs.Boardid)
	Response.Write Tempwrite
End Sub

Sub savepaper()
	Dim username
	Dim password
	Dim title
	Dim content
	userName=Dvbbs.Checkstr(trim(request.form("username")))
	PassWord=Dvbbs.Checkstr(trim(request.form("password")))
	title=Dvbbs.Checkstr(trim(request.form("title")))
	Content=Dvbbs.Checkstr(request.form("Content"))
	If Dvbbs.chkpost=False Then
		Dvbbs.AddErrCode(16)
	End If
	If UserName="" Or Dvbbs.strLength(userName)>Cint(Dvbbs.Forum_setting(41)) Or Dvbbs.strLength(userName) < Cint(Dvbbs.Forum_setting(40)) then
		Dvbbs.AddErrCode(66)
	End If
	If title="" Then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(22)&"&action=OtherErr"
	ElseIf Dvbbs.strLength(title)>80 then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(23)&"&action=OtherErr"
	End If
	If content="" Then
		Dvbbs.AddErrCode(80)
	ElseIf Dvbbs.strLength(content)>500 then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(24)&"&action=OtherErr"
	End If
	Dvbbs.ShowErr()
	'客人不允许发，验证用户
	If cansmallpaper Then
		If Not ChkUserLogin(password,username) Then
			Dvbbs.AddErrCode(12)
			Dvbbs.Showerr()
		End If
		Dim Rs,SQL
		Set Rs=Dvbbs.iCreateObject("adodb.recordset")
		sql="Select userWealth From [Dv_User] Where UserName='"&UserName&"'"
		Rs.open sql,conn,1,3
		If Not(rs.eof and rs.bof) Then
			If CLng(rs("UserWealth"))<Clng(Dvbbs.GroupSetting(46)) Then
				Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(25)&"&action=OtherErr"
			Else
				rs("UserWealth")=rs("UserWealth")-Cint(Dvbbs.GroupSetting(46))
				rs.update
			End If
		Else
			If Dvbbs.userid<>0 or username<>Template.Strings(19) Then
				Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(26)&"&action=OtherErr"
			End If
		End If
		Rs.close:Set Rs=Nothing
	End If
	Dvbbs.ShowErr()
	sql="insert into Dv_smallpaper (s_boardid,s_username,s_title,s_content) values "&_
		"("&_
		Dvbbs.boardid&",'"&_
		username&"','"&_
		title&"','"&_
		content&"')"
		'response.write sql
	Dvbbs.execute(sql)
	'发表小字报成功后RELOAD缓存
	LoadBoardNews_Paper()
	Dvbbs.head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
	Dvbbs.Dvbbs_suc("<li>"&Template.Strings(27))
End Sub

'检查用户身份
Public Function ChkUserLogin(password,username)
	Dim SQL,Rs
	ChkUserLogin=False
	If PassWord<>Dvbbs.MemberWord Then PassWord=md5(PassWord,16)
	'校验用户名和密码是否合法
	If Not IstrueName(UserName) Then Dvbbs.AddErrCode(18)
	If Len(PassWord)<>16 AND Len(PassWord)<>32 Then Dvbbs.AddErrCode(18)
	If UserName=Dvbbs.MemberName Then PassWord=Dvbbs.MemberWord
	Dvbbs.ShowErr()
	SQL = "Select UserGroupID,userpassword,lockuser,TruePassWord From [Dv_User] Where UserName='"&UserName&"' "
	Set Rs=Dvbbs.Execute(SQL)
	If Not Rs.EOF Then
		If PassWord<>rs(1) And PassWord<>rs(3) Then
			ChkUserLogin=False
		ElseIf rs(2)=1 or rs(0)=5 Then
			ChkUserLogin=False
		Else
			ChkUserLogin=True
		End If
	Else
		Exit Function 
	End If:Set Rs = Nothing 
End Function
'通用函数
Function IstrueName(uName)
	IstrueName=False
	If InStr(uName,"=")>0 Then Exit Function
	If InStr(uName,"%")>0 Then Exit Function 
	If InStr(uName,Chr(32))>0 Then Exit Function 
	If InStr(uName,"?")>0 Then Exit Function 
	If InStr(uName,"&")>0 Then Exit Function 
	If InStr(uName,";")>0 Then Exit Function 
	If InStr(uName,",")>0 Then Exit Function 
	If InStr(uName,"'")>0 Then Exit Function 
	If InStr(uName,Chr(34))>0 Then Exit Function 
	If InStr(uName,chr(9))>0 Then Exit Function 
	If InStr(uName,"")>0 Then Exit Function 
	If InStr(uName,"$")>0 Then Exit Function
	IstrueName=True 	
End Function

%>