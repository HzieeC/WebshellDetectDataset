<!--#include file=conn.asp-->
<!-- #include file=inc/const.asp -->
<!-- #include file="inc/dv_clsother.asp" -->
<%
Dvbbs.LoadTemplates("boardhelp")
If DVbbs.BoardID=0 then
	Dvbbs.stats=template.Strings(0)
	Dvbbs.Nav()
	Dvbbs.Head_var 2,0,template.Strings(1),"boardhelp.asp"
Else
	Dvbbs.stats=template.Strings(1)
	Dvbbs.Nav()
	Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
End If
Dim Rs,sql,i,H_title,H_content
call main()
Dvbbs.ActiveOnline()
Dvbbs.footer()
Dvbbs.PageEnd()
Sub main()
Dim TempLateStr,TempHelp,Helptitle,LookID,BoardHelp
Helptitle=Server.HtmlEnCode(Request("title"))
LookID=Request("act")
if LookID="" or not isNUmeric(LookID) Then
	If Dvbbs.FoundIsChallenge or Dvbbs.Forum_ChanSetting(0)="1" Then
	LookID=0
	Else
	LookID=3
	End If
End If
If Request("view")<>"" and Request("view")="faq" Then
FAQContent(LookID)
BoardHelp=H_content
Helptitle=H_title
Else
Select case LookID
Case 0
BoardHelp=BoardHelp0
Case 1
BoardHelp=BoardHelp1
Case 2
BoardHelp=BoardHelp2
Case 3
BoardHelp=BoardHelp3
Case 4
BoardHelp=BoardHelp4
End Select
End IF
TempLateStr=template.html(0)
TempHelp=template.html(2)
TempHelp=Replace(TempHelp,"{$Title}",Helptitle)
TempHelp=Replace(TempHelp,"{$Body}",BoardHelp)
TempLateStr=Replace(TempLateStr,"{$TableWidth}",Dvbbs.mainsetting(0))
TempLateStr=Replace(TempLateStr,"{$Main}",TempHelp)
	If Dvbbs.FoundIsChallenge or Dvbbs.Forum_ChanSetting(0)="1" Then
		TempLateStr=Replace(TempLateStr,"{$GrayLink}",template.Strings(2))
	Else
		TempLateStr=Replace(TempLateStr,"{$GrayLink}","")
	End If
TempLateStr=Replace(TempLateStr,"{$boardid}",Dvbbs.boardid)
TempLateStr=Replace(TempLateStr,"{$BbsFaq}",BoardFAQ)
Response.Write TempLateStr
End Sub

Function BoardFAQ()
Dim TempLeft,TempRs
Set Rs=Dvbbs.Execute("Select H_ID,H_title From dv_help where not h_id=1 and H_ParentID=0 order by H_ParentID ")
If not Rs.Eof Then
SQL=Rs.GetRows(-1)
Rs.close:Set Rs=Nothing
For i=0 to  Ubound(SQL,2)
	TempLeft=TempLeft + "<hr size=1><li><b>" + server.htmlencode(SQL(1,i)) + "</b>"
	Set TempRs = Dvbbs.Execute("Select H_ID,H_ParentID,H_title From dv_help where H_ParentID="&SQL(0,i))
	Do while not TempRs.eof
	TempLeft = TempLeft + "<br>&nbsp;&nbsp;-- <a href=?boardid="&Dvbbs.boardid&"&view=faq&act="&TempRs(0)
	TempLeft = TempLeft + " >"
	TempLeft = TempLeft +	server.htmlencode(TempRs(2))
	TempLeft = TempLeft + "</a>"
	TempRs.movenext
	loop
Next
TempRs.close
set TempRs=nothing
End If
BoardFAQ=TempLeft
End Function

Function FAQContent(SID)
If not IsNumeric(SID) Then Exit Function
Set Rs=Dvbbs.Execute("Select H_ID,H_ParentID,H_title,H_content,H_stype,H_bgimg,H_Addtime From dv_help where H_ID= "&SID)
If not Rs.Eof Then
	H_title=server.htmlencode(Rs(2))
	H_content=Rs(3)
End If
Rs.close:Set Rs=Nothing
End Function

Function BoardHelp0()
	BoardHelp0=template.html(3)
End Function

