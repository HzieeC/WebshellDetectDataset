<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/dv_clsother.asp" -->
<%
If Dvbbs.BoardID=0 Then
	Response.Write("<h1>´íÎó: </h1>²ÎÊý´íÎó¡£")
	Dvbbs.PageEnd()
	Response.End
End If
Dim filename
filename=Request("filename")
filename=Replace(filename,"..","")
If Request.ServerVariables("HTTP_REFERER")="" Or InStr(Request.ServerVariables("HTTP_REFERER"),Request.ServerVariables("SERVER_NAME"))=0 Or filename="" Then
	Call downloadFile(Server.MapPath(Dvbbs.Forum_Info(6)))
Else
	If Dvbbs.Forum_Setting(76)="" Or Dvbbs.Forum_Setting(76)="0" Then Dvbbs.Forum_Setting(76)="UploadFile/"
	If right(Dvbbs.Forum_Setting(76),1)<>"/" Then Dvbbs.Forum_Setting(76)=Dvbbs.Forum_Setting(76)&"/"
	Call downloadFile(Server.MapPath(Dvbbs.Forum_Setting(76)&filename))
End If
Dvbbs.PageEnd()
Sub downloadFile(strFile)
	On error resume next
	Server.ScriptTimeOut=999999
	Dim S,fso,f,intFilelength,strFilename
	strFilename = strFile
	Response.Clear
	Set s = Dvbbs.iCreateObject("ADODB.Stream") 
	s.Open
	s.Type = 1 
	Set fso = Dvbbs.iCreateObject("Scripting.FileSystemObject") 
	If Not fso.FileExists(strFilename) Then
		Call downloadFile(Server.MapPath(Dvbbs.Forum_Info(6)))
		Exit Sub		
	End If
	Set f = fso.GetFile(strFilename)
	intFilelength = f.size
	s.LoadFromFile(strFilename)
	If err Then
	 	Response.Write("<h1>´íÎó: </h1>" & err.Description & "<p>")
		Response.End 
	End If
	Set fso=Nothing
	Dim Data
	Data=s.Read
	s.Close
	Set s=Nothing
	If Response.IsClientConnected Then
		If Not (InStr(LCase(f.name),".gif")>0 Or InStr(LCase(f.name),".jpg")>0 Or InStr(LCase(f.name),".jpeg")>0 Or InStr(LCase(f.name),".bmp")>0 )Then 
			Response.AddHeader "Content-Disposition", "attachment; filename=" & f.name 
		End If
		Response.AddHeader "Content-Length", intFilelength 
 		Response.CharSet = "UTF-8" 
		Response.ContentType = "application/octet-stream"
		Response.BinaryWrite Data
		Response.Flush
	End If
End Sub
%>