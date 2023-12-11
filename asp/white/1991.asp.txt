<!--#include file =conn.asp-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="Dv_plus/Tools/plus_Tools_const.asp" -->
<%
Dim Action,SaleUserName,InputDisable,ToolsCount
Dim TheUserToolsNum
SaleUserName = "系统"
InputDisable = " Disabled " 
Action = Trim(Request("action"))
Dvbbs.stats = "论坛道具操作"

Select Case Action
	Case "BuyTools"
		Dvbbs.stats = Dvbbs.stats & "-购买系统道具"
	Case "BuyUserTools"
		Dvbbs.stats = Dvbbs.stats & "-购买用户道具"
	Case "SellTools"
		Dvbbs.stats = Dvbbs.stats & "-出售道具"
	Case "SaveBuyTools","SaveSellTools"
		Dvbbs.stats = Dvbbs.stats & "-保存道具操作"
End Select

Dvbbs.LoadTemplates("")
Dvbbs.head()
Dv_Tools.ChkToolsLogin

'若是用户购买或转让，更改道具价格为用户自定义价格
If Request("BussID")<>"" and IsNumeric(Request("BussID")) Then
	Dim Rs,Sql,i,BussID,SaleUserID,SaleToolsID
	BussID = Dv_Tools.CheckNumeric(Request("BussID"))
	Sql = "Select ToolsCount,SaleCount,SaleMoney,SaleTicket,UserID,UserName,ToolsID From [Dv_Plus_Tools_Buss] Where ID="& BussID
	Set Rs = Dvbbs.Plus_Execute(Sql)
	If Rs.Eof Then
		Dv_Tools.ShowErr(3)
	Else
		ToolsCount = Clng(Rs(0))
		Dv_Tools.ToolsInfo(4) = Clng(Rs(1))
		Dv_Tools.ToolsInfo(6) = Clng(Rs(2))
		Dv_Tools.ToolsInfo(13) = Clng(Rs(3))
		SaleUserID = Clng(Rs(4))
		SaleUserName = Dvbbs.iHtmlEnCode(Rs(5))
		SaleToolsID = Clng(Rs(6))
	End If
	Rs.Close : Set Rs = Nothing
End If
If Action = "SellTools" Then InputDisable = ""

'相关执行信息
Select Case Action
	Case "BuyTools","BuyUserTools","SellTools"
		'道具信息
		ToolsInfo()
		BuyTools
	Case "SaveBuyTools"
		SaveBuyTools
		'道具信息
		ToolsInfo()
	Case "SaveSellTools"
		SaveSellTools
		'道具信息
		ToolsInfo()
	Case "SaveBuyUserTools"
		SaveBuyUserTools
		'道具信息
		ToolsInfo()
End Select

Dvbbs.mainsetting(0)="98%"
Dvbbs.Footer()
Dvbbs.PageEnd()
'道具信息
Sub ToolsInfo()
If Dv_Tools.ToolsInfo(15)="" Then Dv_Tools.ToolsInfo(15)="Dv_plus/Tools/pic/None.jpg"
Set Rs = Dvbbs.Plus_Execute("Select ToolsCount,SaleCount From [Dv_Plus_Tools_Buss] Where UserID="& Dvbbs.UserID &" and ToolsID="& Dv_Tools.ToolsID)
If Rs.Eof And Rs.Bof Then
	TheUserToolsNum = 0
Else
	TheUserToolsNum = Rs(0) + Rs(1)
