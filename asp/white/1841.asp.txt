<!--#include file =conn.asp-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="Dv_plus/Tools/plus_Tools_const.asp" -->
<%
Dvbbs.stats="论坛道具交易中心"
Dvbbs.LoadTemplates("")
Dvbbs.nav()
If DVbbs.BoardID=0 then
	Dvbbs.Head_var 0,0,"道具交易中心","plus_Tools_Center.asp"
End If
If Dvbbs.userid=0 Then Dvbbs.AddErrCode(6):Dvbbs.Showerr()	'判断用户是否在线。
If DVbbs.Forum_Setting(91)="0" Then Dv_Tools.ShowErr(2)	'交易中心已经关闭
CenterMain()
Dvbbs.Showerr()
Dvbbs.Footer()
Dvbbs.PageEnd()
'--------------------------------------------------------------------------------
'--------------------------------------------------------------------------------
Sub CenterMain()
	Tools_Nav_Link()
%>
<div class="mainbox">
<ul class="tableborder heightd">
	<table border="0" width="<%=Dvbbs.mainsetting(0)%>" cellpadding=2 cellspacing=0 align="center" style="width:100%;">
		<tr>
		<td width="180" valign=top>
		<%UserInfo()%>
		</td>
		<td width=5></td>
		<td width="*" valign=top>
			<table border="0" cellpadding=3 cellspacing=1 align="center" class=Tableborder1 Style="Width:100%">
				<tr>
					<th height=23>系统道具列表</th>
				</tr>
				<tr>
					<td height=23 class=Tablebody1 style="line-height: 15px"><B>说明</B>：<BR>
						1、论坛用户可通过道具中心进行论坛道具的购买、转让等操作，道具的详细使用说明点击购买后可查看<BR>
						2、金币的获取方式为论坛金钱、积分和魅力的转换，以及版主或管理员的奖励，或是会员转让和赠与<BR>
						3、道具所具备的功能、价格、使用限制等可参考相应道具的详细信息中的说明<BR>
						4、针对用户使用的道具通常是点击相应的用户进入该用户资料后点击使用道具连接再选择所使用的道具<BR>
						5、针对帖子使用的道具在进入帖子浏览页面中选择相应的道具使用图片和道具使用连接
					</td>
				</tr>
				<tr>
					<td height=23 class=Tablebody2 align="center">欢迎购买论坛道具，享受更多有趣的论坛功能！</td>
				</tr>
				<tr>
					<td height=25 class=Tablebody1 align="center">
						<a href="UserPay.asp?action=UserCenter">论坛金币转换</a> | 
						<a href="plus_Tools_Center.asp?action=UserBussTools_List&UserID=<%=Dvbbs.UserID%>">我出售的道具</a> | 
						<a href="UserPay.asp?action=UserToolsLog_List">道具操作记录</a> | 
						<a href="javascript:openScript('plus_Tools_InfoSetting.asp?action=0&ToUserID=<%=Dvbbs.UserID%>',600,450)">使用个人道具</a> | 
						<a href="UserPay.asp"><font color=red>购买论坛点券</font></a>
					</td>
				</tr>
			</table>
		</td>
		</tr>
		<tr><td colspan=3><hr style="BORDER: #807d76 1px dotted;height:1px;">
		<%
		Select Case LCase(Request.QueryString("action"))
			Case "usertools_list":UserTools_List
			Case "userbusstools_list":UserBussTools_List
			Case "usercenter":UserCenter
			Case Else
				Tools_List()
		End Select
		%>
		</td></tr>
	</table>
<ul></div>
<%
End Sub

'--------------------------------------------------------------------------------
'道具列表
'--------------------------------------------------------------------------------
Sub Tools_List()
	Response.Write "<script language=""JavaScript"" src=""inc/Pagination.js""></script>"
%>
<table border="0" cellpadding=3 cellspacing=0 align="center" Style="Width:100%"><tr>
<%
	Dim Rs,Sql,Orders,i,ii
	Dim ToolsTitle
	Dim Page,MaxRows,Endpage,CountNum,PageSearch,SqlString
	PageSearch = "action="
	Endpage = 0
	MaxRows = 9
	Page = Request("Page")
	If IsNumeric(Page) = 0 or Page="" Then Page=1
	Page = Clng(Page)
	ii=0
	Dim ToolsPrice,ToolsImg
	Orders = "SysStock Desc"
	Sql = "Select ID,ToolsName,ToolsInfo,IsStar,SysStock,UserStock,UserTicket,UserMoney,BuyType,ToolsImg From [Dv_Plus_Tools_Info] ORDER BY "& Orders
	Set Rs = Dvbbs.iCreateObject ("adodb.recordset")
	If Cint(Dvbbs.Forum_Setting(92))=1 Then
		If Not IsObject(Plus_Conn) Then Plus_ConnectionDatabase
		Rs.Open Sql,Plus_Conn,1,1
	Else
		If Not IsObject(Conn) Then ConnectionDatabase
		Rs.Open Sql,Conn,1,1
	End If
	If Not Rs.eof Then
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
		'SQL=Rs.GetRows(-1)
	Else
		Response.Write "<tr><td class=""Tablebody1"" colspan=""5"" align=""center"">道具还未添加！</td></tr></table>"
		Exit Sub
	End If
	Rs.close:Set Rs = Nothing
	'输出道具列表

	For i=0 To Ubound(SQL,2)
		If SQL(9,i)<>"" Then
			ToolsImg = Server.Htmlencode(SQL(9,i))
		Else
			ToolsImg = "Dv_plus/Tools/pic/None.jpg"
		End If
		ToolsTitle = SQL(2,i)
		If Len(SQL(2,i))>18 Then SQL(2,i) = Left(SQL(2,i),16) & "..."
	%>
