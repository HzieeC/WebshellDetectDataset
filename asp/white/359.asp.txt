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
<% '修改数据到数据表
page=request.querystring("page")
act=request.querystring("act")
keywords=request.querystring("keywords")
article_id=cint(request.querystring("id"))


act1=Request("act1")
If act1="save" Then 
l_id=trim(request.form("l_id"))
ModelName=trim(request.form("name"))
ModelType=trim(request.form("ModelType"))
ModelTheme=trim(request.form("ModelTheme"))
models_memo=trim(request.form("models_memo"))
models_content=trim(request.form("models_content"))
models_content=Replace(models_content,"[textarea","<textarea")
models_content=Replace(models_content,"[/textarea]","</textarea>")


set rs=server.createobject("adodb.recordset")
sql="select id from web_models where ModelType="&ModelType&" and ModelTheme="&ModelTheme&" and [id]<>"&l_id&""
rs.open(sql),cn,1,3
if not rs.eof then
response.Write "<script language='javascript'>alert('该主题下的模板分类已经存在一个模板，请改为其它的模板分类或主题！');history.go(-1);</script>"
else
rs.close
set rs=nothing

set rs=server.createobject("adodb.recordset")
sql="select * from web_models where id="&l_id&""
rs.open(sql),cn,1,3
rs("name")=ModelName
rs("ModelType")=ModelType
rs("ModelTheme")=ModelTheme
rs("memo")=models_memo
rs("content")=models_content
rs.update
rs.close
set rs=nothing

'生成模板文件
Call Model_to_html(l_id)
response.Write "<script language='javascript'>alert('修改成功！');location.href='web_models.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"

end if
end if %>
	<%
Call header()

%>
<% set rs=server.createobject("adodb.recordset")
sql="select * from web_Models where id="&article_id&""
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then%>
  <form id="form1" name="form1" method="post" action="?act1=save&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">
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
	  <th class='tableHeaderText' colspan=2 height=25>修改模板</th>
	<tr>
	  <td height=23 colspan="2" class='forumRow'><table width="100%" border="0" align="center" cellpadding="0" cellspacing="0">
        <tr>
          <td height="20" class='TipTitle'>&nbsp;√ 操作提示</td>
        </tr>
        <tr>
          <td height="30" valign="top" class="TipWords"><p>1、一个主题下的一个模板分类只能有一个模板，比如棕色(Brown)主题的首页模板只能存在一个。</p>
            <p>2、修改完某个模板后，需要手动生成相应页面才会查看到修改模板后的效果。</p>
            <p>3、由于代码冲突，对于html代码中出现的[textarea,[/textarea]代码您不需要感到惊讶，它们会自动替换成&lt;textarea,&lt;/textarea&gt;存放到数据库中。</p>
            <p>4、HTML代码中$xxx$字样均是替换标签，替换标签是用于更换标签为数据库中您添加的内容的。更改模板将谨慎。</p>
        </tr>
        <tr>
          <td height="10">&nbsp;</td>
        </tr>
      </table></td>
	  </tr>
	<tr>
	<td width="15%" height=23 class='forumRow'>模板名称 (必填) </td>
	<td width="85%" class='forumRow'><input name='name' type='text' id='name' size='30' value="<%=rs("name")%>">
	 <input name='l_id' type='hidden' id='l_id' value="<%=rs("id")%>" size='70'>  &nbsp;</td>
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
if rs_1("id")=rs("ModelType") then
response.write "<option value='"&rs_1("id")&"' selected>"&rs_1("name")&"</option>"
else
response.write "<option value='"&rs_1("id")&"'>"&rs_1("name")&"</option>"
end if
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
if rs_1("id")=rs("ModelTheme") then
response.write "<option value='"&rs_1("id")&"' selected>"&rs_1("name")&"</option>"
else
response.write "<option value='"&rs_1("id")&"'>"&rs_1("name")&"</option>"
end if
rs_1.movenext
loop
rs_1.close
set rs_1=nothing
%>      </select></td>
	  </tr>
	<tr>
	  <td class='forumRowHighLight' height=11>备注</td>
	  <td class='forumRowHighLight'><textarea name='models_memo'  cols="100" rows="3" id="models_memo" ><%=rs("memo")%></textarea></td>
	</tr>
	<tr>
	  <td class='forumRow' height=23>模板HTML代码 (必填)</td>
	  <td class='forumRow'> <textarea name='models_content'   cols="100" rows="20" id="models_content"  >
<%
ReplaceContent=Replace(rs("content"),"<textarea","[textarea")
ReplaceContent=Replace(ReplaceContent,"</textarea>","[/textarea]")
response.write ReplaceContent
%></textarea> </td>
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
end if
rs_a.close
set rs_a=nothing
%>
<%
Call DbconnEnd()
 %>