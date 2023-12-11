<%@ CODEPAGE=65001 %>
<%
'///////////////////////////////////////////////////////////////////////////////
'//              Z-Blog
'// 作    者:    
'// 版权所有:    RainbowSoft Studio
'// 技术支持:    rainbowsoft@163.com
'// 程序名称:    
'// 程序版本:    
'// 单元名称:    
'// 开始时间:    
'// 最后修改:    
'// 备    注:    FCK 表情附加
'///////////////////////////////////////////////////////////////////////////////
%>
<% Option Explicit %>
<% On Error Resume Next %>
<% Response.Charset="UTF-8" %>
<% Response.ContentType="application/x-javascript" %>
<!-- #include file="../../c_option.asp" -->
sBasePath	= "<%=ZC_BLOG_HOST%>image/face/";
aImages	= [
<%
	Response.Write "'"& Replace(ZC_EMOTICONS_FILENAME,"|",".gif','") &".gif'"
%>
] ;