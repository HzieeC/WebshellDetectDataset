<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/md5.asp"-->
<!--#include file="inc/code_encrypt.asp"-->
<%
Const QcomicFileExt="bmp|gif|jpeg|png|jpg|swf"
Dim Qcomic_setting,qcomic_code,qcomic_version
qcomic_version = "2.2"

If Dvbbs.qcomic_plus Then
	Qcomic_setting = Split(Dvbbs.qcomic_plus_setting(), "||||")
	qcomic_code=AuthCode(Request("code"), "DECODE", Qcomic_setting(3))
	If instrrev(qcomic_code, "spassword="&Qcomic_setting(2))=0 Then
		Response.Write "wrong password"
		Response.End
	End If
End If
If Request("action")="attach" Then
	qcomic_do_attach(Request("info"))
End If
If Request("action")="network" Then
	qcomic_do_network()
End If
If Request("action")="version" Then
	qcomic_do_version()
End If
If Request("action")="proxy" Then
	qcomic_do_proxy()
End If

Sub qcomic_do_attach(qcomic_info)
	Dim qcomic_xml,objXML,tid,pid,qlength,i
	'qcomic_xml = "<attach><pid>33</pid><tid>30</tid><delimgs><ditem><fname>baidu.gif</fname></ditem></delimgs><newimgs><nitem><fname>baidu1.gif</fname><furl>http://www.baidu.com/img/logo-yy.gif</furl><fsize>1618</fsize></nitem><nitem><fname>baidu2.gif</fname><furl>http://www.baidu.com/img/logo-yy.gif</furl><fsize>1618</fsize></nitem></newimgs></attach>"
	qcomic_xml = qcomic_info
	Set objXML = Server.CreateObject("Microsoft.XMLDOM")
	objXML.validateonparse = true
	objXML.async=false
	objXML.loadXML(qcomic_xml)
	pid = Dvbbs.CheckNumeric(objXML.documentElement.childnodes(0).text)
	tid = Dvbbs.CheckNumeric(objXML.documentElement.childnodes(1).text)
	qlength=objXML.documentElement.getElementsByTagName("ditem").Length

	If instrrev(qcomic_code, "pid="&pid)=0 Then
		Response.Write "wrong password"
		Response.End
	End If

	Redim delimgs(0, qlength-1)
	For i=0 To qlength-1
		delimgs(0,i)=objXML.documentElement.getElementsByTagName("ditem")(i).childnodes(0).text
	Next
	qlength=objXML.documentElement.getElementsByTagName("nitem").Length
	Redim newimgs(2, qlength-1)
	For i=0 To qlength-1
		newimgs(0,i)=objXML.documentElement.getElementsByTagName("nitem")(i).childnodes(0).text
		newimgs(1,i)=objXML.documentElement.getElementsByTagName("nitem")(i).childnodes(1).text
		newimgs(2,i)=objXML.documentElement.getElementsByTagName("nitem")(i).childnodes(2).text
	Next

	Dim TotalUseTable,rs_,uid,uname,fid,topic,body,body_length,body_ubb,isupload
	TotalUseTable = Dvbbs.NowUseBBS
	Set rs_=Dvbbs.Execute("select BoardID,UserName,postuserid,Topic,Body,length,UbbList,isupload from "&TotalUseTable&" where AnnounceID="&pid)
	If Not (rs_.Eof And rs_.Bof) Then
		fid=rs_(0)
		uname=rs_(1)
		uid=rs_(2)
		topic=rs_(3)
		body=rs_(4)
		body_length=rs_(5)
		body_ubb=rs_(6)
		isupload=rs_(7)
		If instrrev(body_ubb, ",2,")=0 Then body_ubb = ",2"&body_ubb
	End If
	Set rs_=Nothing

	Dim info_attaches,total,info_total,idx,fext,newid,FilePath,ChildFilePath
	Set rs_=Dvbbs.Execute("SELECT F_OldName,F_ID,F_Filename FROM Dv_Upfile WHERE F_AnnounceID='"&tid&"|"&pid&"'")
	If Not (rs_.Eof And rs_.Bof) Then
		info_attaches = rs_.GetRows
		info_total = Ubound(info_attaches, 2)
	Else
		info_total = -1
	End If
	Set rs_=Nothing

	If info_total<>-1 Then
		total=Ubound(delimgs, 2)
		For i=0 To total
			idx = qcomic_find_fname(info_attaches, delimgs(0,i), info_total)
			Do While idx <> -1
				call qcomic_delete_fname(info_attaches(1,idx), info_attaches(2,idx))
				fext = qcomic_get_fext(delimgs(0,i))
				body = Replace(body, "[upload="&fext&","&delimgs(0,i)&"]viewFile.asp?ID="&info_attaches(1,idx)&"[/upload]<br/>", "")
				idx = qcomic_find_fname(info_attaches, delimgs(0,i), idx-1)
			Loop
		Next
	End If
	
	'上传目录
	FilePath = CreatePath(CheckFolder)
	'不带系统上传目录的下级目录路径
	ChildFilePath = Replace(FilePath,CheckFolder,"")

	total=Ubound(newimgs, 2)
	For i=0 To total
		idx = qcomic_find_fname(info_attaches, newimgs(0,i), info_total)
		If idx=-1 Then
			newid = qcomic_insert_fname(newimgs(1,i),newimgs(0,i),newimgs(2,i),ChildFilePath,tid,pid,fid,uid,uname,topic)
			If (newid<>-1) Then
				fext = qcomic_get_fext(newimgs(0,i))
				body = body & "[upload="&fext&","&newimgs(0,i)&"]viewFile.asp?ID="&newid&"[/upload]<br/>"
			End If
		End If
	Next

	Set rs_=Dvbbs.Execute("SELECT F_OldName,F_ID,F_Filename FROM Dv_Upfile WHERE F_AnnounceID='"&tid&"|"&pid&"'")
	If Not (rs_.Eof And rs_.Bof) Then
		info_total = 0
	Else
		info_total = -1
	End If
	Set rs_=Nothing
	body = Dvbbs.CheckStr(body)
	body_ubb = Dvbbs.CheckStr(body_ubb)
	If info_total<>-1 Then
		Dvbbs.Execute("update "&TotalUseTable&" set isupload=1,Body='"&body&"',Length="&Len(body)&",Ubblist='"&body_ubb&"' where AnnounceID="&pid)
	Else
		Dvbbs.Execute("update "&TotalUseTable&" set isupload=0,Body='"&body&"',Length="&Len(body)&",Ubblist='"&body_ubb&"' where AnnounceID="&pid)
	End If

	Response.Write "ok"
	Response.End
