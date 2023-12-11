<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
<!-- #include file="Inc/md5.asp" -->


<% '添加数据到数据表
act=Request("act")
If act="save" Then 
l_username=trim(request.form("username"))
l_password=trim(request.form("password"))
l_repassword=trim(request.form("repassword"))
l_class=request.form("class")
l_time=now()

if  l_username="" or l_password="" or l_repassword="" then
response.Write "<script language='javascript'>alert('用户名或密码不能为空！');location.href='admin_add.asp';</script>"
else
if  l_password<>l_repassword then
response.Write "<script language='javascript'>alert('两次输入的密码不一致！');location.href='admin_add.asp';</script>"
else
set rs1=server.createobject("adodb.recordset")
sql="select * from web_admin where username='"&l_username&"'"
rs1.open(sql),cn,1,1
if not rs1.eof and not rs1.bof then
response.Write "<script language='javascript'>alert('此用户名已经存在，请重新命名！');location.href='admin_add.asp';</script>"
else
rs1.close
set rs1=nothing

'加入管理员表
set rs=server.createobject("adodb.recordset")
sql="select * from web_admin"
rs.open(sql),cn,1,3
rs.addnew
rs("username")=l_username
rs("password")=md5(l_password,16)
if l_class<>"" then
rs("class")=cint(l_class)
end if
rs("time")=l_time
rs.update
rs.close
set rs=nothing

response.Write "<script language='javascript'>alert('添加成功！');location.href='admin_list.asp';</script>"
end if 
end if
end if
end if
 %>
 

	<%
Call header()

%>

  <form id="form1" name="form1" method="post" action="?act=save">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.username.value == '' ) {
window.alert('请输入用户名^_^');
document.form1.username.focus();
return false;}

if ( document.form1.password.value == '' ) {
window.alert('请输入密码^_^');
document.form1.password.focus();
return false;}

if ( document.form1.repassword.value == '' ) {
window.alert('请重复输入密码^_^');
document.form1.repassword.focus();
return false;}

return true;}
</script>
	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th class='tableHeaderText' colspan=2 height=25>添加后台用户</th>
	<tr>
	  <td height=23 class='forumRow'>&nbsp;</td>
	  <td class='forumRow'>&nbsp;</td>
	  </tr>
	<tr>
	<td width="15%" height=23 class='forumRowHighLight'>用户名 (必填) </td>
	<td width="85%" class='forumRowHighLight'><input name='username' type='text' id='username' size='30' maxlength="20">
	  &nbsp;</td>
	</tr>
	  <tr>
	    <td class='forumRow' height=23>密码 (必填) </td>
	    <td class='forumRow'><input name='password' type='password' id='password' size='30' maxlength="20"></td>
      </tr>
	  
	  <tr>
	    <td class='forumRowHighLight' height=23>重复密码 <span class="forumRowHighLight">(必填) </span></td>
	    <td class='forumRowHighLight'><input name='repassword' type='password' id='repassword' size='30' maxlength="20"></td>
      </tr>
	  <% if logr() then%>
	    <tr>
	    <td class='forumRow' height=23>用户权限<span class="forumRowHighLight"> </span></td>
	    <td class='forumRow'><label>
	      <input type="radio" name="class" value="9">
	      超级管理员&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
	      <input name="class" type="radio" value="0" checked>
	      一般用户</label></td>
	    </tr>
		<%end if%>
	<tr><td height="50" colspan=2  class='forumRow'><div align="center">
	  <INPUT type=submit value='提交' onClick='javascript:return checksignup1()' name=Submit>
	  </div></td></tr>
	</table>
</form>

<%
Call DbconnEnd()
 %>