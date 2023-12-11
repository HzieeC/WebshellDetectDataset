<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%if Request.QueryString("mark")="southidc" then

id=request("id")
Title=request("Title")
Content=Request("Content")
Language=Request("Language")

If request.form("title")="" Then
Response.Write("<script language=""JavaScript"">alert(""错误：您没输入标题，请返回检查！！"");history.go(-1);</script>")
response.end
end if

If request.form("content")="" Then
Response.Write("<script language=""JavaScript"">alert(""错误：您没输入留言内容，请返回检查！！"");history.go(-1);</script>")
response.end
end if

Set rs = Server.CreateObject("ADODB.Recordset")
sql="select * from Feedback where id="&id
rs.open sql,conn,1,3

rs("Title")=Title
rs("Content")=Content
rs("Language")=Language
rs.update
rs.close
response.redirect "Admin_FeedbackAdd.asp"
end if
%>
<%
id=request.querystring("id")
Set rs = Server.CreateObject("ADODB.Recordset")
rs.Open "Select * From Feedback where id="&id, conn,1,3
%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> <strong><br>
      </strong> <br> <br> <table width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td height="25" class="back_southidc"> 
            <div align="center"><strong>修改管理员公告<br>
              </strong></div></td>
        </tr>
        <tr> 
          <form method="post" action="Admin_FeedbackEdit.asp?mark=southidc">
            <input type=hidden name=id value=<%=id%>>
            <td><div align="center"> 
                <table width="100%" border="0" cellpadding="0" cellspacing="1" bgcolor="#ECF5FF">
                  <tr> 
                    <td width="16%" height="25" bgcolor="#A4B6D7"> <div align="center">公告标题:</div></td>
                    <td width="48%"> <input name="Title" type="text" id="Title" value="<%=rs("Title")%>" size="35" maxlength="100"> 
                    </td>
                    <td width="13%"><div align="center">语言:</div></td>
                    <td width="23%"><select name="Language" id="Language">
                        <option <% if rs("Language")="0" then response.Write "selected" %> value="0">中文</option>
                        <option <% if rs("Language")="1" then response.Write "selected" %> value="1">英文</option>
                      </select></td>
                  </tr>
                  <tr> 
                    <td height="22" bgcolor="#A4B6D7"> <div align="center">公告内容:</div></td>
                    <td colspan="3"><textarea rows="8" name="Content" cols="45"><%=rs("Content")%></textarea> 
                    </td>
                  </tr>
                  <tr> 
                    <td height="25" colspan="4" bgcolor="#E3E3E3"> <div align="center"> 
                        <input name="submit" type="submit" value="确定">
                        &nbsp; 
                        <input name="reset" type="reset" value="从来">
                      </div></td>
                  </tr>
                </table>
              </div></td>
          </form>
        </tr>
      </table></td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->