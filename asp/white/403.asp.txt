<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->

	<%
Call header()
%>


	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>删除模板分类</th>
	
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
sql="select id from web_Models_Type where id="&article_id&""
rs.open(sql),cn,1,3
if rs("id")<11 then
response.Write "<script language='javascript'>alert('为保证网站正常运行，系统默认的9个模板将无法删除！');location.href='Models_Type_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
else
rs.delete
rs.close
set rs=nothing
response.Write "<script language='javascript'>alert('删除成功！');location.href='Models_Type_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
end if
%></td>
          </tr>
        </table>
	    </td>
	</tr>
	</table>


<%
Call DbconnEnd()
 %>