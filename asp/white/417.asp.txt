<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/album_index_to_html.asp" -->

	<%
Call header()
%>


	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>É¾³ýÏà²á</th>
	
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
			Gallery_FolderName=request.querystring("FolderName")
			set rs=server.createobject("adodb.recordset")
sql="select id from web_ad_position where id="&article_id&""
rs.open(sql),cn,1,3
FolderPath="/"&Gallery_FolderName&"/"&rs("id")
rs.delete
rs.close
set rs=nothing

Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath(FolderPath)) Then
call DelFolder(FolderPath)
end if

call album_index_to_html()

response.Write "<script language='javascript'>alert('É¾³ý³É¹¦£¡');location.href='ad_position_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
			%></td>
          </tr>
        </table>
	    </td>
	</tr>
	</table>


<%
Call DbconnEnd()
 %>