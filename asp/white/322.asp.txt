<%

'页面错误提示信息
Sub dvbbs_error()
	Response.Write"<br>"
	Response.Write"<table cellpadding=3 cellspacing=1 align=center class=""tableBorder"" style=""width:75%"">"
	Response.Write"<tr align=center>"
	Response.Write"<th width=""100%"" height=25 colspan=2>错误信息"
	Response.Write"</td>"
	Response.Write"</tr>"
	Response.Write"<tr>"
	Response.Write"<td width=""100%"" class=""Forumrow"" colspan=2>"
	Response.Write ErrMsg
	Response.Write"</td></tr>"
	Response.Write"<tr>"
	Response.Write"<td class=""Forumrow"" valign=middle colspan=2 align=center><a href=""javascript:history.go(-1)""><<返回上一页</a></td></tr>"
	Response.Write"</table>"
	footer()
	Response.End 
End Sub 



Sub footer()
	Response.Write"<table align=center >"
	Response.Write "<tr align=center><td width=""100%"" class=copyright>"
	Response.Write"Dvbbs v7.1 , Copyright (c) 2001-2005 <a href=""http://www.aspsky.net"" target=""_blank""><font color=#708796><b>AspSky<font color=#CC0000>.Net</font></b></font></a>. All Rights Reserved ."
	Response.Write"</td>"
	Response.Write"</tr>"
	Response.Write"</table>"
	Response.Write "</body>"
	Response.Write "</html>"
End Sub
Sub Head()
	Response.Write "<html>"
	Response.Write Chr(10)	
	Response.Write "<head>"
	Response.Write Chr(10)	
	Response.Write "<meta http-equiv=""Content-Type"" content=""text/html; charset=gb2312"">"
	Response.Write Chr(10)	
	Response.Write "<meta name=""description"" content=""Design By www.dvbbs.net"">"
	Response.Write Chr(10)	
	Response.Write "<title>-管理页面</title>"
	Response.Write Chr(10)	
	Response.Write Replace(template.html(1),"{$path}","images/")
	Response.Write Chr(10)
	Response.Write "</head>"
	Response.Write "<script src=""images/admin.js"" type=""text/javascript""></script><script src=""../inc/main.js"" type=""text/javascript""></script>"
	Response.Write Chr(10)
	Dim XMLDOM
	Set XMLDOM=Application(Dvbbs.CacheName&"_ssboardlist").cloneNode(True)
	Response.Write "<script language=""javascript"" type=""text/javascript"">"
	Response.Write "var boardxml='<?xml version=""1.0"" encoding=""gb2312""?>"& replace(XMLDom.documentElement.XML ,"'","\'")&"';"
	Response.Write "</script>"
	Response.Write "<body leftmargin=""0"" topmargin=""0"" marginheight=""0"" marginwidth=""0"">"
	Response.Write Chr(10)
%>
<div class=menuskin id=popmenu onmouseover="clearhidemenu();" onmouseout="dynamichide(event)" style="Z-index:100"></div>
<table cellpadding="3" cellspacing="0" border="0" align=center class="tableBorder1" style="width:100%">
<tr><td height=10></td></tr>
</table>
<%
End Sub
Sub Dv_suc(info)
	Response.Write"<br>"
	Response.Write"<table cellpadding=0 cellspacing=0 align=center class=""tableBorder"" style=""width:75%"">"
	Response.Write"<tr align=center>"
	Response.Write"<th width=""100%"" height=25 colspan=2>成功信息"
	Response.Write"</td>"
	Response.Write"</tr>"
	Response.Write"<tr>"
	Response.Write"<td width=""100%"" class=""forumRowHighlight"" colspan=2 height=25>"
	Response.Write info
	Response.Write"</td></tr>"
	Response.Write"<tr>"
	Response.Write"<td class=""forumRowHighlight"" valign=middle colspan=2 align=center><a href="&Request.ServerVariables("HTTP_REFERER")&" ><<返回上一页</a></td></tr>"
	Response.Write"</table>"
End Sub

Function fixjs(Str)
	If Str <>"" Then
		str = replace(str,"\", "\\")
		Str = replace(str, chr(34), "\""")
		Str = replace(str, chr(39),"\'")
		Str = Replace(str, chr(13), "\n")
		Str = Replace(str, chr(10), "\r")
		str = replace(str,"'", "&#39;")
	End If
	fixjs=Str
End Function
Function enfixjs(Str)
	If Str <>"" Then
		Str = replace(str,"&#39;", "'")
		Str = replace(str,"\""" , chr(34))
		Str = replace(str, "\'",chr(39))
		Str = Replace(str, "\r", chr(10))
		Str = Replace(str, "\n", chr(13))
		Str = replace(str,"\\", "\")
	End If
	enfixjs=Str
End Function


%>