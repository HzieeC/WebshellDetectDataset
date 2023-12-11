<%@ language="VBScript"%>
<%response.Expires = 0%>
<!--#include file="Conn.asp"-->
<!--#include file="Admin.asp"-->
<!--#include file="inc/md5.asp"-->
<!--#include file="inc/function.asp"-->
<%
password=replace(trim(Request("password")),"'","")
UserName=replace(trim(Request("UserName")),"'","")

if strLength(password)>16 or strLength(password)<6 then
 response.write  "<script language=javascript>alert('请输入的密码位数不能小于6位或大于16位!');history.go(-1);</script>"
  response.End
end if

'查找数据库，检查此管理员是否已经存在
set rs=server.createobject("adodb.recordset")
sql="select * from admin where UserName='" & UserName & "'"
rs.open sql,conn,1,1

if rs.recordcount >= 1 then
  response.write  "<script language=javascript>alert('此管理员帐号已经存在，请选用其他帐号!');history.go(-1);</script>"
  response.End
  rs.close
  set rs=nothing
end if

password=md5(password)
set rs=server.createobject("adodb.recordset")
sql="select * from admin"
rs.open sql,conn,1,3

'添加一个管理员帐号到数据库
rs.addnew
rs("UserName")=UserName
rs("PassWord")=password
rs.update
rs.close
set rs=nothing
Response.Redirect "Admin_Manage.asp"
%>