<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
<!-- #include file="../inc/x_to_html/album_index_to_html.asp" -->
<!-- #include file="../inc/x_to_html/album_content_to_html.asp" -->

<% '添加数据到数据表
act=Request("act")
If act="save" Then 
l_name=trim(request.form("name"))
l_url=trim(request.form("url"))
l_position=trim(request.form("position"))
l_image=trim(request.form("web_image"))
l_order=trim(request.form("order"))
l_memo=trim(request.form("memo"))
l_view_yes=trim(request.form("view_yes"))
a_index_push=trim(request.form("a_index_push"))
l_time=now()

'单个添加图片
if l_image="" then
response.redirect "ad_list.asp"
else
set rs=server.createobject("adodb.recordset")
sql="select * from web_ad"
rs.open(sql),cn,1,3
rs.addnew
rs("name")=l_name
rs("url")=l_url
rs("position")=l_position
If IsObjInstalled("Persits.Jpeg")  Then
rs("SmallImage")="small/"&l_image
end if
rs("image")=l_image
if l_order<>"" then
rs("order")=cint(l_order)
end if
rs("memo")=l_memo
rs("view_yes")=cint(l_view_yes)
'rs("index_push")=a_index_push
rs("time")=l_time



rs.update
rs.close
set rs=nothing
end if

'生成相册展示页
set rs_create=server.createobject("adodb.recordset")
sql="select [id],[name],[memo],backmusic from web_ad_position where [id]="&l_position
rs_create.open(sql),cn,1,1
if not rs_create.eof then
a_id=rs_create("id")
a_name=rs_create("name")
a_memo=rs_create("memo")
a_music=rs_create("backmusic")
end if
rs_create.close
set rs_create=nothing
call album_content_to_html(a_id,a_name,a_memo,a_music)
call album_index_to_html()
call index_to_html()
response.Write "<script language='javascript'>alert('添加成功！');location.href='ad_list.asp';</script>"
end if 
 %>
 

	<%
Call header()

%>

  <form id="form1" name="form1" method="post" action="?act=save">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.name.value == '' ) {
window.alert('请输入图片标题^_^');
document.form1.name.focus();
return false;}


if ( document.form1.position.value == '' ) {
window.alert('请选择相册^_^');
document.form1.position.focus();
return false;}

if ( document.form1.web_image.value == '' ) {
window.alert('请上传图片^_^');
document.form1.web_image.focus();
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
	  <th class='tableHeaderText' colspan=2 height=25>添加图片</th>
	<tr>
	  <td height=23 colspan="2" class='forumRow'><table width="100%" border="0" align="center" cellpadding="0" cellspacing="0">
        <tr>
          <td height="20" class='TipTitle'>&nbsp;√ 操作提示</td>
        </tr>
        <tr>
          <td height="30" valign="top" class="TipWords"><p>
<p>1、图片只支持jpg,gif,bmp格式，大小不能超过1024K(1.0M)。</p>
            <p>2、通过数码相机拍摄的图片一般都比较大，达几M，需要通过PHOTOSHOP进行处理。</p>
            <p>3、上传的图片都放在根目录下的photos文件夹内。</p>
			<p>4、图片无法上传可能有以下原因：(1)你的空间不支持FSO组件；(2)你的空间写入权限未打开；(3)你使用的是简易ASP服务器或免费空间进行测试；(4)你的图片格式不对或文件超过限制；(5)图片存放文件夹不存在；(6)你的空间已满；(7)你的空间速度过低；(8)黑客入侵了。</p>
			<p>5、如果你确认以上情况都没有出现的话，那么可以联系程序作者了。</p></td>
        </tr>
        <tr>
          <td height="10">&nbsp;</td>
        </tr>
      </table></td>
	  </tr>
	<tr>
	<td width="15%" height=23 class='forumRowHighLight'>图片标题 (必填)</td>
	<td class='forumRowHighLight'><input name='name' type='text' id='name' size='70'>
	  &nbsp;</td>
	</tr>
	  <tr>
	    <td class='forumRow' height=23>图片链接 </td>
	    <td class='forumRow'><input name='url' type='text' id='url' value="" size='70'>
        &nbsp;以http://开头</td>
      </tr>
	  <tr>
	    <td class='forumRowHighLight' height=23>选择相册<span class="forumRowHighLight"> (必选)</span></td>
	    <td class='forumRowHighLight'><label>
	      <select name="position" id="position">
	       <% set rsp=server.createobject("adodb.recordset")
		   sql="select id,name from web_ad_position "
		   rsp.open(sql),cn,1,1
		   if not rsp.eof and not rsp.bof then
		   do while not rsp.eof 
		   %> <option value="<%=rsp("id")%>"><%=rsp("name")%></option>
            <%
			rsp.movenext
			loop
			end if
			rsp.close
			set rsp=nothing%></select>
	    </label></td>
      </tr>
	  <tr>
	    <td class='forumRow' height=23>上传图片</td>
	    <td width="85%" class='forumRow'><table width="100%" border="0" cellspacing="0" cellpadding="0">

         <tr>
           <td width="22%" ><input name="web_image" type="text" id="web_image"  size="30"></td>
           <td width="78%"  ><iframe width="500" name="ad" frameborder=0 height=30 scrolling=no src=upload_photo.asp></iframe></td>
         </tr>
         <tr>
           <td height="20" colspan="2" ><span style="color: #FF0000">注：</span>此处只能上传一张图片，需要上传多张图片<a href="ad_MutiAdd.asp">请点这里</a>。</td>
          </tr>		 
       </table></td>
      </tr>	  <tr>
	    <td class='forumRowHighLight' height=23>排序</td>
	    <td class='forumRowHighLight'><span class="forumRow">
	      <input name='order' type='text' id='order' value="100" size='20'>
	    &nbsp;只能是数字，数字越小排名越靠前</span></td>
      </tr>
	  <tr>
	  <td class='forumRow' height=11>备注</td>
	  <td class='forumRow'><span class="forumRowHighLight">
	    <textarea name='memo'  cols="100" rows="6" id="memo" ></textarea>
	  </span></td>
	</tr>
	  
	  <tr>
	  <td class='forumRowHighLight' height=23>是否显示</td>
	  <td class='forumRowHighLight'><label>
	    <input type="radio" name="view_yes" value="1" checked>
      是
      &nbsp;
      <input name="view_yes" type="radio" value="0" >
      否</label></td>
	</tr>

	<tr><td height="50" colspan=2  class='forumRow'><div align="center">
	  <INPUT type=submit value='提交' onClick='javascript:return checksignup1()' name=Submit>
	  </div></td></tr>
	</table>
</form>
<table width="800">
<tr><td>
</td></tr></table>
<%
Call DbconnEnd()
 %>