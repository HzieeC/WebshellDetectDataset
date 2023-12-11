<!--#include file="Inc/config.asp"-->
<!--#include file="Inc/upfile_class_other.asp"-->
<%
const upload_type=0   '上传方法：0=无惧无组件上传类，1=FSO上传 2=lyfupload，3=aspupload，4=chinaaspupload

dim upload,file,formName,SavePath,filename,fileExt
dim upNum
dim EnableUpload
dim Forumupload
dim ranNum
dim uploadfiletype
dim msg,founderr
msg=""
founderr=false
EnableUpload=false
SavePath = SaveUpFilesPath
if right(SavePath,1)<>"/" then SavePath=SavePath&"/" '在目录后加(/)
%>
<%
ComeinSTR=lcase(request.servervariables("HTTP_HOST"))
Url=split(ComeinSTR)
yourthing=Url(0)
%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<style type="text/css">
<!--
body {
	font-size: 12px;
}
-->
</style>
</head>
<body class="snail1">
<%
if EnableUploadFile="NO" then
	response.write "系统未开放文件上传功能"
else
	if session("AdminName")="" and session("UserName")="" then
		response.Write("请登录后再使用本功能！")
	else
		select case upload_type
			case 0
				call upload_0()  '使用化境无组件上传类
			case else
				'response.write "本系统未开放插件功能"
				'response.end
		end select
	end if
end if
%>
</body>
</html>
<%
sub upload_0()    '使用化境无组件上传类
	set upload=new upload_file    '建立上传对象
	for each formName in upload.file '列出所有上传了的文件
		set file=upload.file(formName)  '生成一个文件对象
		if file.filesize<100 then
 			msg="请先选择你要上传的文件！"
			founderr=true
		end if
		if file.filesize>(MaxFileSize*1024) then
 			msg="文件大小超过了限制，最大只能上传" & CStr(MaxFileSize) & "K的文件！"
			founderr=true
		end if

		fileExt=lcase(file.FileExt)
		Forumupload=split(UpFileType,"|")
		for i=0 to ubound(Forumupload)
			if fileEXT=trim(Forumupload(i)) then
				EnableUpload=true
				exit for
			end if
		next
		if fileEXT="asp" or fileEXT="asa" or fileEXT="aspx" then
			EnableUpload=false
		end if
		if EnableUpload=false then
			'msg="这种文件类型不允许上传！\n\n只允许上传这几种文件类型：" & UpFileType
                        response.write"<SCRIPT language=JavaScript>alert('这种文件类型不允许上传！\n\n只允许上传这几种文件类型：" & UpFileType & "');"
                        response.write"javascript:history.go(-1)</SCRIPT>"
         founderr=true
		end if
		
		strJS="<SCRIPT language=javascript>" & vbcrlf
		if founderr<>true then
			randomize
			ranNum=int(900*rnd)+100
			filename=SavePath&year(now)&month(now)&day(now)&hour(now)&minute(now)&second(now)&ranNum&"."&fileExt

			file.SaveToFile Server.mappath(FileName)   '保存文件
			msg="上传文件成功！"
			FileType=right(fileExt,3)
			select case FileType
			    case "jpg","gif","png","bmp"				
				case else
		             strJS=strJS & "range.text='[upload=" & FileType & "]" & FileName & "[/upload]';" & vbcrlf
	  		end select			
	  		strJS=strJS & "parent.parent.myform.img.value='" & filename & "';" & vbcrlf
			strJS=strJS & "parent.parent.myform.content.value='" & filename & "';" & vbcrlf
			
		end if
		strJS=strJS & "alert('" & msg & "');" & vbcrlf	  		
	  	strJS=strJS & "</script>"
	  	response.write strJS
		response.write "图片上传成功!文件路径是把  [img]" & filename & "[/img]  复制到上面的编辑框内<br>"
		response.write "http://" & yourthing & "/" & filename & "<br>"
		set file=nothing
	next
	set upload=nothing
end sub
%>
