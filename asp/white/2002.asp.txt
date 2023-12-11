<!--#include file =conn.asp-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/dv_clsother.asp"-->
<!--#include file="inc/GroupPermission.asp"-->
<!-- #include file="Dv_plus/Tools/plus_Tools_const.asp" -->
<%
Dvbbs.stats="论坛道具权限信息"
Dvbbs.LoadTemplates("")
Dvbbs.Head()

Select case Trim(Request.QueryString("orders"))
	Case "0" : Show_UserGroupID
	Case "1" : Show_BoardID
	Case "2" : Show_ToolsInfo
	Case Else
	Show_ToolsInfo
End Select
Dvbbs.Showerr()
ShowFoot()
Dvbbs.mainsetting(0)="98%"
Dvbbs.Footer()
Dvbbs.PageEnd()
'--------------------------------------------------------------------------------
'用户道具列表
'--------------------------------------------------------------------------------
Sub Show_ToolsInfo()

''道具获取参数
'1:目标用户：ToUserID=
'2:帖子：BoardID=&TopicID=&ReplyID=
Dim Str
Str = "Action=0&ToUserID="&Request("ToUserID")&"&BoardID="&Dvbbs.BoardID&"&TopicID="&Request("TopicID")&"&ReplyID="&Request("ReplyID")
%>
<table border="0" cellpadding=3 cellspacing=1 align=center class=Tableborder1 Style="Width:99%">
	<tr>
	<th height=23>道具使用列表</th></tr>
	<tr><td height=23 class=Tablebody1><B>说明</B>：请确认每种道具的说明再进行操作！<BR></td></tr>
</table>
<%
	Dim Sql,Rs,i
	Response.Write "<table border=""0"" cellpadding=3 cellspacing=0 align=center Style=""Width:99%""><tr>"

	Sql = "Select B.ID,B.UserID,B.UserName,B.ToolsID,B.ToolsName,B.ToolsCount,B.SaleCount,B.SaleMoney,B.SaleTicket,I.ToolsInfo,I.ToolsImg From [Dv_Plus_Tools_Buss] B Inner Join [Dv_Plus_Tools_Info] I on I.ID=B.ToolsID where B.UserID="& Dvbbs.UserID &" ORDER BY B.ID Desc"
	Set Rs = Dvbbs.Plus_Execute(Sql)
	If Not Rs.eof Then
		SQL = Rs.GetRows(-1)
	Else
		Response.Write "<td class=TableBody1 valign=Top>您还未有任何道具，请到论坛道具中心购买！</td></tr></table>"
		Exit Sub
	End If
	Rs.close:Set Rs = Nothing
	Dim ToolsImg
	For i=0 To Ubound(SQL,2)
		If SQL(10,i)<>"" Then
			ToolsImg = Server.Htmlencode(SQL(10,i))
		Else
			ToolsImg = "Dv_plus/Tools/pic/None.jpg"
		End If
	%>
<td class=TableBody1 valign=Top>
	<table border=0 cellpadding=2 cellspacing=1 class=Tableborder2 Style="Width:100%" align=center>
		<tr><th height=20 ><%=Server.Htmlencode(SQL(4,i))%></th></tr>
		<tr>
		  <td align=center Width="50%"><a href="plus_Tools_postings.asp?ToolsID=<%=SQL(3,i)%>&<%=Str%>" >
		  <img src="<%=ToolsImg%>" border=0 Title="说明：<%=Server.Htmlencode(SQL(9,i)&"")%>&#13;&#10;数量：<%=SQL(5,i)%>">
		  </a>
		  </td>
		</tr>
		<tr><td><li>数量：<%=SQL(5,i)%></td>
		</tr>
	</table>
</td>
	<%
	If i mod 3 = 2 Then Response.Write "</tr><tr>"
	Next
	Response.Write "</tr></table>"
End Sub

