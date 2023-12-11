<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!-- #include file="inc/dv_clsother.asp" -->
<!-- #include file="Dv_plus/Tools/plus_MagicFace_const.asp" -->
<%
Dvbbs.LoadTemplates("")
Dvbbs.Stats = "魔法表情列表"
Dvbbs.Head()
If Dvbbs.UserID=0 Then Dvbbs.AddErrCode(6):Dvbbs.Showerr()
Dim s,sv
s = Request("s")
If s = "" Or Not IsNumeric(s) Then s = 0
s = Cint(s)
If s <> 0 And s <> 1 Then s = 0
If s = 0 Then
	sv = "表情"
Else
	sv = "头像"
End If
%>
<!-- #include file="Dv_plus/Tools/DrawMagicFace.asp" -->
<script language="javascript">
	document.oncontextmenu = norightclick;
	function select(image1,M,T,A1,A2,A3,A4,A5){
		var UserMoney = <%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text%>;
		var UserTicket = <%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text%>;
		var UserArticle = <%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userpost").text%>;
		var UserWealth = <%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userwealth").text%>;
		var UserEP = <%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userep").text%>;
		var UserCP = <%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usercp").text%>;
		var UserPower = <%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userpower").text%>;
		if(UserMoney < M && UserTicket < T){alert("您没有足够的金币或点券使用此魔法<%=sv%>。");return false;}
		if(UserArticle < A1){alert("您的帖子数没有达到使用魔法<%=sv%>的标准。");return false;}
		if(UserWealth < A2){alert("您的帖子数没有达到使用魔法<%=sv%>的标准。");return false;}
		if(UserEP < A3){alert("您的帖子数没有达到使用魔法<%=sv%>的标准。");return false;}
		if(UserCP < A4){alert("您的帖子数没有达到使用魔法<%=sv%>的标准。");return false;}
		if(UserPower < A5){alert("您的帖子数没有达到使用魔法<%=sv%>的标准。");return false;}
		var image2 = 'dv_plus/tools/magicface/gif/'+image1+'.gif';
		parent.document.images['magicfacepic'].src=image2;
		parent.document.getElementById("magicmoney").innerHTML = M;
		parent.document.getElementById("magicticket").innerHTML = T;
		parent.document.getElementById("smagicface").value = image1;
	}
	function norightclick(e) 
	{
		try{
		   event.cancelBubble = true
		   event.returnValue = false;
		   return false;
		}catch(e){}
	}
</script>
<%
MagicList()
Dvbbs.PageEnd()
Sub MagicList()
	Dim iMagicFaceType,i,stype,Rs,Sql
	iMagicFaceType = Split(MagicFaceType,"|")
	Dim Page,MaxRows,Endpage,CountNum,PageSearch,SqlString
	Endpage = 0
	MaxRows = 21
	Page = Request("Page")
	If IsNumeric(Page) = 0 or Page="" Then Page=1
	Page = Clng(Page)
	stype = Request("stype")
	If IsNumeric(stype) = 0 or stype="" Then stype=-2
	stype = Clng(stype)
%>
<div><img height=3></div>
<table cellspacing=0 cellpadding=3 align=center class=tableBorder2 style="width:98%" border=0>
<tr>
<th align=left height=25 ID="TableTitleLink" colspan=8>魔法<%=sv%>：<a href="?BoardID=<%=Dvbbs.BoardID%>&s=<%=s%>">全部</a> | <a href="?BoardID=<%=Dvbbs.BoardID%>&stype=-1&s=<%=s%>">免费</a> | 
<%
For i = 0 To Ubound(iMagicFaceType)
	If i <> Ubound(iMagicFaceType) Then
		Response.Write "<a href=""?BoardID="&Dvbbs.BoardID&"&stype="&i&"&s="&s&""">"&iMagicFaceType(i)&"</a> | "
	Else
		Response.Write "<a href=""?BoardID="&Dvbbs.BoardID&"&stype="&i&"&s="&s&""">"&iMagicFaceType(i)&"</a>"
	End If
