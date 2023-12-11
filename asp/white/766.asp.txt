<%@language=vbscript codepage=936 %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<!-- #include file="Inc/Head.asp" -->
<%
dim BigClassName,sql
BigClassName=trim(Request("BigClassName"))
if BigClassName<>"" then
	sql="delete from BigClass_New where BigClassName='" & BigClassName & "'"
	conn.Execute sql
	sql="delete from SmallClass_New where BigClassName='" & BigClassName & "'"
	conn.Execute sql
	sql="delete from NEWS where BigClassName='" & BigClassName & "'"
	conn.Execute sql
end if
call CloseConn()      
response.redirect "News_ClassManage.asp"
%>