Sub ShowFoot()
%>
<SCRIPT LANGUAGE="JavaScript">
<!--
function putallid(v){
	var putid="";
	if (thisForm.ID.length!=null){
		for (var i=0;i<thisForm.ID.length;i++){
			if (thisForm.ID[i].checked==true){
				if (putid!="") putid += ",";
				putid += thisForm.ID[i].value; 
				//if (i!=thisForm.ID.length-1){putid += ",";}
			}
		}
	}else{
		if (thisForm.ID.checked==true) putid=thisForm.ID.value;
	}
	if (v){
		v.value=putid;
	}
	window.close();
}
function CheckAll(form)  
  {  
  for (var i=0;i<form.elements.length;i++)  
    {  
    var e = form.elements[i];  
    if (e.name != 'chkall')  
       e.checked = form.chkall.checked;  
    }  
  }
//-->
</SCRIPT>
<%
End Sub

Sub Show_UserGroupID()
	Dim ID,Rs
	Dim ToolsGroupID,ToolsName,Temp,CanUse
	CanUse = False
	Temp = 1
	ID = Trim(Request.QueryString("ID"))
	If ID<>"" And IsNumeric(ID) Then
		ID = Cint(ID)
	Else
		Dvbbs.AddErrCode(34) : Exit Sub
	End If

	Set Rs = Dvbbs.Plus_Execute("Select ToolsName,UserGroupID From [Dv_Plus_Tools_Info] Where ID="& ID)
	If Rs.Eof Then
		Dvbbs.AddErrCode(34) : Exit Sub
	Else
		ToolsName = Dvbbs.iHtmlencode(Rs(0))
		ToolsGroupID = Rs(1)
	End If
	Rs.Close
	If IsNull(ToolsGroupID) Then ToolsGroupID = ""
	If ToolsGroupID<>"" Then ToolsGroupID = ","&Trim(ToolsGroupID)&","
	If Dvbbs.Master And Session("flag")<>"" Then Temp=0
	%>
	<table cellspacing="1" cellpadding="3" align="center" class=tableborder1 Style="width:99%">
	<form name=thisForm onsubmit="putallid(self.opener.PlusTools.ToolsGroupID);">
	<tr><th colspan="2" height=23><%=ToolsName%> -- 用户组权限列表</th></tr>
	<%
	Dim IsSet
	Set Rs=DvBBS.Execute("Select UserGroupID,Title,UserTitle,parentgid From Dv_UserGroups where parentgid<>0 Order By parentgid,UserGroupID")
	Do while not Rs.eof

	If Dvbbs.UserGroupID=Rs(0) Then CanUse=True
	IsSet = SysGroupName(Rs(3))
	%>
	<tr>
	<td width="70%" <%If Dvbbs.UserGroupID=Rs(0) Then Response.Write "class=Tablebody2" Else Response.Write "class=tablebody1"%>><%=IsSet%> <%=Rs(1)%> >> <font color=gray><%=Rs(2)%></font></td>
	<td class=tablebody2 width="30%" align=center>
	<%=iffcheck(InStr(ToolsGroupID,","&Rs(0)&",")>0,Temp,Rs(0))%>
	</td>
	</tr>
	<%
	Rs.movenext
	Loop
	Rs.close
	Set Rs=Nothing
	Response.Write "<tr><td class=tablebody2 colspan=2 align=center><font color="& Dvbbs.mainsetting(1) &">"
	If CanUse=True Then _
		Response.Write "恭喜您，您所属的用户组可以使用该道具！" _
	Else _
		Response.Write "很抱歉，您所属的用户组不可以使用该道具！"
	Response.Write "</font></td></tr><tr><td class=tablebody1 colspan=2 >"
	If Temp=0 Then _
		Response.Write "<INPUT TYPE=""submit"" value=""确认"" >全选:<input type=checkbox name=chkall value=on onclick=""CheckAll(this.form)""> 选取允许的用户组，然后点击确定，当道具资料修改提交后才能生效！" _
	Else _
		Response.Write "<input type=""button"" value=""关闭"" onclick=""window.close()"">"
	Response.Write "</td></tr></form></table>"
