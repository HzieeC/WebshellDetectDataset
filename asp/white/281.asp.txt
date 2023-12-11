<HTML>
<HEAD>
<TITLE>eWebEditor ： 输入框调用上传文件对话框示例</TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<link rel='stylesheet' type='text/css' href='example.css'>
</HEAD>
<BODY>

<p><b>导航 ： <a href="default.asp">示例首页</a> &gt; 输入框调用上传文件对话框示例</b></p>
<p>通过使用这个功能，您可以在任意的输入框中调用编辑器自带的上传文件功能，上传文件和浏览服务器上原已上传的文件，并返回输入框上传文件的路径文件名。您可以在一个网页的任意多个地方调用，支持分类型上传及缩略图调用，并可在编辑器后台设置。一行调用代码，即可为您的网站加入上传文件功能。</p>
<p>点击下面的“上传”按钮，看一下效果，不同的按钮允许上传的文件类型及文件大小不同。</p>


<script language="javascript">
// 参数说明
// s_Type : 文件类型，可用值为"image","flash","media","file"
// s_Link : 文件上传后，用于接收上传文件路径文件名的表单名
// s_Thumbnail : 文件上传后，用于接收上传图片时所产生的缩略图文件的路径文件名的表单名，当未生成缩略图时，返回空值，原图用s_Link参数接收，此参数专用于缩略图
function showUploadDialog(s_Type, s_Link, s_Thumbnail){
	//以下style=coolblue,值可以依据实际需要修改为您的样式名,通过此样式的后台设置来达到控制允许上传文件类型及文件大小
	var arr = showModalDialog("../dialog/i_upload.htm?style=coolblue&type="+s_Type+"&link="+s_Link+"&thumbnail="+s_Thumbnail, window, "dialogWidth:0px;dialogHeight:0px;help:no;scroll:no;status:no");
}
</script>


<FORM method="post" name="myform" action="">

1. 此示例允许上传图片类型文件：<br>
<input type=text name=d_image size=50> <input type=button value="上传图片..." onclick="showUploadDialog('image', 'myform.d_image', '')">
<br><br>
2. 此示例允许上传Flash类型文件：<br>
<input type=text name=d_flash size=50> <input type=button value="上传Flash..." onclick="showUploadDialog('flash', 'myform.d_flash', '')">
<br><br>
3. 此示例允许上传媒体类型文件：<br>
<input type=text name=d_media size=50> <input type=button value="上传媒体..." onclick="showUploadDialog('media', 'myform.d_media', '')">
<br><br>
4. 此示例允许上传附件类型文件：<br>
<input type=text name=d_file size=50> <input type=button value="上传文件..." onclick="showUploadDialog('file', 'myform.d_file', '')">
<br><br>




<INPUT type=button value="查看源文件" onclick="location.replace('view-source:'+location)"> 

</FORM>

</BODY>
</HTML>
