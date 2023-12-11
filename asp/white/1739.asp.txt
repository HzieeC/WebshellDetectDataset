<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<% '添加数据到数据表
act=Request("act")
If act="save" Then 
if request("NewData")=request("OldData") then
response.Write "<script language='javascript'>alert('新的数据库名称不允许与原始数据库名称重复！');history.go(-1);</script>"
response.end
else
Set  fso=CreateObject("Scripting.FileSystemObject")  
filesource=server.MapPath(DataFolder&"/"&request("OldData"))
fileto=server.MapPath(DataFolder&"/"&request("NewData"))
if fso.fileexists(filesource) then

set rs=server.createobject("adodb.recordset")
sql="select * from web_data where dataname='"&request("Newdata")&"' "
rs.open(sql),cn,1,3
if not rs.eof then
rs("memo")=request("memo")
rs("time")=now()
rs.update
else
rs.addnew
rs("dataname")=request("NewData")
rs("memo")=request("memo")
rs("time")=now()
end if
rs.update
rs.close
set rs=nothing

fso.copyfile filesource,fileto,true

else
response.Write "<script language='javascript'>alert('找不到你要备份的原始数据库！');history.go(-1);</script>"
end if
set fso = nothing

response.Write "<script language='javascript'>alert('备份成功！');location.href='Data_list.asp';</script>"

end if
end if
 %>
 

	<%
Call header()

%>

  <form id="form1" name="form1" method="post" action="?act=save">
         <script language='javascript'>
function checksignup1() {
if ( document.form1.OldData.value == '' ) {
window.alert('请输入原始数据库名称^_^');
document.form1.OldData.focus();
return false;}

if ( document.form1.NewData.value == '' ) {
window.alert('请输入备份数据库名称^_^');
document.form1.NewData.focus();
return false;}

return true;}
</script>
	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>

	<tr>
	  <th class='tableHeaderText' colspan=2 height=25>备份数据库</th>
	<tr>
	<tr>
	  <td height=23 colspan="2" class='forumRow'><table width="100%" border="0" align="center" cellpadding="0" cellspacing="0">
        <tr>
          <td height="20" class='TipTitle'>&nbsp;√ 操作提示</td>
        </tr>
        <tr>
          <td height="30" valign="top" class="TipWords"><p>1、你当前的原始数据库存放在：<%=DataPath%>。</p>
            <p>2、建议经常更改你的数据库存放文件夹及数据库名称，以保证数据安全。</p>
            <p>3、建议经常备份你的数据库，确保你的数据不会丢失。</p>
            <p>4、如果数据库过大，可能会耗费过多时间备份，建议耐心等待，不要多次点击提交。</p>
            </td>
        </tr>
        <tr>
          <td height="10">&nbsp;</td>
        </tr>
      </table></td>
	  </tr>
	<td width="15%" height=23 class='forumRow'>原始数据库名称</td>
	<td class='forumRow'><input name='OldData' type='text' id='OldData' size='40'  style="color:#666666" value="<%=DataName%>" readonly>
	  &nbsp;不可修改</td>
	</tr>
	  <tr>
	    <td class='forumRowHighLight' height=23>备份数据库名称</td>
	    <td class='forumRowHighLight'><input name='NewData' type='text' id='NewData' value="<%=Replace(DataName,".mdb","")&"_"&Date()&".mdb"%>" size='40'>
        &nbsp;默认使用原始名称加日期的命名方式，可以修改成其它名称。</td>
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