<%@ LANGUAGE="VBScript" %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<!--#include file="Inc/articlechar.inc"-->

<script language=javascript>
function ConfirmDel()
{
   if(confirm("确定要删除此记录吗？"))
     return true;
   else
     return false;
}
</script>

<%
if Request.QueryString("no")="yes" then
 id= Trim(Request("id"))
 Set Rs = Server.CreateObject("ADODB.Recordset")
 Rs.Open "Delete from Feedback Where id="&id,Conn,1,3
 Set Rs= Nothing
 Response.Redirect "Admin_FeedbackAdd.asp"
end if


if Request.QueryString("mark")="southidc" then
If request.form("title")="" Then
Response.Write("<script language=""JavaScript"">alert(""错误：您没输入标题，请返回检查！！"");history.go(-1);</script>")
response.end
end if

If request.form("content")="" Then
Response.Write("<script language=""JavaScript"">alert(""错误：您没输入留言内容，请返回检查！！"");history.go(-1);</script>")
response.end
end if

Set rs = Server.CreateObject("ADODB.Recordset")
sql="select top 1 * from Feedback where UserName='管理员'"
rs.open sql,conn,1,3

rs.addnew
if request.form("html")="on" then
rs("content")=request.form("content")
else
rs("content")=htmlencode2(request.form("content"))
end if
rs("UserName")=request.form("UserName")
rs("title")=request.form("title")
rs("Publish")="0"
rs("Language")=request.form("Language")
rs("time")=date()
rs.update
rs.close
response.redirect "Admin_FeedbackAdd.asp"
end if

Set rs = Server.CreateObject("ADODB.Recordset")
sql="select  * from Feedback where Username='管理员'"
rs.open sql,conn,1,1
%>

<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> 
      <table width="560" border="0" cellpadding="0" cellspacing="1" bgcolor="#000000">
        <tr> 
          <td height="28" class="back_southidc"> 
            <div align="center"><strong>发布管理员公告 <br>
              </strong></div></td>
        </tr>
        <tr class="tr_southidc"> 
          <form method="POST" action="Admin_FeedbackAdd.asp?mark=southidc">
            <input type=hidden readonly name="UserName"   value="管理员">
            <td><div align="center"> 
                <table width="100%" border="0" cellspacing="1" cellpadding="0">
                  <tr bgcolor="#A4B6D7"> 
                    <td width="16%" height="25"> <div align="center">标&nbsp;&nbsp;题</div></td>
                    <td width="45%"> <input name="title" type="text" size="35" maxlength="100"></td>
                    <td width="17%"><div align="center">语言:</div></td>
                    <td width="22%"><select name="Language" id="Language">
                        <option value="0">中文</option>
                        <option value="1">英文</option>
                      </select></td>
                  </tr>
                  <tr> 
                    <td height="22"> <div align="center">内&nbsp;&nbsp;容</div></td>
                    <td colspan="3"> <textarea rows="8" name="Content" cols="45"></textarea> 
                    </td>
                  </tr>
                  <tr bgcolor="#A4B6D7"> 
                    <td height="25" colspan="4"> <div align="center">是否支持html 
                        <input type="checkbox" name="html" value="on">
                        <input type="submit" value="提交公告" name="B1">
                        <input type="reset" value="全部重写" name="B2">
                      </div></td>
                  </tr>
                </table>
				<table width="100%" border="0" cellpadding="1" cellspacing="1" bgcolor="#000000">
                  <tr bgcolor="#ECF5FF"> 
                    <td width="63%" height="25" bgcolor="#ECF5FF"> <div align="center">公告标题</div>
                      <div align="center"></div></td>
                    <td width="18%"> <div align="center">加入时间</div></td>
                    <td width="9%"> <div align="center">操作</div></td>
                    <td width="10%"> <div align="center">操作</div></td>
                  </tr>
                  <% do while not Rs.eof %>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="22" bgcolor="#ECF5FF"> <div align="center"> <%=rs("Title")%></div>
                      <div align="center"></div></td>
                    <td> <div align="center"><%= FormatDateTime(rs("time"),2) %></div></td>
                    <td> <div align="center"><a href="Admin_FeedbackEdit.asp?id=<%=Rs("id")%>">修改</a></div></td>
                    <td> <div align="center"><a href="Admin_FeedbackAdd.asp?id=<%=Rs("id")%>&amp;no=yes" onClick="return ConfirmDel();">删除</a></div></td>
                  </tr>
                  <%Rs.MoveNext 
loop 
%>
                </table>
              </div></td>
          </form>
        </tr>
      </table>
      <br> <br> </td>
  </tr>
</table>
<%
rs.close
Set Rs = Nothing 
Conn.close
Set Conn = Nothing 
%>
<!-- #include file="Inc/Foot.asp" -->