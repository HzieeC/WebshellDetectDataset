<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<!--#include file="../Inc/articleCHAR.INC"-->
<%if Request.QueryString("mark")="southidc" then
id=request("id")
Title=request("Title")
content=Request("content")
Language=Request("Language")

If title="" Then
response.write "SORRY <br>"
response.write "请输入内容标题!<a href=""javascript:history.go(-1)"">返回重输</a>"
response.end
end if
If content="" Then
response.write "SORRY <br>"
response.write "请输入内容!<a href=""javascript:history.go(-1)"">返回重输</a>"
response.end
end if

Set rs = Server.CreateObject("ADODB.Recordset")
sql="select * from culture where id="&id
rs.open sql,conn,1,3
rs("title")=title
rs("content")=content
rs("Language")=Language
rs("time")=Date()
rs.update
rs.close
response.redirect "Admin_Culture.asp"
end if
%>
<%
id=request.querystring("id")
Set rs = Server.CreateObject("ADODB.Recordset")
rs.Open "Select * From culture where id="&id, conn,1,1
%>
<!-- #include file="Inc/Head.asp" -->
<table width="560" border="0" align="center" cellpadding="2" cellspacing="1" class="table_southidc">
  <tr> 
    <td class="back_southidc" height="25"> <div align="center"><strong>修改文章 <br>
        </strong></div></td>
  </tr>
  <tr> 
    <form method="post" action="Admin_Editculturenews.asp?mark=southidc">
      <input type=hidden name=id value=<%=id%>>
      <td><div align="center"> 
          <table width="100%" border="0" cellspacing="1" cellpadding="0">
            <tr bgcolor="#E7E7E7"> 
              <td width="9%" height="25" bgcolor="#ECF5FF"> <div align="center">标&nbsp;&nbsp;题:</div></td>
              <td width="58%" bgcolor="#ECF5FF"> <input type="text" name="title" maxlength="60" size="45" value="<%=rs("title")%>"> 
              </td>
              <td width="12%" bgcolor="#ECF5FF"><div align="center">语言:</div></td>
              <td width="21%" bgcolor="#ECF5FF"><select name="Language" id="Language">
                  <option <% if rs("Language")="1" then response.Write "selected" %> value="1">中文</option>
                  <option <% if rs("Language")="0" then response.Write "selected" %> value="0">英文</option>
                </select></td>
            </tr>
            <tr bgcolor="#ECF5FF"> 
              <td height="22" colspan="4"><div align="center">内&nbsp;&nbsp;容 </div></td>
            </tr>
            <tr bgcolor="#ECF5FF"> 
              <td height="300" colspan="4"> <input type="hidden" name="content" value="<%=Server.HTMLEncode(rs("Content"))%>"> 
                <iframe ID="eWebEditor1" src="Southidceditor/ewebeditor.asp?id=content&style=southidc" frameborder="0" scrolling="no" width="550" HEIGHT="450"></iframe> 
              </td>
            </tr>
            <tr bgcolor="#E7E7E7"> 
              <td height="25" colspan="4" bgcolor="#ECF5FF"> <div align="center"> 
                  <input name="submit" type="submit" value="确定">
                  &nbsp; 
                  <input name="reset" type="reset" value="从来">
                </div></td>
            </tr>
          </table>
        </div></td>
    </form>
  </tr>
</table>
<% 
rs.close 
set rs=nothing
%>
<!-- #include file="Inc/Foot.asp" -->