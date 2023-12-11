<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%if Request.QueryString("mark")="southidc" then

id=request("id")
Title=Trim(Request("Title"))
Content=Request("Content")
Aboutusorder=Request("Aboutusorder")
Language=Request("Language")

If Title="" Then
response.write "SORRY <br>"
response.write "请输入栏目名称!<a href=""javascript:history.go(-1)"">返回重输</a>"
response.end
end if
If Content="" Then
response.write "SORRY <br>"
response.write "请输入栏目内容!<a href=""javascript:history.go(-1)"">返回重输</a>"
response.end
end if


Set rs = Server.CreateObject("ADODB.Recordset")
sql="select * from Aboutus where id="&id
rs.open sql,conn,1,3

rs("Title")=Title
rs("Content")=Content
rs("Aboutusorder")=Aboutusorder
rs("Language")=Language
rs.update
rs.close
response.redirect "AdminAboutus.asp"
end if
%>
<%
id=request.querystring("id")
Set rs = Server.CreateObject("ADODB.Recordset")
rs.Open "Select * From Aboutus where id="&id, conn,1,3
%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> <table width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td class="back_southidc" height="25"> <div align="center"><strong>修改栏目<br>
              </strong></div></td>
        </tr>
        <tr> 
          <form method="post" name="myform" action="AdminAboutusModify.asp?mark=southidc">
            <input type=hidden name=id value=<%=id%>>
            <td><div align="center"> 
                <table width="100%" border="0" cellspacing="1" cellpadding="0">
                  <tr bgcolor="#ECF5FF"> 
                    <td width="10%" height="25"> <div align="center">栏目名称:</div></td>
                    <td width="33%" height="25" bgcolor="#ECF5FF"> <input type="text" name="title" maxlength="30" size="25" value="<%=rs("Title")%>"> 
                    </td>
                    <td width="14%" bgcolor="#ECF5FF"><div align="center">排序号:</div></td>
                    <td width="12%" bgcolor="#ECF5FF"><input name="Aboutusorder" type="text" id="Aboutusorder" value="<%=rs("Aboutusorder")%>" size="4"></td>
                    <td width="12%" bgcolor="#ECF5FF"><div align="center">语言:</div></td>
                    <td width="19%" bgcolor="#ECF5FF"><select name="Language" id="Language">
                        <option <% if rs("Language")="1" then response.Write "selected" %> value="1">中文</option>
                        <option <% if rs("Language")="0" then response.Write "selected" %> value="0">英文</option>
                      </select></td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="25" colspan="6"> <div align="center">栏目内容</div></td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="300" colspan="6"> <input type="hidden" name="content" value="<%=Server.HTMLEncode(rs("Content"))%>"> 
                      <iframe ID="eWebEditor1" src="Southidceditor/ewebeditor.asp?id=content&style=southidc" frameborder="0" scrolling="no" width="550" HEIGHT="450"></iframe> 
                    </td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="25" colspan="6"> <div align="center"> 
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