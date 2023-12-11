<!--#include file="../inc/conn.asp"-->
<!--#include file="../inc/AutoKey.asp"-->
<%
Dim Action
Action=Request("Action")
Select Case Action
 Case "GetTags" GetTags
End Select

'È¡¹Ø¼ü´Êtags
Sub GetTags()
 Dim Text:Text=UnEscape(lcase(Request("Text")))
 If Text<>"" Then
	 Response.Write Escape(cn_split(text,20))
 End If
End Sub
%>