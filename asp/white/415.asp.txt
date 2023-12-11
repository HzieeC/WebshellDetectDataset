<!--#include file="../inc/access.asp"-->
</style><!--#include file="upload.inc"-->
<%  
dim upload,file,formName,formPath,iCount,filename,fileExt
set upload=new upload_5xSoft ''建立上传对象
formPath="/photos"
 ''在目录后加(/)
if right(formPath,1)<>"/" then formPath=formPath&"/" 
iCount=0
for each formName in upload.file ''列出所有上传了的文件
 set file=upload.file(formName)  ''生成一个文件对象
 if file.filesize<100 then
response.Write "<script language='javascript'>alert('请选择你要上传的图片！');location.href='upload.asp';</script>"
	response.end
 end if
 	
 if file.filesize>1000000 then '上传图片大小限制，100000为100KB，可以继续增大………………
response.Write "<script language='javascript'>alert('图片大小超过了限制，请重新上传！');location.href='upload.asp';</script>"
	response.end
 end if

 fileExt=lcase(right(file.filename,4))

 if fileEXT<>".gif" and fileEXT<>".jpg" and fileEXT<>".bmp" then
response.Write "<script language='javascript'>alert('文件格式不对，请重新上传！');location.href='upload.asp';</script>"
	response.end
 end if 
 randomize
 ranNum=int(90000*rnd)+10000
 filename=formPath&year(now)&month(now)&day(now)&hour(now)&minute(now)&second(now)&fileExt
 filename1=year(now)&month(now)&day(now)&hour(now)&minute(now)&second(now)&fileExt
 
 if file.FileSize>0 then         ''如果 FileSize > 0 说明有文件数据
 file.SaveAs Server.mappath(filename)   ''保存文件
 'response.write "<script>parent.form1.textfield6.value='"&FileName1&"'</'script>"
 response.write  fielname1
  iCount=iCount+1
 end if
 set file=nothing
next
set upload=nothing  ''删除此对象
'response.write "<script>parent.form1.textfield6.value="&Filename1&"<'/script>"
session("upface")="done"
sub HtmEnd(Msg)
 set upload=nothing
 response.write "图片上传成功"
 response.end
end sub
%>
</body>
</html>
<%response.redirect("upload.asp?FileName="&Filename1&"")%>