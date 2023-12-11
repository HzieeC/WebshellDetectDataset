<!--#include file="../inc/AspCms_SettingClass.asp" -->
<%
dim id,rs
id=getForm("id","get")
set rs=conn.exec("select * from {prefix}Content where ContentID="&id,"r1")
if not rs.eof then 
	response.Redirect("/productbuy/?"&rs("sortid")&"_"&id&".html")
end if

%>