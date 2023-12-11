<!-- #include file="../html_clear.asp" -->
<!-- #include file="../web_AdvancedSettings.asp" -->

<%'容错处理
function article_to_html(a_id)
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

'博客文件夹获取
set rs_1=server.createobject("adodb.recordset")
sql="select FolderName from web_Models_type where [id]=2"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Blog_FolderName=rs_1("FolderName")
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
%>
<% 
'模板类型获取
set rs_1=server.createobject("adodb.recordset")
sql="select FileName,FolderName from web_Models_type where [id]=9"
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
<%
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

<% ' 文章内容读取替换
set rs=server.createobject("adodb.recordset")
sql="select * from [article] where [id]="&a_id&""
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then
article_title=rs("title")
article_keywords=rs("keywords")
article_description=rs("description")
article_from_url=rs("from_url")
article_time=rs("time")
article_from_name=rs("from_name")
ArticleContent=rs("content")
article_time=rs("time")
Article_FilePath=rs("file_path")
if rs("from_name")="" then
article_from_name=web_name
end if
if rs("url")<>"" then
article_refresh="<meta http-equiv='refresh' content='0;url="&rs("url")&"'>"
end if

if rs("from_url")="" then
set rs_url=server.createobject("adodb.recordset")
sql="select url from web_article_author where [name]='"&article_from_name&"'"
rs_url.open(sql),cn,1,1
if not rs_url.eof then
article_from_url=rs_url("url")
else
article_from_url="/"
end if
rs_url.close
set rs_url=nothing
end if


'您现在的位置读取替换
set rs1=server.createobject("adodb.recordset")
sql="select [name],[id],[folder] from [category] where [id]="&cint(rs("cid"))&""
rs1.open(sql),cn,1,1
if not rs1.eof and not rs1.bof then
article_position=article_position&"<a href='/"&BlogClass_FolderName&"/"&rs1("folder")&"/'>"&rs1("name")&"</a>"

if rs("pid")<>"" then
set rs2=server.createobject("adodb.recordset")
sql="select [name],[id],[folder] from [category] where [id]="&cint(rs("pid"))&""
rs2.open(sql),cn,1,1
if not rs2.eof and not rs2.bof then
article_position=article_position&" > <a href='/"&BlogClass_FolderName&"/"&rs2("folder")&"/'>"&rs2("name")&"</a>"

if rs("ppid")<>"" then
set rs3=server.createobject("adodb.recordset")
sql="select [name],[id],[folder] from [category] where [id]="&cint(rs("ppid"))&""
rs3.open(sql),cn,1,1
if not rs3.eof and not rs3.bof then
article_position=article_position&" > <a href='/"&BlogClass_FolderName&"/"&rs3("folder")&"/'>"&rs3("name")&"</a>"

end if
rs3.close
set rs3=nothing
end if

end if
rs2.close
set rs2=nothing
end if

end if 
rs1.close
set rs1=nothing

end if 
rs.close
set rs=nothing



'热点关键词读取替换,相关新闻读取替换
if article_keywords<>"" then
article_keywords1=split(article_keywords,"，")
c=ubound(article_keywords1)
article_kw=article_kw&"<div class='ContentTags'>Tags："
for i=0 to c
article_kw=article_kw&"<a href='/"&Search_FolderName&"/index.asp?q="&article_keywords1(i)&"' target='_blank' title='"&article_keywords1(i)&"'>"&article_keywords1(i)&"</a> "

if i=0 then
kw_sql=kw_sql&"where  [id]<"&a_id&"  and view_yes=1 and  ([title] like '%"&article_keywords1(i)&"%'"
else
kw_sql=kw_sql&" or [title] like '%"&article_keywords1(i)&"%'"
end if
next
kw_sql=kw_sql&")"
article_kw=article_kw&"</div>"

set rs=server.createobject("adodb.recordset")
sql="select top 7 [title],[file_path],[hit],[url] from [article] "&kw_sql&" order by [time] desc"
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then
article_refer=article_refer&"<dl><dt>相关文章</dt>"
do while not rs.eof
rs_url="/"&Article_FolderName&"/"&rs("file_path")

