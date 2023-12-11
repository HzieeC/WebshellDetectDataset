<%@EnableSessionState=False%> 
<%
Response.CacheControl = "no-cache"
Response.Expires = -1
response.Write Application(request.querystring("processid"))
%>