<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<%
Dvbbs.LoadTemplates("usermanager")
Dvbbs.Stats=Dvbbs.MemberName&"升级成为VIP用户"
Dvbbs.mainsetting(0)="98%"
Dvbbs.Head()
Dvbbs.ErrType = 1	'转到不显示顶部和导航的错误显示页
Page_Main()
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub Page_Main()
	If Dvbbs.userid=0 Then Dvbbs.AddErrCode(6):Dvbbs.Showerr()	'判断用户是否在线。
	If Dvbbs.Master Then
		Response.redirect "showerr.asp?ErrCodes=<li>论坛管理员不需要执行升级操作。&action=NoHeadErr"
	End If
	Select Case Request("action")
	Case "UpVipUser"
		Call UpVipUser()
	Case Else
		Call JoinVip()
	End Select
End Sub

Sub UpVipUser()
	Dim GroupID,Btype,vipmoney,vipticket
	GroupID = Dvbbs.CheckNumeric(Request.Form("vipgroupid"))
	Btype = Dvbbs.CheckNumeric(Request.Form("Btype"))
	vipmoney = Dvbbs.CheckNumeric(Request.Form("vipmoney"))
	vipticket = Dvbbs.CheckNumeric(Request.Form("vipticket"))
	If GroupID = 0 or not (vipmoney>0 Or vipticket>0) Then
		Response.redirect "showerr.asp?ErrCodes=<li>参数错误，请按要求填写后再进行操作。&action=NoHeadErr"
		Exit Sub
	End If
	Dim Rs,Sql,VipGroupSetting,UpSetting
	Dim MustNum,NeedPoint,UpDats,DayStr
	MustNum = 0
	UpDats = 0
	If IsSqlDataBase=1 Then
		DayStr = "day"
	Else
		DayStr = "'d'"
	End If
	Sql = "SELECT UserGroupID,Title,Usertitle,GroupSetting,GroupPic FROM Dv_UserGroups WHERE ParentGID=5 and UserGroupID="&GroupID
	SET Rs = Dvbbs.Execute(SQL)
	If Not Rs.eof Then
		VipGroupSetting = Split(Rs(3),",")
		UpSetting = Split(VipGroupSetting(71),"§") '升级到该组所需金币数 金币数§点券数§有效天数§最低天数
		If Btype=1 Then	'点券支付
			vipmoney = 0
			If Dvbbs.CheckNumeric(UpSetting(3))>0 Then	'当有最低天数限制
				MustNum = Dvbbs.CheckNumeric(UpSetting(3))*Dvbbs.CheckNumeric(UpSetting(1))/Dvbbs.CheckNumeric(UpSetting(2))
				If MustNum>0 Then
					MustNum = cCur(FormatNumber(MustNum,0))
				Else
					Response.redirect "showerr.asp?ErrCodes=<li>您要支付的点券数不能为0，请重新确认后再进行操作。&action=NoHeadErr"
					Exit Sub			
				End If
			End If
			'If Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text<vipticket or vipticket<MustNum Then
			If CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)<vipticket or vipticket<MustNum Then
				Response.redirect "showerr.asp?ErrCodes=<li>您的点券数不足，请重新确认后再进行操作。&action=NoHeadErr"
				Exit Sub
			End If
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text) - vipticket
			UpDats = vipticket*Dvbbs.CheckNumeric(UpSetting(2))/Dvbbs.CheckNumeric(UpSetting(1))
			UpDats = Int(FormatNumber(UpDats,0))
		Else	'金币支付
			vipticket = 0
			If Dvbbs.CheckNumeric(UpSetting(3))>0 Then	'当有最低天数限制
				MustNum = Dvbbs.CheckNumeric(UpSetting(3))*Dvbbs.CheckNumeric(UpSetting(0))/Dvbbs.CheckNumeric(UpSetting(2))
				If MustNum>0 Then
					MustNum = cCur(FormatNumber(MustNum,0))
				Else
					Response.redirect "showerr.asp?ErrCodes=<li>您要支付的金币数不能为0，请重新确认后再进行操作。&action=NoHeadErr"
					Exit Sub			
				End If			
			End If
			If CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)< vipmoney or vipmoney<MustNum Then
				Response.redirect "showerr.asp?ErrCodes=<li>您的金币数不足，请重新确认后再进行操作。&action=NoHeadErr"
				Exit Sub
			End If
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text) - vipmoney
			UpDats = vipmoney*Dvbbs.CheckNumeric(UpSetting(2))/Dvbbs.CheckNumeric(UpSetting(0))
			UpDats = Int(FormatNumber(UpDats,0))
		End If
		If Not IsDate(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@vip_startime").text) Then
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@vip_startime").text = Now()
		End If
		If Not IsDate(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@vip_endtime").text) Then
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@vip_endtime").text = Now()
		End If
		Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@vip_endtime").text = DateAdd("d", UpDats, Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@vip_endtime").text)
		Sql = "UPDATE [Dv_User] Set UserGroupID="&GroupID&",UserClass='"&Dvbbs.Checkstr(Rs(2))&"',TitlePic='"&Dvbbs.Checkstr(Rs(4))&"',UserMoney=UserMoney-"&vipmoney&",UserTicket = UserTicket-"&vipticket
		If Dvbbs.VipGroupUser Then
			Sql = Sql &",Vip_EndTime = Dateadd("&DayStr&","&UpDats&",Vip_EndTime) Where UserID="&Dvbbs.UserID
		Else
			Sql = Sql &",Vip_StarTime = "&SqlNowString&",Vip_EndTime = Dateadd("&DayStr&","&UpDats&","&SqlNowString&")  Where UserID="&Dvbbs.UserID
		End If
		'Response.Write sql
		Dvbbs.Execute(Sql)
		Dim LogMsg
		LogMsg = "恭喜您：操作成功，获得( "&Rs(1)&"--"&Rs(2)&" ) <b>"&UpDats &"</b> 天的使用期限，金币减少<b>"&vipmoney&"</b>,点券减少<b>"&vipticket&"</b>。"
		Call Dvbbs.ToolsLog(0,0,vipmoney,vipticket,6,LogMsg,Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text&"|"&Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)
		Dvbbs.Dvbbs_Suc(LogMsg)
	Else
		Response.redirect "showerr.asp?ErrCodes=<li>参数错误，所选取的VIP用户组不存在，请按要求填写后再进行操作。&action=NoHeadErr"
		Exit Sub
	End If
	Rs.close:Set Rs = Nothing
