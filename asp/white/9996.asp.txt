<!--#include file="inc/conn.asp" -->
<!--#include file="inc/Skin_css.asp"-->
<%
dim Action,ID,VoteType,VoteOption,sqlVote,rsVote
Action=trim(Request("Action"))
ID=Trim(request("ID"))
VoteType=Trim(request("VoteType"))
VoteOption=trim(request("VoteOption"))
If Action = "Vote" And Id<> "" And VoteOption<>""  Then
	if VoteType="Single" then
		conn.execute "Update Vote set answer" & VoteOption  & "= answer" & VoteOption & "+1 where ID=" & ID
	else
		dim arrOptions
		if instr(VoteOption,",")>0 then
			arrOptions=split(VoteOption,",")
			dim i
			for i=0 to ubound(arrOptions)
				conn.execute "Update Vote set answer" & cint(trim(arrOptions(i)))  & "= answer" & cint(trim(arrOptions(i))) & "+1 where ID=" & Clng(ID)
			next
		else
			conn.execute "Update Vote set answer" & VoteOption  & "= answer" & VoteOption & "+1 where ID=" & Clng(ID)
		end if
	end if
	session("Voted")="True"
End If
if ID<>"" then
	sqlVote="Select * from Vote Where ID=" & Clng(ID)
else
	sqlVote="select top 1 * from Vote order by ID desc"
end if
Set rsVote = Server.CreateObject("ADODB.Recordset")
rsVote.open sqlVote,conn,1,1
%>

