<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/dv_clsother.asp"-->
<%
Response.Expires = -9999
Response.AddHeader "pragma", "no-cache"
Response.AddHeader "cache-ctrol", "no-cache"
Dim server_v1
server_v1=Cstr(Request.ServerVariables("HTTP_REFERER"))
server_v1=Lcase(server_v1)

If not (instr(server_v1,"activeonline.asp")>0) and Request.servervariables("REQUEST_METHOD") = "POST" Then
	Dvbbs.Stats=Request("state")
	Dvbbs.ActiveOnline1()
%>
var allonline=<%=MyBoardOnline.Forum_Online%>;
<%if Dvbbs.BoardId=0 Then%>
var useronline=<%=MyBoardOnline.Forum_UserOnline%>;
var guestonline=<%=MyBoardOnline.Forum_GuestOnline%>;
<%
else
%>
var useronline=<%=MyBoardOnline.Board_UserOnline%>;
var guestonline=<%=MyBoardOnline.Board_GuestOnline%>;
<%
End If
%>
<%
End If
Dvbbs.PageEnd()
%>