<td class=TableBody1 valign=Top width="33%">
	<table border=0 cellpadding=2 cellspacing=1 class=Tableborder2 Style="Width:100%" align="center">
		<tr><th height=20 colspan="3"><%=Server.Htmlencode(SQL(1,i))%></th></tr>
		<tr><td height=20 colspan="3" Class=TableBody1 title="<%=Server.Htmlencode(ToolsTitle&"")%>"><li><%=Server.Htmlencode(SQL(2,i)&"")%></td></tr>
		<tr>
		  <td align="center" Width="50%" rowspan="5"><%If Cint(SQL(3,i))=1 Then Response.Write "<A href=""JavaScript:openScript('plus_Tools_pay.asp?action=BuyTools&ToolsID="&SQL(0,i)&"',600,450)"">"%><img src="<%=ToolsImg%>" border=0 ><%If Cint(SQL(3,i))=1 Then Response.Write "</a>"%></td>
		  <td width="15%" height="20" Class=TableBody1>金币</td>
		  <td width="35%" height="20" Class=TableBody1><font color="<%=Dvbbs.mainsetting(1)%>"><%= SQL(7,i) %></font></td>
		</tr>
		<tr>
		  <td Class=TableBody1>点券</td>
		  <td Class=TableBody1><font color="<%=Dvbbs.mainsetting(1)%>"><%= SQL(6,i) %></font></td>
		</tr>
		<tr>
		<td Class=TableBody1>购买<br>方式</td>
		<td Class=TableBody1><%=Dv_Tools.BuyType(SQL(8,i))%></td>
		</tr>
		<tr>
		<td Class=TableBody1>系统<br>库存</td>
		<td Class=TableBody1>
		<%
		If SQL(4,i)<0 Then
			Response.Write "系统道具"
		Else
			Response.Write SQL(4,i)
		End If
		%></td>
		</tr>
		<tr>
		<td colspan="2" Class=TableBody1 align="center">
			<%
			If Cint(SQL(3,i))=1 Then
				Response.Write "<A href=""JavaScript:openScript('plus_Tools_pay.asp?action=BuyTools&ToolsID="&SQL(0,i)&"',600,450)"">购买</a>"
			Else
				Response.Write "关闭"
			End If
			%>
		</td>
		</tr>
	</table>
