<!--#include FILE="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/chkinput.asp" -->
<%
'Edit by yangzheng 2003-11-24
'Response.Write dvbbs.skinid
Dim PageSid
PageSid = Dvbbs.Skinid
Dvbbs.LoadTemplates("usermanager")
Dvbbs.Skinid = PageSid
Dim Dhlint,Updepth
Dhlint=template.html(0)'&"<br>"
Updepth=template.Strings(0)
Dvbbs.LoadTemplates("show")
Dvbbs.stats=template.Strings(2)
Dvbbs.nav()
Dim Rs,sql
Dim TopicCount
Dim Redcolor
Dim Pcount,endpage,star,page_count

If Request("star")="" or not isnumeric(request("star")) Then
	star=1
Else
	star=clng(request("star"))
End If

If Dvbbs.UserID=0 Then
	Dvbbs.AddErrCode(6)
End If

Dvbbs.ShowErr()
Redcolor=Dvbbs.Mainsetting(1)
Dvbbs.Head_var 0,0,Updepth,"usermanager.asp"

Select Case request("action")
	Case "edit"
		call edit()
	Case "fsave"
		call filesave()
	Case "fadd"
		call addnew()	
	Case "fsnew"
		call savenew()
	Case  "fdel"
		call fdel()
	Case "alldel"
		call alldel()
	Case Else 
		Call main()
End Select 

Dvbbs.ShowErr()
Call Dvbbs.activeonline()
Call Dvbbs.footer()
Dvbbs.PageEnd()

Sub main()
	Dim Mylist,sname,stype,searchsql,MainTable,Toplist
	Mylist=Split(Template.Strings(4),"||")
	stype=Request("Stype")
	TopicCount=filenum("")
	If stype="" or not  isnumeric(stype)  Then
		sname=template.Strings(3)
		searchsql=""
		TopicCount=filenum("")
	Else
		Select Case stype
			Case 1
				sname=Split(Dvbbs.lanstr(5),"||")(1)
				searchsql="F_Type=1 and"
			Case 2
				sname=Split(Dvbbs.lanstr(5),"||")(2)
				searchsql="F_Type=2 and"
			Case 3
				sname=Split(Dvbbs.lanstr(5),"||")(3)
					searchsql="F_Type=3 and"
			Case 4
				sname=Split(Dvbbs.lanstr(5),"||")(4)
				searchsql="F_Type=4 and"
			Case 0
				sname=Split(Dvbbs.lanstr(5),"||")(0)
				searchsql="F_Type=0 and"
			Case Else
				sname=Template.Strings(3)
				searchsql=""
			End Select
		TopicCount=filenum(clng(stype))
	End If
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
	Dim F_Type,F_typename,useradmin,readme
	Set rs=dvbbs.execute("select * from [DV_Upfile] where  "&searchsql&"  F_UserID="&Dvbbs.UserID&" order by F_ID desc ")
	If Rs.EOF Then
		Toplist=Template.html(9)
	Else
		If TopicCount mod Cint(Dvbbs.Forum_Setting(11))=0 Then
			Pcount= TopicCount \ Cint(Dvbbs.Forum_Setting(11))
		Else
			Pcount= TopicCount \ Cint(Dvbbs.Forum_Setting(11))+1
		End If
		RS.MoveFirst
		If star > Pcount Then star = Pcount
		If star < 1 Then star = 1
		RS.Move (star-1) * Dvbbs.Forum_Setting(11)
		page_count=0
		Do while not rs.eof and page_count < Cint(Dvbbs.Forum_Setting(11))
		F_Type=rs("F_Type")
		If F_Type=1 Then
			F_typename=Split(Dvbbs.lanstr(5),"||")(1)
		ElseIf F_Type=2 Then
			F_typename=Split(Dvbbs.lanstr(5),"||")(2)
		ElseIf F_Type=3 Then
			F_typename=Split(Dvbbs.lanstr(5),"||")(3)
		ElseIf F_Type=4 Then
			F_typename=Split(Dvbbs.lanstr(5),"||")(4)
		Else
			F_typename=Split(Dvbbs.lanstr(5),"||")(0)
		End If
		If Rs("F_Readme")<>"" or not isnull(rs("F_Readme")) Then
			If Len(Rs("F_Readme"))>26 Then
				readme=""&left(Dvbbs.HtmlEnCode(replace(rs("F_Readme"),chr(10)," ")),26)&"..."
			Else
				readme=Dvbbs.HtmlEnCode(rs("F_Readme"))
			End If
		End If
		If Dvbbs.GroupSetting(48)=1 Then
			If (instr(rs("F_Filename"),"//") or instr(rs("F_Filename"),":")) and cint(rs("F_Flag"))=1 Then
				useradmin=" <input type=checkbox class=chkbox name=delid value="""&rs("F_ID")&"""> "
			Else
				useradmin=" <input type=checkbox  class=chkbox value="""" disabled> "
			End If
			useradmin=useradmin+"<a href=?action=edit&editid="&rs("F_ID")&"  >"&Mylist(0)&"</a>  | <a href=fileshow.asp?action=send&id="&rs("F_ID")&"  >"&Mylist(1)&"</a>"
		Else
			useradmin=" —— "
		End If
		page_count = page_count + 1
		Toplist=Toplist&Template.html(8)
		Toplist=Replace(Toplist,"{$F_FileType}",Rs("F_FileType")&"")
		Toplist=Replace(Toplist,"{$boardid}",Rs("F_BoardID"))
		Toplist=Replace(Toplist,"{$F_ID}",Rs("F_ID"))
		Toplist=Replace(Toplist,"{$readme}",readme)
		Toplist=Replace(Toplist,"{$F_FileSize}",Rs("F_FileSize")&"")
		Toplist=Replace(Toplist,"{$F_DownNum}",Rs("F_DownNum"))
		Toplist=Replace(Toplist,"{$F_ViewNum}",Rs("F_ViewNum"))
		Toplist=Replace(Toplist,"{$F_AddTime}",Rs("F_AddTime"))
		Toplist=Replace(Toplist,"{$F_typename}",F_typename)
		Toplist=Replace(Toplist,"{$useradmin}",useradmin)
		rs.movenext
		loop
	End If
	Rs.close:Set Rs=Nothing
	MainTable=template.html(7)
	MainTable=Replace(MainTable,"{$Filelist}",Toplist)
	MainTable=Replace(MainTable,"{$username}",Dvbbs.Membername)
	If Pcount>0 Then MainTable=Replace(MainTable,"{$ShowPage}",ShowPage(star,Pcount,TopicCount,Cint(Dvbbs.Forum_Setting(11)),redcolor))
	MainTable=Replace(MainTable,"{$ShowPage}","")
	Response.Write Dhlint&MainTable
