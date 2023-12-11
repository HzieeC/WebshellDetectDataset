<!-- #include file="../inc/access.asp" -->
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<LINK href="../Style.css" rel=stylesheet type=text/css>
<style type="text/css">
<!--
.STYLE1 {font-size: 12px}
-->
</style>
</head>
<body leftmargin="0" topmargin="0">
<table bg>
  <form name="form" method="post" action="upfile_photo.asp?item=<%=request.querystring("item")%>" enctype="multipart/form-data" >
  <tr>
    <td width="212"><input type="hidden" name="filepath" value="uploadImages">
    <input type="hidden" name="act" value="upload">
    <input class=c type="file" name="file1" size=10 >
    <input type="submit" name="Submit" value="上传" class=c>    </td>
    <td width="232"><span class="forumRow STYLE1">&nbsp;图片格式:jpg,gif,bmp;大小:&lt; 1000K</span></td>
  </tr></form>
</table>
<%


if request.QueryString("filename")<>"" then 
itemname=request.querystring("item")
if itemname<>"" then
response.write "<script>parent.form1."&itemname&".value='"&request.QueryString("filename")&"'</script>"
else
response.write "<script>parent.form1.web_image.value='"&request.QueryString("filename")&"'</script>"
 end if
 end if
 %>