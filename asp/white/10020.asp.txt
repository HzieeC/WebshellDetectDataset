<!--#include file="inc/conn.asp"--> 
<%id=trim(request("id"))
com_name=Replace(Request.Form("com_name"),"'","''") 
content=Replace(Request.Form("com_content"),"'","''") 
if id="" then%>
请选择你要评论的文章!<a href="Shownews.asp?id=<%=id%>">请返回</a>
<%
Response.end 
end if
if com_name="" then%>
请输入你的大名!<a href="Shownews.asp?id=<%=id%>">请返回</a>
<%
Response.end 
end if
if content="" then%>
评论内容不能为空?!<a href="Shownews.asp?id=<%=id%>">请返回</a>
<%
Response.end 
end if
set rs=server.CreateObject("adodb.recordset")
rs.Open "select * from comment",conn,1,3
rs.addnew
rs("com_name")=com_name
rs("com_content")=content
rs("com_typeid")=id
rs("com_date")=date
rs.update
rs.close
set rs=nothing
response.redirect "Shownews.asp?id="&id
%>



