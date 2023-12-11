<!-- #include file="../access.asp" -->
<!-- #include file="../html_clear.asp" -->
<!-- #include file="../web_AdvancedSettings.asp" -->

<%'容错处理
function index_to_html()
On Error Resume Next
%>
<%
'首页基本信息内容读取替换
set rs=server.createobject("adodb.recordset")
sql="select web_name,web_slogan,web_url,web_image,web_title,web_keywords,web_description,web_copyright,web_person,web_birthdate,web_birthplace,web_email,web_shortintro,web_theme,web_ThemeID from web_settings"
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then
web_name=rs("web_name")
web_url=rs("web_url")
web_slogan=rs("web_slogan")
web_image=rs("web_image")
web_title=rs("web_title")
web_keywords=rs("web_keywords")
web_description=rs("web_description")
web_copyright=rs("web_copyright")
web_person=rs("web_person")
web_birthdate=rs("web_birthdate")
web_birthplace=rs("web_birthplace")
web_email=rs("web_email")
web_shortintro=rs("web_shortintro")
web_theme=rs("web_theme")
web_themeID=rs("web_ThemeID")
end if
rs.close
set rs=nothing
%>
<% '文件夹获取
'博客文件夹获取
set rs_1=server.createobject("adodb.recordset")
sql="select FolderName from web_Models_type where [id]=2"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Blog_FolderName=rs_1("FolderName")
end if
rs_1.close
set rs_1=nothing

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

<% '读取模板内容
'模板类型获取
set rs_1=server.createobject("adodb.recordset")
sql="select FileName,FolderName from web_Models_type where [id]=1"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Model_FileName=rs_1("FileName")
Model_FolderName=rs_1("FolderName")
end if
rs_1.close
set rs_1=nothing

Set fso=Server.CreateObject("Scripting.FileSystemObject") 
Set htmlwrite=fso.OpenTextFile(Server.MapPath("/templates/"&web_theme&"/"&Model_FileName)) 
replace_code=htmlwrite.ReadAll() 
htmlwrite.close 
%>
<%
replace_code=replace(replace_code,"$web_name$",web_name)
replace_code=replace(replace_code,"$web_url$",web_url)
replace_code=replace(replace_code,"$web_image$",web_image)
replace_code=replace(replace_code,"$web_title$",web_title)
replace_code=replace(replace_code,"$web_keywords$",web_keywords)
replace_code=replace(replace_code,"$web_description$",web_description)
replace_code=replace(replace_code,"$web_copyright$",web_copyright)
replace_code=replace(replace_code,"$web_slogan$",web_slogan)
replace_code=replace(replace_code,"$web_person$",web_person)
replace_code=replace(replace_code,"$web_birthdate$",web_birthdate)
replace_code=replace(replace_code,"$web_birthplace$",web_birthplace)
replace_code=replace(replace_code,"$web_email$",web_email)
replace_code=replace(replace_code,"$web_shortintro$",web_shortintro)
replace_code=replace(replace_code,"$web_theme$",web_theme)
replace_code=replace(replace_code,"$Search_FolderName$",Search_FolderName)
replace_code=replace(replace_code,"$Blog_FolderName$",Blog_FolderName)

'顶部导航
set rs=server.createobject("adodb.recordset")
sql="select * from web_menu where view_yes=1 order by [order]"
rs.open(sql),cn,1,1
if not rs.eof then
web_menu=web_menu&"<ul>"
for i=1 to rs.recordcount
if rs("blank_yes")=1 then
BlankYes="target='_blank'"
else
BlankYes=""
end if
if Model_FolderName&"/"=rs("url") then
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

replace_code=replace(replace_code,"$web_menu$",web_menu)



'顶部幻灯
set rs=server.createobject("adodb.recordset")
sql="select top 5 [title],[image],[file_path] from [article]  where slide_yes=1 and view_yes=1 order by [time] desc"
rs.open(sql),cn,1,1
if not rs.eof then
do while not rs.eof 
rs_url="/"&Article_FolderName&"/"&rs("file_path")
web_slide=web_slide&"<div class='slide'><a href='"&rs_url&"' target='_blank'> <img src='/images/up_images/"&rs("image")&"' width='590' height='300' alt='"&rs("title")&"' /> </a></div> "
rs.movenext
loop
end if
rs.close
set rs=nothing

replace_code=replace(replace_code,"$web_slide$",web_slide)



'热门搜索读取替换
set rs=server.createobject("adodb.recordset")
sql="select top 1 [name] from web_keywords where index_yes=1 order by [time] desc"
rs.open(sql),cn,1,1
if not rs.eof  then
web_indexsearch=rs("name")
end if
rs.close
set rs=nothing

replace_code=replace(replace_code,"$web_indexsearch$",web_indexsearch)



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

