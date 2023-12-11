<!--#include file="inc/conn.asp" -->
<%
Sub PutToShopBag( Product_Id, ProductList )
   If Len(ProductList) = 0 Then
      ProductList = "'" & Product_Id & "'"
   ElseIf InStr( ProductList, Product_Id ) <= 0 Then
      ProductList = ProductList & ", '" & Product_Id & "'"
   End If
End Sub
%>

<html>
<head>
<title>购物车</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<meta name="Author" content="heweiqun">
<meta name="Contact" content="hdz2008@163.com">
<link href="mt_style.css" rel="stylesheet" type="text/css">
</head>
<body bgcolor="#FFFFFF" text="#000000" leftmargin="0" topmargin="0" onload="self.focus();">

<%
'如果购买车为空，转入提示界面
ProductList = Session("ProductList")
If Len(ProductList) = 0 Then
   Response.write "<table  height=100% width=100%  ><tr><td align=center>您好！目前您的购物车为空，是不是 <a href='javascript:window.close()'><b>继续购物</b></a>？</td></tr></table>"
   response.end
end if

'判断是否支付，如支付转到支付页面，并生成在线支付所需数据参数
if request("payment")="开始支付" then
response.redirect "payment.asp"
end if

'取消要购买物品的处理
Quatityt=""
subslist=""

If Request("cmdShow") = "Yes" Then
   ProductList = ""
   Products = Split(Request("Product_Id"), ", ")
   For I=0 To UBound(Products)
      PutToShopBag Products(I), ProductList
   Next
   Session("ProductList") = ProductList
End If

if productlist<>"" then
  Set rsc=Server.CreateObject("ADODB.RecordSet") 
  sqlc="select * from Product where Product_Id in ("&productlist&") order by Product_Id"
  rsc.open sqlc,conn,1,1
else
	Response.write "<table  height=100% width=100%  ><tr><td align=center>您好！目前您的购物车为空，是不是需要 <a href='../helpcenter.asp' target='blank_'><b>帮助</b></a>？还是 <a href='javascript:window.close()'><b>继续购物</b></a>？</td></tr></table>"
   response.end
end if
%>
<table width="630" border="0" cellspacing="0" cellpadding="0" align="center">
  <tr>           
    <td colspan="4" height="47">&nbsp;&nbsp;&nbsp;&nbsp;欢迎您的光临，您所订购的商品清单如下：</td>
        </tr>
        <tr> 
          <td colspan="4" height="88">
      <table border="0" cellspacing="1" cellpadding="0" align="center" width="567" bgcolor="#000000">
        <form action="order_check.asp" method="POST" name="check">
          <tr bgcolor="#FFFFFF"> 
            <td width="56" height="25"> 
              <div align="center"><font color="B0266D">取消</font></div>
            </td>
            <td width="283"> 
              <div align="center"><font color="B0266D">商 品 名 称</font></div>
            </td>
            <td width="57"> 
              <div align="center"><font color="B0266D">数 量</font></div>
            </td>
            <td width="85"> 
              <div align="center"><font color="B0266D">单 价</font></div>
            </td>
			<td width="85"> 
              <div align="center"><font color="B0266D">合 计</font></div>
            </td>
          </tr>
<%
Sum = 0
While Not rsc.EOF
 Quatity = CInt( Request( "Q_" & rsc("Product_Id")) )
  If Quatity <= 0 Then 
       Quatity = CInt( Session(rsc("Product_Id")) )
      If Quatity <= 0 Then Quatity = 1
  End If

  if len(Quatityt)=0 then
    Quatityt=Quatity
  else
    Quatityt=Quatityt&","&Quatity
  end if

  if len(subslist)=0 then
   subslist=rsc("Title")
  else
   subslist=subslist&","&rsc("Title")
  end if

  Session(rsc("Product_Id")) = Quatity
  Sum = Sum + csng(rsc("Price")) * Quatity
  Sum=FormatNumber(Sum,1) 
%> 
          <tr bgcolor="#FFFFFF"> 
            <td width="56" height="20" align="center"> 
              <input type="CheckBox" name="Product_Id" value="<%=rsc("Product_Id")%>" Checked>
                <input type="hidden" name="subs" value="<%=rsc("Title")%>">          
            </td>
            <td width="283">&nbsp;&nbsp;<font color="B0266D"><%=rsc("Title")%></font></td>
            <td width="57" align="center"> 
              <input type="Text" name="<%="Q_" & rsc("Product_Id")%>" value="<%=Quatity%>" size="2" class="form">
           
            </td>
            <td width="85" align=center><font color="B0266D"><%=FormatNumber(rsc("Price"),1)%></font></td>
			<td width="85" align=center><font color="B0266D"><%=FormatNumber(csng(rsc("Price"))*Quatity,1)%></font></td>
          </tr>
          <%
rsc.MoveNext
Wend
rsc.close
set rsc=nothing
%> 
          <tr bgcolor="#FFFFFF"> 
            <td colspan="2" height="35" align="center">             
                <input type="submit" name="order" value="更新数量"> &nbsp;&nbsp;&nbsp;
                <input type="submit" name="payment" value="开始支付"> &nbsp;&nbsp;&nbsp; 
                <input type="button" value="继续购物" language=javascript onClick="javascript:window.close()" name="button">
                <input type="hidden" name="cmdShow" value="Yes">
                </td>
            <td colspan="3" align="right"> 
              <b> <font color="B0266D">总价格  = <%=Sum%>&nbsp;&nbsp;</font></b>
            </td>
          </tr>
        </form>
		<tr><td colspan="5" bgcolor="#ffffff">
<UL>
			<LI>填写好需要的数量后，点击‘更新数量’
			<LI>如果要取消某个商品，取消选择的勾号，再点击‘更新数量’
		</UL></td></tr>
      </table> 
 </td>
</tr>
</table>
</body>
</html>