<% Option Explicit %>
<!--#include file = "../asp/config.asp"-->
<%
'######################################
' eWebEditor v5.5 - Advanced online web based WYSIWYG HTML editor.
' Copyright (c) 2003-2008 eWebSoft.com
'
' For further information go to http://www.ewebsoft.com/
' This copyright notice MUST stay intact for use.
'######################################
%>
<%
If Session("eWebEditor_User") = "" Then
Response.Write "<script language=javascript>top.location.href='login.asp';</script>"
Response.End
End If
Call CheckAndUpdateConfig()
Dim sAction, sPosition
sAction = UCase(Trim(Request.QueryString("action")))
sPosition = "当前位置："
Sub Header()
Response.Write "<html><head>"
Response.Write "<meta http-equiv='Content-Type' content='text/html; charset=gb2312'>" & _
"<title>eWebEditor在线编辑器 - 后台管理</title>" & _
"<link rel='stylesheet' type='text/css' href='private.css'>" & _
"<script language='javascript' src='private.js'></script>"
Response.Write "</head>"
Response.Write "<body>"
Response.Write "<a name=top></a>"
End Sub
Sub Footer()
Response.Write "<table border=0 cellpadding=0 cellspacing=0 align=center width='100%'>" & _
"<tr><td height=40></td></tr>" &_
"<tr><td><hr size=1 color=#000000 width='60%' align=center></td></tr>" &_
"<tr>" & _
"<td align=center>Copyright  &copy;  2003-2008  <b>eWebEditor<font color=#CC0000>.net</font></b> <b>eWebSoft<font color=#CC0000>.com</font></b>, All Rights Reserved .</td>" & _
"</tr>" & _
"<tr>" & _
"<td align=center><a href='mailto:service@ewebsoft.com'>service@ewebsoft.com</a></td>" & _
"</tr>" & _
"</table>"
Response.Write "</body></html>"
End Sub
Function IsSafeStr(str)
Dim s_BadStr, n, i
s_BadStr = "' 　&<>?%,;:()`~!@#$^*{}[]|+-=" & Chr(34) & Chr(9) & Chr(32)
n = Len(s_BadStr)
IsSafeStr = True
For i = 1 To n
If Instr(str, Mid(s_BadStr, i, 1)) > 0 Then
IsSafeStr = False
Exit Function
End If
Next
End Function
Function outHTML(str)
Dim sTemp
sTemp = str
outHTML = ""
If IsNull(sTemp) = True Then
Exit Function
End If
sTemp = Replace(sTemp, "&", "&amp;")
sTemp = Replace(sTemp, "<", "&lt;")
sTemp = Replace(sTemp, ">", "&gt;")
sTemp = Replace(sTemp, Chr(34), "&quot;")
sTemp = Replace(sTemp, Chr(10), "<br>")
outHTML = sTemp
End Function
Function inHTML(str)
Dim sTemp
sTemp = str
inHTML = ""
If IsNull(sTemp) = True Then
Exit Function
End If
sTemp = Replace(sTemp, "&", "&amp;")
sTemp = Replace(sTemp, "<", "&lt;")
sTemp = Replace(sTemp, ">", "&gt;")
sTemp = Replace(sTemp, Chr(34), "&quot;")
inHTML = sTemp
End Function
Sub GoError(str)
Response.Write "<script language=javascript>alert('" & str & "\n\n系统将自动返回前一页面...');history.back();</script>"
Response.End
End Sub
Function GetTrueLen(str)
Dim l, t, c, i
l = Len(str)
t = l
For i = 1 To l
c = Asc(Mid(str, i, 1))
If c < 0 Then c = c + 65536
If c > 255 Then t = t + 1
Next
GetTrueLen = t
End Function
Sub WriteConfig()
Dim i, n, sConfig
sConfig = "<" & "%" & Vbcrlf
sConfig = sConfig & "Dim sUsername, sPassword" & Vbcrlf
sConfig = sConfig & "sUsername = """ & sUsername & """" & Vbcrlf
sConfig = sConfig & "sPassword = """ & sPassword & """" & Vbcrlf
sConfig = sConfig & Vbcrlf
Dim nConfigStyle, sConfigStyle, aTmpStyle
Dim nConfigToolbar, sConfigToolbar, aTmpToolbar, sTmpToolbar
Dim s_Order, s_ID, a_Order, a_ID
nConfigStyle = 0
sConfigStyle = ""
nConfigToolbar = 0
sConfigToolbar = ""
For i = 1 To UBound(aStyle)
If aStyle(i) <> "" Then
aTmpStyle = Split(aStyle(i), "|||")
If aTmpStyle(0) <> "" Then
nConfigStyle = nConfigStyle + 1
sConfigStyle = sConfigStyle & "aStyle(" & nConfigStyle & ") = """ & aStyle(i) & """" & Vbcrlf
s_Order = ""
s_ID = ""
For n = 1 To UBound(aToolbar)
If aToolbar(n) <> "" Then
aTmpToolbar = Split(aToolbar(n), "|||")
If aTmpToolbar(0) = CStr(i) Then
If s_ID <> "" Then
s_ID = s_ID & "|"
s_Order = s_Order & "|"
End If
s_ID = s_ID & CStr(n)
s_Order = s_Order & aTmpToolbar(3)
End If
End If
Next
If s_ID <> "" Then
a_ID = Split(s_ID, "|")
a_Order = Split(s_Order, "|")
For n = 0 To UBound(a_Order)
a_Order(n) = Clng(a_Order(n))
a_ID(n) = Clng(a_ID(n))
Next
a_ID = Sort(a_ID, a_Order)
For n = 0 To UBound(a_ID)
nConfigToolbar = nConfigToolbar + 1
aTmpToolbar = Split(aToolbar(a_ID(n)), "|||")
sTmpToolbar = nConfigStyle & "|||" & aTmpToolbar(1) & "|||" & aTmpToolbar(2) & "|||" & aTmpToolbar(3)
sConfigToolbar = sConfigToolbar & "aToolbar(" & nConfigToolbar & ") = """ & sTmpToolbar & """" & Vbcrlf
Next
End If
End If
End If
Next
sConfigStyle = "Dim aStyle()" & Vbcrlf & "Redim aStyle(" & nConfigStyle & ")" & Vbcrlf & sConfigStyle
sConfigToolbar = "Dim aToolbar()" & Vbcrlf & "Redim aToolbar(" & nConfigToolbar & ")" & Vbcrlf & sConfigToolbar
sConfig = sConfig & sConfigStyle & Vbcrlf & sConfigToolbar & "%" & ">"
Call WriteFile("../asp/config.asp", sConfig)
End Sub
Sub CheckAndUpdateConfig()
Dim n_Old, n_New, i, s
n_Old = UBound(Split(aStyle(1),"|||"))
n_New = 67
If n_Old<66 Or n_Old>=n_New Then
Exit Sub
End If
s = ""
For i=n_Old+1 To n_New
s = s & "|||"
Select Case i
Case 67
s = s & "0"
End Select
Next
For i = 1 To UBound(aStyle)
aStyle(i) = aStyle(i) & s
Next
Call WriteConfig()
End Sub
Sub WriteFile(s_FileName, s_Text)
On Error Resume Next
Dim fso, file
Set fso = Server.CreateObject("Scripting.FileSystemObject")
Set file = fso.CreateTextFile(Server.Mappath(s_FileName), True)
file.Write(s_Text)
file.Close
Set file = Nothing
Set fso = Nothing
End Sub
Function Sort(aryValue, aryOrder)
Dim i, KeepChecking
Dim FirstOrder, SecondOrder
Dim FirstValue, SecondValue
KeepChecking = TRUE
Do Until KeepChecking = FALSE
KeepChecking = FALSE
For i = 0 to UBound(aryOrder)
If i = UBound(aryOrder) Then Exit For
If aryOrder(i) > aryOrder(i+1) Then
FirstOrder = aryOrder(i)
SecondOrder = aryOrder(i+1)
aryOrder(i) = SecondOrder
aryOrder(i+1) = FirstOrder 
FirstValue = aryValue(i)
SecondValue = aryValue(i+1)
aryValue(i) = SecondValue
aryValue(i+1) = FirstValue
KeepChecking = TRUE 
End If
Next
Loop
Sort = aryValue
End Function 
Function InitSelect(s_FieldName, a_Name, a_Value, v_InitValue, s_AllName, s_Attribute)
Dim i
InitSelect = "<select name='" & s_FieldName & "' size=1 " & s_Attribute & ">"
If s_AllName <> "" Then
InitSelect = InitSelect & "<option value=''>" & s_AllName & "</option>"
End If
For i = 0 To UBound(a_Name)
InitSelect = InitSelect & "<option value=""" & inHTML(a_Value(i)) & """"
If a_Value(i) = v_InitValue Then
InitSelect = InitSelect & " selected"
End If
InitSelect = InitSelect & ">" & outHTML(a_Name(i)) & "</option>"
Next
InitSelect = InitSelect & "</select>"
End Function
Function InitCheckBox(s_FieldName, s_Value, s_InitValue)
If s_Value = s_InitValue Then
InitCheckBox = "<input type=checkbox name='" & s_FieldName & "' value='" & s_Value & "' checked>"
Else
InitCheckBox = "<input type=checkbox name='" & s_FieldName & "' value='" & s_Value & "'>"
End If
End Function
%>