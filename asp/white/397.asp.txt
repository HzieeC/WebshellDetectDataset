<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
<!-- #include file="../inc/x_to_html/album_index_to_html.asp" -->
<% '添加数据到数据表
page=request.querystring("page")
act=request.querystring("act")
keywords=request.querystring("keywords")
article_id=cint(request.querystring("id"))


act1=Request("act1")
If act1="save" Then 
l_id=trim(request.form("l_id"))
l_name=trim(request.form("name"))
l_backmusic=trim(request.form("backmusic"))
l_memo=trim(request.form("memo"))
l_time=now()

set rs=server.createobject("adodb.recordset")
sql="select * from web_ad_position where id="&l_id&""
rs.open(sql),cn,1,3
rs("name")=l_name
rs("backmusic")=l_backmusic
rs("memo")=l_memo
rs.update
rs.close
set rs=nothing
call album_index_to_html()

response.Write "<script language='javascript'>alert('修改成功！');location.href='ad_position_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
end if 
 %>
 

	<%
Call header()

%>
<% set rs=server.createobject("adodb.recordset")
sql="select * from web_ad_position where id="&article_id&""
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then%>
  <form id="form1" name="form1" method="post" action="?act1=save&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.name.value == '' ) {
window.alert('请输入相册名称^_^');
document.form1.name.focus();
return false;}

return true;}
</script>
	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th class='tableHeaderText' colspan=2 height=25>修改相册</th>
	<tr>
	<td width="15%" height=23 class='forumRow'>相册名称 (必填) </td>
	<td width="85%" class='forumRow'><input name='name' type='text' id='name'  value="<%=rs("name")%>" size='70'>
	 <input name='l_id' type='hidden' id='l_id' value="<%=rs("id")%>" size='70'>
	  &nbsp;</td>
	</tr>
	<tr>
	<td width="15%" height=23 class='forumRowHighLight'>相册背景音乐 (选填) </td>
	<td width="85%" class='forumRowHighLight'><input name='BackMusic' type='text' id='BackMusic' size='70' value="<%=rs("backmusic")%>" maxlength="200">
	  &nbsp;直接填入音乐的地址，如：http://www.hitux.com/music/sample.mp3</td>
	</tr>	<tr>
	  <td class='forumRow' height=11>备注</td>
	  <td class='forumRow'><textarea name='memo'  cols="100" rows="6" id="memo" ><%=rs("memo")%></textarea></td>
	</tr>
	<tr><td height="50" colspan=2  class='forumRow'><div align="center">
	  <INPUT type=submit value='提交' onClick='javascript:return checksignup1()' name=Submit>
	  </div></td></tr>
	</table>
</form>
<%
else
response.write"未找到数据"
end if%>
<%
Call DbconnEnd()
 %>