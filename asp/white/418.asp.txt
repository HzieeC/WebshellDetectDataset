<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
<!-- #include file="Inc/md5.asp" -->

<% 
article_id=cint(request.querystring("id"))

act=Request("act")
If act="save" Then 
l_username=trim(request.form("username"))
l_password=trim(request.form("password"))
l_new_password=trim(request.form("new_password"))
l_renew_password=trim(request.form("renew_password"))
l_time=now()
l_password_md5=md5(l_password,16)
if  l_password="" or l_new_password="" or l_renew_password="" then
response.Write "<script language='javascript'>alert('密码不能为空！');location.href='admin_edit.asp?id="&article_id&"';</script>"
else
set rs1=server.createobject("adodb.recordset")
sql="select * from web_admin where password='"&l_password_md5&"' and username='"&l_username&"'"
rs1.open(sql),cn,1,1
if  rs1.eof  then
response.Write "<script language='javascript'>alert('原有密码不正确，请重新输入！');location.href='admin_edit.asp?id="&article_id&"';</script>"
else
rs1.close
set rs1=nothing

if  l_new_password<>l_renew_password then
response.Write "<script language='javascript'>alert('两次输入的新密码不一致！');location.href='admin_edit.asp?id="&article_id&"';</script>"
else

set rs=server.createobject("adodb.recordset")
sql="select * from web_admin where username='"&l_username&"'"
rs.open(sql),cn,1,3
rs("password")=md5(l_new_password,16)
rs.update
rs.close
set rs=nothing

response.Write "<script language='javascript'>alert('修改成功！');location.href='admin_list.asp';</script>"
end if 
end if
end if
end if
 %>
 

	<%
Call header()

%>
<%
set rs=server.createobject("adodb.recordset")
sql="select * from web_admin where id="&article_id&""
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then
%>

  <form id="form1" name="form1" method="post" action="?act=save&id=<%=rs("id")%>">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.password.value == '' ) {
window.alert('请输入原有密码^_^');
document.form1.password.focus();
return false;}

if ( document.form1.new_password.value == '' ) {
window.alert('请输入新密码^_^');
document.form1.new_password.focus();
return false;}

if ( document.form1.renew_password.value == '' ) {
window.alert('请重复输入新密码^_^');
document.form1.renew_password.focus();
return false;}

return true;}
</script>
	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th class='tableHeaderText' colspan=2 height=25>修改用户密码</th>
	<tr>
	  <td height=23 class='forumRow'>用户名</td>
	  <td class='forumRow'><span class="forumRowHighLight">
	    <input name='username' type='text' id='username' size='30' maxlength="20"  value="<%=rs("username")%>" readonly>
	  </span></td>
	  </tr>
	<tr>
	<td width="15%" height=23 class='forumRowHighLight'>原有密码 (必填) </td>
	<td width="85%" class='forumRowHighLight'><input name='password' type='password' id='password' size='30'   maxlength="20">
	  &nbsp;</td>
	</tr>
	  <tr>
	    <td class='forumRow' height=23>新密码 (必填) </td>
	    <td class='forumRow'><input name='new_password' type='password' id='new_password' size='30'   maxlength="20"></td>
      </tr>
	  
	  <tr>
	    <td class='forumRowHighLight' height=23>重复新密码 <span class="forumRowHighLight">(必填) </span></td>
	    <td class='forumRowHighLight'><input name='renew_password' type='password' id='renew_password' size='30'   maxlength="20"></td>
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