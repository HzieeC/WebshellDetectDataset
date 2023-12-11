<!--#include file="AspCms_MainClass.asp" -->
<%
Dim ContentID
ContentID=filterPara(getForm("id","get"))
conn.Exec  "update {prefix}Content set Visits=Visits+1 where ContentID="&ContentID&"", "exe"
%>