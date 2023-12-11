<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/dv_clsother.asp"-->
<%
Dim XmlDom,Root,forum

Call CreatForumXml()


Sub CreatForumXml()
	Set XmlDom = Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument")
	XmlDom.appendChild(XmlDom.createElement("Dvbbs"))
	Set forum = XmlDom.documentElement.appendChild(XmlDom.createNode(1,"forum",""))
	Rem ---------------------- 节点 ----------------------
	forum.setAttribute "Type",Dvbbs.forum_info(0)
	forum.setAttribute "URL",Dvbbs.forum_info(1)
	forum.setAttribute "Online",MyBoardOnline.Forum_Online
	forum.setAttribute "GuestOnline",MyBoardOnline.Forum_GuestOnline
	forum.setAttribute "UserOnline",MyBoardOnline.Forum_UserOnline
	forum.setAttribute "User",Dvbbs.CacheData(10,0)
	forum.setAttribute "Topic",Dvbbs.CacheData(7,0)
	forum.setAttribute "Post",Dvbbs.CacheData(8,0)
	forum.setAttribute "TodayPost",Dvbbs.CacheData(9,0)
	forum.setAttribute "YesterdayPost",Dvbbs.CacheData(11,0)
	Rem ---------------------- 节点 ----------------------

	Response.Clear
	Response.CharSet="gb2312"  '数据集
	Response.ContentType="text/xml"  '数据流格式定义
	Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"&vbNewLine
	Response.Write XmlDom.xml
	Response.End
End Sub

Set XmlDom = Nothing
Dvbbs.PageEnd()
%>