End Sub

Sub Show_BoardID()
	Dim ID,Rs
	Dim ToolsBoardID,ToolsName,Temp
	Temp=1
	ID = Trim(Request.QueryString("ID"))
	If ID<>"" And IsNumeric(ID) Then
		ID = Cint(ID)
	Else
		Dvbbs.AddErrCode(34) : Exit Sub
	End If

	Set Rs = Dvbbs.Plus_Execute("Select ToolsName,BoardID From [Dv_Plus_Tools_Info] Where ID="& ID)
	If Rs.Eof Then
		Dvbbs.AddErrCode(34) : Exit Sub
	Else
		ToolsName = Dvbbs.iHtmlencode(Rs(0))
		ToolsBoardID = Rs(1)
	End If
	Rs.Close:Set Rs=Nothing
	If IsNull(ToolsBoardID) Then ToolsBoardID = ""
	If ToolsBoardID<>"" Then ToolsBoardID = ","&Trim(ToolsBoardID)&","
	If Dvbbs.Master And Session("flag")<>"" Then Temp=0
	%>
	<table cellspacing="1" cellpadding="3" align="center" class=tableborder1 Style="width:99%">
	<form name=thisForm onsubmit="putallid(self.opener.document.PlusTools.ToolsBoardID);">
	<tr><th colspan="2" height=23><%=ToolsName%> -- 版块权限列表</th></tr>
	<%
	''论坛版块列表
	Dim i,ii,BoardSetting,Loadboard
	Dim Node,xpath
	If Dvbbs.GroupSetting(37) ="1" Then xpath="[@hidden=0]"
	For Each Node in Application(Dvbbs.CacheName&"_boardlist").documentElement.selectNodes("board"& xpath)
		Response.Write "<tr><td"
			Response.Write " class=tablebody2>"
			If Node.attributes.getNamedItem("depth").text>"0" Then
				For ii=1 To Clng(Node.attributes.getNamedItem("depth").text)
					Response.Write "&nbsp;&nbsp;"
				Next
			End If
			If Node.attributes.getNamedItem("child").text>"0" Then
				Response.Write "<img src=""skins/default/plus.gif"">"
			Else
				Response.Write "<img src=""skins/default/nofollow.gif"">"
			End If
			Response.Write Node.attributes.getNamedItem("boardtype").text
			Response.Write "</td><td class=tablebody1>"&iffcheck(InStr(ToolsBoardID,","&Node.attributes.getNamedItem("boardid").text&",")>0,Temp,Node.attributes.getNamedItem("boardid").text)&"</td>"
			Response.Write "</tr>"
	Next
	Response.Write "<tr><td class=tablebody1 colspan=2 >"
	If Temp=0 Then _
		Response.Write "<INPUT TYPE=""submit"" value=""确认"" >全选:<input type=checkbox name=chkall value=on onclick=""CheckAll(this.form)""> 选取允许使用的版块，然后点击确定，当道具资料修改提交后才能生效！" _
	Else _
		Response.Write "<input type=""button"" value=""关闭"" onclick=""window.close()"">"
	Response.Write "</td></tr></form></table>"
End Sub

Function iffcheck(iBoolean,itype,iStr)
	If itype=0 Then
		iffcheck = "<INPUT TYPE=""checkbox"" NAME=""ID"" value="&iStr
		If iBoolean=True Then
			iffcheck = iffcheck & " checked>"
		Else
			iffcheck = iffcheck & " >"
		End If
	Else
		If iBoolean=True Then
			iffcheck = "√"
		Else
			iffcheck = "<font color="&Dvbbs.Mainsetting(1)&">x</font>"
		End If
	End If
End Function
%>