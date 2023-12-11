<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<%
Dvbbs.LoadTemplates("usermanager")
Dvbbs.Stats=Dvbbs.MemberName&template.Strings(0)
Dvbbs.Nav()
Dvbbs.Head_var 0,0,template.Strings(0),"usermanager.asp"

Dim Sql,Rs,TempStr
If dvbbs.userid=0 Then
	Dvbbs.AddErrCode(6)
	Dvbbs.Showerr()
Else
	If Request("Action") = "CheckUser" Then
		CheckUser()
	Else
		Main()
	End If
End If
Dvbbs.ActiveOnline()
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub Main()
	Response.Write Template.Html(0)
	Dim FoundPassport
	Set Rs=Dvbbs.Execute("Select Passport,IsChallenge From Dv_User Where UserID = " & Dvbbs.UserID)
	If Rs.Eof And Rs.Bof Then
		Rs.Close:Set Rs=Nothing
		Dvbbs.AddErrCode(6)
		Dvbbs.Showerr()
	Else
		If Rs("IsChallenge") = 1 And Rs("Passport") <> "" And Not IsNull(Rs("Passport")) Then
			 FoundPassport = Rs("Passport")
		End If
	End If
	Rs.Close:Set Rs=Nothing
%>
<BR>
<table cellpadding=3 cellspacing=1 align=center class=tableborder1>
<tr><th colspan="2" width="100%">论坛通行证设置</th></tr>
<tr>
<td width="100%" colspan=2 class=tablebody1>
<B>说明</B>：
<li>论坛通行证可让您自由通行于国内大部分的网络论坛，您可以在论坛中设置绑定或解除绑定论坛通行证帐号</li>
<li>如果您尚未绑定论坛通行证，无论您是否注册了论坛通行证，您都可以直接输入论坛通行证帐号，系统将自动引导您完成整个过程</li>
<li>如果您希望更改已绑定论坛通行证，请先解除绑定后进行绑定或注册操作</li>
</td></tr>
<form action="?Action=CheckUser" method=POST name="theForm">
<%
If FoundPassport <> "" Then
%>
<tr>
<td class=tablebody1 width="40%"><B>您的论坛帐号已绑定论坛通行证</B>：</td>
<td class=tablebody1> <%=FoundPassport%>
</td></tr>
<tr align="center">
<td colspan="2" width="100%"  class=tablebody2>
<input type=Submit value="解除论坛通行证绑定" name="Submit">
</td></tr>
<%
Else
%>
<tr>
<td class=tablebody1 width="40%"><B>请输入论坛通行证帐号</B>：</td>
<td class=tablebody1> <input type="text" name="passport" value="" size=30 maxlength=13>
</td></tr>
<tr align="center">
<td colspan="2" width="100%"  class=tablebody2>
<input type=Submit value="论坛通行证绑定或注册" name="Submit">
</td></tr>
<%
End If
%>
</form></table>
<%
End Sub

Sub CheckUser()
	If Request("Submit") = "解除论坛通行证绑定" Then
		Dvbbs.Execute("Update Dv_User Set Passport='',IsChallenge=0 Where UserID = " & Dvbbs.UserID)
		Dvbbs.Dvbbs_Suc("<li>解除论坛通行证绑定论坛帐号成功！")
	Else
		Response.Redirect "login.asp?action=chk&passport=" & Request("passport")
	End If
End Sub
%>