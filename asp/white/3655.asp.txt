<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%
Set Rs = Server.CreateObject("ADODB.Recordset")
Rs.ActiveConnection = Conn
Rs.Open "Select * from Paydefault Order BY id"

if Request.QueryString("mark")="yes" then
id= Trim(Request("id"))
Set Rs = Server.CreateObject("ADODB.Recordset")
Rs.ActiveConnection = Conn
Rs.Open "Delete from Paydefault Where id="&id,Conn,1,3
Set Rs= Nothing
Set Conn = Nothing
Response.Redirect "Admin_Pay.asp"
end if

if Request.QueryString("mark")="southidc" then
paytype=request.form("paytype")
paymentmessage=request.form("paymentmessage")

If paytype="" Then
Response.Write("<script language=""JavaScript"">alert(""错误：您没输入支付银行，请返回检查！！"");history.go(-1);</script>")
response.end
end if

If paymentmessage=""Then
Response.Write("<script language=""JavaScript"">alert(""错误：您没有输入支付说明，请返回检查！！"");history.go(-1);</script>")
response.end
end if

Set rs = Server.CreateObject("ADODB.Recordset")
sql="select * from Paydefault "
rs.open sql,conn,1,3
rs.addnew
rs("paytype")=paytype
rs("paymentmessage")=paymentmessage
rs.update
Response.Redirect "Admin_Pay.asp"
end if
%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> <strong><br>
      </strong> <table  width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td class="back_southidc" height="35"> <div align="center"><strong>支付管理</strong></div></td>
        </tr>
        <tr> 
          <form method="post" action="Admin_Pay.asp?mark=southidc">
            <td bgcolor="F2F8FF"> 
              <table width="100%" border="0" align="center" cellpadding="3" cellspacing="2">
                <tr> 
                  <td width="21%" height="24" bgcolor="#A4B6D7"> 
                    <div align="center">支付银行</div></td>
                  <td width="79%" bgcolor="#ECF5FF">
<input name="paytype" type="text" id="paytype" size="40" maxlength="40"></td>
                </tr>
                <tr> 
                  <td height="22" bgcolor="#A4B6D7">
<div align="center">支付说明</div></td>
                  <td bgcolor="#ECF5FF">
<textarea name="paymentmessage" cols="40" rows="5" id="paymentmessage"></textarea></td>
                </tr>
                <tr bgcolor="#A4B6D7"> 
                  <td height="22" colspan="2"> 
                    <div align="center"> 
                      <input type="submit" name="Submit" value="提交">
                      <input type="reset" name="Submit2" value="重置">
                    </div></td>
                </tr>
              </table>
              <br> 
              <table width="100%" border="0" cellspacing="2" cellpadding="3">
                <tr bgcolor="#A4B6D7"> 
                  <td width="22%" height="25"> 
                    <div align="center">支付银行</div></td>
                  <td width="46%" bgcolor="#A4B6D7"> 
                    <div align="center">支付说明</div></td>
                  <td width="9%"> 
                    <div align="center">操作</div></td>
                  <td width="9%"> 
                    <div align="center">操作</div></td>
                </tr>
                <% do while not Rs.eof %>
                <tr bgcolor="#DFEBF2"> 
                  <td height="22"> 
                    <div align="center"><%=rs("paytype")%></div></td>
                  <td> 
                    <div align="center"><%=Rs("paymentmessage")%></div></td>
                  <td> 
                    <div align="center"><a href="Manage_editpay.asp?id=<%=Rs("id")%>">修改</a></div></td>
                  <td> 
                    <div align="center"><a href="Admin_Pay.asp?id=<%=Rs("id")%>&amp;mark=yes">删除</a></div></td>
                </tr>
                <%Rs.MoveNext 
loop 
%>
              </table>
<%  Set Rs = Nothing 
Set Conn = Nothing 
%> </td>
          </form>
        </tr>
        <tr> 
          <td height="22">&nbsp;</td>
        </tr>
      </table></td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->