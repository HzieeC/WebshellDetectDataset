<!-- #include file="../inc/AntiAttack.asp" -->
<!--#include file="../inc/conn.asp"  -->
<%
id1=request.querystring("id")
set rs=server.createobject("adodb.recordset")
sql="select [hit],[ip],[id] from [article] where id="&id1&""
rs.open(sql),cn,1,3
if not rs.eof then
rs("hit")=rs("hit")+1

view_id=request.cookies(cstr(rs("id")))
if view_id="" then
rs("ip")=rs("ip")+1
response.cookies(cstr(rs("id")))=rs("id")
response.cookies(cstr(rs("id"))).expires=date()+1
end if
rs.update
hit=rs("hit")

end if
rs.close
set rs=nothing
%>
document.write(<%=hit%>)