End If
Rs.Close
Set Rs=Nothing
%>
<table border="0" cellpadding=3 cellspacing=1 align=center class=Tableborder1 Style="Width:99%">
<form name=PlusTools method=post>
    <tr>
      <th height="23" colspan="3"><%=Dv_Tools.ToolsInfo(1)%> -- 道具信息</th>
    </tr>
    <tr>
      <td width="30%" rowspan="18" class="tablebody1" align=center>
	  <font size=6><b><%=Dv_Tools.ToolsInfo(1)%></b></font>
	  <br><img src="<%=Dv_Tools.ToolsInfo(15)%>" border=0>
	  </td>
      <td width="70%" height="20" class="tablebody1" colspan="2">道具说明：<hr style="BORDER: #807d76 1px dotted;height:1px;"><%=Dv_Tools.ToolsInfo(2)%></td>
    </tr>
	<tr>
      <th height="23" class="tablebody1" colspan="2">购买说明</th>
    </tr>
	<tr>
      <td width="30%" height="20" class="tablebody1" align=Right>需要金币：</td>
      <td width="40%" class="tablebody1"><font color="<%= Dvbbs.mainsetting(1) %>"><B><%=Dv_Tools.ToolsInfo(6)%></B></font></td>
    </tr>
	<tr>
      <td height="20" class="tablebody1" align=Right>需要点券：</td>
      <td class="tablebody1"><font color="<%= Dvbbs.mainsetting(1) %>"><B><%=Dv_Tools.ToolsInfo(13)%></B></font></td>
    </tr>
    <tr>
      <td height="20" class="tablebody1" align=Right>购买方式：</td>
      <td class="tablebody1">
	  	<%
		If Dv_Tools.ToolsInfo(4)<=0 Then
			Response.Write "暂停购买"
		Else
			Response.Write Dv_Tools.BuyType(Dv_Tools.ToolsInfo(14))
		End IF
		%>
		</td>
    </tr>
    <tr>
      <td height="23" class="tablebody1" align=Right>可购买道具数量：</td>
      <td class="tablebody1"><B><%=Dv_Tools.ToolsInfo(4)%></B></td>
    </tr>
	<tr>
      <th height="23" class="tablebody1" colspan="2">使用限制</th>
    </tr>
    <tr>
      <td height="23" class="tablebody1" align=Right>使用用户帖子数至少：</td>
      <td class="tablebody1"><%=Dv_Tools.ToolsInfo(7)%></td>
    </tr>
    <tr>
      <td height="23" class="tablebody1" align=Right>使用用户金钱数至少：</td>
      <td class="tablebody1"><%=Dv_Tools.ToolsInfo(8)%></td>
    </tr>
	<tr>
      <td height="23" class="tablebody1" align=Right>使用用户积分值至少：</td>
      <td class="tablebody1"><%=Dv_Tools.ToolsInfo(9)%></td>
    </tr>
	<tr>
      <td height="23" class="tablebody1" align=Right>使用用户魅力值至少：</td>
      <td class="tablebody1"><%=Dv_Tools.ToolsInfo(10)%></td>
    </tr>
    <tr>
      <td height="23" class="tablebody1" align=Right>目标用户帖子数至少：</td>
      <td class="tablebody1"><%=Dv_Tools.ToolsSetting(0)%></td>
    </tr>
    <tr>
      <td height="23" class="tablebody1" align=Right>目标用户金钱数至少：</td>
      <td class="tablebody1"><%=Dv_Tools.ToolsSetting(1)%></td>
    </tr>
	<tr>
      <td height="23" class="tablebody1" align=Right>目标用户积分值至少：</td>
      <td class="tablebody1"><%=Dv_Tools.ToolsSetting(2)%></td>
    </tr>
	<tr>
      <td height="23" class="tablebody1" align=Right>目标用户魅力值至少：</td>
      <td class="tablebody1"><%=Dv_Tools.ToolsSetting(3)%></td>
    </tr>
	<tr>
      <td height="23" class="tablebody1" align=Right>允许使用的用户组或等级：</td>
      <td class="tablebody1">
	  <Select Name="ToolsGroupID" Size=1>
<%
	Set Rs=Dvbbs.Execute("Select UserGroupID,UserTitle From Dv_UserGroups Where UserGroupID In ("&Dv_Tools.ToolsInfo(11)&") Order By UserGroupID")
	If Rs.Eof And Rs.Bof Then
		Response.Write "<option value=0>没有用户可使用此道具</option>"
	End If
	Do While Not Rs.Eof
		Response.Write "<option value="&Rs(0)&">"&Server.HtmlEncode(Rs(1))&"</option>"
	Rs.MoveNext
	Loop
	Rs.Close
	Set Rs=Nothing
