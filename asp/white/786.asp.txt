<!--#include file="Inc/config.asp"-->
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<style type="text/css">
<!--
BODY{
BACKGROUND-COLOR: #F2F8FF;
font-size:9pt
}
.tx1 { height: 20px;font-size: 9pt; border: 1px solid; border-color: #000000; color: #0000FF}
-->
</style>
</head>
<body leftmargin="0" topmargin="0">
<%
if EnableUploadFile="Yes" and (session("AdminName")<>"" or session("UserName")<>"") then
%>
<form action="upfile_Other.asp" method="post" name="form1" enctype="multipart/form-data">
  <input name="FileName" type="FILE" class="tx1" size="20">
  <input type="submit" name="Submit" value="上传" style="border:1px double rgb(88,88,88);font:9pt">
</form>
<%
end if
%>
</body>
</html>