End Sub

Function qcomic_find_fname(arr, fname, idx)
	Dim i
	fname = Left(Dvbbs.Checkstr(fname),50)
	For i=idx To 0 Step -1
		If arr(0,i)=fname Then
			qcomic_find_fname = i
			Exit Function
		End If
	Next
	qcomic_find_fname = -1
End Function

Function qcomic_get_fext(fname)
	Dim pos
	pos = instrrev(fname, ".")
	qcomic_get_fext = Right(fname, Len(fname)-pos)

End Function

Sub qcomic_delete_fname(fid, fpath)
	fid = Dvbbs.CheckNumeric(fid)
	Dim objFSO,Filepath
	Filepath = Dvbbs.Forum_Setting(76)&fpath
	Set objFSO = Dvbbs.iCreateObject("Scripting.FileSystemObject")
	If objFSO.FileExists(Server.MapPath(Filepath)) Then
		objFSO.DeleteFile(Server.MapPath(Filepath))
	End If
	Dvbbs.Execute("Delete from Dv_Upfile Where F_ID="&fid)
End Sub

Function qcomic_insert_fname(furl,fname,fsize,ChildFilePath,tid,pid,fid,uid,uname,topic)
	Dim http,imgcont,objStream,Filepath,fpath,fext,rs_,newid
	fext=qcomic_get_fext(fname)
	If InStr("|"&QcomicFileExt&"|","|"&fext&"|")=0 Then	'非法文件后缀，程序中止。
		Response.Write "wrong FileExt!"
		Response.End
	End If
	fpath=ChildFilePath&FormatName(fext, fname)
	Filepath = Dvbbs.Forum_Setting(76)&fpath

	furl = Dvbbs.Checkstr(furl)
	fname = Dvbbs.Checkstr(fname)
	fsize = Dvbbs.CheckNumeric(fsize)
	ChildFilePath = Dvbbs.Checkstr(ChildFilePath)
	tid = Dvbbs.CheckNumeric(tid)
	pid = Dvbbs.CheckNumeric(pid)
	fid = Dvbbs.CheckNumeric(fid)
	uid = Dvbbs.CheckNumeric(uid)
	uname = Dvbbs.Checkstr(uname)
	topic = Dvbbs.Checkstr(topic)
	
	set http=server.createobject("MSXML2.XMLHTTP")
	Http.open "GET",furl,false
	Http.send()
	imgcont=Http.responseBody
	set http=nothing

	Set objStream = Server.CreateObject("ado"&"db"&"."&"str"&"eam")
	objStream.Type =1
	objStream.Open
	objstream.write imgcont
	If objstream.Size=clng(fsize) Then
		objstream.SaveToFile server.mappath(Filepath),2
		
		Dvbbs.Execute("Insert into Dv_upFile (F_AnnounceID,F_BoardID,F_UserID,F_Username,F_Filename,F_Viewname,F_FileType,F_Type,F_FileSize,F_Flag,F_Readme,F_OldName) values ('"&tid&"|"&pid&"',"&fid&","&uid&",'"&uname&"','"&fpath&"','','"&fext&"',0,"&fsize&",4,'"&Left(topic,250)&"','"&Left(fname,50)&"')")
		Set rs_=Dvbbs.Execute("Select top 1 F_ID From Dv_upFile order by F_ID desc")
		newid=rs_(0)
		rs_.Close
		Set rs_=nothing

		qcomic_insert_fname = newid
	Else
		qcomic_insert_fname = -1
	End If
	objstream.Close()
	set objstream=nothing
