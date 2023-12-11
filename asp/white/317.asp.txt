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
l_image=trim(request.form("web_image"))
l_order=trim(request.form("order"))
l_memo=trim(request.form("memo"))
l_view_yes=trim(request.form("view_yes"))
l_index_push=trim(request.form("index_push"))
l_time=now()

set rs=server.createobject("adodb.recordset")
sql="select * from web_article_author where id="&l_id&""
rs.open(sql),cn,1,3
rs("name")=l_name
rs("url")=l_url
rs("image")=l_image
if l_order<>"" then
rs("order")=cint(l_order)
end if
rs("memo")=l_memo
rs("view_yes")=l_view_yes
rs("index_push")=l_index_push
rs.update
rs.close
set rs=nothing
call index_to_html()
response.Write "<script language='javascript'>alert('修改成功！');location.href='author_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
end if 
 %>
 

	<%
Call header()

%>
<% set rs=server.createobject("adodb.recordset")
sql="select * from web_article_author where id="&article_id&""
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then%>
  <form id="form1" name="form1" method="post" action="?act1=save&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.name.value == '' ) {
window.alert('请输入网站名称^_^');
document.form1.name.focus();
return false;}

if ( document.form1.url.value == '' ) {
window.alert('请输入网站网址^_^');
document.form1.url.focus();
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
	  <th class='tableHeaderText' colspan=2 height=25>修改文章来源链接</th>
	<tr>
	<td width="15%" height=23 class='forumRow'>网站名称 (必填) </td>
	<td class='forumRow'><input name='name' type='text' id='name'  value="<%=rs("name")%>"size='70'>
	 <input name='l_id' type='hidden' id='l_id' value="<%=rs("id")%>" size='70'>
	  &nbsp;</td>
	</tr>
	  <tr>
	    <td class='forumRowHighLight' height=23>网站网址 (必填) </td>
	    <td class='forumRowHighLight'><input name='url' type='text' id='url' value="<%=rs("url")%>" size='70'>
        &nbsp;以http://开头</td>
      </tr>
	  <tr>
	    <td class='forumRow' height=23>网站LOGO</td>
	    <td width="85%" class='forumRow'><table width="100%" border="0" cellspacing="0" cellpadding="0">
         <tr>
           <td width="22%" ><input name="web_image" type="text" id="web_image"  value="<%=rs("image")%>" size="30"></td>
           <td width="78%"  ><iframe width="500" name="ad" frameborder=0 height=30 scrolling=no src="upload.asp"></iframe></td>
         </tr>
       </table></td>
      </tr><tr>
	    <td class='forumRowHighLight' height=23>排序</td>
	    <td class='forumRowHighLight'><span class="forumRow">
	      <input name='order' type='text' id='order' value="<%=rs("order")%>" size='20' maxlength="5">
	    &nbsp;只能是数字，数字越小排名越靠前，默认为100，将排在最后面</span></td>
      </tr><tr>
	  <td class='forumRow' height=11>备注</td>
	  <td class='forumRow'><textarea name='memo'  cols="100" rows="6" id="memo" ><%=rs("memo")%></textarea></td>
	</tr>
	  
	  <tr>
	  <td class='forumRowHighLight' height=23>是否显示</td>
	  <td class='forumRowHighLight'><label>
	       <input type="radio" name="view_yes" value="1"<%
		if rs("view_yes")=1 then
		response.write "checked"
		end if%>>
      是
      &nbsp;
      <input name="view_yes" type="radio" value="0" <%if rs("view_yes")=0 then
		response.write "checked"
		end if%>>
      否</label></td>
	</tr>
	<tr>
	  <td class='forumRowHighLight' height=23>是否推荐</td>
	  <td class='forumRowHighLight'><label>
	    <input type="radio" name="index_push" value="1" <%
		if rs("index_push")=1 then
		response.write "checked"
		end if%>>
      是
      &nbsp;
      <input name="index_push" type="radio" value="0" <%if rs("index_push")=0 then
		response.write "checked"
		end if%>>
      否</label></td>
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