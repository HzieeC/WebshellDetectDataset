<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
<!-- #include file="../inc/x_to_html/article_to_html.asp" -->
<!-- #include file="../inc/x_to_html/blog_index_to_html.asp" -->
<!-- #include file="../inc/x_to_html/blog_class_list_to_html.asp" -->
	<%
Call header()
%>
<%
'文章内容文件夹获取
set rs_1=server.createobject("adodb.recordset")
sql="select FileName,FolderName from web_Models_type where [id]=9"
rs_1.open(sql),cn,1,1
if not rs_1.eof and rs_1("FolderName")<>"" then
ArticleContent_FolderName="/"&rs_1("FolderName")
end if
rs_1.close
set rs_1=nothing
%>

	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>审核文章</th>
	
	<tr><td height="400" valign="top"  class='forumRow'><br>
	    <table width="90%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" bgcolor="#B1CFF8"><div align="center"></div></td>
          </tr>
          <tr>
            <td height="100">
			<%page=request.querystring("page")
			act=request.querystring("act")
			keywords=request.querystring("keywords")
			article_id=cint(request.querystring("id"))
			set rs_v=server.createobject("adodb.recordset")
sql="select id,view_yes,file_path from article where id="&article_id&""
rs_v.open(sql),cn,1,3
FilePath=rs_v("file_path")
if rs_v("view_yes")=0 then
rs_v("view_yes")=1
a_id=rs_v("id")
call article_to_html(a_id)
else
rs_v("view_yes")=0
'先判断文件是否存在，否则删除
Set fso=Server.CreateObject("Scripting.FileSystemObject")
If fso.FileExists(Server.MapPath(ArticleContent_FolderName&"/"&FilePath)) then
FilePath=ArticleContent_FolderName&"/"&FilePath
call DelFile(FilePath)
end if
end if
rs_v.update
rs_v.close
set rs_v=nothing

call blog_index_to_html()
call index_to_html()
response.Write "<script language='javascript'>alert('修改成功！');location.href='article_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
			%></td>
          </tr>
        </table>
	    </td>
	</tr>
	</table>


<%
Call DbconnEnd()
 %>