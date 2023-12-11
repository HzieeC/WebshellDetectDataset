<!-- #include file="../access.asp" -->
<!-- #include file="../html_clear.asp" -->
<%'容错处理
function album_index_to_html( )
On Error Resume Next
%>
<% '文件夹获取
'文章文件夹获取
set rs_1=server.createobject("adodb.recordset")
sql="select FolderName from web_Models_type where [id]=9"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Article_FolderName=rs_1("FolderName")
end if
rs_1.close
set rs_1=nothing

'分类文件夹获取
set rs_1=server.createobject("adodb.recordset")
sql="select FolderName from web_Models_type where [id]=3"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
BlogClass_FolderName=rs_1("FolderName")
end if
rs_1.close
set rs_1=nothing

'相册展示页文件夹获取
set rs_1=server.createobject("adodb.recordset")
sql="select FolderName from web_Models_type where [id]=5"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Gallery_FolderName=rs_1("FolderName")
end if
rs_1.close
set rs_1=nothing

'相册列表页文件夹获取
set rs_1=server.createobject("adodb.recordset")
sql="select FolderName from web_Models_type where [id]=4"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Album_FolderName=rs_1("FolderName")
end if
rs_1.close
set rs_1=nothing

'搜索文件夹获取
set rs_1=server.createobject("adodb.recordset")
sql="select FolderName from web_Models_type where [id]=11"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Search_FolderName=rs_1("FolderName")
end if
rs_1.close
set rs_1=nothing
%>

<%
'模板类型获取
set rs_1=server.createobject("adodb.recordset")
sql="select FileName,FolderName from web_Models_type where [id]=4"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Model_FileName=rs_1("FileName")
if rs_1("FolderName")<>"" then
Model_FolderName="/"&rs_1("FolderName")
end if
end if
rs_1.close
set rs_1=nothing
%>
<!--common use start-->
<%
'首页基本信息内容读取替换
set rs=server.createobject("adodb.recordset")
sql="select web_name,web_url,web_slogan,web_image,web_title,web_copyright,web_person,web_birthdate,web_birthplace,web_email,web_shortintro,web_theme from web_settings"
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then
web_name=rs("web_name")
web_url=rs("web_url")
web_slogan=rs("web_slogan")
web_image=rs("web_image")
web_title=rs("web_title")
web_copyright=rs("web_copyright")
web_person=rs("web_person")
web_birthdate=rs("web_birthdate")
web_birthplace=rs("web_birthplace")
web_email=rs("web_email")
web_shortintro=rs("web_shortintro")
web_theme=rs("web_theme")
end if
rs.close
set rs=nothing


'顶部导航
set rs=server.createobject("adodb.recordset")
sql="select * from web_menu where view_yes=1 order by [order]"
rs.open(sql),cn,1,1
if not rs.eof then
web_menu=""
web_menu=web_menu&"<ul>"
for i=1 to rs.recordcount
if rs("blank_yes")=1 then
BlankYes="target='_blank'"
else
BlankYes=""
end if
if "/"&Album_FolderName&"/"=rs("url") then
web_menu=web_menu&"<li class='current_page_item'><a href='"&rs("url")&"' "&BlankYes&">"&rs("name")&"</a></li> "
else
web_menu=web_menu&"<li><a href='"&rs("url")&"' "&BlankYes&">"&rs("name")&"</a></li> "
end if
rs.movenext
next
web_menu=web_menu&"</ul>"
end if
rs.close
set rs=nothing


'顶部幻灯
set rs=server.createobject("adodb.recordset")
sql="select top 5 [name],[image],[url],[position] from web_ad  where index_push=1 and view_yes=1 order by [order]"
rs.open(sql),cn,1,1
if not rs.eof then
do while not rs.eof 
if rs("url")<>"" then
img_position=rs("url")
else
img_position="/"&Gallery_FolderName&"/"&rs("position")
end if
web_slide=web_slide&"<TD class='td_f'><A href='"&img_position&"' target='_blank' title='"&rs("name")&"'><IMG src='/photos/"&rs("image")&"'></A></TD>"
rs.movenext
loop
end if
rs.close
set rs=nothing


