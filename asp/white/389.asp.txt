<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
<!-- #include file="../inc/x_to_html/album_index_to_html.asp" -->

	<%
Call header()
%>


	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>删除图片</th>
	
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
if request("action")="AllDel" then
Num=request.form("Selectitem").count 
if Num=0 then 
response.Write "<script language='javascript'>alert('请选择要删除的数据！');location.href='ad_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
Response.End 
end if 
Selectitem=request.Form("Selectitem") 
article_id=split(Selectitem,",")
c=ubound(article_id)
for i=0 to c		
			set rs=server.createobject("adodb.recordset")
sql="select [image],SmallImage from web_ad where id="&cint(article_id(i))&""
rs.open(sql),cn,1,3
FilePath="/photos/"&rs("image")
if rs("SmallImage")<>"" then
FilePath1="/photos/"&rs("SmallImage")
end if
rs.delete
rs.close
set rs=nothing
'先判断文件是否存在，否则删除
Set fso=Server.CreateObject("Scripting.FileSystemObject")
If fso.FileExists(Server.MapPath(FilePath)) then
call DelFile(FilePath)
end if
if FilePath1<>"" then
FilePath=FilePath1
'先判断文件是否存在，否则删除
Set fso=Server.CreateObject("Scripting.FileSystemObject")
If fso.FileExists(Server.MapPath(FilePath)) then
call DelFile(FilePath)
end if
end if

next

else
			article_id=cint(request.querystring("id"))
			set rs=server.createobject("adodb.recordset")
sql="select [image],SmallImage from web_ad where id="&article_id&""
rs.open(sql),cn,1,3
FilePath="/photos/"&rs("image")
if rs("SmallImage")<>"" then
FilePath1="/photos/"&rs("SmallImage")
end if
rs.delete
rs.close
set rs=nothing
'先判断文件是否存在，否则删除
Set fso=Server.CreateObject("Scripting.FileSystemObject")
If fso.FileExists(Server.MapPath(FilePath)) then
call DelFile(FilePath)
end if
if FilePath1<>"" then
FilePath=FilePath1
'先判断文件是否存在，否则删除
Set fso=Server.CreateObject("Scripting.FileSystemObject")
If fso.FileExists(Server.MapPath(FilePath)) then
call DelFile(FilePath)
end if
end if

end if

'更新下首页和相册
call album_index_to_html()
call index_to_html()

response.Write "<script language='javascript'>alert('删除成功！');location.href='ad_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
			%>
			</td>
          </tr>
        </table>
	    </td>
	</tr>
	</table>


<%
Call DbconnEnd()
 %>