%>
	  </Select>
	  <!--<INPUT TYPE="hidden" NAME="ToolsGroupID" value="">
	  <input type="button" value="详细查看" onclick="PlusOpen('plus_Tools_InfoSetting.asp?orders=0&id=<%=Dv_Tools.ToolsID%>',650,500)">--></td>
    </tr>
	<tr>
      <td height="23" class="tablebody1" align=Right>允许使用的版块：</td>
      <td class="tablebody1">
	  <Select Name=ToolsBoardID Size=1>
<%
	Dim ToolsBoardList,ii,iii
	ii = 0
	ToolsBoardList="," & Dv_Tools.ToolsInfo(12) & ","
	Dim Nodelist,Node
	Set Nodelist=Application(Dvbbs.CacheName&"_boardlist").cloneNode(True).documentElement.getElementsByTagName("board")
	For Each Node in Nodelist
		If InStr(ToolsBoardList,"," & Node.attributes.getNamedItem("boardid").text & ",") > 0 Then
			ii = ii + 1
			Response.Write "<option value=" & Node.attributes.getNamedItem("boardid").text & ">"
			Select Case Clng(Node.attributes.getNamedItem("depth").text)
			Case 0
				Response.Write "╋"
			Case 1
				Response.Write "&nbsp;&nbsp;├"
			End Select
			If Clng(Node.attributes.getNamedItem("depth").text)>1 Then
				For ii=2 To Clng(Node.attributes.getNamedItem("depth").text)
					Response.Write "&nbsp;&nbsp;│"
				Next
				Response.Write "&nbsp;&nbsp;├"
			End If
			Response.Write " " & Node.attributes.getNamedItem("boardtype").text & "</option>"
		End If
	Next
	If ii = 0 Then Response.Write "<option value=0>没有版面可使用此道具</option>"
%>
	  </Select>
	  <!--<INPUT TYPE="hidden" NAME="ToolsBoardID" value="">
	  <input type="button" value="详细查看" onclick="PlusOpen('plus_Tools_InfoSetting.asp?orders=1&id=<%=Dv_Tools.ToolsID%>',650,500)">--></td>
    </tr>
    <tr>
      <td height="23" colspan="2" class=Tablebody2>
	  </td>
    </tr>
</form>
</table>
<%
End Sub

'---------------------------------------------------------------
'道具购买
'---------------------------------------------------------------
Sub BuyTools()
Dim ReAction,ActName
Select Case Action
	Case "BuyTools"
		ReAction = "SaveBuyTools"
		ActName = "购买"
	Case "BuyUserTools"
		ReAction = "SaveBuyUserTools"
		ActName = "购买"
	Case "SellTools"
		ReAction = "SaveSellTools"
		ActName = "转让"
End Select

