<%@language=vbscript codepage=936 %>
<!--#include file="Conn.asp"-->
<!--#include file="Admin.asp"-->
<!-- #include file="Inc/Head.asp" -->
<%
dim BigClassName,sql
BigClassName=trim(Request("BigClassName"))
if BigClassName<>"" then
	sql="delete from BigClass_down where BigClassName='" & BigClassName & "'"
	conn.Execute sql
	sql="delete from SmallClass_down where BigClassName='" & BigClassName & "'"
	conn.Execute sql
	sql="delete from download where BigClassName='" & BigClassName & "'"
	conn.Execute sql
end if
call CloseConn()      
response.redirect "Down_ClassManage.asp"
%>


