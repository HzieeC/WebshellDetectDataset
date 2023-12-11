<!--#include file="Conn.asp"-->
<!--#include file="inc/const.asp"-->
<%
Dim ajaxPro
ajaxPro=CInt(Request.Form("ajaxPost"))
If ajaxPro=1 Then
	ajaxPro=True
Else
	ajaxPro=False Rem 非ajax登陆
End If
Dim comeurl
Dim TruePassWord
Dim EnterCount
If Request.Cookies("count")<>"" Then
	EnterCount=Request.Cookies("count")
Else
	EnterCount=Request.QueryString("count")'地址栏带参数
End If
Dvbbs.LoadTemplates("login")
Select Case request("winaction")
	Case "winlogin"
			If Dvbbs.UserID=0 Then 
				MainWin()
			Else 
			Response.Redirect "index.asp"
			End If 
	Case Else
		Response.Redirect "index.asp"
End Select
Dvbbs.PageEnd()

Sub MainWin()
Dim TempStr
Dim Comeurl,tmpstr
If Request.ServerVariables("HTTP_REFERER")<>"" Then 
		tmpstr=split(Request.ServerVariables("HTTP_REFERER"),"/")
		Comeurl=tmpstr(UBound(tmpstr))
Else
		If isUrlreWrite = 1 Then
			Comeurl="index.html"
		Else
			Comeurl="index.asp"
		End If
End If
TempStr=template.html(25)
If Dvbbs.Forum_setting(79)=0 Then 
TempStr=Replace(TempStr,"{$getcode}","")
Else 
TempStr = Replace(TempStr,"{$getcode}",Replace(template.html(23),"{$codestr}",Dvbbs.GetCode2()))
End If 
TempStr = Replace(TempStr,"{$comeurl}",Comeurl)
Response.Write TempStr
Response.End
TempStr="":Comeurl="":tmpstr=""
End Sub 
%>