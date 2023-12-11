<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/dv_clsother.asp"-->
<!--#include file="inc/dv_ubbcode.asp"-->
<%
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
Dvbbs.LoadTemplates("dispbbs")
Dim Star
Star=Request("Star")
If Star="" Or Not IsNumeric(Star) Then Star=1
Star=Clng(Star)

Response.Write "<html><head><meta http-equiv=""Content-Type"" content=""text/html; charset=gb2312""></head><body>"
Showtitle()
Showtree()
Response.Write "</body></html>"
Dvbbs.PageEnd()
Sub Showtitle()
	Dim treeData
	treeData=template.html(2)
	treeData=Replace(treeData,"{$treeloop}",template.html(4))
	treeData=Replace(treeData,chr(10),"")
	treeData=Replace(treeData,chr(13),"")
	Response.Write "<script language=""javascript"">"
	Response.Write Chr(10)
	Response.Write "var ISAPI_ReWrite = "&isUrlreWrite&";"
	Response.Write Chr(10)
	Response.Write "var treedata='"
	Response.Write Replace(Replace(Replace(Replace(treeData,"\","\\"),"'","\'"),VbCrLf,"\n"),chr(10),"")
	Response.Write "';"
	Response.Write vbNewLine
	Response.Write Chr(10)
	Response.Write "</script>"
	Response.flush
End Sub 
Sub Showtree()
	'接收参数
	Dim AnnounceID,ReplyID,TotalUseTable,openid
	Dim template_html3
	template_html3=template.html(3)
	openid=Request("openid")
	If openid="" Or Not IsNumeric(openid) Then openid=0
	openid=CLng(openid)
	AnnounceID=Request("ID")
	If AnnounceID="" Or Not IsNumeric(AnnounceID) Then Exit Sub 
	AnnounceID=CLng(AnnounceID)
	ReplyID=Request("ReplyID")
	If ReplyID="" Or Not IsNumeric(ReplyID) Then ReplyID=AnnounceID
	ReplyID=CLng(ReplyID)
	Dim SQl,Rs
	sql="Select PostTable from Dv_topic where topicID="&Announceid
	Set rs=Dvbbs.Execute(sql)
	If Not Rs.eof Then
		 TotalUseTable=Rs(0)
	End If
	Dim tmpstr,treedata,blank,i,j,RecordCount,PageCount
	PageCount=0
	
	If TotalUseTable <>"" Then 
		Set Rs=Dvbbs.iCreateObject("adodb.recordset")
		sql="select t.AnnounceID,t.parentID,t.BoardID,t.UserName,t.PostUserid,t.Topic,t.DateAndTime,t.length,t.RootID,t.layer,t.orders,t.Expression,t.body,t.locktopic,u.LockUser,t.signflag from "&TotalUseTable&" t left outer Join [dv_user] U On T.postuserid=u.userid where BoardID="&Dvbbs.BoardID&" and t.RootID="&Announceid&" and t.BoardID<>777 and t.BoardID<>444 order by t.RootID desc,t.orders"
		rs.open sql,conn,1,1
		j=0
		RecordCount=Rs.RecordCount
		If Not(Rs.EOF And Rs.BOF) Then
			Rs.PageSize=Cint(Dvbbs.Board_Setting(27))
			Rs.AbsolutePage=Star
			PageCount=Rs.PageCount
			Do while Not Rs.EOF
				treedata=template_html3
				For i=1 to Rs(9)
					blank=blank&"&nbsp;"
				Next
				If Rs("locktopic")=2 Then 
					treedata=Replace(treedata,"{$topic}","==此发言已被管理员屏蔽==")
				ElseIf Rs("locktopic")=2 Then 
					treedata=Replace(treedata,"{$topic}","==此发言被固封等待解除中==")
				ElseIf Rs("LockUser")=1 Then
					treedata=Replace(treedata,"{$topic}","==此人已被管理员锁定==")
				ElseIf Rs("LockUser")=2 Then
					treedata=Replace(treedata,"{$topic}","==此人所有发言已被管理员屏蔽==")
				Else
					If Rs("topic")="" or isnull(rs("topic")) Then 
						treedata=Replace(treedata,"{$topic}",cutStr(Reubbcode(Rs("body")),35))
					Else
						treedata=Replace(treedata,"{$topic}",cutStr(Rs("Topic"),35))
					End If
				End If			
				If Rs(0)=openid Then
					treedata=Replace(treedata,"{$alertcolor}",Dvbbs.mainsetting(1))
				Else
					treedata=Replace(treedata,"{$alertcolor}","")
				End If 
				treedata=Replace(treedata,"{$announceid}",Rs(0))
				treedata=Replace(treedata,"{$boardid}",Rs(2))
				If Dvbbs.Board_Setting(68)="1" And Rs(15)=2 And Not Dvbbs.BoardMaster Then
					treedata=Replace(treedata,"{$username}","匿名")
				Else
					treedata=Replace(treedata,"{$username}",Rs(3))
				End If
				treedata=Replace(treedata,"{$DateAndTime}",Rs(6))
				If Rs(7)=0 Then 
					treedata=Replace(treedata,"{$length}",template.Strings(14))
				Else
					treedata=Replace(treedata,"{$length}",Rs(7)&template.Strings(13))
				End If
				treedata=Replace(treedata,"{$rootid}",Rs(8))
				treedata=Replace(treedata,"{$Expression}",Rs(11))
				treedata=Replace(treedata,"{$blank}",blank)
				blank=""
				tmpstr=tmpstr&treedata
				Rs.MoveNext
				j=j+1
				If j=Cint(Dvbbs.Board_Setting(27)) Then Exit Do
			Loop
		End If
		Rs.close
		Set rs=Nothing 
	End If
	
	tmpstr = Dvbbs.ArchiveHtml(tmpstr)
	Dim template_html2
	template_html2=Replace(template.html(2),"{$treeloop}",tmpstr)
	template_html2=Replace(template_html2,chr(10),"")
	template_html2=Replace(template_html2,chr(13),"")
	Response.Write "<script language=""javascript"">"
	Response.Write Chr(10)
	Response.Write "var treedata='"
	Response.Write Replace(Replace(Replace(Replace(template_html2,"\","\\"),"'","\'"),VbCrLf,"\n"),chr(10),"")
	Response.Write "';"
	Response.Write vbNewLine
	Response.Write "parent.document.getElementById(""postlist"").innerHTML=treedata;"
	Dim PageSearch
	PageSearch = "loadtree.asp?boardid="&Dvbbs.BoardID&"&replyid="&ReplyID&"&id="&AnnounceID&"&page="&Star&"&skin=1&openid="&openid&""
	Response.Write Chr(10)
	Response.Write "</script>"
	call ShowPageList(Star,RecordCount,Cint(Dvbbs.Board_Setting(27)),PageCount,PageSearch)
End Sub

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

'Star,RecordCount,Cint(Dvbbs.Board_Setting(27)),PageCount
Sub ShowPageList(Star,Record_Count,Pag_size,Page_Count,Search)
%>
	<script language="JavaScript" src="inc/Pagination.js"></script>
	<span id="LoadPageList">
	<SCRIPT LANGUAGE="JavaScript">
	<!--
		PageList(<%=Star%>,10,<%=Cint(Dvbbs.Board_Setting(27))%>,<%=Record_Count%>,'<%=Search%>',6)
	//-->
	</SCRIPT>
	</span>
	<SCRIPT LANGUAGE="JavaScript">
	<!--
		parent.document.getElementById('showpagelist').innerHTML = document.getElementById('LoadPageList').innerHTML;
	//-->
	</SCRIPT>
<%
End sub
%>