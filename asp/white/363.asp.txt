<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->

	<%
Call header()
%>
<%
'分类文件夹获取
set rs_1=server.createobject("adodb.recordset")
sql="select FolderName from web_Models_type where [id]=3"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
BlogClass_FolderName=rs_1("FolderName")
end if
rs_1.close
set rs_1=nothing%>

	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>删除分类</th>
	
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
			set rs=server.createobject("adodb.recordset")
sql="select id from [category] where id="&article_id&""
rs.open(sql),cn,1,3
FolderPath="/"&BlogClass_FolderName&"/"&rs("id")
rs.delete

Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath(FolderPath)) Then
call DelFolder(FolderPath)
end if

response.Write "<script language='javascript'>alert('删除成功！');location.href='category_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
rs.close
set rs=nothing
			%></td>
          </tr>
        </table>
	    </td>
	</tr>
	</table>


<%
Call DbconnEnd()
 %>