replace_code=replace(replace_code,"$web_slidealbum$",web_slidealbum)



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

replace_code=replace(replace_code,"$web_category$",web_category)


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

replace_code=replace(replace_code,"$web_hotart$",web_hotart)


'友情链接
set rs=server.createobject("adodb.recordset")
sql="select [name],[url],NoFollow from [web_link] where view_yes=1 order by [order]"
rs.open(sql),cn,1,1
if not rs.eof then
web_link=""
do while not rs.eof
if rs("NoFollow")=1 then
NoFollow="rel='NoFollow'"
else
NoFollow=""
end if
web_link=web_link&"<a href='"&rs("url")&"' target='_blank' "&NoFollow&">"&rs("name")&"</a> "
rs.movenext
loop
else
web_link="暂无链接。"
end if
rs.close
set rs=nothing

replace_code=replace(replace_code,"$web_link$",web_link)


'首页文章列表
set rs=server.createobject("adodb.recordset")
sql="select top "&web_ListCount&" [id],[title],[content],[cid],[keywords],[url],[author],[file_path],index_push,[time],[hit] from [article] where view_yes=1 order by index_push desc,time desc"
rs.open(sql),cn,1,1
if not rs.eof then
do while not rs.eof

rs_url="/"&Article_FolderName&"/"&rs("file_path")


article_kw=""
if rs("keywords")<>"" then
rs_keywords=split(rs("keywords"),"，")
c=ubound(rs_keywords)
article_kw=article_kw&"<div class='ArticleTags'>Tags："
for i=0 to c
article_kw=article_kw&"<a href='/"&Search_FolderName&"/index.asp?q="&rs_keywords(i)&"' target='_blank' title='"&rs_keywords(i)&"'>"&rs_keywords(i)&"</a> "
next
article_kw=article_kw&"</div>"
end if

article_class=""
set rs_class=server.createobject("adodb.recordset")
sql="select [id],[name],[folder] from category where id="&rs("cid")
rs_class.open(sql),cn,1,1
if not rs_class.eof then
article_class="<a href='/"&BlogClass_FolderName&"/"&rs_class("folder")&"/'>["&rs_class("name")&"]</a>&nbsp;&nbsp;&nbsp;&nbsp;"
end if
rs_class.close
set rs_class=nothing

'文章评论总数
set rs_c=server.createobject("adodb.recordset")
sql="select [id] from web_article_comment where view_yes=1 and  article_id="&rs("id")&" "
rs_c.open(sql),cn,1,1
if not rs_c.eof then
article_count=rs_c.recordcount
else
article_count=0
end if
rs_c.close
set rs_c=nothing


web_articlelist=web_articlelist&"<div class='post'>"
if rs("index_push")=1 then
web_articlelist=web_articlelist&"<h2 class='title'><span style='color:#ff0000'>[置顶]</span> <a href='"&rs_url&"' target='_blank'>"&left(rs("title"),23)&"</a></h2>"
else
web_articlelist=web_articlelist&"<h2 class='title'><a href='"&rs_url&"' target='_blank'>"&left(rs("title"),23)&"</a></h2>"
end if
web_articlelist=web_articlelist&"<p class='meta'>"
if web_ListTime=1 then
web_articlelist=web_articlelist&rs("time")
end if
if web_ListAuthor=1 then
web_articlelist=web_articlelist&"&nbsp;&nbsp;Posted by "&rs("author")
end if
web_articlelist=web_articlelist&"</p>"
if web_ListKeywords=1 then
web_articlelist=web_articlelist&article_kw
end if
web_articlelist=web_articlelist&"<div class='entry'>"
if web_ListIntro=1 then
web_articlelist=web_articlelist&"<p>"&left(Clearhtml(rs("content")),300)&"...</p>"
end if
web_articlelist=web_articlelist&"<p class='links'>"&article_class&"<a href='"&rs_url&"' target='_blank'>查看全文("&rs("hit")&")</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href='"&rs_url&"#comment' target='_blank'>发表评论("&article_count&")</a></p>"
web_articlelist=web_articlelist&"</div></div>"


rs.movenext
loop
end if
rs.close
set rs=nothing

replace_code=replace(replace_code,"$web_articlelist$",web_articlelist)
%>

<% '判断文件夹是否存在，否则创建
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath("/"&Model_FolderName))=false Then
NewFolderDir="/"&Model_FolderName
call CreateFolderB(NewFolderDir)
end if
%>

<%'声明HTML文件名,指定文件路径
if Model_FolderName<>"" then
filepath="/"&Model_FolderName&"/index.html"
else
filepath="/index.html"
end if
%>

<% '生成首页静态文件
Set fout = fso.CreateTextFile(Server.MapPath(filepath))
fout.WriteLine replace_code
fout.close
fso.close
set fso=nothing
end function
%>