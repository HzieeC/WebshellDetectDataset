<!--#include file="../Inc/Conn.Asp"-->
<%
Dim ID
ID = LaoYRequest(Request("ID"))

If IsNumeric(ID) and len(ID) > 0 and len(ID) < 10 Then
	On Error Resume Next
	IF Session("Count"&ID&"")<>1 then
	Conn.execute "Update ["&tbname&"_Article] Set [Hits]=[Hits]+1 Where [ID]=" & ID
	IF IsHits2=1 then Session("Count"&ID&"")=1
	End if

	Set rs=server.createobject("adodb.recordset")
	Sql="Select [Hits] From ["&tbname&"_Article] Where [ID]="& ID
	Rs.open sql,conn,1,1
	If Not Rs.Eof Then Response.Write "document.write('" & Rs(0) & "');"
	Rs.Close : Set Rs = Nothing
	Conn.Close : Set Conn = Nothing
End If
%>