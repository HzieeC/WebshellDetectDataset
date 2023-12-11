<!--#include file=conn.asp-->
<!--#Include File="inc/const.asp"-->
<!--#Include File="inc/dv_clsother.asp"-->
<%
Response.Expires=0
If Dvbbs.BoardID = 0 Then
	Response.Write "参数错误"
	Dvbbs.PageEnd()
	Response.End
End If
If Dvbbs.GroupSetting(2)="0"  Then
	Response.Write "<script language=""javascript"">alert('您没有权限查看贴子!')</script>"
	Dvbbs.PageEnd()
	Response.End
End If
Dvbbs.LoadTemplates("index")
Dim outtext,iouttext
Dim num
Dim allnum
Dim RootID
Dim rs,SQL,i
RootID=request("rootID")
If RootID="" Or Not IsNumeric(RootID) Or (Request("action") <> "1" And Request("action")<>"0") Then Response.End
Dim TempStr
TempStr=Split(template.html(2),"||")
Response.Write "<html><head><meta http-equiv=""Content-Type"" content=""text/html; charset=gb2312""></head><body>"
If Request("action")="1" Then showtree()
If Request("action")="0" Then closetree()
Response.Write "</body></html>"
Dvbbs.PageEnd()
Sub closeTree()
	TempStr(4)=Replace(TempStr(4),"{$rootid}",rootid)
	TempStr(4)=Replace(TempStr(4),"{$boardid}",Dvbbs.BoardID)
	Response.Write TempStr(4)
