<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<%
Dvbbs.LoadTemplates("usermanager")
Dvbbs.Stats=Dvbbs.MemberName&template.Strings(4)
Dvbbs.Nav()
Dvbbs.Head_var 0,0,template.Strings(0),"usermanager.asp"
Dim Sql,Rs,TempStr,ErrCodes
Dim Sms_max
Sms_max=Cint(Dvbbs.GroupSetting(35))				'用户组短信限制，条数
If Sms_max = 0 Then Sms_Max = 9999
If Cint(Dvbbs.GroupSetting(16)) = 0 And Cint(Dvbbs.GroupSetting(32)) = 0 Then	'不能修改个人资料且不能发短信时
	Dvbbs.AddErrCode(28)
End If
If Dvbbs.userid=0 Then
	Dvbbs.AddErrCode(6)
End if
Dvbbs.Showerr()

Dim boxName,smstype,readaction,turl,TempLateStr
main()
If ErrCodes<>"" Then Response.redirect "showerr.asp?ErrCodes="&ErrCodes&"&action=OtherErr"
Dvbbs.Showerr()
Dvbbs.ActiveOnline()
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub main()
Dim i,SmsCount,DelCount,boxinfo
SmsCount=0
boxinfo=split(template.Strings(63),",")
Set Rs=Dvbbs.Execute("Select Count(id) From DV_Message Where incept='"&Dvbbs.MemberName&"'")
SmsCount=Rs(0)
'以下判断为自动删除多出来的短消息(2003-12-26 FIX BY YZ)
If SmsCount>Sms_max Then
	i=SmsCount-Sms_max
	Set Rs=Dvbbs.Execute("Select top "&i&" id From DV_Message Where incept='"&Dvbbs.MemberName&"' Order by id,delR Desc")
	While not rs.eof
		Dvbbs.Execute("Delete From DV_Message Where id="&rs(0))
		rs.movenext
	Wend
	SmsCount=Sms_max
End if
Rs.close:Set Rs=Nothing
TempLateStr=template.html(12)

Sql="Select * From Dv_Message "
Select Case request("action")
	Case "inbox"
		TempLateStr=Replace(TempLateStr,"{$Sms_inbox}", template.pic(19))
		TempLateStr=Replace(TempLateStr,"{$Sms_outbox}", template.pic(1))
		TempLateStr=Replace(TempLateStr,"{$Sms_issend}", template.pic(2))
		TempLateStr=Replace(TempLateStr,"{$Sms_recycle}", template.pic(3))
		boxName = boxinfo(0)'"收件箱"
		smstype = "inbox"
		readaction = "read"
		turl = "readsms"
		Sql = Sql + " where issend = 1 and delR = 0 and incept = '"&Dvbbs.MemberName&"' order by flag,id desc"
	Case "outbox"
		TempLateStr=Replace(TempLateStr,"{$Sms_inbox}", template.pic(0))
		TempLateStr=Replace(TempLateStr,"{$Sms_outbox}", template.pic(20))
		TempLateStr=Replace(TempLateStr,"{$Sms_issend}", template.pic(2))
		TempLateStr=Replace(TempLateStr,"{$Sms_recycle}", template.pic(3))
		boxName = boxinfo(1)'"草稿箱"
		smstype = "outbox"
		readaction = "edit"
		turl = "sms"
		Sql = Sql + " where sender = '"&Dvbbs.MemberName&"' and issend = 0 and delS = 0 order by id desc"
	Case "issend"
		TempLateStr=Replace(TempLateStr,"{$Sms_inbox}", template.pic(0))
		TempLateStr=Replace(TempLateStr,"{$Sms_outbox}", template.pic(1))
		TempLateStr=Replace(TempLateStr,"{$Sms_issend}", template.pic(21))
		TempLateStr=Replace(TempLateStr,"{$Sms_recycle}", template.pic(3))
		boxName = boxinfo(2)'"已发送的消息"
		smstype = "issend"
		readaction = "outread"
		turl = "readsms"
		Sql = Sql + " where sender = '"&Dvbbs.MemberName&"' and issend = 1 and delS = 0 order by id desc"
	Case "recycle"
		TempLateStr=Replace(TempLateStr,"{$Sms_inbox}", template.pic(0))
		TempLateStr=Replace(TempLateStr,"{$Sms_outbox}", template.pic(1))
		TempLateStr=Replace(TempLateStr,"{$Sms_issend}", template.pic(2))
		TempLateStr=Replace(TempLateStr,"{$Sms_recycle}", template.pic(22))
		boxName = boxinfo(3)'"垃圾箱"
		smstype = "recycle"
		readaction = "read"
		turl = "readsms"
		Sql = Sql + " where ((sender = '"&trim(Dvbbs.MemberName)&"' and delS = 1) or (incept = '"&trim(Dvbbs.MemberName)&"' and delR = 1)) and not delS = 2 order by id desc"
	Case Else
		TempLateStr=Replace(TempLateStr,"{$Sms_inbox}", template.pic(19))
		TempLateStr=Replace(TempLateStr,"{$Sms_outbox}", template.pic(1))
		TempLateStr=Replace(TempLateStr,"{$Sms_issend}", template.pic(2))
		TempLateStr=Replace(TempLateStr,"{$Sms_recycle}", template.pic(3))
		boxName = boxinfo(0)'"收件箱"
		smstype = "inbox"
		readaction = "read"
		turl = "readsms"
		Sql = Sql + " where issend = 1 and delR = 0 and incept = '"&Dvbbs.MemberName&"' order by flag,id desc"