End Sub

'分页代码
Function ShowPage(CurrentPage,Pcount,totalrec,PageNum,redcolor)
	Dim SearchStr,Stype
	If Request("Stype")="" Then
		Stype = 5
	Else
		Stype = CInt(Request("Stype"))
	End If
	SearchStr="Boardid="&Dvbbs.boardid&"&Stype="&Stype&"&reaction=onlineinfo"
	ShowPage=template.html(10)
	ShowPage=Replace(ShowPage,"{$CurrentPage}",CurrentPage)
	ShowPage=Replace(ShowPage,"{$Pcount}",Pcount)
	ShowPage=Replace(ShowPage,"{$PageNum}",PageNum)
	ShowPage=Replace(ShowPage,"{$totalrec}",totalrec)
	ShowPage=Replace(ShowPage,"{$SearchStr}",SearchStr)
	ShowPage=Replace(ShowPage,"{$redcolor}",redcolor)
End Function

'编辑文件
Sub edit()
	Dim editid
	Dim F_Type,F_typename,filename,chefile,con,body
	Dim F_Username,postid,F_rootid,F_bbsid,F_Flag
	Dim myurl,upfilexs
	Dim Tempwrite,checked1,checked2

	myurl=false
	editid=trim(request("editid"))
	If Not IsNumeric(editid) or IsNull(editid) Then
		Dvbbs.AddErrCode(34)
		Exit Sub
	End If
	set rs=Dvbbs.execute("select * from [DV_Upfile] where F_ID="&editid)
	if rs.eof and rs.bof then
		Dvbbs.AddErrCode(34)
		exit sub
	else
		F_Username=rs("F_Username")
		F_Type=rs("F_Type")
		filename=rs("F_Filename")
		F_Flag=rs("F_Flag")
		con=rs("F_Readme")

		If instr(filename,"//")=0 or instr(filename,":")=0 then
			myurl=True
			If Trim(Dvbbs.Forum_Setting(76))="" Or Dvbbs.Forum_Setting(76)="0" Then Dvbbs.Forum_Setting(76)="UploadFile/"
			If right(Dvbbs.Forum_Setting(76),1)<>"/" Then Dvbbs.Forum_Setting(76)=Dvbbs.Forum_Setting(76)&"/"
			filename = Trim(Dvbbs.Forum_Setting(76)) & filename
		End if
		If F_Type=1 then
			F_typename="<img src='"&filename&"' style='border: 1 solid #000000' width=80 height=80>"
		Else
			F_typename=rs("F_FileType")&Template.Strings(5)
		End if

		If con<>"" then
			body=replace(con,"<br>",chr(13))
			body=replace(body,"&nbsp;","")
			body=body+chr(13)
		End if
	
		If myurl then 
			If instr(rs("F_AnnounceID"),"|") then
				postid=split(rs("F_AnnounceID"),"|")
				F_rootid=postid(0)
				F_bbsid=postid(1)
			End if

			upfilexs="<tr><td valign=middle align=right  class=tablebody2 >"&Template.Strings(6)&"</td><td width=""100%"" height=""26"" class=tablebody1>"
			If Dvbbs.Master Or Dvbbs.superboardmaster Or Dvbbs.boardmaster Or Dvbbs.GroupSetting(48)=1 then
				upfilexs=upfilexs&Split(Template.Strings(7),"||")(0)
				upfilexs=upfilexs&"<input type=text name=""F_BoardID"" value="""&Rs("F_BoardID")&""" size=8>&nbsp;"
				upfilexs=upfilexs&Split(Template.Strings(7),"||")(1)
				upfilexs=upfilexs&"<input type=text name=""F_AnnounceID"" value="""&Rs("F_AnnounceID")&""">&nbsp;<font color="
				upfilexs=upfilexs&Dvbbs.Mainsetting(1)&">"
				upfilexs=upfilexs&Template.Strings(8)&"</font>"
			Else
				upfilexs=upfilexs&"<input type=hidden name=""F_BoardID"" value="
				upfilexs=upfilexs&Rs("F_BoardID")
				upfilexs=upfilexs&"><input type=hidden name=""F_AnnounceID"" value="
				upfilexs=upfilexs&Rs("F_AnnounceID")&">"
			End if
			upfilexs=upfilexs&"<a href=""dispbbs.asp?Boardid="
			upfilexs=upfilexs&Rs("F_BoardID")
			upfilexs=upfilexs&"&ID="
			upfilexs=upfilexs&F_Rootid
			upfilexs=upfilexs&"&replyID="
			upfilexs=upfilexs&F_bbsid
			upfilexs=upfilexs&"&skin=1"" target=""_blank"" title="
			upfilexs=upfilexs&Template.Strings(9)
			upfilexs=upfilexs&">[<font color="
			upfilexs=upfilexs&Dvbbs.Mainsetting(1)
			upfilexs=upfilexs&">"
			upfilexs=upfilexs&Template.Strings(11)
			upfilexs=upfilexs&"</font>]</a></td></tr>"
		Else
			upfilexs=upfilexs&"<input type=hidden name=""F_BoardID"" value="
			upfilexs=upfilexs&Rs("F_BoardID")
			upfilexs=upfilexs&"><tr><td width=""15%"" height=""26"" class=tablebody2 align=right>"
			upfilexs=upfilexs&Template.Strings(10)
			upfilexs=upfilexs&"</td><td width=""85%""  height=""26"" class=tablebody1 align=left><input type=text name=""fileurl"" size=""80%"" value="
			upfilexs=upfilexs&Rs("F_Filename")
			upfilexs=upfilexs&"></td></tr>"
		End if
		Tempwrite=Template.html(11)
		If F_Flag<2 then
			Tempwrite=Replace(Tempwrite,"{$checked1}","checked")
			Tempwrite=Replace(Tempwrite,"{$checked2}","")
		Else
			Tempwrite=Replace(Tempwrite,"{$checked1}","")
			Tempwrite=Replace(Tempwrite,"{$checked2}","checked")
		End if
		Tempwrite=Replace(Tempwrite,"{$f_username}",Dvbbs.HtmlEnCode(F_Username))
		Tempwrite=Replace(Tempwrite,"{$f_boardid}",rs("F_BoardID"))
		Tempwrite=Replace(Tempwrite,"{$f_id}",rs("F_ID"))
		Tempwrite=Replace(Tempwrite,"{$f_typename}",F_typename)
		Tempwrite=Replace(Tempwrite,"{$upfilexs}",upfilexs)
		Tempwrite=Replace(Tempwrite,"{$body}",Dvbbs.HtmlEnCode(body))
		Tempwrite=Replace(Tempwrite,"{$username}",Dvbbs.Membername)''o 08.01.22 17:25'
		Response.Write Dhlint&Tempwrite
	End if
	rs.close:set rs=nothing
End sub

'保存修改
Sub filesave()
	If Dvbbs.GroupSetting(48)=0 Then
		Dvbbs.AddErrCode(28)
		Exit Sub
	End If 
	dim saveid,F_Readme,Fflag,fileurl
	dim F_BoardID,F_AnnounceID

	F_BoardID=dvbbs.checkStr(trim(request.form("F_BoardID")))
	F_AnnounceID=dvbbs.checkStr(trim(request.form("F_AnnounceID")))
	F_Readme=dvbbs.checkStr(trim(request.form("F_Readme")))
	saveid=trim(request.form("saveid"))
	Fflag=trim(request.form("Fflag"))

	If not isnumeric(Fflag) or not isnumeric(F_BoardID) then 
		Dvbbs.AddErrCode(32)
		Exit sub
	End if
	if not isnumeric(saveid) or isnull(saveid) then 
		Dvbbs.AddErrCode(34)
		exit sub
	end if

	if  F_Readme="" or isnull(F_Readme) then 
		Dvbbs.AddErrCode(83)
		exit sub
	end if

	If strLength(F_Readme)>250 then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(0)&"&action=OtherErr"
	End if
	fileurl=dvbbs.checkStr(trim(request.form("fileurl")))
	If fileurl<>"" then
		if instr(fileurl,"/")=0 or instr(fileurl,"://")=0  or instr(fileurl,".")=0 then
			Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(13)&"&action=OtherErr"
		end if
		dvbbs.execute("update [DV_Upfile] set F_Filename='"&fileurl&"',F_Readme='"&F_Readme&"' ,F_Flag="&Fflag&" where  F_ID="&saveid)
	Else
		dvbbs.execute("update [DV_Upfile]  set F_BoardID="&F_BoardID&",F_AnnounceID='"&F_AnnounceID&"',F_Readme='"&F_Readme&"' ,F_Flag="&Fflag&" where  F_ID="&saveid)
	End if
	Dvbbs.Dvbbs_suc("<li>"&Template.Strings(14))
End sub

'新增文件
Sub addnew()
	Dim Tempwrite
	Dim filetypelist
	Dim iupload,i
	iupload=Dvbbs.lanstr(5)
	iupload=split(iupload,"||")
	For i=0 to ubound(iupload)
		filetypelist=filetypelist & "<option value="&i&">"&iupload(i)&"</option>"
	Next
	Tempwrite=Template.html(12)
	Tempwrite=Replace(Tempwrite,"{$filetypelist}",filetypelist)
	Tempwrite=Replace(Tempwrite,"{$username}",Dvbbs.Membername)''o 08.01.24 13:57'
	Response.Write Dhlint&Tempwrite
End sub 

'保存新增文件
Sub savenew()
	Dim sucmsg
	If dvbbs.GroupSetting(48)=0 then
		Dvbbs.AddErrCode(28)
		Exit sub
	End if
	Dim F_Readme,fileurl,filetype,fileExt,fileExt_a,filename
	Dim F_Type,F_Flag
	F_Readme=dvbbs.checkStr(trim(request.form("F_Readme")))
	fileurl=dvbbs.checkStr(trim(request.form("fileurl")))
	filetype=trim(request.form("filetype"))
	F_Flag=trim(request.form("Fflag"))

	if  fileurl="" or isnull(fileurl) then 
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(15)&"&action=OtherErr"
	else
		if dvbbs.strLength(fileurl)>250 then
			Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(16)&"&action=OtherErr"
		end if
	end if

	if  F_Readme="" or isnull(F_Readme) then 
		Dvbbs.AddErrCode(83)
		exit sub
	end if
	if dvbbs.strLength(F_Readme)>250 then
		Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(12)&"&action=OtherErr"
		Exit sub
	end if
	if not isnumeric(F_Flag) then
		Dvbbs.AddErrCode(34)
		Exit sub
	end if

		if instr(fileurl,"/")=0 or instr(fileurl,"://")=0  or instr(fileurl,".")=0 then
			Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(17)&"&action=OtherErr"
			exit sub
			else
			filename=split(fileurl,"/")
			fileExt=lcase(filename(ubound(filename)))
		end if

		fileExt_a=split(fileExt,".")
		fileExt=lcase(fileExt_a(ubound(fileExt_a)))
		
		if fileEXT="asp" and fileEXT="asa" and fileEXT="aspx" then
			Response.redirect "showerr.asp?ErrCodes=<li>"&template.Strings(18)&"&action=OtherErr"
			exit sub
		end if
		
		if lcase(fileExt)="gif" or lcase(fileExt)="jpg" or lcase(fileExt)="jpeg" or lcase(fileExt)="bmp" or lcase(fileExt)="png" then
		F_Type=1
		elseif lcase(fileExt)="swf" or lcase(fileExt)="swi" then
		F_Type=2
		elseif lcase(fileExt)="mid" or lcase(fileExt)="wav" or lcase(fileExt)="mp3" or lcase(fileExt)="rmi" or lcase(fileExt)="cda" then
		F_Type=3
		elseif lcase(fileExt)="avi" or lcase(fileExt)="wov" or lcase(fileExt)="asf" or lcase(fileExt)="mpg" or lcase(fileExt)="mpeg" or lcase(fileExt)="ra" or lcase(fileExt)="ram" then
		F_Type=4
		else
		F_Type=0
		end if
		dvbbs.BoardID=0
		dvbbs.execute("insert into dv_upfile (F_BoardID,F_UserID,F_Username,F_Filename,F_FileType,F_Type,F_Readme,F_Flag ) values ("&dvbbs.BoardID&","&Dvbbs.UserID&",'"&dvbbs.membername&"','"&replace(fileurl,"|","")&"','"&replace(fileExt,".","")&"',"&F_Type&",'"&F_Readme&"',"&F_Flag&" )")

	Dvbbs.Dvbbs_suc("<li>"&Template.Strings(19))
end sub

'删除文件
Sub fdel()
	Dim delid,fixid
	If dvbbs.GroupSetting(48)=0 then
		Dvbbs.AddErrCode(28)
		Exit Sub 
	End if
	delid=replace(request("delid"),"'","")
	delid=replace(delid,";","")
	delid=replace(delid,"--","")
	delid=replace(delid,")","")
	fixid=replace(delid," ","")
	fixid=replace(fixid,",","")
	If Not IsNumeric(fixid) Then
		Dvbbs.AddErrCode(35)
		Exit Sub
	End If
	If delid="" or IsNull(delid) Then 
		Dvbbs.AddErrCode(42)
		Exit Sub
	Else
		If Dvbbs.master or Dvbbs.superboardmaster or Dvbbs.boardmaster Then
		dvbbs.execute("delete from DV_Upfile where F_ID in ("&delid&")")
		Else
			dvbbs.execute("delete from DV_Upfile where F_Flag<>2 and F_UserID="&Dvbbs.userID&" and F_ID in ("&delid&")")
		End If
		Dvbbs.Dvbbs_suc("<li>"&Template.Strings(20))
	End if
End sub

'删除所有文件
Sub alldel()
	If dvbbs.GroupSetting(48)=0 then
		Dvbbs.AddErrCode(28)
		Exit  Sub 
	End If 
	Dim delid,fixid
	delid=replace(request("delid"),"'","")
	fixid=replace(delid," ","")
	fixid=replace(fixid,",","")
	If Not IsNumeric(fixid) Then
		Dvbbs.AddErrCode(35)
		Exit Sub
	End If
	If delid="" or isnull(delid) Then
		Dvbbs.AddErrCode(42)
		Exit Sub
	End If
	dvbbs.execute("delete from DV_Upfile where F_Flag=1 and  F_UserID="&Dvbbs.userid)
	Dvbbs.Dvbbs_suc("<li>"&Template.Strings(21))
End Sub 

Function filenum(types)
	Dim stype
	If IsNumeric(types)  Then
		Select case types
			case 0
				stype="F_Type=0 and"
			Case 1
				stype="F_Type=1 and"
			Case 2
				stype="F_Type=2 and"
			Case 3
				stype="F_Type=3 and"
			Case 4
				stype="F_Type=4 and"
			Case Else
				stype=""
		End Select
	Else
		stype=""
	End If
	Rs=Dvbbs.execute("Select Count(F_ID) From DV_Upfile Where  "&stype&" F_UserID="&Dvbbs.userid&"")
	filenum=Rs(0)
	Set rs=nothing
	If IsNull(filenum) Then filenum=0
End Function

%>