<HTML>
<HEAD>
<TITLE>调查结果</TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<style type="text/css">
A:link    {	 color: #333333;TEXT-DECORATION: none	}
A:visited {	 color: #333333;TEXT-DECORATION: none	}
A:active  {	 color: #003300;TEXT-DECORATION: none	}
A:hover   {	 color: #003300;TEXT-DECORATION: underline overline	}
.navtrail {  COLOR: #eeeeee; FONT-SIZE: 12px; LINE-HEIGHT: 12px }
A.navtrail:link {  COLOR: #eeeeee; CURSOR: w-resize }
A.navtrail:visited {  COLOR: #eeeeee; CURSOR: w-resize }
A.navtrail:active {  COLOR: #eeeeee; CURSOR: w-resize }
A.navtrail:hover {  COLOR: #eeeeee; CURSOR: e-resize }
INPUT{BORDER-TOP-WIDTH: 1px; PADDING-RIGHT: 1px; PADDING-LEFT: 1px; BORDER-LEFT-WIDTH: 1px; FONT-SIZE: 9pt; BORDER-LEFT-COLOR: #cccccc; BORDER-BOTTOM-WIDTH: 1px; BORDER-BOTTOM-COLOR: #cccccc; PADDING-BOTTOM: 1px; BORDER-TOP-COLOR: #cccccc; PADDING-TOP: 1px; HEIGHT: 18px; BORDER-RIGHT-WIDTH: 1px; BORDER-RIGHT-COLOR: #cccccc}
<!--
td {  font-family: "宋体"; font-size: 9pt; color: #333333; text-decoration: none}
-->
</style>
</HEAD>
<BODY leftmargin="0" topmargin="0" marginwidth="0" marginheight="0">
<table  width="600" border="0" align="center" cellpadding="0" cellspacing="0">
  <tr> 
    <td class="tdbg_left">
<%
if Action="Vote"  then
	response.write "<font size='4'>"
    if Session("UserName")<>"" then response.write Session("UserName") & ":"
	response.write "<br><br>&nbsp;&nbsp;非常感谢您的投票！</font><br>"
end if
		%>
      <table width="600" border="0" align="center" cellpadding="2" cellspacing="0" class="border">
        <tr align="center" class="title"> 
          <td height="35" colspan="3"><strong>调 查 结 果</strong></td>
  </tr>
  <tr class="tdbg"><td>
   <table width="600" border="0" align="center" cellpadding="0" cellspacing="2">
              <tr> 
                <td width="140" align="right"><div align="center"><strong>调查内容：</strong></div></td>
                <td colspan="2"><%=rsVote("Title")%></td>
              </tr>
              <tr> 
                <td width="140" align="right"><div align="center"><strong>总投票数：</strong></div></td>
                <td colspan="2"> 
 <%
  dim totalVote
  totalVote=0
  for i=1 to 8
  	if rsVote("Select" & i)="" then exit for
	totalVote=totalVote+rsVote("answer"& i)
  next
  response.Write(totalVote & "票")
  if totalVote=0 then totalVote=1
  %>
                </td>
              </tr>
              <tr> 
                <td colspan="3" align="center">&nbsp;</td>
              </tr>
              <tr> 
                <td width="140" align="center"><strong>投票选项</strong></td>
                <td width="64" align="right"><div align="center"><strong>票数</strong></div></td>
                <td width="388" align="center"><strong>百分比</strong></td>
              </tr>
              <%
  for i=1 to 8
  	if trim(rsVote("Select" & i) & "")="" then exit for
  %>
              <tr> 
                <td width="140" align="right"><div align="center"><%=rsVote("Select"& i)%> 
                  </div></td>
                <td align="right"> 
                  <div align="center"><%
	response.write rsVote("answer"& i)
	%>
                  </div></td>
                <td> 
                  <%
	dim perVote
	perVote=round(rsVote("answer"& i)/totalVote,4)
	response.write "<img src='Images/topBar_bg.gif' width='" & round(360*perVote) & "' height='15' align='absmiddle'>"
	perVote=perVote*100
	if perVote<1 and perVote<>0 then
		response.write "&nbsp;0" & perVote & "%"
	else
		response.write "&nbsp;" & perVote & "%"
	end if
	%>
                </td>
              </tr>
              <% next %>
            </table>
  </td>
  </tr>
</table>
      <table width="63%" border="0" align="center" cellpadding="0" cellspacing="0">
        <tr>
          <td>
            <%
		if Session("UserName")<>"" then
			response.write Session("UserName") & ":"
		end if 
	    response.Write "<br><br>您还没有投票，请您在此投下您宝贵的一票！"
		response.write "<form name='VoteForm' method='post' action='vote.asp'>"
		response.write rsVote("Title") & "<br>"
		if rsVote("VoteType")="Single" then
			for i=1 to 8
				if trim(rsVote("Select" & i) & "")="" then exit for
				response.Write "<input type='radio' name='VoteOption' value='" & i & "'>" & rsVote("Select" & i) & "<br>"
			next
		else
			for i=1 to 8
				if trim(rsVote("Select" & i) & "")="" then exit for
				response.Write "<input type='checkbox' name='VoteOption' value='" & i & "'>" & rsVote("Select" & i) & "<br>"
			next
		end if
		response.write "<br><input name='VoteType' type='hidden'value='" & rsVote("VoteType") & "'>"
		response.write "<input name='Action' type='hidden' value='Vote'>"
		response.write "<input name='ID' type='hidden' value='" & rsVote("ID") & "'>"
		response.write "&nbsp;&nbsp;&nbsp;&nbsp;<a href='javascript:VoteForm.submit();'><img src='images/voteSubmit.gif' width='52' height='18' border='0'></a>&nbsp;&nbsp;"
        response.write "<a href='Vote.asp?ID=" & rsVote("ID") & "&Action=Show' target='_blank'><img src='images/voteView.gif' width='52' height='18' border='0'></a>"
		response.write "</form>"
%>
          </td>
        </tr>
      </table> 
      <p align="center">【<a href="javascript:window.close();">关闭窗口</a>】<br>
        <br>
      </p>
		</td>
  </tr>
</table>
</BODY>
</HTML>
<%
rsVote.Close()
Set rsVote = Nothing
call CloseConn()
%>