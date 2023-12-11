<% Option Explicit %>
<%
'######################################
' eWebEditor v5.5 - Advanced online web based WYSIWYG HTML editor.
' Copyright (c) 2003-2008 eWebSoft.com
'
' For further information go to http://www.ewebsoft.com/
' This copyright notice MUST stay intact for use.
'######################################
%>
<!--#include file="config.asp"-->
<%
Server.ScriptTimeOut = 1800
Dim sType, sStyleName, sCusDir, sAction
Dim nTreeIndex
Dim sAllowExt, sUploadDir, sBaseUrl, sUploadContentPath, nAllowBrowse, nCusDirFlag
Dim sPathShareImage, sPathShareFlash, sPathShareMedia, sPathShareOther
Call InitParam()
sAction = UCase(Trim(Request.QueryString("action")))
Select Case sAction
Case "FILE"
Call OutScript(GetFileList())
Case Else
sAction = "FOLDER"
Call OutScript(GetFolderList())
End Select
Function GetFileList()
Dim s_ReturnFlag, s_FolderType, s_Dir
Dim s_CurrDir
s_ReturnFlag = Trim(Request.QueryString("returnflag"))
s_FolderType = Trim(Request.QueryString("foldertype"))
s_Dir = Trim(Request("dir"))
Select Case s_FolderType
Case "upload"
s_CurrDir = sUploadDir
Case "shareimage"
sAllowExt = ""
s_CurrDir = sPathShareImage
Case "shareflash"
sAllowExt = ""
s_CurrDir = sPathShareFlash
Case "sharemedia"
sAllowExt = ""
s_CurrDir = sPathShareMedia
Case Else
s_FolderType = "shareother"
sAllowExt = ""
s_CurrDir = sPathShareOther
End Select
s_Dir = Replace(s_Dir, "\", "/")
s_Dir = Replace(s_Dir, "../", "")
s_Dir = Replace(s_Dir, "./", "")
If Left(s_Dir,1)="/" Then
s_Dir = ""
End If
Dim s_Dir2
s_Dir2 = Replace(s_Dir, "/", "\")
If s_Dir <> "" Then
If CheckValidDir(s_CurrDir & s_Dir2) = True Then
s_CurrDir = s_CurrDir & s_Dir2
Else
s_Dir = ""
End If
End If
If CheckValidDir(s_CurrDir) = False Then
GetFileList = "var arr = new Array();" & VbCrlf & "parent.setFileList('" & s_ReturnFlag & "', '" & s_FolderType & "', '" & s_Dir & "', arr);"
Exit Function
End If
Dim s_List, i
Dim o_FSO, o_Folder, o_Files, o_File, s_FileName
On Error Resume Next
Set o_FSO = Server.CreateObject("Scripting.FileSystemObject")
Set o_Folder = o_FSO.GetFolder(s_CurrDir)
If Err.Number>0 Then
GetFileList = "var arr = new Array();" & VbCrlf & "parent.setFileList('" & s_ReturnFlag & "', '" & s_FolderType & "', '" & s_Dir & "', arr);"
Exit Function
End If
Set o_Files = o_Folder.Files
i = -1
s_List = ""
For Each o_File In o_Files
s_FileName = o_File.Name
If CheckValidExt(s_FileName) = True Then
i = i + 1
s_List = s_List & "arr[" & i & "]=new Array(""" & s_FileName & """, """ & GetSizeUnit(o_File.size) & """,""" & FormatTime(o_File.DateLastModified, 1) & """);" & VbCrlf
End If
Next
Set o_Folder = Nothing
Set o_Files = Nothing
Set o_FSO = Nothing
s_List = "var arr = new Array();" & VbCrlf & s_List & "parent.setFileList('" & s_ReturnFlag & "', '" & s_FolderType & "', '" & s_Dir & "', arr);"
GetFileList = s_List
End Function
Function GetFolderList()
Dim s_List
s_List = ""
s_List = "var arrUpload = new Array();" & VbCrlf
s_List = s_List & "var arrShareImage = new Array();" & VbCrlf
s_List = s_List & "var arrShareFlash = new Array();" & VbCrlf
s_List = s_List & "var arrShareMedia = new Array();" & VbCrlf
s_List = s_List & "var arrShareOther = new Array();" & VbCrlf
nTreeIndex = 0
s_List = s_List & GetFolderTree(sUploadDir, "Upload", 1)
sAllowExt = ""
Select Case sType
Case "FILE"
nTreeIndex = 0
s_List = s_List & GetFolderTree(sPathShareImage, "ShareImage", 1)
nTreeIndex = 0
s_List = s_List & GetFolderTree(sPathShareFlash, "ShareFlash", 1)
nTreeIndex = 0
s_List = s_List & GetFolderTree(sPathShareMedia, "ShareMedia", 1)
nTreeIndex = 0
s_List = s_List & GetFolderTree(sPathShareOther, "ShareOther", 1)
Case "MEDIA"
nTreeIndex = 0
s_List = s_List & GetFolderTree(sPathShareMedia, "ShareMedia", 1)
Case "FLASH"
nTreeIndex = 0
s_List = s_List & GetFolderTree(sPathShareFlash, "ShareFlash", 1)
Case Else
nTreeIndex = 0
s_List = s_List & GetFolderTree(sPathShareImage, "ShareImage", 1)
End Select
s_List = s_List & "parent.setFolderList(arrUpload, arrShareImage, arrShareFlash, arrShareMedia, arrShareOther);"
GetFolderList = s_List
End Function
Function GetFolderTree(s_Dir, s_Flag, n_Indent)
Dim o_FSO, o_Folder, o_SubFolder
Err.Clear
On Error Resume Next
Set o_FSO = Server.CreateObject("Scripting.FileSystemObject")	
Set o_Folder = o_FSO.GetFolder(s_Dir)
If Err.Number>0 Then
GetFolderTree = ""
Exit Function
End If
Dim s_List, s_Folder, i, n_Count, s_LastFlag
s_List = ""
i = 0
n_Count = o_Folder.SubFolders.Count
For Each o_SubFolder In o_Folder.SubFolders
i = i + 1
If i < n_Count Then
s_LastFlag = "0"
Else
s_LastFlag = "1"
End If
s_Folder = o_SubFolder.Name
s_List = s_List & "arr" & s_Flag & "[" & nTreeIndex & "]=new Array(""" & s_Folder & """," & n_Indent & ", " & s_LastFlag & ");" & VbCrlf
nTreeIndex = nTreeIndex + 1
s_List = s_List & GetFolderTree(s_Dir & s_Folder & "\", s_Flag, n_Indent+1)
Next
Set o_Folder = Nothing
Set o_FSO = Nothing
GetFolderTree = s_List
End Function
Sub OutScript(str)
Response.Write "<HTML><HEAD><meta http-equiv='Content-Type' content='text/html; charset=gb2312'><TITLE>eWebEditor</TITLE></head><body>"
Response.Write "<script language=javascript>" & str & "</script>"
Response.Write "</body></html>"
Response.End
End Sub
Function CheckValidExt(s_FileName)
If sAllowExt = "" Then
CheckValidExt = True
Exit Function
End If
Dim i, aExt, sExt
sExt = LCase(Mid(s_FileName, InStrRev(s_FileName, ".") + 1))
CheckValidExt = False
aExt = Split(LCase(sAllowExt), "|")
For i = 0 To UBound(aExt)
If aExt(i) = sExt Then
CheckValidExt = True
Exit Function
End If
Next
End Function
Sub InitParam()
sType = UCase(Trim(Request.QueryString("type")))
sStyleName = Trim(Request.QueryString("style"))
sCusDir = Trim(Request.QueryString("cusdir"))
Dim i, aStyleConfig, bValidStyle
bValidStyle = False
For i = 1 To Ubound(aStyle)
aStyleConfig = Split(aStyle(i), "|||")
If Lcase(sStyleName) = Lcase(aStyleConfig(0)) Then
bValidStyle = True
Exit For
End If
Next
If bValidStyle = False Then
OutScript("alert('Invalid Style.')")
End If
sBaseUrl = aStyleConfig(19)
nAllowBrowse = CLng(aStyleConfig(43))
nCusDirFlag = Clng(aStyleConfig(61))
If nAllowBrowse <> 1 Then
OutScript("alert('Do not allow browse!')")
End If
If nCusDirFlag <> 1 Then
sCusDir = ""
Else
If sCusDir <> "" Then
sCusDir = Replace(sCusDir, "\", "/")
If Left(sCusDir, 1) = "/" Or Left(sCusDir, 1) = "." Or Right(sCusDir, 1) = "." Or InStr(sCusDir, "./") > 0 Or InStr(sCusDir, "/.") > 0 Or InStr(sCusDir, "//") > 0 Then
sCusDir = ""
Else
If Right(sCusDir, 1) <> "/" Then
sCusDir = sCusDir & "/"
End If
End If
End If
End If
sUploadDir = aStyleConfig(3)
If sBaseUrl<>"3" Then
If Left(sUploadDir, 1) <> "/" Then
sUploadDir = "../" & sUploadDir
End If
sUploadDir = Server.Mappath(sUploadDir)
End If
sUploadDir = GetSlashPath(sUploadDir)
sUploadDir = sUploadDir & Replace(sCusDir,"/", "\")
Select Case sType
Case "FILE"
sAllowExt = ""
Case "MEDIA"
sAllowExt = "rm|mp3|wav|mid|midi|ra|avi|mpg|mpeg|asf|asx|wma|mov"
Case "FLASH"
sAllowExt = "swf"
Case Else
sAllowExt = "bmp|jpg|jpeg|png|gif"
End Select
sPathShareImage = GetSlashPath(Server.Mappath("../sharefile/image/"))
sPathShareFlash = GetSlashPath(Server.Mappath("../sharefile/flash/"))
sPathShareMedia = GetSlashPath(Server.Mappath("../sharefile/media/"))
sPathShareOther = GetSlashPath(Server.Mappath("../sharefile/other/"))
End Sub
Function GetSlashPath(str)
If Right(str,1)<>"\" Then
GetSlashPath = str & "\"
Else
GetSlashPath = str
End If
End Function
Function CheckValidDir(s_Dir)
Dim oFSO
Set oFSO = Server.CreateObject("Scripting.FileSystemObject")
CheckValidDir = oFSO.FolderExists(s_Dir)
Set oFSO = Nothing	
End Function
Function GetSizeUnit(n_Size)
GetSizeUnit = FormatNumber((n_Size / 1024), 2, -1, 0, 0) & " KB"
End Function
Function FormatTime(s_Time, n_Flag)
Dim y, m, d, h, mi, s
FormatTime = ""
If IsDate(s_Time) = False Then Exit Function
y = cstr(year(s_Time))
m = cstr(month(s_Time))
If len(m) = 1 Then m = "0" & m
d = cstr(day(s_Time))
If len(d) = 1 Then d = "0" & d
h = cstr(hour(s_Time))
If len(h) = 1 Then h = "0" & h
mi = cstr(minute(s_Time))
If len(mi) = 1 Then mi = "0" & mi
s = cstr(second(s_Time))
If len(s) = 1 Then s = "0" & s
Select Case n_Flag
Case 1
FormatTime = y & "-" & m & "-" & d & " " & h & ":" & mi & ":" & s
Case 2
FormatTime = y & "-" & m & "-" & d
Case 3
FormatTime = h & ":" & mi & ":" & s
Case 4
FormatTime = y & m & d
Case 5
FormatTime = y & m & d & h & mi & s
End Select
End Function
%>