</td>
	<%
	If ii=2 Then
		Response.Write "</tr><tr>"
		ii=0
	Else
		ii=ii+1
	End If
	'If i mod 3 = 1 Then Response.Write "</tr><tr>"
	Next
	Response.Write "</tr></table>"
	Response.Write "<SCRIPT>PageList("&Page&",3,"&MaxRows&","&CountNum&","""&PageSearch&""",1);</SCRIPT>"
End Sub


'--------------------------------------------------------------------------------
'道具转让列表
'--------------------------------------------------------------------------------
Sub Userbuy_List

End Sub

'--------------------------------------------------------------------------------
'用户道具列表
'--------------------------------------------------------------------------------
Sub UserTools_List
%>
<table border="0" cellpadding="3" cellspacing="1" align="center" class="Tableborder1" Style="Width:100%">
	<tr>
		<th height="23" width="20%" style="text-align:center;">道具名称</th>
		<th width="15%" style="text-align:center;">道具数量</th>
		<th width="15%" style="text-align:center;">出售数量</th>
		<th width="15%" style="text-align:center;">金币</th>
		<th width="15%" style="text-align:center;">点券</th>
		<th width="20%" style="text-align:center;">操作</th>
	</tr>
<%
	Dim Rs,Sql,i,ToolsPrice
	Sql = "Select ID,UserID,UserName,ToolsID,ToolsName,ToolsCount,SaleCount,UpdateTime,SaleMoney,SaleTicket From [Dv_Plus_Tools_Buss] where UserID="& Dvbbs.UserID &" ORDER BY ToolsCount Desc"
	Set Rs = Dvbbs.Plus_Execute(Sql)
	If Not Rs.eof Then
		SQL=Rs.GetRows(-1)
	Else
		Response.Write "<tr><td class=""Tablebody1"" colspan=""6"" align=""center"">道具还未添加！</td></tr></table>"
		Exit Sub
	End If
	Rs.close:Set Rs = Nothing

	'输出道具列表
	For i=0 To Ubound(SQL,2)
	ToolsPrice = "金币：<font color="& Dvbbs.mainsetting(1) &">"& SQL(8,i) &"</font> + 点券：<font color="& Dvbbs.mainsetting(1) &">"& SQL(9,i) &"</font>"
%>
	<tr>
	<td class="Tablebody1" align="center" height=24><%=SQL(4,i)%></td>
	<td class="Tablebody1" align="center"><%=SQL(5,i)%></td>
	<td class="Tablebody1" align="center"><%=SQL(6,i)%></td>
	<td class="Tablebody1" align="center"><font color="<%= Dvbbs.mainsetting(1) %>"><%=SQL(8,i)%></font></td>
	<td class="Tablebody1" align="center"><font color="<%= Dvbbs.mainsetting(1) %>"><%=SQL(9,i)%></font></td>
	<td class="Tablebody1" align="center">
	<A href="JavaScript:openScript('plus_Tools_pay.asp?action=SellTools&ToolsID=<%=SQL(3,i)%>&BussID=<%=SQL(0,i)%>',600,450)">转让</A></td>
	</tr>
<%
	Next
	Response.Write "</table>"
End Sub



'--------------------------------------------------------------------------------
'用户道具转让中心列表
'--------------------------------------------------------------------------------
Sub UserBussTools_List

	Dim Rs,Sql,i,ToolsPrice
	Dim Page,MaxRows,Endpage,CountNum,PageSearch,SqlString
	PageSearch = "action=UserBussTools_List"
	Endpage = 0
	MaxRows = 20
	Page = Request("Page")
	If IsNumeric(Page) = 0 or Page="" Then Page=1
	Page = Clng(Page)
	Response.Write "<script language=""JavaScript"" src=""inc/Pagination.js""></script>"

	If Request.QueryString("UserID")<>"" and IsNumeric(Request.QueryString("UserID")) Then _
	SqlString = "and UserID="&Dv_Tools.CheckNumeric(Request.QueryString("UserID"))

%>
<table border="0" cellpadding=3 cellspacing=1 align="center" class=Tableborder1 Style="Width:100%">
	<tr>
	<th height=23 width="20%" style="text-align:center;">道具名称</th>
	<th width="15%" style="text-align:center;">出售者</th>
	<th width="15%" style="text-align:center;">金币</th>
	<th width="15%" style="text-align:center;">点券</th>
	<th width="15%" style="text-align:center;">出售数量</th>
	<th width="20%" style="text-align:center;">操作</th>
	</tr>
<%
	Response.Write "<tr><td class=""tablebody1"" colspan=""6"" height=24><B>小贴士</B>：自己出售的道具不想继续出售只需点购买然后填写回购的数量即可，此操作将不扣除您的货币</td></tr>"
	Sql = "Select ID,UserID,UserName,ToolsID,ToolsName,ToolsCount,SaleCount,UpdateTime,SaleMoney,SaleTicket From [Dv_Plus_Tools_Buss] where SaleCount>0 "&SqlString&" ORDER BY SaleCount Desc"
	'Response.Write Sql
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
	Else
		Response.Write "<tr><td class=""Tablebody1"" colspan=""6"" align=""center"">道具还未添加！</td></tr></table>"
		Exit Sub
	End If
	Rs.close:Set Rs = Nothing
	
	'输出道具列表
	For i=0 To Ubound(SQL,2)
	ToolsPrice = "金币：<font color="& Dvbbs.mainsetting(1) &">"& SQL(8,i) &"</font> + 点券：<font color="& Dvbbs.mainsetting(1) &">"& SQL(9,i) &"</font>"
%>
	<tr>
	<td class="Tablebody1" align="center" height=24><%=SQL(4,i)%></td>
	<td class="Tablebody1" align="center"><%=SQL(2,i)%></td>
	<td class="Tablebody1" align="center"><font color="<%= Dvbbs.mainsetting(1) %>"><%=SQL(8,i)%></font></td>
	<td class="Tablebody1" align="center"><font color="<%= Dvbbs.mainsetting(1) %>"><%=SQL(9,i)%></font></td>
	<td class="Tablebody1" align="center"><%=SQL(6,i)%></td>
	<td class="Tablebody1" align="center"><A href="JavaScript:openScript('plus_Tools_pay.asp?action=BuyUserTools&ToolsID=<%=SQL(3,i)%>&BussID=<%=SQL(0,i)%>',600,450)">购买</A></td>
	</tr>
<%
	Next
	Response.Write "</table>"
	PageSearch=Replace(Replace(PageSearch,"\","\\"),"""","\""")
	Response.Write "<SCRIPT>PageList("&Page&",3,"&MaxRows&","&CountNum&","""&PageSearch&""",1);</SCRIPT>"
End Sub
%>