%>
<form name=PlusTools action="?action=<%=ReAction%>" method=post>
<table border="0" cellpadding=3 cellspacing=1 align=center class=Tableborder1 Style="Width:99%">
    <tr>
      <th height="23" colspan="2">道具交易操作</th>
    </tr>
	<tr>
		<td height="23" class="tablebody1" colspan=2>
	  您目前有 <B><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text%></B> 个金币和 <B><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text%></B> 张点券，拥有该道具 <B><%=TheUserToolsNum%></B> 个</td>
    </tr>
	<%
	If Action = "BuyTools" Then
	%>
	<tr>
		<td height="23" class="tablebody1" width="30%">购买方式：</td>
		<td class="tablebody1">
	  	<%
		If Dv_Tools.ToolsInfo(4)<=0 Then
			Response.Write "暂停购买"
		Else
			Response.Write Dv_Tools.BuyType(Dv_Tools.ToolsInfo(14))
		End IF
		%>
		</td>
    </tr>
	<%
	End If
	%>
	<tr>
		<td height="23" class="tablebody1" width="30%">出售方：</td>
		<td class="tablebody1"><%=SaleUserName%></td>
    </tr>
    <tr>
		<td height="23" class="tablebody1" width="30%"><%=ActName%>数量：</td>
		<td class="tablebody1">
		<INPUT TYPE="Text" name="ToolsSum" value="1"><%'=Dv_Tools.ToolsInfo(4)%>
		</td>
    </tr>

    <tr>
		<td height="23" class="tablebody1" width="30%"><%=ActName%>需要金币单价：</td>
		<td class="tablebody1">
		<INPUT TYPE="Text" name="ToolsMoney" value="<%=Dv_Tools.ToolsInfo(6)%>"<%=InputDisable%>>
		</td>
    </tr>
    <tr>
		<td height="23" class="tablebody1" width="30%"><%=ActName%>需要点券单价：</td>
		<td class="tablebody1">
		<INPUT TYPE="Text" name="ToolsTicket" value="<%=Dv_Tools.ToolsInfo(13)%>"<%=InputDisable%>>
		</td>
    </tr>
    <tr>
		<td height="23" class="tablebody1" width="30%">交易支付方式：</td>
		<td class="tablebody1">
<%
Select Case Action
	Case "BuyTools"
%>
		<SELECT NAME="BuyType">
		<option value="0"<%If Cint(Dv_Tools.ToolsInfo(14))=0 Then%> Selected<%End If%>>金币
		<option value="1"<%If Cint(Dv_Tools.ToolsInfo(14))=1 or Cint(Dv_Tools.ToolsInfo(14))=3 Then%> Selected<%End If%>>点券
		<option value="2"<%If Cint(Dv_Tools.ToolsInfo(14))=2 Then%> Selected<%End If%>>金币+点券
		</option>
		</SELECT>
<%
	Case "BuyUserTools"
		If Clng(Dv_Tools.ToolsInfo(6))>0 And Clng(Dv_Tools.ToolsInfo(13))=0 Then
			Response.Write "购买此用户转让的道具需要花费您 <B>"&Dv_Tools.ToolsInfo(6)&"</B> 个金币"
		ElseIf Clng(Dv_Tools.ToolsInfo(13))>0 And Clng(Dv_Tools.ToolsInfo(6))=0 Then
			Response.Write "购买此用户转让的道具需要花费您 <B>"&Dv_Tools.ToolsInfo(13)&"</B> 张点券"
		ElseIf Clng(Dv_Tools.ToolsInfo(13))>0 And Clng(Dv_Tools.ToolsInfo(6))>0 Then
			Response.Write "购买此用户转让的道具需要同时花费您 <B>"&Dv_Tools.ToolsInfo(6)&"</B> 个金币和 <B>"&Dv_Tools.ToolsInfo(13)&"</B> 张点券"
		End If
	Case "SellTools"
		Response.Write "发布转让信息，填写金币或点券数值则使用金币或点券都能购买，如果两者都填写则购买用户必须同时支付相应的金币和点券才能购买"
End Select
%>
		</td>
    </tr>
	<tr><td height="23" colspan="2" class=Tablebody2 align=center>
	<INPUT TYPE="submit" value="决定<%=ActName%>">
	<INPUT TYPE="hidden" name="ToolsID" value="<%=Dv_Tools.ToolsID%>">
	<INPUT TYPE="hidden" name="BussID" value="<%=BussID%>">
	</td></tr>
</table>
</form>
<%
End Sub

