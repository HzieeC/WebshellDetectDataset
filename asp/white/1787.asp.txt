<%@ CODEPAGE=65001 %>
<% Option Explicit %>
<% Response.CodePage=65001 %>
<% Response.Charset="UTF-8" %>
<!--#include file="upload_5xsoft.inc"-->
<!--#include file="JSON_2.0.4.asp"-->
<%
if session("log_name")="" then	
	showError("没有登录不能上传文件!") 
end if

dim server_v1,server_v2
server_v1=Cstr(Request.ServerVariables("HTTP_REFERER"))
server_v2=Cstr(Request.ServerVariables("SERVER_NAME"))
if mid(server_v1,8,len(server_v2))<>server_v2 then
	showError("请不要非法提交数据!") 
end if

' KindEditor ASP
'
' 本ASP程序是演示程序，建议不要直接在实际项目中使用。
' 如果您确定直接使用本程序，使用之前请仔细确认相关安全设置。
'

Dim savePath, saveUrl, fileTypes, maxSize, fileName, fileExt, newFileName, filePath, fileUrl
Dim upload, file, fso, ranNum, hash
Dim badchar
dim i
badchar=array(";","*","%","@","&","<",">",".php",".dll",".exe",".asp",",","..")
'文件保存目录路径
savePath = "/Images/Up_Images/"
'文件保存目录URL
saveUrl = "/Images/Up_Images/"
'定义允许上传的文件扩展名
fileTypes = "gif,jpg,png,bmp"
'最大文件大小
maxSize = 500000

Set upload = new upload_5xsoft
Set file = upload.file("imgFile")

If file.fileSize < 1 Then
	Set upload = nothing
	Set file = nothing
	showError("请选择文件。")
End If

Set fso = Server.CreateObject("Scripting.FileSystemObject")
If Not fso.FolderExists(Server.mappath(savePath)) Then
	Set upload = nothing
	Set file = nothing
	
	showError("上传目录不存在。")
End If

If file.fileSize > maxSize Then
	Set upload = nothing
	Set file = nothing
	showError("上传文件大小超过500K，请优化图片大小后再上传！")
End If

fileName = file.filename

for i = 0 to ubound(badchar)
  if instr(fileName , badchar(i)) > 1 then
  Set upload = nothing
	Set file = nothing
	showError("上传文件包含非法字符。")
  end if
next

fileExt = mid(fileName, InStrRev(fileName, ".") + 1)
randomize
ranNum = int(9000000 * rnd) + 10000
newFileName = year(now) & month(now) & day(now) & hour(now) & minute(now) & second(now) & ranNum & "." & fileExt

If instr(fileTypes, lcase(fileExt)) < 1 Then
	Set upload = nothing
	Set file = nothing
	showError("上传文件扩展名是不允许的扩展名。")
End If



filePath = Server.mappath(savePath & newFileName)
fileUrl = saveUrl & newFileName

file.saveAs filePath

Set upload = nothing
Set file = nothing

If Not fso.FileExists(filePath) Then
	showError("上传文件失败。")
End If

Response.AddHeader "Content-Type", "text/html; charset=UTF-8"
Set hash = jsObject()
hash("error") = 0
hash("url") = fileUrl
hash.Flush
Response.End

Function showError(message)
	Response.AddHeader "Content-Type", "text/html; charset=UTF-8"
	Dim hash
	Set hash = jsObject()
	hash("error") = 1
	hash("message") = message
	hash.Flush
	Response.End
End Function
%>
