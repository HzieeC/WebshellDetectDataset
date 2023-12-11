<!--#include file="Inc/config.asp"-->
<html>
<head>
<title>上传文件</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<link rel="stylesheet" type="text/css" href="editor_dialog.css">
<SCRIPT language=javascript>
function check() 
{
	var strFileName=document.form1.FileName.value;
	if (strFileName=="")
	{
    	alert("请选择要上传的文件");
		document.form1.FileName.focus();
    	return false;
  	}
}
</SCRIPT>
</head>
<body bgColor=menu leftmargin="5" topmargin="0">
<%
if EnableUploadFile="Yes" and session("AdminName")<>"" then
%>
<form action="Upfile_Dialog.asp" method="post" name="form1" onSubmit="return check()" enctype="multipart/form-data">
  <input name="FileName" type="FILE" class="tx1" size="35">
  <input type="submit" name="Submit" value="上传">
  <input name="DialogType" type="hidden" id="DialogType" value="<%=trim(request("DialogType"))%>">
</form>
<%
else
	response.write "对不起，本网站不允许上传文件！"
end if
%>
</body>
</html>