End Sub 
Sub showtree()
	Dim Star,page
	Star=Request("Star")
	If Star="" Or Not IsNumeric(Star) Then Star=1
	Star=Clng(Star)
	page=star
	Dim MyTempStr,ii
	num=0
	outtext="　　　&nbsp;&nbsp;"
	Dim totalusetable
	Set Rs=Dvbbs.Execute("Select child,PostTable from dv_topic where topicid="&rootid)
	allnum=rs(0)
	totalusetable=rs(1)
	Dim Board_Setting27
	TempStr(3)=Replace(TempStr(3),"{$rootid}",rootid)
	TempStr(3)=Replace(TempStr(3),"{$boardid}",Dvbbs.BoardID)
	TempStr(3)=Replace(TempStr(3),"{$alertcolor}",Dvbbs.mainsetting(1))
	Response.Write TempStr(3)
	Response.flush
	Board_Setting27=Dvbbs.Board_Setting(27)
	SQL="select T.layer,t.rootid,t.announceid,t.body,t.username,t.postuserid,t.topic,t.locktopic,u.LockUser,t.signflag from "&totalusetable&" t left outer Join [dv_user] U On T.postuserid=u.userid where t.boardid="& Dvbbs.boardid &" and t.rootid="& rootid &" and t.parentid>0 order by t.rootid desc,t.orders"
	If Not IsObject(Conn) Then ConnectionDatabase
	Set Rs=Dvbbs.iCreateObject("adodb.recordset")
	rs.open sql,conn,1,1
	If Not (Rs.Eof And Rs.Bof) Then
		If allnum <> Rs.RecordCount Then
			allnum=Rs.RecordCount
			Dvbbs.Execute("Update dv_topic Set child="&allnum&" Where topicid="&rootid)
		End If
		Rs.PageSize=Cint(Dvbbs.Board_Setting(27))
		Rs.AbsolutePage=Star
		SQL=Rs.GetRows(Rs.PageSize)
		Response.Write "<Script Language=JavaScript>"
		Response.Write "var tmpstr='';"	
		For i=0 To Ubound(SQL,2)
			MyTempStr=MyTempStr & TempStr(1)
			For ii=0 to SQL(0,i)
				If ii>0 Then
					iouttext = iouttext & outtext
				Else
					iouttext = outtext
				End If
			Next
			MyTempStr=Replace(MyTempStr,"{$nbsplength}",outtext)
			MyTempStr=Replace(MyTempStr,"{$pic_nofollow}",Dvbbs.mainpic(10))
			MyTempStr=Replace(MyTempStr,"{$looprootid}",SQL(1,i))
			MyTempStr=Replace(MyTempStr,"{$announceid}",SQL(2,i))
			star=int((allnum-num)/Board_Setting27)+1
			If star>1 Then
				MyTempStr=Replace(MyTempStr,"{$star}",star)
			Else
				MyTempStr=Replace(MyTempStr,"{$star}",1)
			End if
			MyTempStr=Replace(MyTempStr,"{$atitle}",Dvbbs.HtmlEncode(SQL(6,i)))
			If SQL(7,i)=2 Then
				MyTempStr=Replace(MyTempStr,"{$title}","==此发言已被管理员屏蔽==")
			ElseIf SQL(7,i)=3 Then
				MyTempStr=Replace(MyTempStr,"{$title}","==此发言被固封等待解除==")
			ElseIf SQL(8,i)=1 Then
				MyTempStr=Replace(MyTempStr,"{$title}","==此人已被管理员锁定==")
			ElseIf SQL(8,i)=2 Then
				MyTempStr=Replace(MyTempStr,"{$title}","==此人所有发言已被管理员屏蔽==")
			Else	
				If SQL(6,i)="" Then
					MyTempStr=Replace(MyTempStr,"{$title}"," "&cutStr(reUBBCode(SQL(3,i)),35) )
				Else
					MyTempStr=Replace(MyTempStr,"{$title}"," "& cutStr(SQL(6,i),35))
				End If
			End If
			If Dvbbs.Board_Setting(68)="1" And SQL(9,i)=2 Then
				If Dvbbs.BoardMaster Then
					MyTempStr=Replace(MyTempStr,"{$userid}",SQL(5,i))
					MyTempStr=Replace(MyTempStr,"{$username}",Dvbbs.iHtmlEncode(SQL(4,i))&"  <font color='gray'>(匿名)</font>")
				Else
					MyTempStr=Replace(MyTempStr,"{$userid}","")
					MyTempStr=Replace(MyTempStr,"{$username}","匿名用户")
				End If
			Else
				MyTempStr=Replace(MyTempStr,"{$userid}",SQL(5,i))
				MyTempStr=Replace(MyTempStr,"{$username}",Dvbbs.iHtmlEncode(SQL(4,i)))
			End If
			num=num+1
			iouttext=""
		Next
		MyTempStr=Replace(MyTempStr,"{$boardid}",Dvbbs.BoardID)
		MyTempStr=Replace(TempStr(0),"{$loadtopicreplyloop}",MyTempStr)
		MyTempStr=Replace(Replace(Replace(Replace(Replace(Replace(MyTempStr,"\","\\"),"'","\'"),VbCrLf,""),chr(13),""),"<br />",""),"</P><P>","")
		MyTempStr=Replace(MyTempStr,Chr(10),"")
		Response.Write "tmpstr='"&MyTempStr&"';"	
		MyTempStr=""
		SQL=Null
		TempStr(2)=Replace(TempStr(2),"{$rootid}",rootid)
		TempStr(2)=Replace(TempStr(2),Chr(10),"")
		Response.Write "tmpstr+=showpage("&page&","&Rs.RecordCount&","&Rs.PageSize&","&Rs.PageCount&");"		
		Response.Write TempStr(2)
		Response.Write "</Script>"
		TempStr=Null
	End If
	Set Rs=Nothing
End Sub 
Function dvHTMLEncode(fString)
	If Not IsNull(fString) Then
		fString = replace(fString, ">", "&gt;")
		fString = replace(fString, "<", "&lt;")
		fString = Replace(fString, CHR(32), "<i></i>&nbsp;")
		fString = Replace(fString, CHR(9), "&nbsp;")
		fString = Replace(fString, CHR(34), "&quot;")
		fString = Replace(fString, CHR(39), "&#39;")
		fString = Replace(fString, CHR(13), "")
		fString = Replace(fString, CHR(10) & CHR(10), "</P><P> ")
		fString = Replace(fString, CHR(10), "<br> ")
		fString=Dvbbs.ChkBadWords(fString)
		dvHTMLEncode = fString
	End If
End Function
Function reUBBCode(strContent)
	Dim re
	Set re=new RegExp
	re.IgnoreCase =True
	re.Global=True
	strContent=replace(strContent,"&nbsp;"," ")
	re.Pattern="(\[QUOTE\])(.[^\[]*)(\[\/QUOTE\])"
	strContent=re.Replace(strContent,"$2")
	re.Pattern="(\[point=*([0-9]*)\])(.[^\[]*)(\[\/point\])"
	strContent=re.Replace(strContent,"&nbsp;")
	re.Pattern="(\[post=*([0-9]*)\])(.[^\[]*)(\[\/post\])"
	strContent=re.Replace(strContent,"&nbsp;")
	re.Pattern="(\[power=*([0-9]*)\])(.[^\[]*)(\[\/power\])"
	strContent=re.Replace(strContent,"&nbsp;")
	re.Pattern="(\[usercp=*([0-9]*)\])(.[^\[]*)(\[\/usercp\])"
	strContent=re.Replace(strContent,"&nbsp;")
	re.Pattern="(\[money=*([0-9]*)\])(.[^\[]*)(\[\/money\])"
	strContent=re.Replace(strContent,"&nbsp;")
	re.Pattern="(\[replyview\])(.[^\[]*)(\[\/replyview\])"
	strContent=re.Replace(strContent,"&nbsp;")
	re.Pattern="(\[usemoney=*([0-9]*)\])(.[^\[]*)(\[\/usemoney\])"
	strContent=re.Replace(strContent,"&nbsp;")
	re.Pattern="\[username=(.[^\[]*)\](.[^\[]*)\[\/username\]"
	strContent=re.Replace(strContent,"&nbsp;")
	strContent=replace(strContent,"<I></I>","")
	set re=Nothing
	reUBBCode=strContent
End Function
'截取指定字符
Function cutStr(str,strlen)
	'去掉所有HTML标记
	Dim re
	Set re=new RegExp
	re.IgnoreCase =True
	re.Global=True
	re.Pattern="<(.[^>]*)>"
	str=re.Replace(str,"")
	set re=Nothing
	str=Replace(str,chr(10),"")
	str = Dvbbs.HTMLEncode(str)
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
%>