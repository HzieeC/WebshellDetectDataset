<%
Response.Redirect "index.asp?"&Request.ServerVariables("QUERY_STRING")
%>