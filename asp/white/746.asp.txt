<%@language=vbscript codepage=936 %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%
dim ID,sql,rs
ID=trim(Request("ID"))
if ID<>"" then
	sql="delete from Aboutus where ID=" & Clng(ID)
	conn.Execute sql
end if
call CloseConn()      
response.redirect "AdminAboutus.asp"
%>