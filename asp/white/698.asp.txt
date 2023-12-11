<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->

<script language="javascript">
<!--

function ConfirmDel()
{
   if(confirm("确定要删除选中的项目吗？一旦删除将不能恢复！"))
     return true;
   else
     return false;	 
}

function form_onsubmit()
{
if (document.form_admin.Password.value!=document.form_admin.ConPassword.value)
{
alert ("新密码和确认密码不一致。");
document.form_admin.Password.value='';
document.form_admin.ConPassword.value='';
document.form_admin.Password.focus();
return false;
}
}
//-->
</SCRIPT>

<%if Request.QueryString("mark")="southidc" then
dim sql,rs,ID
ID=request("ID")
set rs=server.createobject("adodb.recordset")
sql="delete from admin where Id="& ID
rs.open sql,conn,1,3
set rs=nothing
conn.close
response.redirect "Admin_Manage.asp"
end if
%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> <strong><br>
      </strong> <table width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td class="back_southidc" width="598" height="25" > <div align="center"><strong>添加管理员</strong></div></td>
        </tr>
        <tr class="tr_southidc"> 
          <FORM name="form_admin" method="post"  onsubmit="return form_onsubmit()" action="Admin_ManageSave.asp" >	
            <td><table width="50%" border="0" align="center" >
                <tr> 
                  <td height="25" colspan="2">　</td>
                </tr>
                <tr> 
                  <td width="29%" height="22"> <div align="right">管理员帐号：</div></td>
                  <td width="71%"><input name="UserName" type="text" id="UserName" size="16" maxlength="20"></td>
                </tr>
                <tr> 
                  <td height="22"> <div align="right">管理员密码：</div></td>
                  <td><input name="Password" type="password" size="16" maxlength="20"></td>
                </tr>
                <tr> 
                  <td height="22"> <div align="right">密码确认：</div></td>
                  <td><input name="ConPassword" type="password" size="16" maxlength="20"></td>
                </tr>
                <tr> 
                  <td height="22" colspan="2"><div align="center">
                      <INPUT type=submit value='确认添加' name=Submit2>
                    </div></td>
                </tr>
              </table></td>
          </form>
        </tr>
      </table>
      <br>
      <table width="560" height="61" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr>
          <td align="center" bgcolor="#FFFFFF"><br>
            用户名和密码任何一项都<span style="font-weight: bold; color: #FF0000">不要</span>设置为admin<br>
            <br>
            密码的的规范：<span style="font-weight: bold; color: #FF0000">字母+特殊符号+数字</span>任意混合 例：jiumu++--2010 <br>
            <br>
            用户自己<span style="font-weight: bold; color: #FF0000">密码设置过于简单</span>出现问题我们不负任何责任<br>
            <br>
            用户的<span style="font-weight: bold; color: #FF0000">FTP的密码</span>最好也要符合以上规范 例：123456或19850901 之类的最危险 <br>
            <br>
            为了网站的安全运行，请不要闲复杂，这是基本规则<br>
            <br></td>
        </tr>
      </table>
      <br> 
      <table width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td width="553" height="25" class="back_southidc"> <div align="center"><strong>管理员帐号管理</strong></div></td>
        </tr>
        <tr class="tr_southidc">          
            <td><br> 
              <table width="100%" border="0" align="center" cellpadding="0" cellspacing="2">
                <tr bgcolor="#A4B6D7"> 
                  <td width="28%" height="25"> 
                    <div align="center">管理员帐号</div></td>
                  <td width="28%"> 
                    <div align="center">管理员密码</div></td>
                  <td width="24%"> 
                    <div align="center">操作</div></td>
                  <td width="20%"> 
                    <div align="center">删除</div></td>
                </tr>
<%				
set rs=server.createobject("adodb.recordset")
sql="select * from admin"
rs.open sql,conn,1,1
do while not rs.eof
%>
                <tr bgcolor="#DFEBF2"> 
                  <td height="22"> 
                    <div align="center"><%=rs("UserName")%></div></td>
                  <td> 
                    <div align="center"><%=rs("PassWord")%></div></td>
                  <td> 
                    <div align="center">
                      <%response.write "<a href='Admin_ManageEdit.asp?ID="&rs("Id")&"'>修改密码</a>"%>
                    </div></td>
                  <td> 
                    <div align="center">
					<a href="Admin_Manage.asp?id=<%=rs("id")%>&mark=southidc" onClick="return ConfirmDel();">删除</a>                    
                    </div></td>
                </tr>
<%
rs.movenext
loop
rs.close
%>
              </table></td>         
        </tr>
      </table>
    </td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->