'---------------------------------------------------------------
'保存道具购买（与系统交易）
'---------------------------------------------------------------
Sub SaveBuyTools()
	If Not Dvbbs.ChkPost Then
		Dvbbs.AddErrCode(42)
		Dvbbs.Showerr()
		Exit Sub
	End If
	Dim ToolsSum,BuyType,SucMsg
	Dim ToolsMoney,ToolsTicket
	Dv_Tools.ChkUserGroup
	ToolsSum = Dv_Tools.CheckNumeric(Request.Form("ToolsSum"))
	BuyType = Request.Form("BuyType")
	If Clng(Dv_Tools.ToolsInfo(4))<=0 Then
		Dv_Tools.ShowErr(4)
		Exit Sub
	End If

	If ToolsSum<0 Then ToolsSum=0
	If ToolsSum>10 Then
		Response.redirect "showerr.asp?ErrCodes=<li>系统设置每次最多只能购买10个！&action=NoHeadErr"
		Exit Sub
	End If
	Dv_Tools.BuySum = ToolsSum		'设置购买数据
	Dv_Tools.ChkBuyTools(BuyType)	'验证购买权限
	
	ToolsMoney = Int(Dv_Tools.ToolsInfo(6))*ToolsSum
	ToolsTicket = Int(Dv_Tools.ToolsInfo(13))*ToolsSum
	If ToolsMoney<0 Then ToolsMoney=0
	If ToolsTicket<0 Then ToolsTicket=0
	'保存购买道具
	Set Rs = Dvbbs.iCreateObject("adodb.recordset")
	Sql = "Select * From [Dv_Plus_Tools_Buss] where UserID="& Dvbbs.UserID &" and ToolsID="& Dv_Tools.ToolsID
	Dvbbs.SqlQueryNum=Dvbbs.SqlQueryNum+1
	If Cint(Dvbbs.Forum_Setting(92))=1 Then
		If Not IsObject(Plus_Conn) Then Plus_ConnectionDatabase
		Rs.Open Sql,Plus_Conn,1,3
	Else
		If Not IsObject(Conn) Then ConnectionDatabase
		Rs.Open Sql,conn,1,3
	End If
	If Rs.eof and Rs.bof then
		Rs.addnew
		Rs("UserName") = Dvbbs.Membername
		Rs("ToolsName") = Dv_Tools.ToolsInfo(1)
		Rs("UserID") = Dvbbs.UserID
		Rs("ToolsID") = Dv_Tools.ToolsID
		Rs("ToolsCount") = ToolsSum
	Else
		Rs("ToolsCount") = Rs("ToolsCount")+ToolsSum
	End If
	Rs.Update
	Rs.Close
	Set Rs = Nothing
	'减少系统库存和增加用户库存
	Dvbbs.Plus_Execute("UPDATE Dv_Plus_Tools_Info Set SysStock = SysStock-"& ToolsSum &",UserStock=UserStock+"& ToolsSum &" where ID="&Dv_Tools.ToolsID)
	'更新用户当前信息
	If Cint(Dv_Tools.ToolsInfo(14))=3 Then
		If BuyType = 0 Then
			ToolsTicket = 0
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)-ToolsMoney
		ElseIf BuyType = 1 Then
			ToolsMoney = 0
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)-ToolsTicket
		Else
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)-ToolsMoney
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)-ToolsTicket
		End IF
	Else
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)-ToolsMoney
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)-ToolsTicket
	End If

	Dvbbs.Execute("UPDATE Dv_User Set UserMoney = "& Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text &",UserTicket="& Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text &" where UserID="& Dvbbs.UserID)
	'插入事件记录
	'---------------------------------------------------------------
	SucMsg = "向系统购买道具："&Dv_Tools.ToolsInfo(1)&",数量：<b>"&ToolsSum&"</b>,花费金币："&ToolsMoney&"，花费点券："&ToolsTicket&"。"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,ToolsSum,ToolsMoney,ToolsTicket,4,SucMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	'---------------------------------------------------------------
	SucMsg = SucMsg & " 道具购买成功！"
	Dvbbs.Dvbbs_Suc(SucMsg)
End Sub

