<!--#include file="AspCms_MainClass.asp" -->
<%
Dim ContentID
ContentID=filterPara(getForm("id","get"))
Dim rsObj
set rsObj=conn.Exec("select Visits from {prefix}Content where ContentID="&ContentID&"", "r1")
if not rsObj.eof then echo "document.write("&rsObj(0)&")"
rsObj.close : set rsObj=nothing
%>