End Sub

'申请及续费VIP表单
Sub JoinVip()
	Call JS_VipGroupInfo()
	Response.Write template.html(21)
	Call UserInfo()
End Sub

'构造VIP用户组信息JS对象
Sub JS_VipGroupInfo()
	Dim Rs,Sql,VipGroupSetting,i
	Sql = "SELECT UserGroupID,Title,Usertitle,GroupSetting FROM Dv_UserGroups WHERE ParentGID=5"
	SET Rs = Dvbbs.Execute(SQL)
	If Not Rs.eof Then
		SQL=Rs.GetRows(-1)
		Rs.close:Set Rs = Nothing
	Else
		'未添加VIP用户组
		Response.redirect "showerr.asp?ErrCodes=<li>系统还未添加VIP用户组，请联系系统管理员。&action=NoHeadErr"
		Exit Sub
	End If
	Dim VID,VTitle,VUTitle,VMSetting,VTSetting,VSetting
	Dim NMoney,Mdays,Ldays,NTicket
	Response.Write VBNewline
	Response.Write "<SCRIPT LANGUAGE=""JavaScript"">" & VBNewline
	Response.Write "<!--" & VBNewline
	Response.Write "function VipGroupConfig(){" & VBNewline
	For i=0 To Ubound(SQL,2)
		VID = VID & SQL(0,i)
		VTitle = VTitle &""""& Replace(Replace(Replace(Replace(SQL(1,i),"\","\\"),"""","\"""),VbCrLf,""),chr(13),"") &""""
		VUTitle = VUTitle &""""& Replace(Replace(Replace(Replace(SQL(2,i),"\","\\"),"""","\"""),VbCrLf,""),chr(13),"") &""""
		VSetting = Split(SQL(3,i),",")
		VMSetting = Split(VSetting(71),"§") '升级到该组所需金币数 金币数§点券数§有效天数§最低天数
		NMoney = NMoney & VMSetting(0)
		NTicket = NTicket & VMSetting(1)
		Mdays = Mdays & VMSetting(2)
		Ldays = Ldays & VMSetting(3)
		If i<Ubound(SQL,2) Then
			VID = VID & ","
			VTitle = VTitle & ","
			VUTitle = VUTitle & ","
			NMoney = NMoney & ","
			Mdays = Mdays & ","
			Ldays = Ldays & ","
			NTicket = NTicket & ","
		End If
	Next
	Response.Write "this.UserGroupID = ["&VID&"];" & VBNewline
	Response.Write "this.Title = ["&VTitle&"];" & VBNewline
	Response.Write "this.Usertitle = ["&VUTitle&"];" & VBNewline
	Response.Write "this.NMoney = ["&NMoney&"];" & VBNewline
	Response.Write "this.NTicket = ["&NTicket&"];" & VBNewline
	Response.Write "this.days = ["&Mdays&"];" & VBNewline
	Response.Write "this.Ldays = ["&Ldays&"];" & VBNewline
	Response.Write "}" & VBNewline
	Response.Write "//-->" & VBNewline
	Response.Write "</SCRIPT>" & VBNewline
End Sub

'--------------------------------------------------------------------------------
'用户信息
'--------------------------------------------------------------------------------
Sub UserInfo()
	Dim Sql,Rs,UserToolsCount
%>
<div id="GetUserInfo" Style="display:none;">
<table border="0" cellpadding=3 cellspacing=1 align=center class=Tableborder1 Style="Width:100%;">
	<tr>
	<th height=23 >个人资料</th>
	</tr>
	<tr>
	<td align=center class=TableBody1>
	<table border="0" cellpadding=3 cellspacing=1 align=center Style="Width:90%">
	<tr><td class=TableBody2>金币：<B><font color="<%=Dvbbs.mainsetting(1)%>"><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text%></font></B> 个</td></tr>
	<tr><td class=TableBody1>点券：<B><font color="<%=Dvbbs.mainsetting(1)%>"><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text%></font></B> 张</td></tr>
	<tr><td class=TableBody2>金钱：<%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userwealth").text%></td></tr>
	<tr><td class=TableBody1>文章：<%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userpost").text%></td></tr>
	<tr><td class=TableBody2>积分：<%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userep").text%></td></tr>
	<tr><td class=TableBody1>魅力：<%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usercp").text%></td></tr>
	<tr><td class=TableBody2>威望：<%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userpower").text%></td></tr>
	<tr><td class=TableBody2>VIP权限登记时间：<br><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@vip_startime").text%></td></tr>
	<tr><td class=TableBody2>VIP权限截止时间：<br><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@vip_endtime").text%></td></tr>
	<tr><td class=TableBody1></td></tr>
	</table>
	</td>
	</tr>
</table>
</div>
<SCRIPT LANGUAGE="JavaScript">
<!--
document.getElementById("UserInfo").innerHTML = document.getElementById("GetUserInfo").innerHTML;
//-->
</SCRIPT>
<%
End Sub
%>