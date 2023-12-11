<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->

<% '添加数据到数据表
act=Request("act")
If act="save" Then 
l_name=trim(request.form("name"))
l_memo=trim(request.form("memo"))
l_number=trim(request.form("number"))
l_order=trim(request.form("order"))
l_time=now()

set rs=server.createobject("adodb.recordset")
sql="select * from web_menu_type"
rs.open(sql),cn,1,3
rs.addnew
rs("name")=l_name
rs("memo")=l_memo
rs("number")=l_number
if l_order<>"" then
rs("order")=l_order
end if
rs("time")=l_time

rs.update
rs.close
set rs=nothing
response.Write "<script language='javascript'>alert('添加成功！');location.href='menu_type_list.asp';</script>"
end if 
 %>
 

	<%
Call header()

%>

  <form id="form1" name="form1" method="post" action="?act=save">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.name.value == '' ) {
window.alert('请输入分类名称^_^');
document.form1.name.focus();
return false;}

if ( document.form1.number.value == '' ) {
window.alert('请输入导航个数^_^');
document.form1.number.focus();
return false;}

if(document.form1.number.value.search(/^([0-9]*)([.]?)([0-9]*)$/)   ==   -1)   
      {   
  window.alert("导航个数只能是数字^_^");   
document.form1.number.focus();
return false;}

if(document.form1.order.value.search(/^([0-9]*)([.]?)([0-9]*)$/)   ==   -1)   
      {   
  window.alert("排序只能是数字^_^");   
document.form1.order.focus();
return false;}

return true;}
</script>
	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th class='tableHeaderText' colspan=2 height=25>添加导航分类</th>
	<tr>
	<td width="15%" height=23 class='forumRowHighLight'>分类名称 (必填) </td>
	<td width="85%" class='forumRowHighLight'><input name='name' type='text' id='name' size='70' maxlength="30">
	  &nbsp;</td>
	</tr>
	<tr>
	  <td class='forumRow' height=11>导航个数 (必填) </td>
	  <td class='forumRow'><input name='number' type='text' id='number' size='20' maxlength="2"> 
      只能是数字 </td>
	  </tr>
	<tr>
	  <td class='forumRowHighLight' height=11>排序</td>
	  <td class='forumRowHighLight'><input name='order' type='text' id='order' value="1" size='20' maxlength="2">
只能是数字，数字越小排名越靠前</td>
	  </tr>
	<tr>
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