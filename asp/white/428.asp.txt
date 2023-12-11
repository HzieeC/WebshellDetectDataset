<!-- #include file="antiattack.asp" -->
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
%>

<%
'网站基本信息内容读取替换
set rs=server.createobject("adodb.recordset")
sql="select web_name,web_url,web_slogan,web_image,web_title,web_copyright,web_person,web_birthdate,web_birthplace,web_shortintro,web_theme from web_settings"
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
web_shortintro=rs("web_shortintro")
web_theme=rs("web_theme")
end if
rs.close
set rs=nothing
%>