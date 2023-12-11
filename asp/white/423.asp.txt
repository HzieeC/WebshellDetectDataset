<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->

	<%
Call header()
%>


	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>删除后台用户</th>
	
	<tr><td height="400" valign="top"  class='forumRow'><br>
	    <table width="90%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" bgcolor="#B1CFF8"><div align="center"></div></td>
          </tr>
          <tr>
            <td height="100">
			<%
			article_id=cint(request.querystring("id"))
			username=request.querystring("username")
			
			set rs=server.createobject("adodb.recordset")
sql="select id from web_admin where id="&article_id&""
rs.open(sql),cn,1,3
if rs("id")<>1 then
rs.delete
else
response.Write "<script language='javascript'>alert('用户admin不能删除！');location.href='admin_list.asp';</script>"
end if
rs.close
set rs=nothing

response.Write "<script language='javascript'>alert('删除成功！');location.href='admin_list.asp';</script>"
			%></td>
          </tr>
        </table>
	    </td>
	</tr>
	</table>


<%
Call DbconnEnd()
 %>