<%@ LANGUAGE="VBScript" %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%
dim sql
dim rs
on error resume next
com_typeid=request("com_typeid")
set rs=server.createobject("adodb.recordset")
sql="delete from Comment where com_ID="&request("CommentID")
rs.open sql,conn,1,1
set rs=nothing
conn.close
set conn=nothing
response.redirect "AdminShownews.asp?id="&com_typeid
%>