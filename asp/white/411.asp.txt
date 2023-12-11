<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
<% '添加数据到数据表
act=Request("act")
If act="save" Then 
l_name=trim(request.form("name"))
l_url=trim(request.form("url"))
l_image=trim(request.form("web_image"))
l_memo=trim(request.form("memo"))
l_view_yes=trim(request.form("view_yes"))
l_time=now()

set rs=server.createobject("adodb.recordset")
sql="select * from web_theme where [folder]='"&l_url&"'"
rs.open(sql),cn,1,3
if not rs.eof then
response.Write "<script language='javascript'>alert('该文件夹已经存在，请重新命名！');history.back;</script>"
else
rs.addnew
rs("name")=l_name
rs("folder")=l_url 
rs("image")=l_image
rs("memo")=l_memo
rs("view_yes")=cint(l_view_yes)
rs("time")=l_time
rs.update
rs.close
set rs=nothing
%>
<% '判断主题文件夹是否存在，否则创建，需要在二个地方创建Templates,css
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath("/Templates/"&l_url))=false Then
NewFolderDir="/Templates/"&l_url
call CreateFolderB(NewFolderDir)
end if
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath("/css/"&l_url))=false Then
NewFolderDir="/css/"&l_url
call CreateFolderB(NewFolderDir)
end if
%>
<%
response.Write "<script language='javascript'>alert('添加成功！');location.href='ThemeSetting.asp';</script>"
end if 
end if
 %>
 

	<%
Call header()

%>

  <form id="form1" name="form1" method="post" action="?act=save">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.name.value == '' ) {
window.alert('请输入主题名称^_^');
document.form1.name.focus();
return false;}

if ( document.form1.url.value == '' ) {
window.alert('请输入主题文件夹^_^');
document.form1.url.focus();
return false;}


return true;}
</script>
	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th class='tableHeaderText' colspan=2 height=25>添加网站主题</th>
	<tr>
	<td width="15%" height=23 class='forumRow'>主题名称 (必填) </td>
	<td class='forumRow'><input name='name' type='text' id='name' size='40' maxlength="40">
	  &nbsp;</td>
	</tr>
	  <tr>
	    <td class='forumRowHighLight' height=23>主题文件夹 (必填) </td>
	    <td class='forumRowHighLight'><input name='url' type='text' id='url' size='40' maxlength="20">
          <span style="color: #FF0000">&nbsp;只能是英文，提交后不可修改</span></td>
      </tr>
	  <tr>
	    <td class='forumRow' height=23>主题图片</td>
	    <td width="85%" class='forumRow'><table width="100%" border="0" cellspacing="0" cellpadding="0">
         <tr>
           <td width="22%" ><input name="web_image" type="text" id="web_image"  size="30"></td>
           <td width="78%"  ><iframe width="500" name="ad" frameborder=0 height=30 scrolling=no src=upload.asp></iframe></td>
         </tr>
       </table></td>
      </tr><tr>
	  <td class='forumRowHighLight' height=11>备注</td>
	  <td class='forumRowHighLight'><textarea name='memo'  cols="100" rows="6" id="memo" ></textarea></td>
	</tr>
	  
	  <tr>
	  <td class='forumRow' height=23>是否可用</td>
	  <td class='forumRow'><label>
	    <input name="view_yes" type="radio" value="1" checked="checked">
      是
      &nbsp;
      <input name="view_yes" type="radio" value="0">
      否</label></td>
	</tr>
	<tr><td height="50" colspan=2  class='forumRow'><div align="center">
	  <INPUT type=submit value='提交' onClick='javascript:return checksignup1()' name=Submit>
	  </div></td></tr>
	</table>
</form>

<%
Call DbconnEnd()
 %>