'---------------------------------------------------------------
'保存道具出售(转让)
'---------------------------------------------------------------
Sub SaveSellTools()
	If Not Dvbbs.ChkPost Then
		Dvbbs.AddErrCode(42)
		Dvbbs.Showerr()
		Exit Sub
	End If
	Dv_Tools.ChkUserGroup
	Dim ToolsSum,ToolsMoney,ToolsTicket,UpToolsCount,UpSaleCount,SucMsg
	ToolsSum = Dv_Tools.CheckNumeric(Request.Form("ToolsSum"))
	ToolsMoney = Dv_Tools.CheckNumeric(Request.Form("ToolsMoney"))
	ToolsTicket = Dv_Tools.CheckNumeric(Request.Form("ToolsTicket"))
	If ToolsSum<0 Then ToolsSum=0
	If ToolsMoney<0 Then ToolsMoney=0
	If ToolsTicket<0 Then ToolsTicket=0
	If ToolsTicket=0 And ToolsMoney=0 Then Dv_Tools.ShowErr(16):Exit Sub

	Dv_Tools.ToolsInfo(4) = Clng(Dv_Tools.ToolsInfo(4))
	If ToolsCount<ToolsSum or ToolsSum=0 Then Dv_Tools.ShowErr(9):Exit Sub

	If Dv_Tools.ToolsInfo(4)>0 Then
		If Dv_Tools.ToolsInfo(4)<ToolsSum Then
			UpToolsCount = ToolsCount-(ToolsSum-Dv_Tools.ToolsInfo(4))
		Else
			UpToolsCount = ToolsCount+(Dv_Tools.ToolsInfo(4)-ToolsSum)
		End If
		UpSaleCount = ToolsSum
	Else
		UpToolsCount = ToolsCount-ToolsSum
		UpSaleCount = Dv_Tools.ToolsInfo(4)+ToolsSum
	End If
	
	Dvbbs.Plus_Execute("UPDATE [Dv_Plus_Tools_Buss] Set ToolsCount = "& UpToolsCount &",SaleCount="& UpSaleCount &",SaleMoney="& ToolsMoney &",SaleTicket="& ToolsTicket &" where ID="& BussID)
	
	'插入事件记录
	'---------------------------------------------------------------
	SucMsg = "转让道具："&Dv_Tools.ToolsInfo(1)&",数量：<b>"&ToolsSum&"</b>。"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,ToolsSum,ToolsMoney,ToolsTicket,2,SucMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	'---------------------------------------------------------------
	SucMsg = SucMsg & " 道具转让成功！"
	Dvbbs.Dvbbs_Suc(SucMsg)
	'---------------------------------------------------------------
End Sub

