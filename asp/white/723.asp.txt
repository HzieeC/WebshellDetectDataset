<%@ LANGUAGE = VBScript %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%if Request.QueryString("mark")="market" then
 HomeMarket=Request("content")
 EnHomeMarket=Request("Encontent")
 set rs=server.createobject("adodb.recordset")
 sql="select * from Market"
 rs.open sql,conn,1,3
 rs("HomeMarket")=HomeMarket
 rs("EnHomeMarket")=EnHomeMarket
 rs.update
 rs.close
 response.redirect "Admin_HomeMarket.asp"
end if
sql="select * from Market"
Set rs_Market= Server.CreateObject("ADODB.Recordset")
rs_Market.open sql,conn,1,1
%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <strong><br>
      </strong> <table width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td height="25" class="back_southidc"> 
            <div align="center"><strong>国 内 市 场 设 置</strong></div></td>
        </tr>
        <tr> 
          <form method="POST" action="Admin_HomeMarket.asp?mark=market">
            <td><table width="100%" border="0" cellspacing="1" cellpadding="0">
                <tr>
                  <td height="20" bgcolor="#ECF5FF"><div align="center">中文</div></td>
                </tr>
                <tr> 
                  <td height="300" bgcolor="#ECF5FF"> <input type="hidden" name="content" value="<%=Server.HTMLEncode(rs_Market("HomeMarket"))%>"> 
                    <iframe ID="eWebEditor1" src="Southidceditor/ewebeditor.asp?id=content&style=southidc" frameborder="0" scrolling="no" width="550" HEIGHT="450"></iframe>                  </td>
                </tr>
                <tr bgcolor="#ECF5FF"> 
                  <td height="30"> <div align="center"> 
                      <input type="submit" value=" 修 改 "
name="cmdok">
                      &nbsp; 
                      <input type="reset" value=" 重 写 "
name="cmdcancel">
                    </div></td>
                </tr>
              </table></td>
          </form>
        </tr>
      </table>
      <strong> </strong></td>
  </tr>
</table>
<%
rs_Market.close
set rs_Market=nothing
%>
<!-- #include file="Inc/Foot.asp" -->