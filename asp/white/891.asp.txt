<%@ LANGUAGE="VBScript" %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%
dim SQL, Rs, contentID,CurrentPage
CurrentPage = request("Page")
contentID=request("ID")

set rs=server.createobject("adodb.recordset")
sqltext="delete from admin where Id="& contentID
rs.open sqltext,conn,3,3
set rs=nothing
response.redirect "Manage_Admin.asp"
%>