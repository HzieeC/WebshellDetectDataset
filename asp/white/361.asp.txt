<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<% '添加数据到数据表
act=Request("act")
If act="save" Then 
Set  fso  =  CreateObject("Scripting.FileSystemObject")  
filesource =server.MapPath(DataFolder&"/"&request("MemoData"))
fileto = server.MapPath(DataFolder&"/"&request("OldData"))
fso.copyfile filesource,fileto,true
set fso = nothing


response.Write "<script language='javascript'>alert('还原成功！');location.href='Data_list.asp';</script>"

end if
 %>
 

	<%
Call header()

%>

  <form id="form1" name="form1" method="post" action="?act=save">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.MemoData.value == '' ) {
window.alert('请选择需要还原的备份数据^_^');
document.form1.MemoData.focus();
return false;}

return true;}
</script>
	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th class='tableHeaderText' colspan=2 height=25>还原数据库</th>
	<tr>
	  <td height=23 colspan="2" class='forumRow'><table width="100%" border="0" align="center" cellpadding="0" cellspacing="0">
        <tr>
          <td height="20" class='TipTitle'>&nbsp;√ 操作提示</td>
        </tr>
        <tr>
          <td height="30" valign="top" class="TipWords"><p>1、还原数据库操作会将备份数据库覆盖你的原始数据库，操作前请确保你的备份数据库是最新的数据。</p>
            <p>2、如果数据库过大，可能会耗费过多时间还原，建议耐心等待，不要多次点击提交。</p>
            </td>
        </tr>
        <tr>
          <td height="10">&nbsp;</td>
        </tr>
      </table></td>
	  </tr>
	<tr>
	<td width="15%" height=23 class='forumRow'>已备份数据列表</td>
	<td class='forumRow'><select name="MemoData">
	<option value="">请选择需要还原的备份数据</option>
<%set rs = server.CreateObject("adodb.recordset")
s_sql="select * from web_data order by [time] desc"
rs.Open (s_sql),cn,1,1
if not rs.eof then
Do while not rs.eof 
%>
<option value="<%=rs("dataname")%>"
 <% if rs("id")=trim(request.querystring("id")) then
response.write "selected"
end if
%>><%=rs("dataname")%></option>
<%
rs.moveNext  
loop
end if 
rs.close
set rs=nothing

%>
</select></td>
	</tr>
	  <tr>
	    <td class='forumRowHighLight' height=23>还原后数据库名称</td>
	    <td class='forumRowHighLight'><input name="OldData" type="text" id="OldData" value="<%=DataName%>" readonly  style="color:#666666" size="40">
        &nbsp;不可修改。</td>
      </tr>
	<tr><td height="50" colspan=2  class='forumRow'><div align="center">
	  <INPUT type=submit value='提交' onClick='javascript:return checksignup1()' name=Submit>
	  </div></td></tr>
	</table>
</form>

<%
Call DbconnEnd()
 %>