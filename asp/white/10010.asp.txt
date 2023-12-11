<!--#include file="inc/conn.asp"-->
<%
function cutstr(tempstr,tempwid)
if len(tempstr)>tempwid then
cutstr=left(tempstr,tempwid)&"..."
else
cutstr=tempstr
end if
end function%>
<html>
<head>
<title>图片新闻</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<link rel="stylesheet" type="text/css" href="mt_style.css">
</head>
<body>
<%
sql="select top 1 * from news where firstImageName<>'' and ok=true order by id desc"
set rs=conn.execute(sql)
if not rs.eof then
%>
<table width="103%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td width="56%" align="center"><div align="right"><a href="shownews.asp?id=<%=rs("id")%>" target="_blank" ><img src="<%=trim(rs("firstImageName"))%>" width="150" height="180" border="0"></a></div></td>
    <td width="44%" align="center"><div align="left"><a href="shownews.asp?id=<%=rs("id")%>" target="_blank"> 
                                  <%=cutstr(rs("Content"),80)%></a></div></td>
  </tr>
  <tr> 
    <td height="25" colspan="2" align="center"><a href="shownews.asp?id=<%=rs("id")%>" target="_blank" ><%=left(rs("title"),12)%></a></td>
  </tr>
</table>
<%
else 
response.write "<tr><td align=center colspan=2 bgcolor=#E8E8F4>尚无收录</td></tr>" 
end if 
rs.close 
set conn=nothing
%>
</body>
</html>