'热门搜索读取替换
set rs=server.createobject("adodb.recordset")
sql="select top 1 [name] from web_keywords where index_yes=1 order by [time] desc"
rs.open(sql),cn,1,1
if not rs.eof  then
web_indexsearch=rs("name")
end if
rs.close
set rs=nothing


'右侧最新相册图片
set rs=server.createobject("adodb.recordset")
sql="select top "&web_SideImage&" [name],[image],[position],[url] from web_ad  where view_yes=1 order by [time] desc"
rs.open(sql),cn,1,1
if not rs.eof then
web_slidealbum=""
for i=1 to web_SideImage
web_slidealbum=web_slidealbum&"<div class='slide'>"
web_slidealbum=web_slidealbum&"<A href='/"&Gallery_FolderName&"/"&rs("position")&"/' target='_blank' title='"&rs("name")&"'><img class='diapo' src='/photos/"&rs("image")&"' alt='"&rs("name")&"' width='210' ></a>"
web_slidealbum=web_slidealbum&"<div class='text'>"
web_slidealbum=web_slidealbum&"<span class='title'>"&rs("name")&"</span>"
web_slidealbum=web_slidealbum&"</div></div>"
rs.movenext
next
else
web_slidealbum="暂无图片。"
end if
rs.close
set rs=nothing


'右侧文章分类
set rs=server.createobject("adodb.recordset")
sql="select top "&web_SideClass&" [name],[id],[folder] from [category] where index_push=1 and ClassType=1 order by time"
rs.open(sql),cn,1,1
if not rs.eof then
web_category=""
web_category=web_category&"<ul>"
for i=1 to web_SideClass
set rs_article=server.createobject("adodb.recordset")
sql="select [id] from [article] where cid='"&rs("id")&"'"
rs_article.open(sql),cn,1,1
rs_article_count=rs_article.recordcount
rs_article.close
set rs_article=nothing
web_category=web_category&"<li><a href='/"&BlogClass_FolderName&"/"&rs("folder")&"/'>"&rs("name")&"</a> ("&rs_article_count&") <a href='/rss/Feed.asp?CatID="&rs("id")&"' target='_blank'><img src='/images/rss_icon.gif'></a></li>"
rs.movenext
next
web_category=web_category&"</ul>"
else
web_category="暂无分类。"
end if
rs.close
set rs=nothing


'右侧热门文章
set rs=server.createobject("adodb.recordset")
sql="select top "&web_SideHot&" [title],[hit],[url],file_path from [article] where view_yes=1 order by [hit] desc"
rs.open(sql),cn,1,1
if not rs.eof then
web_hotart=""
web_hotart=web_hotart&"<ul>"
for i=1 to web_SideHot 
rs_url="/"&Article_FolderName&"/"&rs("file_path")

web_hotart=web_hotart&"<li><a href='"&rs_url&"' target='_blank'>"&left(rs("title"),17)&"</a> ("&rs("hit")&")</li>"
rs.movenext
next
web_hotart=web_hotart&"</ul>"
else
web_hotart="暂无信息。"
end if
rs.close
set rs=nothing



%>
<!--common use end-->
<%
 '文章列表读取替换
sql="select [id] from [web_ad_position] where view_yes=1  order by [time] desc"
set rs1=server.createObject("ADODB.Recordset")
rs1.open sql,cn,1,1
%><%
if not rs1.eof then
rs1.pagesize=10
totalpage=rs1.pagecount

for j=1 to totalpage
sql="select [name],[id],[time] from [web_ad_position] where view_yes=1  order by [time] desc"
set rs=server.createObject("ADODB.Recordset")
rs.open sql,cn,1,1
if not rs.eof then
rscount=rs.recordcount
whichpage=j 
rs.pagesize=10
totalpage=rs.pagecount
rs.absolutepage=whichpage
howmanyrecs=0
list_block=""
do while not rs.eof and howmanyrecs<rs.pagesize
%><%
set rs_img=server.createobject("adodb.recordset")
sql="select [image] from web_ad where [position]="&rs("id")&"  and view_yes=1 order by [order]"
rs_img.open(sql),cn,1,1
if not rs_img.eof then
album_image=rs_img("image")
else
album_image="nophotos.jpg"
end if
album_count=rs_img.recordcount
rs_img.close
set rs_img=nothing