'---------------------------------------------------------------
'保存道具购买（用户间交易）
'---------------------------------------------------------------
Sub SaveBuyUserTools()
	If Not Dvbbs.ChkPost Then
		Dvbbs.AddErrCode(42)
		Dvbbs.Showerr()
		Exit Sub
	End If
	Dv_Tools.ChkUserGroup
	Dim ToolsSum,ToolsMoney,ToolsTicket,UpToolsCount,UpSaleCount,BuyType,SucMsg
	Dv_Tools.ChkUserGroup
	ToolsSum = Dv_Tools.CheckNumeric(Request.Form("ToolsSum"))
	BuyType = Dv_Tools.CheckNumeric(Request.Form("BuyType"))
	If ToolsSum<0 Then ToolsSum=0
	If Int(Dv_Tools.ToolsInfo(4)) = 0 or ToolsSum>Int(Dv_Tools.ToolsInfo(4)) OR ToolsSum = 0 Then Dv_Tools.ShowErr(8):Exit Sub '库存不足
	ToolsMoney = Dv_Tools.ToolsInfo(6)*ToolsSum
	ToolsTicket = Dv_Tools.ToolsInfo(13)*ToolsSum
	If ToolsMoney<0 Then ToolsMoney=0
	If ToolsTicket<0 Then ToolsTicket=0
	If ToolsMoney = 0 And ToolsTicket = 0 Then Dv_Tools.ShowErr(7):Exit Sub

	'判断用户是否具有购买权限
	If SaleUserID<>Dvbbs.UserID Then
		If CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)<ToolsMoney Or CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)<ToolsTicket Then Dv_Tools.ShowErr(7):Exit Sub
	Else
		Dvbbs.Plus_Execute("UPDATE [Dv_Plus_Tools_Buss] Set ToolsCount = ToolsCount+"& ToolsSum &",SaleCount=SaleCount-"& ToolsSum &" where ID="& BussID)
		'插入事件记录
		'---------------------------------------------------------------
		SucMsg = "与自已购回道具："&Dv_Tools.ToolsInfo(1)&",数量：<b>"&ToolsSum&"</b>。"
		Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,ToolsSum,ToolsMoney,ToolsTicket,4,SucMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
		'---------------------------------------------------------------
		SucMsg = SucMsg & "道具信息已更新。"
		Dvbbs.Dvbbs_Suc(SucMsg)
		Exit Sub
	End If

	'更新卖方数据(减少售出数量)
	Dvbbs.Plus_Execute("UPDATE [Dv_Plus_Tools_Buss] Set SaleCount=SaleCount-"& ToolsSum &" where ID="& BussID)
	Dvbbs.Execute("UPDATE Dv_User Set UserMoney = UserMoney+"& ToolsMoney &",UserTicket=UserTicket+"& ToolsTicket &" where UserID="& SaleUserID)
	'更新买方数据(减少售出数量)
	'保存购买道具(若未找到道具添加新的记录，已有道具只需更新个人库存)
	Set Rs = Dvbbs.iCreateObject("adodb.recordset")
	Sql = "Select * From [Dv_Plus_Tools_Buss] where UserID="& Dvbbs.UserID &" and ToolsID="& Dv_Tools.ToolsID
	If Cint(Dvbbs.Forum_Setting(92))=1 Then
		If Not IsObject(Plus_Conn) Then Plus_ConnectionDatabase
		Rs.Open Sql,Plus_Conn,1,3
	Else
		If Not IsObject(Conn) Then ConnectionDatabase
		Rs.Open Sql,conn,1,3
	End IF
	If Rs.eof and Rs.bof then
		Rs.addnew
		Rs("UserName") = Dvbbs.Membername
		Rs("ToolsName") = Dv_Tools.ToolsInfo(1)
		Rs("UserID") = Dvbbs.UserID
		Rs("ToolsID") = Dv_Tools.ToolsID
		Rs("ToolsCount") = ToolsSum
	Else
		Rs("ToolsCount") = Rs("ToolsCount")+ToolsSum
	End If
	Rs.Update
	Rs.Close : Set Rs = Nothing
	'更新用户当前信息
	'If Cint(Dv_Tools.ToolsInfo(14))=3 Then
	'	If BuyType = 0 Then
	'		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)-ToolsMoney
	'	ElseIf BuyType = 1 Then
	'		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)-ToolsTicket
	'	Else
	'		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)-ToolsMoney
	'		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)-ToolsTicket
	'	End IF
	'Else
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text) - ToolsMoney
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text) - ToolsTicket
	'End If

	Dvbbs.Execute("UPDATE Dv_User Set UserMoney = "& Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text &",UserTicket="& Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text &" where UserID="& Dvbbs.UserID)
	'插入事件记录
	'---------------------------------------------------------------
	SucMsg = "向"&SaleUserName&"购买道具："&Dv_Tools.ToolsInfo(1)&",数量：<b>"&ToolsSum&"</b>,花费金币："&ToolsMoney&"，花费点券："&ToolsTicket&"。"
	Call Dvbbs.ToolsLog(Dv_Tools.ToolsID,ToolsSum,ToolsMoney,ToolsTicket,4,SucMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
	'---------------------------------------------------------------
	SucMsg = SucMsg & "道具信息已更新。"
	Dvbbs.Dvbbs_Suc(SucMsg)
	'---------------------------------------------------------------
End Sub

%>