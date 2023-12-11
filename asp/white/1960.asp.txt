<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/dv_clsother.asp"-->
<%
Dvbbs.LoadTemplates("")
If Dvbbs.UserID=0 Then
	Dvbbs.AddErrCode(24)
	Dvbbs.showerr()
Else
	If Dvbbs.Master Then
		Response.redirect "index.asp?boardid="&Dvbbs.Boardid
	End If
End If
dvbbs.stats="交费进入认证论坛"
Dvbbs.Nav()
Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
Dvbbs.Showerr()
Select Case request("action")
	Case "subinfo"
		subinfo()
	Case Else
		Main()
End Select
Dvbbs.activeonline()
Dvbbs.footer()
Dvbbs.PageEnd()
Sub Main()
Dim UseMondy,UseTicket
UseMondy = Dvbbs.Board_Setting(62)
UseTicket = Dvbbs.Board_Setting(63)
If Dvbbs.VipGroupUser Then
	UseMondy = UseMondy * Dvbbs.Board_Setting(66)
	UseTicket = UseTicket * Dvbbs.Board_Setting(66)
End If
%>
<table cellpadding=3 cellspacing=1 align=center class=tableborder1>
<form action="pay_boardlimited.asp?action=subinfo&boardid=<%=Dvbbs.BoardID%>" method=post>
<tr><th align=center colspan=2>付费进入认证论坛</td></tr>
<tr><td class=tablebody1 colspan=2 height=24>访问该版面规则为：获得 <B><%=Dvbbs.Board_Setting(64)%></B> 个月的访问权限将花费您 <B><%=UseMondy%></B> 个金币 或 <B><%=UseTicket%></B> 张点券。
</td></tr>
<tr><td class=tablebody1 colspan=2 height=24>您目前共有 金币 <B><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text%></B> 个 ＋ 点券 <B><%=Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text%></B> 张 的货币单位可供支付 （<a href="UserPay.asp?action=UserCenter" target="_blank"><font color=red>兑换论坛金币</font></a> | <a href="UserPay.asp" target="_blank"><font color=red>购买论坛点券</font></a> | <a href="plus_Tools_SmsPay.asp"><font color=red>点播手机短信获奖论坛点券</font></a>）</td></tr>
<tr><td class=tablebody1 colspan=2 height=24 style="line-height : 15px"><B>购买操作说明</B>：<BR>
1、只需要金币购买的，则会扣除您相应的金币，不够则不能操作<BR>
2、只需要点券购买的，则会扣除您相应的点券，点券充值请看<a href="plus_Tools_Center.asp">道具中心</a>，不够则不能操作<BR>
3、金币和点券都能购买的，优先扣除相应的点券数，如果不够则从您的金币中扣除，如果两者均不够则不能操作<BR>
<%If Dvbbs.forum_setting(43)="1" Then%>
4、Vip用户只需花费 <B><font color=red><%=Dvbbs.Board_Setting(62)*Dvbbs.Board_Setting(66)%></font></B> 个金币 或 <B><font color=red><%=Dvbbs.Board_Setting(63)*Dvbbs.Board_Setting(66)%></font></B> 张点券 （<a href="JoinVipGroup.asp" target="_blank"><font color=red>现以升级成为VIP用户</font></a>）
<%End If%>
</td></tr>
<tr><td align=center class=tablebody2 colspan=2><input type=submit value="确认支付"></td></tr>
</form>
</table>
<%
End Sub

Sub Subinfo()
	Dim GetUserMoney,GetUserTicket,iUserInfo,ChkPoint
	GetUserMoney = 0
	GetUserTicket = 0
	GetUserMoney = Abs(Clng(Dvbbs.Board_Setting(62)))
	GetUserTicket = Abs(Clng(Dvbbs.Board_Setting(63)))
	ChkPoint = False
	If Dvbbs.VipGroupUser Then
		GetUserMoney =GetUserMoney * cCur(Dvbbs.Board_Setting(66))
		GetUserTicket = GetUserTicket * cCur(Dvbbs.Board_Setting(66))
	End If
	If  CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text) <  GetUserTicket Then
		Response.redirect "showerr.asp?ErrCodes=<li>您的点券数目不够，不能购买进入论坛服务&action=OtherErr"
	ElseIf CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text) < GetUserMoney Then
		Response.redirect "showerr.asp?ErrCodes=<li>您的金币数目不够，不能购买进入论坛服务&action=OtherErr"
	Else
	If GetUserTicket >0 Then
		If CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text)-GetUserTicket>=0 Then
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userticket").text) - GetUserTicket
		End If
	End If
	If GetUserMoney >0 Then
		If CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text)-GetUserMoney>=0 Then
			Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text = CCur(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@usermoney").text) - GetUserMoney
		End If
	End If
	Add_SuperBoardUser(Dvbbs.MemberName)
	Dvbbs.Execute("Update Dv_User Set UserMoney = UserMoney - "&GetUserMoney&",UserTicket = UserTicket - "&GetUserTicket&" Where UserID=" & Dvbbs.UserID)
	%>
	<table cellpadding=3 cellspacing=1 align=center class=tableborder1>
	<tr><th align=center colspan=2>付费进入版面操作成功</td></tr>
	<tr><td class=tablebody1 colspan=2 height=24>您支付了 <B><%=GetUserMoney%></B> 个金币 或 <B><%=GetUserTicket%></B> 张点券，获得 <%=Dvbbs.BoardType%> 版面截止到 <B><%=DateAdd("m",1,Now())%></B> 的访问权限</td></tr>
	<tr><td class=tablebody1 colspan=2 height=24><li><a href="list.asp?boardid=<%=Dvbbs.BoardID%>">进入<%=Dvbbs.BoardType%></a></td></tr>
	</table>
<%
End If
End Sub

Function Add_SuperBoardUser(UserName)
	Dim Rs,SuperBoardUser,SuperBoardUser_List,SuperBoardUser_List_A,i
	Dim BoardUser
	UserName = Replace(UserName,",","")
	If Not Application(Dvbbs.CacheName &"_boarddata_" & Dvbbs.Boardid).documentElement.selectSingleNode("boarddata/@boarduser") Is Nothing Then boarduser=Application(Dvbbs.CacheName &"_boarddata_" & Dvbbs.Boardid).documentElement.selectSingleNode("boarddata/@boarduser").text
	If BoardUser="" Then
		SuperBoardUser = UserName & "=" & Now()
	Else
		'清除该用户原购买信息
		BoardUser=Split(BoardUser,",")
		For i=0 To Ubound(BoardUser)
			SuperBoardUser_List_A = Split(BoardUser(i),"=")
			If Trim(Lcase(SuperBoardUser_List_A(0))) <> Trim(Lcase(UserName)) Then
				If i=0 Then
					SuperBoardUser = BoardUser(i)
				Else
					SuperBoardUser = SuperBoardUser & "," & BoardUser(i)
				End If
			End If
		Next
		If SuperBoardUser<>"" Then
			SuperBoardUser = SuperBoardUser & "," & UserName & "=" & Now()
		Else
			SuperBoardUser = UserName & "=" & Now()
		End If
	End If
	Dvbbs.Execute("Update Dv_Board Set BoardUser='"&Replace(SuperBoardUser,"'","")&"' Where BoardID=" & Dvbbs.BoardID)
	Dvbbs.LoadBoardData Dvbbs.Boardid
End Function
%>