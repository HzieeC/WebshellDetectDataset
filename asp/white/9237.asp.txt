<%
SQL_injdata = "'|;|and|exec|insert|select|delete|update|count|*|%|chr|mid|master|truncate|char|declare"
SQL_inj = split(SQL_Injdata,"|")
If Request.Form<>"" Then
For Each Sql_Post In Request.Form
For SQL_Data=0 To Ubound(SQL_inj)
if instr(Request.Form(Sql_Post),Sql_Inj(Sql_DATA))>0 Then
response.write "<script language='javascript'>"
response.write "alert('网站安全提示：请不要在提交的内容中包含非法字符，不能包含半角'、;、*、%等符号！');"
response.write "location.href='javascript:history.go(-1)';"
response.write "</script>"
response.end
end if
next
next
end if
If Request.QueryString<>"" Then
For Each SQL_Get In Request.QueryString
For SQL_Data=0 To Ubound(SQL_inj)
if instr(Request.QueryString(SQL_Get),Sql_Inj(Sql_DATA))>0 Then
response.write "<script language='javascript'>"
response.write "alert('网站安全提示：请不要在提交的内容中包含非法字符，不能包含半角'、;、*、%等符号！');"
response.write "location.href='javascript:history.go(-1)';"
response.write "</script>"
response.end
end if
next
Next
end If
%>