article_refer=article_refer&"<dd><a href='"&rs_url&"' title='"&rs("title")&"'  target='_blank'>"&left(rs("title"),40)&"</a><span class='HighLight'>&nbsp;("&rs("Hit")&"人浏览)</span></dd>"
rs.movenext
loop
article_refer=article_refer&"</dl>"
end if

rs.close
set rs=nothing
end if


'文章评论列表
set rs=server.createobject("adodb.recordset")
sql="select [content],recontent,[ip],[time],[name] from web_article_comment where view_yes=1 and  article_id="&a_id&"   order by [time] "&web_FeedTime&""
rs.open(sql),cn,1,1
article_count=rs.recordcount
if not rs.eof then
rs_order=0
do while not rs.eof
if rs("name")<>"" then
comment_name=rs("name")
else
comment_name=rs("ip")
end if
if rs("recontent")<>"" then
comment_replay="<br><b>[博主回复]&nbsp;&nbsp;</b><span>"&rs("recontent")&"</span>"
else
comment_replay=""
end if
rs_order=rs_order+1
article_comment=article_comment&"<dl><dt>"&rs_order&"楼 <b>"&comment_name&"</b>&nbsp;&nbsp;发表于&nbsp;&nbsp;"&rs("time")&"</dt>"
article_comment=article_comment&"<dd>"&rs("content")&comment_replay&"</dd></dl>"
rs.movenext
loop
article_comment=article_comment&"<div  class='clearfix'></div>"
end if
rs.close
set rs=nothing
%>
<%
ArticlePageContent=split(ArticleContent,"<hr />")
c=ubound(ArticlePageContent)
if c>0 then
for i=0 to c

if i=0 then
PageNO=""
else
PageNO=i+1
PageNO="("&PageNO&")"
end if
%>
<%
'分页部分
PageList=""
PageList=PageList&"<div class='t_page ColorLink'>"
PageList=PageList&"当前页数：<span class='FontRed'>" & i+1 & "</span>/" & c+1 &"&nbsp;"
PageList=PageList&"<a href="&Article_FilePath&">" & "首页" & "</a>"
select case i
case 0
PageList=PageList&"&nbsp;&nbsp;上一页&nbsp;&nbsp;"
case 1
PageList=PageList&"<a href="&Article_FilePath&">" & "上一页" & "</a>"
case else
PageList=PageList&"<a href="&i-1&Article_FilePath&">" & "上一页" & "</a>"
end select
   
for counter=0 to c

if counter=0 then
if counter=i then
PageList=PageList&"&nbsp;&nbsp;1&nbsp;&nbsp;"
else
PageList=PageList&"<a  href="&Article_FilePath&">" & 1 & "</a> "
end if

else
if counter=i then
PageList=PageList&"&nbsp;&nbsp;"&counter+1&"&nbsp;&nbsp;"
else
PageList=PageList&"<a  href="&counter&Article_FilePath&">" & counter+1 & "</a> "
end if

end if
next

if i=c then
PageList=PageList&"&nbsp;&nbsp;下一页&nbsp;&nbsp;"
else
PageList=PageList&"<a href="&i+1&Article_FilePath&">" & "下一页" & "</a>"
end if

PageList=PageList&"<a href="&c&Article_FilePath&">" & "尾页" & "</a></div>"
%>
<%
'读取模板内容
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
replace_code=replace(replace_code,"$web_menu$",web_menu)
replace_code=replace(replace_code,"$web_slide$",web_slide)
replace_code=replace(replace_code,"$web_indexsearch$",web_indexsearch)
replace_code=replace(replace_code,"$PageNO$",PageNO)

replace_code=replace(replace_code,"$web_slidealbum$",web_slidealbum)
replace_code=replace(replace_code,"$web_category$",web_category)
replace_code=replace(replace_code,"$web_hotart$",web_hotart)
replace_code=replace(replace_code,"$web_link$",web_link)