list_block=list_block&"<div class='albumblock'><div class='inner'>"
list_block=list_block&"<a href='/"&Gallery_FolderName&"/"&rs("id")&"/' target='_blank' title='"&rs("name")&"'><img src='/photos/"&album_image&"' width='260px' height='196'/>"
list_block=list_block&"<div class='albumtitle'>"&left(rs("name"),14)&"<span>("&album_count&")</span><br><span>("&year(rs("time"))&"/"&month(rs("time"))&"/"&day(rs("time"))&")</span></div></a></div></div>"
%>
<%
rs.movenext
howmanyrecs=howmanyrecs+1
loop
list_block=list_block&"<div class='clearfix'></div>"
end if
rs.close
set rs=nothing
%>
<%
'分页部分
list_page=""
list_page=list_page&"<div class='t_page'>"
list_page=list_page&"总数："&rscount&"条&nbsp;&nbsp;当前页数：<span class='FontRed'>" & j & "</span>/" & totalpage &""
list_page=list_page&"<a href=index.html"&">" & "首页" & "</a>"
if whichpage=1 then
list_page=list_page&"&nbsp;&nbsp;上一页&nbsp;&nbsp;"
else
if j=2  then
list_page=list_page&"<a href=index.html"&">" & "上一页" & "</a>"
else
list_page=list_page&"<a href=list_"&j-1&".html"&">" & "上一页" & "</a>"
end if
end if

if totalpage-j>4 then
PageNO=j+4
else
PageNO=totalpage
end if

for counter=j to PageNO
if counter=1 then
list_page=list_page&"<a href=index.html"&">" & counter & "</a> "
else
if counter=j then
list_page=list_page&"<a href=list_"&counter&".html"&"><span class='FontRed'>" & counter & "</span></a> "
else
list_page=list_page&"<a href=list_"&counter&".html"&">" & counter & "</a> "
end if
end if
next

if (whichpage>totalpage or whichpage=totalpage) then
list_page=list_page&"&nbsp;&nbsp;下一页&nbsp;&nbsp;"
else
list_page=list_page&"<a href=list_"&j+1&".html"&">" & "下一页" & "</a>"
end if
if totalpage=1 then
list_page=list_page&"<a href=index.html"&">" & "尾页" & "</a></div>"
else
list_page=list_page&"<a href=list_"&totalpage&".html"&">" & "尾页" & "</a></div>"
end if
%>
<%
 '读取模板内容
Set fso=Server.CreateObject("Scripting.FileSystemObject") 
Set htmlwrite=fso.OpenTextFile(Server.MapPath("/templates/"&web_theme&"/"&Model_FileName)) 
replace_code=htmlwrite.ReadAll() 
htmlwrite.close 
%>

<%'循环列表替换内容
list_block=list_block&list_page
replace_code=replace(replace_code,"$list_block$",list_block)   
%>
<%'循环其它替换内容
replace_code=replace(replace_code,"$web_name$",web_name)
replace_code=replace(replace_code,"$web_url$",web_url)
replace_code=replace(replace_code,"$web_image$",web_image)
replace_code=replace(replace_code,"$web_title$",web_title)
replace_code=replace(replace_code,"$web_copyright$",web_copyright)
replace_code=replace(replace_code,"$web_slogan$",web_slogan)
replace_code=replace(replace_code,"$web_person$",web_person)
replace_code=replace(replace_code,"$web_birthdate$",web_birthdate)
replace_code=replace(replace_code,"$web_birthplace$",web_birthplace)
replace_code=replace(replace_code,"$web_email$",web_email)
replace_code=replace(replace_code,"$web_shortintro$",web_shortintro)
replace_code=replace(replace_code,"$web_hotart$",web_hotart)
replace_code=replace(replace_code,"$web_theme$",web_theme)
replace_code=replace(replace_code,"$Search_FolderName$",Search_FolderName)
replace_code=replace(replace_code,"$Album_FolderName$",Album_FolderName)

