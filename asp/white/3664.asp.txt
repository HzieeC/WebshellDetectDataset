<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%if Request.QueryString("mark")="southidc" then

id=request("id")
paytype=request("paytype")
paymentmessage=Request("paymentmessage")

If paytype="" Then
response.write "SORRY <br>"
response.write "请输入支付银行"
response.end
end if
If paymentmessage="" Then
response.write "SORRY <br>"
response.write "支付说明不能为空"
response.end
end if

Set rs = Server.CreateObject("ADODB.Recordset")
sql="select * from Paydefault where id="&id
rs.open sql,conn,1,3

rs("paytype")=paytype
rs("paymentmessage")=paymentmessage
rs.update
rs.close
response.redirect "Admin_Pay.asp"
end if
%>
<%
id=request.querystring("id")
Set rs = Server.CreateObject("ADODB.Recordset")
rs.Open "Select * From Paydefault where id="&id, conn,1,3
%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> <strong><br>
      </strong> <br> <br> <table  width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td class="back_southidc" height="25"> <div align="center"><strong>修改支付<br>
              </strong></div></td>
        </tr>
        <tr> 
          <form method="post" action="Admin_PayEdit.asp?mark=southidc">
            <input type=hidden name=id value=<%=id%>>
            <td><div align="center"> 
                <table width="100%" border="0" cellspacing="1" cellpadding="0">
                  <tr> 
                    <td width="8%" height="25" bgcolor="#A4B6D7"> 
                      <div align="center">支付银行</div></td>
                    <td width="62%" bgcolor="#E3E3E3">
<input name="paytype" type="text" id="paytype" value="<%=rs("paytype")%>" size="40" maxlength="40">
                    </td>
                  </tr>
                  <tr> 
                    <td height="22" bgcolor="#A4B6D7">
<div align="center">支付说明</div></td>
                    <td bgcolor="#E3E3E3"><textarea name="paymentmessage" cols="40" rows="5" id="paymentmessage"><%=rs("paymentmessage")%></textarea></td>
                  </tr>
                  <tr bgcolor="#A4B6D7"> 
                    <td height="25" colspan="2"> 
                      <div align="center"> 
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