<!--#include file="../../Inc/Const.Asp"-->
<%

Dim ID
ID = Request("ID") ' 文章编号

If IsNumeric(ID) and len(ID) > 0 and len(ID) < 10 Then
	Dim Plus
	Set Plus = New Cls_Plus
	Plus.Open("count") ' 打开配置文件
	If Plus.Config("state") = 0 Then Response.End
	On Error Resume Next
	Call DB("Update [{pre}Content] Set [Views]=[Views]+1 Where [ID]=" & ID,0) ' 更新统计
	If Plus.Config("show") = 1 then
		Dim Rs : Set Rs = DB("Select [Views] From [{pre}Content] Where [ID]=" & ID,1)
		If Not Rs.Eof Then Response.Write "document.write('" & Rs(0) & "');"
		Rs.Close : Set Rs = Nothing
	End If
	Conn.Close : Set Conn = Nothing
	Set Plus = Nothing
End If

%>