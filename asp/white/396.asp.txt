<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/Model_to_html.asp" -->
<%'判断模板管理权限是否开启
set  rs_a=server.createobject("adodb.recordset")
sql="select web_ModelEdit from web_settings"
rs_a.open(sql),cn,1,1
if rs_a("web_ModelEdit")=0 then
response.Write "<script language='javascript'>alert('您的模板管理权限尚未开启！请在“网站信息设置”里将“后台模板管理”设置为“开启”状态！');history.go(-1);</script>"
else
%>
<%act=Request("act")
If act="save" Then 
ModelName=trim(request.form("name"))
ModelType=trim(request.form("ModelType"))
ModelTheme=trim(request.form("ModelTheme"))
models_memo=trim(request.form("models_memo"))
models_content=trim(request.form("models_content"))
models_time=now()

set rs=server.createobject("adodb.recordset")
sql="select * from web_models where ModelType="&ModelType&" and ModelTheme="&ModelTheme&""
rs.open(sql),cn,1,3
if not rs.eof then
response.Write "<script language='javascript'>alert('该主题下的模板分类已经存在一个模板，请不要重复添加！');history.back;</script>"
else
rs.addnew
rs("name")=ModelName
rs("ModelType")=ModelType
rs("ModelTheme")=ModelTheme
rs("memo")=models_memo
rs("content")=models_content
rs("time")=models_time
rs.update
rs.close
set rs=nothing

'生成模板文件
set rs=server.createobject("adodb.recordset")
sql="select [id] from web_models where ModelType="&ModelType&" and ModelTheme="&ModelTheme&" and [name]='"&ModelName&"' order by [time] desc"
rs.open(sql),cn,1,1
if not rs.eof then
l_id=rs("id")
end if
rs.close
set rs=nothing
Call Model_to_html(l_id)

response.Write "<script language='javascript'>alert('添加成功！');location.href='web_models.asp';</script>"
end if
end if %>
	<%
Call header()

%>

  <form id="form1" name="form1" method="post" action="?act=save">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.name.value == '' ) {
window.alert('请输入网站模板名称^_^');
document.form1.name.focus();
return false;}

if ( document.form1.ModelType.value == '' ) {
window.alert('请选择模板分类^_^');
document.form1.ModelType.focus();
return false;}

if ( document.form1.ModelTheme.value == '' ) {
window.alert('请选择模板主题^_^');
document.form1.ModelTheme.focus();
return false;}

if ( document.form1.models_content.value == '' ) {
window.alert('请输入模板内容^_^');
document.form1.models_content.focus();
return false;}
return true;}
</script>
	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th class='tableHeaderText' colspan=2 height=25>添加模板</th>
	<tr>
	  <td height=23 colspan="2" class='forumRow'><table width="100%" border="0" align="center" cellpadding="0" cellspacing="0">
        <tr>
          <td height="20" class='TipTitle'>&nbsp;√ 操作提示</td>
        </tr>
        <tr>
          <td height="30" valign="top" class="TipWords"><p>1、一个主题下的一个模板分类只能有一个模板，比如棕色(Brown)主题的首页模板只能存在一个。</p>
            </td>
        </tr>
        <tr>
          <td height="10">&nbsp;</td>
        </tr>
      </table></td>
	  </tr>
	<tr>
	<td width="15%" height=23 class='forumRow'>模板名称 (必填) </td>
	<td width="85%" class='forumRow'><input name='name' type='text' id='name' size='30'>
	  &nbsp;</td>
	</tr>
	<tr>
	<td class='forumRowHighLight' height=23>模板分类 (必选)</td>
    <td class='forumRowHighLight'><label>
      <select name="ModelType" id="ModelType">
        <option>请选择分类</option>
<% set rs_1=server.createobject("adodb.recordset")
sql="select [name],[id] from web_models_type where view_yes=1"
rs_1.open(sql),cn,1,1
do while not rs_1.eof 
response.write "<option value='"&rs_1("id")&"'>"&rs_1("name")&"</option>"
rs_1.movenext
loop
rs_1.close
set rs_1=nothing
%>
      </select>
    </label></td>
	</tr>
	<tr>
	  <td class='forumRow' height=11>模板主题 (必选) </td>
	  <td class='forumRow'><select name="ModelTheme" id="ModelTheme">
        <option>请选择分类</option>
<% set rs_1=server.createobject("adodb.recordset")
sql="select [name],[id] from web_theme where view_yes=1"
rs_1.open(sql),cn,1,1
do while not rs_1.eof 
response.write "<option value='"&rs_1("id")&"'>"&rs_1("name")&"</option>"
rs_1.movenext
loop
rs_1.close
set rs_1=nothing
%>      </select></td>
	  </tr>
	<tr>
	  <td class='forumRowHighLight' height=11>备注</td>
	  <td class='forumRowHighLight'><textarea name='models_memo'  cols="100" rows="3" id="models_memo" ></textarea></td>
	</tr>
	<tr>
	  <td class='forumRow' height=23>模板HTML代码 (必填)</td>
	  <td class='forumRow'> <textarea name='models_content'   cols="100" rows="20" id="models_content"  ></textarea> </td>
	</tr>
	<tr><td height="50" colspan=2  class='forumRow'><div align="center">
	  <INPUT type=submit value='提交' onClick='javascript:return checksignup1()' name=Submit>
	  </div></td></tr>
	</table>
</form>
<%
end if
rs_a.close
set rs_a=nothing
%>
<%
Call DbconnEnd()
 %>