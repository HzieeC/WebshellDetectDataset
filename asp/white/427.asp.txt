<%
Function CreateFolderB(NewFolderDir)
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath(NewFolderDir)) Then
response.Write "<script language='javascript'>alert('该文件夹已经创建！');history.go(-1);</script>"
else
Fso.CreateFolder(Server.MapPath(NewFolderDir))
end if
set fso=nothing
End Function
%>