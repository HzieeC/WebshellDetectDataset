<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->

	<%
Call header()
%>


	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>改变用户权限</th>
	
	<tr><td height="400" valign="top"  class='forumRow'><br>
	    <table width="90%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" bgcolor="#B1CFF8"><div align="center"></div></td>
          </tr>
          <tr>
            <td height="100">
			<%
			article_id=cint(request.querystring("id"))
						
			set rs=server.createobject("adodb.recordset")
sql="select id,class from web_admin where id="&article_id&""
rs.open(sql),cn,1,3
if not rs.eof and not rs.bof then
if logr() then
if rs("class")=0 then
rs("class")=9
else
rs("class")=0
end if
rs.update
rs.close
set rs=nothing
response.Write "<script language='javascript'>alert('修改成功！');location.href='admin_list.asp';</script>"
else
response.Write "<script language='javascript'>alert('您是普通用户，没有权限进行修改！');location.href='admin_list.asp';</script>"

end if
else
response.Write "<script language='javascript'>alert('没有找到相关数据！');location.href='admin_list.asp';</script>"
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