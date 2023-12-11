<!-- #include file="../access.asp" -->
<!-- #include file="../html_clear.asp" -->

<%'容错处理
function Model_to_html(l_id)
On Error Resume Next
%>
<%
'模板类型和主题获取
set rs_1=server.createobject("adodb.recordset")
sql="select FileName from web_Models_type where [id]="&ModelType
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Model_FileName=rs_1("FileName")
end if
rs_1.close
set rs_1=nothing

set rs_1=server.createobject("adodb.recordset")
sql="select [folder] from web_theme where [id]="&ModelTheme
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Theme_Folder=rs_1("folder")
end if
rs_1.close
set rs_1=nothing

%>


<%
'模板内容读取替换
set rs=server.createobject("adodb.recordset")
sql="select [content] from web_models where id="&l_id
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then
Model_Content=rs("content")
end if
rs.close
set rs=nothing
%>
<% '读取模板内容
Set fso=Server.CreateObject("Scripting.FileSystemObject") 
Set htmlwrite=fso.OpenTextFile(Server.MapPath("/templates/common/template.html")) 
replace_code=htmlwrite.ReadAll() 
htmlwrite.close 
%>
<%
replace_code=replace(replace_code,"$Model_Content$",Model_Content)

%>
<% '判断主题文件夹是否存在，否则创建
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath("/Templates/"&Theme_Folder))=false Then
NewFolderDir="/Templates/"&Theme_Folder
call CreateFolderB(NewFolderDir)
end if
%>

<%'声明HTML文件名,指定文件路径
filepath="/Templates/"&Theme_Folder&"/"&Model_FileName
%>

<% '生成静态文件
Set fout = fso.CreateTextFile(Server.MapPath(filepath))
fout.WriteLine replace_code
fout.close
fso.close
set fso=nothing
end function
%>