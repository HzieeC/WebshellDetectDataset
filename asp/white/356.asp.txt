<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
<!-- #include file="../inc/x_to_html/album_index_to_html.asp" -->

<% '添加数据到数据表
act=Request("act")
If act="save" Then 
l_name=trim(request.form("name"))
l_memo=trim(request.form("memo"))
l_backmusic=trim(request.form("backmusic"))
l_time=now()

set rs=server.createobject("adodb.recordset")
sql="select * from web_ad_position"
rs.open(sql),cn,1,3
rs.addnew
rs("name")=l_name
rs("backmusic")=l_backmusic
rs("memo")=l_memo
rs("time")=l_time

rs.update
rs.close
set rs=nothing
%>

<%
call album_index_to_html()

response.Write "<script language='javascript'>alert('添加成功！');location.href='ad_position_list.asp';</script>"
end if 
 %>
 

	<%
Call header()

%>

  <form id="form1" name="form1" method="post" action="?act=save">
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
	  <th class='tableHeaderText' colspan=2 height=25>添加新的相册</th>
	<tr>
	  <td height=23 colspan="2" class='forumRow'><table width="100%" border="0" align="center" cellpadding="0" cellspacing="0">
        <tr>
          <td height="20" class='TipTitle'>&nbsp;√ 操作提示</td>
        </tr>
        <tr>
          <td height="30" valign="top" class="TipWords"><p>1、相册背景音乐可能只会在某些相册中生效，音乐地址必须是绝对地址。</p></td>
        </tr>
        <tr>
          <td height="10">&nbsp;</td>
        </tr>
      </table></td>
	  </tr>
	<tr>
	<td width="15%" height=23 class='forumRow'>相册名称 (必填) </td>
	<td width="85%" class='forumRow'><input name='name' type='text' id='name' size='70' maxlength="30">
	  &nbsp;</td>
	</tr>
	<tr>
	<td width="15%" height=23 class='forumRowHighLight'>相册背景音乐 (选填) </td>
	<td width="85%" class='forumRowHighLight'><input name='BackMusic' type='text' id='BackMusic' size='70' maxlength="200">
	  &nbsp;直接填入音乐的地址，如：http://www.hitux.com/music/sample.mp3</td>
	</tr>	<tr>
	  <td class='forumRow' height=11>备注</td>
	  <td class='forumRow'><textarea name='memo'  cols="100" rows="6" id="memo" ></textarea></td>
	</tr>
	<tr><td height="50" colspan=2  class='forumRow'><div align="center">
	  <INPUT type=submit value='提交' onClick='javascript:return checksignup1()' name=Submit>
	  </div></td></tr>
	</table>
</form>

<%
Call DbconnEnd()
 %>