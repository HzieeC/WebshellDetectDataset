<%@language=vbscript codepage=936 %>
<!--#include file="Conn.asp"-->
<!--#include file="Admin.asp"-->
<%
dim SmallClassID,sql
SmallClassID=trim(Request("SmallClassID"))
if SmallClassID<>"" then
	sql="delete from SmallClass where SmallClassID="&Cint(SmallClassID)&""
	conn.Execute sql
end if
call CloseConn()      
response.redirect "ClassManage.asp"
%>