replace_code=replace(replace_code,"$web_menu$",web_menu)
replace_code=replace(replace_code,"$web_slide$",web_slide)
replace_code=replace(replace_code,"$web_slidealbum$",web_slidealbum)
replace_code=replace(replace_code,"$web_category$",web_category)
replace_code=replace(replace_code,"$web_link$",web_link)
replace_code=replace(replace_code,"$web_indexsearch$",web_indexsearch)
%>

<% '判断文件夹是否存在，否则创建
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath(Model_FolderName))=false Then
NewFolderDir=Model_FolderName
call CreateFolderB(NewFolderDir)
end if
%>
<%
filepath=Model_FolderName&"/list_"&j&".html"
filepath_index=Model_FolderName&"/index.html"
if j>1 then
Set fso=Server.CreateObject("Scripting.FileSystemObject")
Set f=fso.CreateTextFile(Server.MapPath(filepath),true)
f.WriteLine replace_code
f.close
end if
if j=1 then
Set f=fso.CreateTextFile(Server.MapPath(filepath_index),true)
f.WriteLine replace_code
f.close
end if
%>
<%
next
else
j=1
%>
<%
 '读取模板内容
Set fso=Server.CreateObject("Scripting.FileSystemObject") 
Set htmlwrite=fso.OpenTextFile(Server.MapPath("/templates/"&web_theme&"/"&Model_FileName)) 
replace_code=htmlwrite.ReadAll() 
htmlwrite.close 
%>

<%'循环列表替换内容
replace_code=replace(replace_code,"$list_block$","暂无信息。")   
%>
<%'循环其它替换内容
replace_code=replace(replace_code,"$web_name$",web_name)
replace_code=replace(replace_code,"$web_url$",web_url)
replace_code=replace(replace_code,"$web_image$",web_image)
replace_code=replace(replace_code,"$web_title$",web_title)
replace_code=replace(replace_code,"$web_copyright$",web_copyright)
replace_code=replace(replace_code,"$web_slogan$",web_slogan)
replace_code=replace(replace_code,"$web_person$",web_person)
replace_code=replace(replace_code,"$web_birthdate$",web_birthdate)
replace_code=replace(replace_code,"$web_birthplace$",web_birthplace)
replace_code=replace(replace_code,"$web_email$",web_email)
replace_code=replace(replace_code,"$web_shortintro$",web_shortintro)
replace_code=replace(replace_code,"$web_hotart$",web_hotart)
replace_code=replace(replace_code,"$web_theme$",web_theme)
replace_code=replace(replace_code,"$Search_FolderName$",Search_FolderName)
replace_code=replace(replace_code,"$Album_FolderName$",Album_FolderName)

replace_code=replace(replace_code,"$web_menu$",web_menu)
replace_code=replace(replace_code,"$web_slide$",web_slide)
replace_code=replace(replace_code,"$web_slidealbum$",web_slidealbum)
replace_code=replace(replace_code,"$web_category$",web_category)
replace_code=replace(replace_code,"$web_link$",web_link)
replace_code=replace(replace_code,"$web_indexsearch$",web_indexsearch)
%>

<% '判断文件夹是否存在，否则创建
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath(Model_FolderName))=false Then
NewFolderDir=Model_FolderName
call CreateFolderB(NewFolderDir)
end if
%>
<%
filepath_index=Model_FolderName&"/index.html"
Set fso=Server.CreateObject("Scripting.FileSystemObject")
Set f=fso.CreateTextFile(Server.MapPath(filepath_index),true)
f.WriteLine replace_code
f.close
%>
<%end if
rs1.close
set rs1=nothing%>

<%end function%>