Next
%>
| <a href="UserPay.asp" target=_blank>点券</a>
</th>
</tr>
<tr>
<!--
Modify By Dv_Xiaoxian，For:Del. Old Link 
Date:2008.0.14
-->
<td rowspan="5" width="5"  style="border-right : 1px dotted gray;background-color:#F0F0F0;"><div align=right style="float:right;font-size:11px;word-spacing: 2;font-family:Verdana;font-weight:bold;writing-mode:tb-rl;" title="动网先锋魔法表情"><font style="color:#CC00FF;"><font style="color:#33CCFF;">DV</font>-<font style="color:#D98200;">M</font>agic<font style="color:#D98200;">F</font>ace</font></div>
</td>
<td class="tablebody2" colspan=7>&nbsp;您目前有 <font color=red><B><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text%></B></font> 个金币和 <font color=red><B><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text%></B></font> 张点券</td>
</tr>
<%
	Dim MagicSetting,ii,iii,svsql
	iii = 1
	If s = 0 Then
		svsql = ",tMoney,tTicket"
	Else
		svsql = ",iMoney,iTicket"
	End If
	If stype = -2 Then
		Sql="Select ID,Title,MagicFace_s,MagicType "&svsql&",MagicSetting From Dv_Plus_Tools_MagicFace Order By ID Desc"
	ElseIf stype = -1 Then
		Sql="Select ID,Title,MagicFace_s,MagicType "&svsql&",MagicSetting From Dv_Plus_Tools_MagicFace Where iMoney = 0 Or tMoney = 0 Order By ID Desc"
	Else
		Sql="Select ID,Title,MagicFace_s,MagicType "&svsql&",MagicSetting From Dv_Plus_Tools_MagicFace Where MagicType = "&stype&" Order By ID Desc"
	End If
	Set Rs = Dvbbs.iCreateObject ("adodb.recordset")
	If Cint(Dvbbs.Forum_Setting(92))=1 Then
		If Not IsObject(Plus_Conn) Then Plus_ConnectionDatabase
		Rs.Open Sql,Plus_Conn,1,1
	Else
		If Not IsObject(Conn) Then ConnectionDatabase
		Rs.Open Sql,conn,1,1
	End If
	If Not (Rs.Eof And Rs.Bof) Then
		CountNum = Rs.RecordCount
		If CountNum Mod MaxRows=0 Then
			Endpage = CountNum \ MaxRows
		Else
			Endpage = CountNum \ MaxRows+1
		End If
		Rs.MoveFirst
		If Page > Endpage Then Page = Endpage
		If Page < 1 Then Page = 1
		If Page >1 Then 				
			Rs.Move (Page-1) * MaxRows
		End if
		SQL=Rs.GetRows(MaxRows)
		For i=0 To Ubound(SQL,2)
			MagicSetting = Split(SQL(6,i),"|")
			If ii = 0 Then Response.Write "<tr>"
			Response.Write "<td valign=""top"" class=tablebody1 style=""width:60"">"

			Response.Write "<div align=""center"" bgcolor=""F8F8F8"">"
			Response.Write "<img src=""dv_plus/tools/magicface/gif/"&SQL(2,i)&".gif"" title="""&SQL(1,i)&""" width=""40"" height=""39"" border=""0"" alt="""&SQL(1,i)&""" class=ImgOnclick onClick=""select('"&SQL(2,i)&"',"&SQL(4,i)&","&SQL(5,i)&","&MagicSetting(0)&","&MagicSetting(1)&","&MagicSetting(2)&","&MagicSetting(3)&","&MagicSetting(4)&")"" onMouseDown=""DispMagicEmot(event,"&SQL(2,i)&",350,500)"">"
			Response.Write "</div><div align=""center"" style=""padding:2px"">"
			Response.Write "<font color=gray style=""font-size: 11px"" title=""使用该魔法表情需要"&SQL(4,i)&"个金币或"&SQL(5,i)&"张点券"">"&SQL(4,i)&" / "&SQL(5,i)&"</font></td>"

			Response.Write "</div>"
			Response.Write "</td>"
			If ii = 6 Then Response.Write "</tr>"
			ii = ii + 1
			If ii = 7 Then
				ii = 0
				iii = iii + 1
			End If
		Next
	End If
	Rs.Close
	Set Rs=Nothing
	Select Case ii
	Case 1
		Response.Write "<td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td></tr>"
	Case 2
		Response.Write "<td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td></tr>"
	Case 3
		Response.Write "<td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td></tr>"
	Case 4
		Response.Write "<td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td></tr>"
	Case 5
		Response.Write "<td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td></tr>"
	Case 6
		Response.Write "<td width=""56"" class=tablebody1></td></tr>"
	End Select
	Select Case iii
	Case 1
		Response.Write "<tr><td width=""56"" height=""65"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td></tr>"
		Response.Write "<tr><td width=""56"" height=""65"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td></tr>"
	Case 2
		Response.Write "<tr><td width=""56"" height=""65"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td><td width=""56"" class=tablebody1></td></tr>"
	End Select
%>
</table>

  <table width="403" border="0" cellspacing="0" cellpadding="0" height="26" align=center>
	<form action="?" method="post">
    <tr>
      <td width="9" align="right" height="22">&nbsp;</td>
      <td width="90" height="22"><img src="dv_plus/tools/magicface/dot.gif" width="7" height="7">
        <b><font color="#D98200">
<%
If stype = -2 Then Response.Write "全部"
If stype = -1 Then Response.Write "免费"
For i = 0 To Ubound(iMagicFaceType)
	If i = stype Then
		Response.Write iMagicFaceType(i)
		Exit For
	End If
Next
%>
        </font></b></td>
	  <td width="120"><font color=blue>点击选中 右键预览</font></td>
	  <td width="58" height="22" align="right">
	  
<%
If Page = 1 Then
	If Endpage > 1 Then Response.Write "<a href='?BoardID="&Dvbbs.BoardID&"&stype="&stype&"&Page="&Page+1&"&s="&s&"'>下页</a>"
Else
	If Endpage = Page Then
		Response.Write "<a href='?BoardID="&Dvbbs.BoardID&"&stype="&stype&"&Page="&Page-1&"&s="&s&"'>上页</a>"
	Else
		Response.Write "<a href='?BoardID="&Dvbbs.BoardID&"&stype="&stype&"&Page="&Page-1&"&s="&s&"'>上页</a>&nbsp;"
		Response.Write "<a href='?BoardID="&Dvbbs.BoardID&"&stype="&stype&"&Page="&Page+1&"&s="&s&"'>下页</a>"
	End If
End If
%>
	  </td>
	  <td width="50" align="right"><font class=RedFont><%=Page%></font>/<%=Endpage%>&nbsp;&nbsp;</td>
	  <td width="26" align="right"><input type="text" name="Page" size="1" style="BORDER-top: #C4C4C4 1px solid; BORDER-bottom: #C4C4C4 1px solid; BORDER-left: #C4C4C4 1px solid; BORDER-right: #C4C4C4 1px solid; font-size: 12px;"></td>
	  <td width="18">&nbsp;页 </td>
      <td width="44" valign="center" height="22" align="center">
		<INPUT TYPE="hidden" NAME="stype" value="<%=Stype%>"><INPUT TYPE="hidden" NAME="s" value="<%=s%>">
        <input type="image" src="dv_plus/tools/magicface/bt_go.gif" onClick="this.form.submit()" style="cursor:hand" name="Submit" value="Submit">
      </td>
    </tr>
	</form>
  </table>
<%
End Sub
%>