<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->

	<%
Call header()
%>
<script language="JavaScript">
<!--
function ask(msg) {
	if( msg=='' ) {
		msg='警告：删除后将不可恢复，可能造成意想不到后果？';
	}
	if (confirm(msg)) {
		return true;
	} else {
		return false;
	}
}
//-->
</script>




	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>后台用户管理</th>
	
	<tr><td height="400" valign="top"  class='forumRow'><br>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" class='TipTitle'>&nbsp;√ 操作提示</td>
          </tr>
          <tr>
            <td height="30" valign="top" class="TipWords"><p>1、后台管理员分两个级别：超级管理员和普通管理员，普通管理员没有“基本设置”、“模板管理”权限。</p>
                <p>2、建议经常更换密码，保障后台安全。</p>
            </td>
          </tr>
          <tr>
            <td height="10" ></td>
          </tr>
        </table>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" class='forumRowHighLight'>&nbsp;| <a href="admin_add.asp">添加新的用户</a></td>
          </tr>
          <tr>
            <td height="30">&nbsp;</td>
          </tr>
      </table><%set rs=server.createobject("adodb.recordset")
	  if logr() then
sql="select * from web_admin order by class desc"
else
sql="select * from web_admin where class<>9 order by class desc"
end if
rs.open(sql),cn,1,1
if not rs.eof and not rs.bof then
%>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="2">
          <tr>
            <td width="22%" height="30" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">用户名</div></td>
            <td width="26%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">用户权限<%if logr() then 
			response.write "(点击可升降权限)"
			end if%></div></td>
            <td width="28%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">添加时间</div></td>
            <td width="24%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">操作</div></td>
          </tr>
		  <%
		  do while not rs.eof
		  %>
          <tr>
            <td height="35" class='forumRowHighLight'>
            <div align="center"><%=rs("username")%></div></td>
            <td class='forumRowHighLight'>
              <div align="center">
                <a href="admin_class.asp?id=<%=rs("id")%>" ><%if rs("class")=9 then
			response.write "<span style='color: #FF0000'>超级管理员</span>"
			else
			response.write "一般用户"
			end if%></a>
            </div></td>
            <td class='forumRowHighLight'><div align="center"><%=rs("time")%></div></td>
            <td class='forumRowHighLight'>
            <div align="center" id="loginform"><a href="admin_edit.asp?id=<%=rs("id")%>" >修改密码</a>
              <%If logr() Then%>  | <a href="javascript:if(ask('警告：删除后将不可恢复，可能造成意想不到后果？')) location.href='admin_del.asp?id=<%=rs("id")%>&username=<%=rs("username")%>';">删除</a>
              <%end if %></div></td>
          </tr>
		  <%
		  rs.movenext
		  loop
		  else
		  response.write "暂无模板！"
		  end if 
		  %>
      </table>
	    <br></td>
	</tr>
	</table>

<%
Call DbconnEnd()
 %>