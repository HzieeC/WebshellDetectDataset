<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
<%'判断模板管理权限是否开启
set  rs_a=server.createobject("adodb.recordset")
sql="select web_ModelEdit from web_settings"
rs_a.open(sql),cn,1,1
if rs_a("web_ModelEdit")=0 then
response.Write "<script language='javascript'>alert('您的模板管理权限尚未开启！请在“网站信息设置”里将“后台模板管理”设置为“开启”状态！');history.go(-1);</script>"
else
%><% '添加数据到数据表
page=request.querystring("page")
act=request.querystring("act")
keywords=request.querystring("keywords")
article_id=cint(request.querystring("id"))

act1=Request("act1")
If act1="save" Then 
l_id=trim(request.form("l_id"))
l_name=trim(request.form("name"))
FolderName=trim(request.form("FolderName"))
l_image=trim(request.form("web_image"))
l_memo=trim(request.form("memo"))
l_view_yes=trim(request.form("view_yes"))

'检测文件夹名是否与系统文件夹名重复
SystemFolderName="admincool|css|KKKeditor|images|inc|js|photos|swf|Templates"
SystemFolder=split(SystemFolderName,"|")
c=ubound(SystemFolder)
for i=1 to c
if FolderName=SystemFolder(i) then
response.Write "<script language='javascript'>alert('目标文件夹与系统文件夹名重复，请重新命名！');history.go(-1);</script>"
end if
next

set rs_1=server.createobject("adodb.recordset")
sql="select [id] from web_models_type where ( FolderName='"&FolderName&"' and FolderName<>'' ) and [id]<>"&l_id
rs_1.open(sql),cn,1,3
if not rs_1.eof then
response.Write "<script language='javascript'>alert('目标文件夹重复，请重新命名！');history.go(-1);</script>"
else
set rs=server.createobject("adodb.recordset")
sql="select * from web_models_type where id="&l_id&""
rs.open(sql),cn,1,3
FolderName_Old=rs("FolderName")
rs("name")=l_name
rs("FolderName")=FolderName
rs("image")=l_image
rs("memo")=l_memo
rs("view_yes")=cint(l_view_yes)
rs.update
rs.close
set rs=nothing
%>
<% '如果文件夹做了修改，删除之前的文件夹
if FolderName_Old<>"" and FolderName_Old<>FolderName then
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath("/"&FolderName_Old))=true Then
FolderPath="/"&FolderName_Old
call DelFolder(FolderPath)
end if
end  if
 '判断文件夹是否存在，否则创建
Set Fso=Server.CreateObject("Scripting.FileSystemObject") 
If Fso.FolderExists(Server.MapPath("/"&FolderName))=false Then
NewFolderDir="/"&FolderName
call CreateFolderB(NewFolderDir)
end if

%>
<%
response.Write "<script language='javascript'>alert('修改成功！');location.href='Models_type_list.asp?page="&page&"&act="&act&"&keywords="&keywords&"';</script>"
end if 
rs_1.close
set rs_1=nothing
end if%>
 

	<%
Call header()

%>
<% set rs=server.createobject("adodb.recordset")
sql="select * from web_Models_Type where id="&article_id&""
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then%>
  <form id="form1" name="form1" method="post" action="?act1=save&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.name.value == '' ) {
window.alert('请输入模板分类名称^_^');
document.form1.name.focus();
return false;}
if ( document.form1.url.value == '' ) {
window.alert('请输入模板文件名^_^');
document.form1.url.focus();
return false;}

return true;}
</script>
	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th class='tableHeaderText' colspan=2 height=25>修改模板分类</th>
	<tr>
	<td width="15%" height=23 class='forumRow'>分类名称 (必填) </td>
	<td class='forumRow'><input name='name' type='text' id='name'  value="<%=rs("name")%>" size='70'>
	 <input name='l_id' type='hidden' id='l_id' value="<%=rs("id")%>" size='70'>
	  &nbsp;</td>
	</tr>
	  <tr>
	    <td class='forumRowHighLight' height=23>模板文件名</td>
	    <td class='forumRowHighLight'><input name='url' type='text' id='url' value="<%=rs("FileName")%>" size='40' maxlength="70" readonly>
       <span style="color: #FF0000">&nbsp;无法修改</span></td>
      </tr>
	  <tr>
	    <td class='forumRow' height=23>目标文件夹 (必填) </td>
	    <td class='forumRow'><input name='FolderName' type='text' id='FolderName' value="<%=rs("FolderName")%>" size='40' maxlength="70" >
        <span style="color: #FF0000">&nbsp;例如：Cool，文件夹在根目录下，请慎重命名，勿与系统文件夹名重复，尽量少做修改！</span></td>
      </tr>	 
	  <tr>
	    <td class='forumRow' height=23>主题图片</td>
	    <td width="85%" class='forumRow'><table width="100%" border="0" cellspacing="0" cellpadding="0">
         <tr>
           <td width="22%" ><input name="web_image" type="text" id="web_image"  value="<%=rs("image")%>" size="30"></td>
           <td width="78%"  ><iframe width="500" name="ad" frameborder=0 height=30 scrolling=no src="upload.asp"></iframe></td>
         </tr>
       </table></td>
      </tr><tr>
	  <td class='forumRowHighLight' height=11>备注</td>
	  <td class='forumRowHighLight'><textarea name='memo'  cols="100" rows="6" id="memo" ><%=rs("memo")%></textarea></td>
	</tr>
	  
	  <tr>
	  <td class='forumRow' height=23>是否可用</td>
	  <td class='forumRow'><label>
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