<!-- #include file="conn.asp" -->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/dv_clsother.asp"-->
<!--#include file="inc/dv_ubbcode.asp"-->
<!-- #include file="inc/ubblist.asp" -->
<%
Dim IsThisBoardMaster '确定当前用户是否本版版主，防止下面的操作影响到 Dvbbs.BoardMaster导致出错
IsThisBoardMaster = Dvbbs.BoardMaster
Dim dv_ubb
Set dv_ubb=new Dvbbs_UbbCode
dv_ubb.posttype=1
Dvbbs.loadtemplates("")
Dim replyid,Announceid
Dim Replyid_a, AnnounceID_a, RootID_a,T_GetMoneyType
if request("Announceid")<>"" and isnumeric(request("Announceid")) then Announceid=request("Announceid") else  Announceid=0
If request("replyid")="" Then
	replyid=Announceid
ElseIf not Dvbbs.isInteger(request("replyid")) Then
	replyid=Announceid
Else
	replyid=request("replyid")
End If
Dim TotalUseTable
TotalUseTable=Request("TotalUseTable")
If TotalUseTable="" Then
	TotalUseTable=Dvbbs.NowUseBBS
Else
	TotalUseTable=checktable(TotalUseTable)
End If
Dvbbs.Stats="预览帖子"
Dvbbs.head()
Dim Username,PostBuyUser
Dim abgcolor,bgcolor
abgcolor="tablebody1"
bgcolor="tablebody2"
Dim EmotPath
EmotPath=Split(Dvbbs.Forum_emot,"|||")(0)		'em心情路径
Response.Write "<p>&nbsp;</p>"
Response.Write "<table cellpadding=3 cellspacing=1 align=center class=tableborder1>"
Response.Write "<TBODY> "
Response.Write "<TR>"
Response.Write "<Th height=25>帖子预览</Th>"
Response.Write "</TR>"
Response.Write "<TR>"
Response.Write "<TD class=tablebody1 height=24>"
Response.Write "<b>"
Response.Write  Dvbbs.htmlencode(request.form("Dvtitle"))
Response.Write "</b>"
Response.Write "</TD></TR></TBODY></TABLE><br>"
Response.Write "<table cellpadding=3 cellspacing=1 align=center class=tableborder1>"
Response.Write "<TR>"
Response.Write "<TD class=tablebody1>"
Dim theBody
theBody=request.form("theBody")
ubblists=ubblist(theBody)&"39,"
theBody = replace(theBody,"&gt;",">")
theBody = replace(theBody,"&lt;","<")
theBody = dv_ubb.Dv_UbbCode(theBody,Dvbbs.UserGroupID,1,0)
theBody = replace(theBody,"	","  ")
theBody = replace(theBody,"&amp;nbsp;"," ")
Response.Write theBody
Response.Write "</TD></TR></TBODY></TABLE>"
Set dv_ubb=Nothing
Dvbbs.Footer()
Dvbbs.PageEnd()
Function checktable(Table)
	Table=Right(Trim(Table),2)
	If Not IsNumeric(table) Then Table=Right(Trim(Table),1)
	If Not IsNumeric(table) Then Dvbbs.AddErrCode(30)
	checktable="Dv_bbs"&table
End Function
%>