End Function

Sub qcomic_do_network()
	Dim XmlHttp,qcomic_stime,qcomic_etime
	qcomic_stime = Now()
	Set XmlHttp = CreateObject("Micro"&"soft"&"."&"XML"&"HTTP")
	XmlHttp.Open "GET","http://comic.qihoo.com/dvbbs/ping.php",false
	XmlHttp.setRequestHeader "Content-Type", "application/x-www-form-urlencoded"
	XmlHttp.send()
	Response.Write xmlhttp.responseText
	qcomic_etime = Now()
	Response.Write qcomic_stime & " ==== " & qcomic_etime
	Response.End
End Sub

Sub qcomic_do_version()
	Response.Write qcomic_version
	Response.End
End Sub

Sub qcomic_do_proxy()
	Dim pos,rurl,rhost
	rurl = Request("url")
	pos = instr(8, rurl, "/")
	rhost = Left(rurl, pos)
	If (instr(rhost, "qihoo.com")) Then
		Response.Redirect Request("url")
	Else
		Response.Write "Wrong param"
	End If
	Response.End
End Sub

'读取上传目录
Function CheckFolder()
	If Dvbbs.Forum_Setting(76)="" Or Dvbbs.Forum_Setting(76)="0" Then Dvbbs.Forum_Setting(76)="UploadFile/"
	CheckFolder = Replace(Replace(Dvbbs.Forum_Setting(76),Chr(0),""),".","")
	'在目录后加(/)
	If Right(CheckFolder,1)<>"/" Then CheckFolder=CheckFolder&"/"
End Function

'按月份自动明名上传文件夹,需要ＦＳＯ组件支持。
Function CreatePath(PathValue)
	Dim objFSO,Fsofolder,uploadpath
	'以年月创建上传文件夹，格式：2003－8
	uploadpath = year(now) & "-" & month(now)
	If Right(PathValue,1)<>"/" Then PathValue = PathValue&"/"
	On Error Resume Next
	Set objFSO = Dvbbs.iCreateObject("Scripting.FileSystemObject")
		If objFSO.FolderExists(Server.MapPath(PathValue & uploadpath))=False Then
			objFSO.CreateFolder Server.MapPath(PathValue & uploadpath)
		End If
		If Err.Number = 0 Then
			CreatePath = PathValue & uploadpath & "/"
		Else
			CreatePath = PathValue
		End If
	Set objFSO = Nothing
End Function

'日期时间定义文件名
Function FormatName(Byval FileExt,Byval FileName)
	Dim RanNum,TempStr
	Randomize
	RanNum = Int(90000*rnd)+10000
	TempStr = Year(now) & Month(now) & Day(now) & Hour(now) & Minute(now) & Second(now) & RanNum & "." & FileExt
	FormatName = TempStr
End Function

'Response.Write "zzz"
'http://localhost/dvbbs2/qcomic.asp?action=attach&code=DJNYGBHgCzoZb1J1rvKg4Oj6sh0J32Ct
'http://www.hbjjrb.com/Jishu/ASP/200704/17150.html
%>