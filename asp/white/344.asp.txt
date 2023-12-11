<% Server.ScriptTimeOut=5000 %>
<!--#include file="../inc/access.asp"  -->
<!--#include FILE="UpLoadClass.asp"-->
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<title>Upload</title>
<style>
<!--
*  
BODY{background:#E7EBEF;}       

-->
</style>
<body leftmargin="0" topmargin="2" class="p9">
<%
Action = Trim(Request.QueryString("Action"))
fField = Trim(Request.QueryString("Field"))
fFieldSize = Trim(Request.QueryString("FieldSize"))
FF = Trim(Request.QueryString("FF"))
FS = Cint(Trim(Request.QueryString("FS")))
FT = Trim(Request.QueryString("FT"))

If Action = "upFile" Then call upFile()
If Action = "upPic" then call upPic()
dim Uprequest
'============================================================upFile
sub upFile()
if Request.QueryString("submit")="upFile" then

FS=trim(request("FS"))

Set Uprequest=new UpLoadClass
    Uprequest.SavePath="/"&FF&"/"   '保存路径
    Uprequest.MaxSize=1024*FS  '限制大小 2 MB
    Uprequest.FileType=FT  '文件类型	
    AutoSave=true
    Uprequest.open
	fMaxSize = Uprequest.MaxSize/1024
  if Uprequest.form("file_Err")<>0  then
  select case Uprequest.form("file_Err")
  case 1:str1="<div style=""padding-top:3px;padding-bottom:3px;""> <font color=blue>上传不成功!文件超过 "&fMaxSize&"KB [<a href='javascript:history.go(-1)'>重新上传</a>]</font></div>"
  case 2:str1="<div style=""padding-top:3px;padding-bottom:3px;""> <font color=blue>上传不成功!文件格式不对 [<a href='javascript:history.go(-1)']>重新上传</a>]</font></div>"
  case 3:str1="<div style=""padding-top:3px;padding-bottom:3px;""> <font color=blue>上传不成功!文件太大且格式不对 [<a href='javascript:history.go(-1)'>重新上传</a>]</font></div>"
  end select
  response.write str1
  else
size=Uprequest.Form("file_size")
 		if size<1024 then  
 		   size=(size\1024)  
 		   showsize=size & " BT"  
 		end if  
 		if size>1024 then  
 		   size=(size/1024)  
 		   showsize=formatnumber(size,2)		  
 		end if 
response.write "<script language=""javascript"">parent.form1."&fField&".value='"&Uprequest.Form("file")&"';</script>"
response.write "<script language=""javascript"">parent.form1."&fFieldSize&".value='"&showsize&"';</script>"
response.write "<div style=""padding-top:5px;padding-bottom:3px;""> <font color=red>文件上传成功</font> [<a href='javascript:history.go(-1)'>重新上传</a>]</div>"
end if
Set Uprequest=nothing
end if
response.write "<form name=form action='?action=upFile&submit=upFile&Field="&fField&"&FieldSize="&fFieldSize&"&FF="&FF&"&FS="&FS&"&FT="&FT&"'    method=post enctype=multipart/form-data>"
response.write "<input type=file name=file class='tx' size='20'>&nbsp;"
response.write "<input type=submit name=submit value=上传 class=""tx1"">"
response.write "</form>"
end sub
'========================================
%>
</body>
</html>
<%set Uprequest=nothing%>