End Select
TempLateStr=Replace(TempLateStr,"{$Sms_address}", template.pic(4))
TempLateStr=Replace(TempLateStr,"{$Sms_write}", template.pic(5))
TempLateStr=Replace(TempLateStr,"{$Sms_issend_1}", template.pic(9))
TempLateStr=Replace(TempLateStr,"{$Sms_issend_2}", template.pic(10))
TempLateStr=Replace(TempLateStr,"{$Sms_news}", template.pic(11))
TempLateStr=Replace(TempLateStr,"{$Sms_olds}", template.pic(12))
Response.Write Template.Html(0)
Response.Write TempLateStr
Call smsbox()
'For i = 0 to SmsCount
	Response.Write ShowTable("Sms_bar","Sms_txt",SmsCount,Sms_max)
	'Response.Flush
'Next
End Sub

Sub smsbox()
	Dim newstyle
	Dim currentPage,page_count,totalrec,Pcount,PageListNum
	PageListNum = Cint(Dvbbs.Forum_Setting(11))
	currentPage = request("page")
	If currentpage = "" or not IsNumeric(currentpage) Then
		currentpage = 1
	Else
		currentpage = clng(currentpage)
	End If
	Response.Write "<script>dvbbs_usersms_smsbox_top('"&smstype&"')</script>"
	Set Rs = Dvbbs.iCreateObject("adodb.recordset")
	If Not IsObject(Conn) Then ConnectionDatabase
	Rs.open sql,conn,1,1
		Dvbbs.SqlQueryNum = Dvbbs.SqlQueryNum+1
		If Rs.eof and Rs.bof Then
		Response.Write "<script>dvbbs_usersms_smsbox_emp('"&boxname&"')</script>"
		Else
			Rs.PageSize = PageListNum
			Rs.AbsolutePage = currentpage
			page_count = 0
			totalrec = Rs.recordcount
		Do While Not Rs.EOF and (not page_count = Rs.PageSize)
			Response.Write VbCrLf
			Response.Write "<script>dvbbs_usersms_smsbox_loop("
			Response.Write Rs("flag")
			Response.Write ",'"
			Response.Write smstype
			Response.Write "','"
			Response.Write EncodeJS(Rs("sender"))
			Response.Write "','"
			Response.Write EncodeJS(Rs("incept"))
			Response.Write "','"
			Response.Write EncodeJS(Rs("title"))
			Response.Write "','"
			Response.Write Rs("sendtime")
			Response.Write "',"
			Response.Write len(Rs("content"))
			Response.Write ","
			Response.Write Rs("id")
			Response.Write ",'"
			Response.Write readaction
			Response.Write "')</script>"
			Response.Write VbCrLf
			page_count = page_count + 1
		Rs.movenext
		Loop
		End If
		Rs.close:Set Rs = nothing
		If totalrec Mod PageListNum = 0 Then
     		Pcount =  totalrec \ PageListNum
  		Else
     		Pcount =  totalrec \ PageListNum+1
  		End If
		If page_count = 0 Then CurrentPage = 0
	Response.Write ShowPage(CurrentPage,Pcount,totalrec,PageListNum)
	Response.Write VbCrLf
	Response.Write "<script>dvbbs_usersms_smsbox_footer('"&boxname&"')</script>"
End Sub

'分页输出
Function ShowPage(CurrentPage,Pcount,totalrec,PageNum)
	Dim SearchStr
	SearchStr = "action="&Request("action")
	ShowPage = template.html(16)
	ShowPage = Replace(ShowPage,"{$colSpan}",6)
	ShowPage = Replace(ShowPage,"{$CurrentPage}",CurrentPage)
	ShowPage = Replace(ShowPage,"{$Pcount}",Pcount)
	ShowPage = Replace(ShowPage,"{$PageNum}",PageNum)
	ShowPage = Replace(ShowPage,"{$totalrec}",totalrec)
	ShowPage = Replace(ShowPage,"{$SearchStr}",SearchStr)
	ShowPage = Replace(ShowPage,"{$redcolor}",Dvbbs.mainsetting(1))
End Function

'（图片对象名称，标题对象名称，更新数，总数）
Function ShowTable(SrcName,TxtName,str,c)
Dim Tempstr,Src_js,Txt_js,TempPercent
If C = 0 Then C = 99999999
Tempstr = str/C
TempPercent = FormatPercent(tempstr,0,-1)
Src_js = "document.getElementById(""" + SrcName + """)"
Txt_js = "document.getElementById(""" + TxtName + """)"
	ShowTable = VbCrLf + "<script>"
	ShowTable = ShowTable + Src_js + ".width=""" & FormatNumber(tempstr*600,0,-1) & """;"
	ShowTable = ShowTable + Src_js + ".title=""容量上限为："&c&"条，总共已储存（"&str&"）条论坛短信！"";"
	ShowTable = ShowTable + Txt_js + ".innerHTML="""
	If FormatNumber(tempstr*100,0,-1) < 80 Then
		ShowTable = ShowTable + "已使用:" & TempPercent & """;"
	Else
		ShowTable = ShowTable + "<font color=\"""&Dvbbs.mainsetting(1)&"\"">已使用:" & TempPercent & ",请赶快清理！</font>"";"
	End If
	ShowTable = ShowTable + "</script>"
End Function

Function EncodeJS(str)
str = Dvbbs.HtmlEncode(str)
str = Replace(Replace(Replace(Replace(str,"\","\\"),"'","\'"),VbCrLf,"\n"),chr(13),"")
EnCodeJs = str
End Function
%>