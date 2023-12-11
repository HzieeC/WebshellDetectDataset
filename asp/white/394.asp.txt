<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
<% '添加数据到数据表
page=request.querystring("page")
act=request.querystring("act")
keywords=request.querystring("keywords")
article_id=cint(request.querystring("id"))


act1=Request("act1")
If act1="save" Then 
l_id=trim(request.form("l_id"))
l_name=trim(request.form("name"))
l_url=trim(request.form("url"))
l_position=trim(request.form("position"))
l_image=trim(request.form("web_image"))
l_order=trim(request.form("order"))
l_memo=trim(request.form("memo"))
l_view_yes=trim(request.form("view_yes"))
l_blank_yes=trim(request.form("blank_yes"))
l_time=now()

set rs=server.createobject("adodb.recordset")
sql="select * from web_menu where id="&l_id&""
rs.open(sql),cn,1,3
rs("name")=l_name
rs("url")=l_url
rs("position")=l_position
rs("blank_yes")=l_blank_yes
rs("image")=l_image
if l_order<>"" then
rs("order")=cint(l_order)
end if
rs("memo")=l_memo
rs.update
rs.close
set rs=nothing

response.Write "<script language='javascript'>alert('修改成功！');location.href='menu_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
end if 
 %>
 

	<%
Call header()

%>
<% set rs=server.createobject("adodb.recordset")
sql="select * from web_menu where id="&article_id&""
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then%>
  <form id="form1" name="form1" method="post" action="?act1=save&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.name.value == '' ) {
window.alert('请输入导航名称^_^');
document.form1.name.focus();
return false;}

if ( document.form1.position.value == '' ) {
window.alert('请选择导航分类^_^');
document.form1.position.focus();
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
	  <th class='tableHeaderText' colspan=2 height=25>修改导航</th>
	<tr>
	<td width="15%" height=23 class='forumRowHighLight'>导航名称 (必填) </td>
	<td class='forumRowHighLight'><input name='name' type='text' id='name'  value="<%=rs("name")%>"size='70'>
	 <input name='l_id' type='hidden' id='l_id' value="<%=rs("id")%>" size='70'>
	  &nbsp;</td>
	</tr>
	  <tr>
	    <td class='forumRow' height=23>导航链接</td>
	    <td class='forumRow'><input name='url' type='text' id='url' value="<%=rs("url")%>" size='70'></td>
      </tr>
	 <tr>
	    <td class='forumRowHighLight' height=23>导航分类 (必选) </td>
	    <td class='forumRowHighLight'><label>
	      <select name="position" id="position">
	       <% set rsp=server.createobject("adodb.recordset")
		   sql="select id,name from web_menu_type order by [order]"
		   rsp.open(sql),cn,1,1
		   if not rsp.eof and not rsp.bof then
		   do while not rsp.eof 
		   %> <option value="<%=rsp("id")%>" <%if rsp("id")=cint(rs("position")) then
		response.write "selected"
		end if%>><%=rsp("name")%></option>
            <%
			rsp.movenext
			loop
			end if
			rsp.close
			set rsp=nothing%></select>
	    </label></td>
      </tr> 
	  <tr>
	    <td class='forumRow' height=23>图片</td>
	    <td width="85%" class='forumRow'><table width="100%" border="0" cellspacing="0" cellpadding="0">
         <tr>
           <td width="22%" ><input name="web_image" type="text" id="web_image"  value="<%=rs("image")%>" size="30"></td>
           <td width="78%"  ><iframe width="500" name="ad" frameborder=0 height=30 scrolling=no src="upload.asp"></iframe></td>
         </tr>
       </table></td>
      </tr><tr>
	    <td class='forumRowHighLight' height=23>排序</td>
	    <td class='forumRowHighLight'><span class="forumRow">
	      <input name='order' type='text' id='order' value="<%=rs("order")%>" size='20'>
	    &nbsp;只能是数字，数字越小排名越靠前</span></td>
      </tr><tr>
	  <td class='forumRow' height=11>备注</td>
	  <td class='forumRow'><textarea name='memo'  cols="100" rows="6" id="memo" ><%=rs("memo")%></textarea></td>
	</tr>
	  <tr>
	  <td class='forumRowHighLight' height=23>打开方式</td>
	  <td class='forumRowHighLight'><label>
	       <input type="radio" name="blank_yes" value="1"<%
		if rs("blank_yes")=1 then
		response.write "checked"
		end if%>>
      新窗口
      &nbsp;
      <input name="blank_yes" type="radio" value="0" <%if rs("blank_yes")=0 then
		response.write "checked"
		end if%>>
      原窗口</label></td>
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