<%@language=vbscript codepage=936 %>
<!--#include file="Conn.asp"-->
<!--#include file="Admin.asp"-->
<!-- #include file="Inc/Head.asp" -->
<%
dim SmallClassID,sql
SmallClassID=trim(Request("SmallClassID"))
SmallClassName=trim(Request("SmallClassName"))
if SmallClassID<>"" then
	sql="delete from SmallClass_down where SmallClassID="&Cint(SmallClassID)&""
	conn.Execute sql
	sql="delete from download where SmallClassName='" & SmallClassName & "'"
	conn.Execute sql
end if
call CloseConn()      
response.redirect "Down_ClassManage.asp"
%>