Function BoardHelp1()
Dim TempLateStr
TempLateStr=template.html(5)
TempLateStr=Replace(TempLateStr,"{$RedClor}",Dvbbs.mainsetting(1))
'金钱
TempLateStr=Replace(TempLateStr,"{$RegUpMoney}",Dvbbs.Forum_user(0))
TempLateStr=Replace(TempLateStr,"{$LoginUpMoney}",Dvbbs.Forum_user(4))
TempLateStr=Replace(TempLateStr,"{$PostUpMoney}",Dvbbs.Forum_user(1))
TempLateStr=Replace(TempLateStr,"{$ReplyUpMoney}",Dvbbs.Forum_user(2))
TempLateStr=Replace(TempLateStr,"{$IsBestUpMoney}",Dvbbs.Forum_user(15))
TempLateStr=Replace(TempLateStr,"{$IsDelUpMoney}",Dvbbs.Forum_user(3))
'积分
TempLateStr=Replace(TempLateStr,"{$RegUpEP}",Dvbbs.Forum_user(5))
TempLateStr=Replace(TempLateStr,"{$LoginUpEP}",Dvbbs.Forum_user(9))
TempLateStr=Replace(TempLateStr,"{$PostUpEP}",Dvbbs.Forum_user(6))
TempLateStr=Replace(TempLateStr,"{$ReplyUpEP}",Dvbbs.Forum_user(7))
TempLateStr=Replace(TempLateStr,"{$IsBestUpEP}",Dvbbs.Forum_user(17))
TempLateStr=Replace(TempLateStr,"{$IsDelUpEP}",Dvbbs.Forum_user(8))
'魅力
TempLateStr=Replace(TempLateStr,"{$RegUpCP}",Dvbbs.Forum_user(10))
TempLateStr=Replace(TempLateStr,"{$LoginUpCP}",Dvbbs.Forum_user(14))
TempLateStr=Replace(TempLateStr,"{$PostUpCP}",Dvbbs.Forum_user(11))
TempLateStr=Replace(TempLateStr,"{$ReplyUpCP}",Dvbbs.Forum_user(12))
TempLateStr=Replace(TempLateStr,"{$IsBestUpCP}",Dvbbs.Forum_user(16))
TempLateStr=Replace(TempLateStr,"{$IsDelUpCP}",Dvbbs.Forum_user(13))
BoardHelp1=TempLateStr
End Function

Function BoardHelp2()
Dim TempLateStr,userclasslist
TempLateStr=template.html(6)
TempLateStr=Split(TempLateStr,"||")
set rs=Dvbbs.execute("Select usertitle,MinArticle,GroupPic From Dv_UserGroups where not Minarticle=-1 order by MinArticle")
do while not rs.eof
	userclasslist=userclasslist&TempLateStr(1)
	userclasslist=Replace(userclasslist,"{$GroupName}",RS(0))
	userclasslist=Replace(userclasslist,"{$MinArticle}",RS(1))
	userclasslist=Replace(userclasslist,"{$GroupPic}",RS(2))
rs.movenext
loop
rs.close
set rs=nothing
BoardHelp2=TempLateStr(0)&userclasslist
End Function

Function BoardHelp3()
BoardHelp3=template.html(4)
End Function

Function BoardHelp4()
Dim TempLateStr,MaxLengh
if Dvbbs.BoardID>0 Then
TempLateStr=template.html(7)
MaxLengh=Clng(Dvbbs.Board_Setting(16))/1024
TempLateStr=Replace(TempLateStr,"{$OpenHtml}",iif(Dvbbs.Board_Setting(5),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenUBB}",iif(Dvbbs.Board_Setting(6),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenImage}",iif(Dvbbs.Board_Setting(7),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenFlash}",iif(Dvbbs.Board_Setting(44),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenPlayer}",iif(Dvbbs.Board_Setting(9),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenEmcode}",iif(Dvbbs.Board_Setting(8),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenUpLoad}",iif(Dvbbs.Board_Setting(3),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$MaxLengh}",MaxLengh)
TempLateStr=Replace(TempLateStr,"{$OpenMoney}",iif(Dvbbs.Board_Setting(10),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenEP}",iif(Dvbbs.Board_Setting(11),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenCP}",iif(Dvbbs.Board_Setting(12),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenPower}",iif(Dvbbs.Board_Setting(13),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenArticle}",iif(Dvbbs.Board_Setting(14),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenReplay}",iif(Dvbbs.Board_Setting(15),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenBuy}",iif(Dvbbs.Board_Setting(23),"可用","不可用"))
End If
BoardHelp4=TempLateStr
TempLateStr=template.html(8)
TempLateStr=Replace(TempLateStr,"{$OpenHtml}",iif(Dvbbs.Forum_Setting(66),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenUBB}",iif(Dvbbs.Forum_Setting(65),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenImage}",iif(Dvbbs.Forum_Setting(67),"可用","不可用"))
TempLateStr=Replace(TempLateStr,"{$OpenFlash}",iif(Dvbbs.Forum_Setting(71),"可用","不可用"))
BoardHelp4=BoardHelp4&TempLateStr
End Function

Function iif(expression,returntrue,returnfalse)
If expression=1 Then
	iif=returntrue
Else
	iif=returnfalse
End If
End Function

%>