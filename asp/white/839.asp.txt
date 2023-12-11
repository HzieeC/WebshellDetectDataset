<!--#include file="Inc/syscode.asp"-->
<%dim bookid
bookid=request.QueryString("id")%>
<html>
<head>
<title>图片</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<%
set rs2=server.CreateObject("adodb.recordset")
rs2.open "select * from img where id="&bookid,conn,1,1
%>
<%if trim(rs2("img"))<>"" then
	  response.write "<div align='center'><img src="&trim(rs2("img"))&" border=0 ></div>"
else
	  response.Write "<img src=img/nopic.jpg width=65 height=96 alt=暂时没有图片！>"
end if%>
<div align="center"><BR>
  <%=trim(rs2("title"))%><BR>
</div>
</body>
</html>