replace_code=replace(replace_code,"$article_id$",a_id) 
replace_code=replace(replace_code,"$article_title$",article_title)
replace_code=replace(replace_code,"$article_keywords$",article_keywords)
replace_code=replace(replace_code,"$article_description$",article_description)
replace_code=replace(replace_code,"$article_from_url$",article_from_url)
replace_code=replace(replace_code,"$article_time$",article_time)
replace_code=replace(replace_code,"$article_from_name$",article_from_name)
replace_code=replace(replace_code,"$article_content$",ArticlePageContent(i))
replace_code=replace(replace_code,"$article_count$",article_count)
replace_code=replace(replace_code,"$article_position$",article_position)
replace_code=replace(replace_code,"$article_next$",article_next)
replace_code=replace(replace_code,"$article_comment$",article_comment)
replace_code=replace(replace_code,"$article_kw$",article_kw)
replace_code=replace(replace_code,"$article_refer$",article_refer)
replace_code=replace(replace_code,"$article_refresh$",article_refresh)

replace_code=replace(replace_code,"$PageList$",PageList)

%>
<% '判断文件夹是否存在，否则创建
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath(Model_FolderName))=false Then
NewFolderDir=Model_FolderName
call CreateFolderB(NewFolderDir)
end if
%>
<%'声明HTML文件名,指定文件路径
if i=0 then
filepath=Model_FolderName&"/"&Article_FilePath
else
filepath=Model_FolderName&"/"&i&Article_FilePath
end if
%>
<% '生成静态文件
Set fout = fso.CreateTextFile(Server.MapPath(filepath))
fout.WriteLine replace_code
fout.close
fso.close
set fso=nothing
%>
<%
next
else
%>
<%
'读取模板内容
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
replace_code=replace(replace_code,"$web_menu$",web_menu)
replace_code=replace(replace_code,"$web_slide$",web_slide)
replace_code=replace(replace_code,"$web_indexsearch$",web_indexsearch)
replace_code=replace(replace_code,"$PageNO$","")

replace_code=replace(replace_code,"$web_slidealbum$",web_slidealbum)
replace_code=replace(replace_code,"$web_category$",web_category)
replace_code=replace(replace_code,"$web_hotart$",web_hotart)
replace_code=replace(replace_code,"$web_link$",web_link)

replace_code=replace(replace_code,"$article_id$",a_id) 
replace_code=replace(replace_code,"$article_title$",article_title)
replace_code=replace(replace_code,"$article_keywords$",article_keywords)
replace_code=replace(replace_code,"$article_description$",article_description)
replace_code=replace(replace_code,"$article_from_url$",article_from_url)
replace_code=replace(replace_code,"$article_time$",article_time)
replace_code=replace(replace_code,"$article_from_name$",article_from_name)
replace_code=replace(replace_code,"$article_content$",ArticleContent)
replace_code=replace(replace_code,"$article_count$",article_count)
replace_code=replace(replace_code,"$article_position$",article_position)
replace_code=replace(replace_code,"$article_next$",article_next)
replace_code=replace(replace_code,"$article_comment$",article_comment)
replace_code=replace(replace_code,"$article_kw$",article_kw)
replace_code=replace(replace_code,"$article_refer$",article_refer)
replace_code=replace(replace_code,"$article_refresh$",article_refresh)

replace_code=replace(replace_code,"$PageList$",PageList)
%>
<% '判断文件夹是否存在，否则创建
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath(Model_FolderName))=false Then
NewFolderDir=Model_FolderName
call CreateFolderB(NewFolderDir)
end if
%>
<%'声明HTML文件名,指定文件路径
filepath=Model_FolderName&"/"&Article_FilePath
%>
<% '生成静态文件
Set fout = fso.CreateTextFile(Server.MapPath(filepath))
fout.WriteLine replace_code
fout.close
fso.close
set fso=nothing
%>
<%
end if
%>
<%
end function
%>