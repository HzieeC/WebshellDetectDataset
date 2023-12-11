<%@ LANGUAGE="VBScript" %>
<!--#include file="Inc/Util.asp" -->
<%
Head="您放入购物车的物品已全数退回！"

Session("ProductList") = ""
%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<title>清空购物车</title>
</head>
<body topmargin="5" bgcolor="#eeeeee">
<div align="center"><center>
<table width="100%" border="0" class="table1" bordercolor="#62ACFF" cellspacing="0" >
<tr>
<td width="80%" valign="top">　<p align="center" ><%=Head%></p>
<p align="center">　<br><input type="button" value="关闭" name="B2" onclick="window.close();" style="font-size: 9pt"></td>
</tr>
</table>
</center></div>
</body>
</html>