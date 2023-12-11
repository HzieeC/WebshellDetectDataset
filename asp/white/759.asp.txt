<%@language=vbscript codepage=936 %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<!-- #include file="Inc/Head.asp" -->
<%
dim SmallClassID,sql
SmallClassID=trim(Request("SmallClassID"))
SmallClassName=trim(Request("SmallClassName"))
if SmallClassID<>"" then
	sql="delete from SmallClass_New where SmallClassID="&Cint(SmallClassID)&""
	conn.Execute sql
	sql="delete from NEWS where SmallClassName='" & SmallClassName & "'"
	conn.Execute sql
end if
call CloseConn()      
response.redirect "News_ClassManage.asp"
%>


