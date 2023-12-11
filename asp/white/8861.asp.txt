<!--#include file="Inc/Sysconfig.Asp"-->
<%
'On Error Resume Next
Dim Fname,Hits
Fname=Request("FileName")
Fname=Trim(Fname)
Fname=Replace(Fname,"'","''")
Hits=Yxbbs.Execute("Select Hits From [Yx_Upfile] where FileName='"&Fname&"'")(0)
If Hits="" Then Hits="н╢ж╙"
	Response.Write "document.write('<font color=#999999>[обть: "&Hits&" ╢н]</font>');"
Set Yxbbs=Nothing
%>