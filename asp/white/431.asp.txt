<%
'网站高级设置读取
set rs=server.createobject("adodb.recordset")
sql="select * from web_AdvancedSettings"
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then
web_ListTime=rs("web_ListTime")
web_ListIntro=rs("web_ListIntro")
web_ListCount=rs("web_ListCount")
web_ListKeywords=rs("web_ListKeywords")
web_ListAuthor=rs("web_ListAuthor")
web_FeedComment=rs("web_FeedComment")
web_FeedAdvice=rs("web_FeedAdvice")
web_FeedCount=rs("web_FeedCount")
web_FeedTime=rs("web_FeedTime")
web_SideImage=rs("web_SideImage")
web_SideClass=rs("web_SideClass")
web_SideHot=rs("web_SideHot")
end if
rs.close
set rs=nothing
%>