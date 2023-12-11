<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include File="inc/Upload_Class.asp"-->
<%
If Dvbbs.UserID=0 Then Response.End
Dim useFor
useFor = Trim(Request("useFor"))
If Request("t")="1" Then
	Upfile_Main()
Else
	Main()
End If
Dvbbs.PageEnd()
Sub Main()
	Dvbbs.LoadTemplates("usermanager")
	Dvbbs.Stats=Dvbbs.MemberName&template.Strings(1)
	Dvbbs.Head()
	Dim PostRanNum
	Randomize
	PostRanNum = Int(900*rnd)+1000
	Session("UploadCode") = Cstr(PostRanNum)
	Session("upface")=""'o 08.01.25 10:47
%>
	<table border="0"  cellspacing="0" cellpadding="0" width="100%">
	<tr>
	<td class="tablebody2">
	<form name="form" method="post" action="reg_upload.asp?t=1&useFor=<%=useFor%>" enctype="multipart/form-data">
	<INPUT TYPE="hidden" NAME="UploadCode" value="<%=PostRanNum%>">
	<input type="hidden" name="filepath" value="uploadFace">
	<input type="hidden" name="act" value="upload">
	<input type="file" name="file1">
	<input type="hidden" name="fname">
	<%If useFor<>"grouplogo" Then%>
		<input type="submit" name="Submit" value="上传" onclick="fname.value=file1.value,parent.document.theForm.Submit.disabled=true,parent.document.theForm.Submit2.disabled=true;" />
	<%Else%>
		<input type="submit" name="Submit" value="上传" />
	<%End If%>
	</form>
</body>
</html>
<%
End Sub

Sub Upfile_Main()
	Dvbbs.LoadTemplates("usermanager")
	Dvbbs.Stats = Dvbbs.MemberName & Template.Strings(1)
	Dvbbs.Head()
	If useFor<>"grouplogo" Then
%>
		<table width="100%" height="100%" border=0  cellspacing="0" cellpadding="0">
		<tr><td class=tablebody1 width="100%" height="100%" >
		<script>
			parent.document.theForm.Submit.disabled=false;
			parent.document.theForm.Submit2.disabled=false;
		</script>
<%
	End if
	UploadFile
%>
</td></tr></table>
</body>
</html>
<%
End Sub
'---------------------------------------------------------------
'头像上传开始
'---------------------------------------------------------------
Sub UploadFile()
	'-----------------------------------------------------------------------------
	'提交验证
	'-----------------------------------------------------------------------------
	If Not Dvbbs.ChkPost Then
		Exit Sub
	End If
	If Session("upface")="done" And useFor<>"grouplogo" Then
		Response.Write "您已经上传了头像"
		Exit Sub
	End If
	If SysSetting(Dvbbs.Forum_UploadSetting(0)) = False or Clng(Dvbbs.Forum_Setting(53)) = 0 And useFor<>"grouplogo" Then
		Response.Write "本系统未开放上传了头像功能"
		Exit Sub
	End If
	If Dvbbs.UserID>0 And useFor<>"grouplogo" Then
		If Clng(Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@userpost").text)>Clng(Dvbbs.Forum_Setting(54)) Then
			UpUserFace()	'删除旧的头像文件
		Else
			Response.Write "只有文章数多于"& Dvbbs.Forum_Setting(54) &"篇才可以自定义头像！"
			Exit Sub
		End If
	End If
	'-----------------------------------------------------------------------------
	Dim Upload,FilePath,FormName,File,F_FileName
	Dim UserID
	UserID = ""
	If Dvbbs.UserID>0 Then UserID = Dvbbs.UserID&"_"
	FilePath = "UploadFace/"
	Set Upload = New UpFile_Cls
		Upload.UploadType			= Cint(Dvbbs.Forum_UploadSetting(2))	'设置上传组件类型
		Upload.UploadPath			= FilePath								'设置上传路径
		Upload.MaxSize				= Int(Dvbbs.Forum_UploadSetting(1))		'单位 KB
		Upload.InceptMaxFile		= 1										'每次上传文件个数上限
		Upload.InceptFileType		= "gif,jpg,bmp,jpeg,png"				'设置上传文件限制
		Upload.RName				= UserID
		Upload.ChkSessionName		= "UploadCode"
		'执行上传
		Upload.SaveUpFile
		If Upload.ErrCodes<>0 Then
			Response.write "错误："& Upload.Description & "[ <a href=""reg_upload.asp"">重新上传</a> ]"
			Exit Sub
		End If
		If Upload.Count > 0 Then
			For Each FormName In Upload.UploadFiles
				Set File = Upload.UploadFiles(FormName)
					F_FileName = FilePath & File.FileName
					If useFor="grouplogo" Then
						Response.Write "<script>parent.document.getElementById('grouplogo').value='" &F_FileName& "';</script>"
					Else
						Response.Write "<script>parent.document.images['face'].src='" &F_FileName& "';parent.document.theForm.myface.value='"&F_FileName&"';</script>"
						If File.FileWidth>0 and File.FileHeight>0 Then
							Response.Write "<script>parent.document.images['face'].width='" &File.FileWidth& "';parent.document.images['face'].height='"&File.FileHeight&"';</script>"
							Response.Write "<script>parent.document.theForm.height.value='" &File.FileHeight& "';parent.document.theForm.width.value='"&File.FileWidth&"';</script>"
						End If
					End If
					Session("upface")="done"
					Response.Write "图片"& F_FileName &"上传成功!"
				Set File = Nothing
			Next
		Else
			Response.write "请正确选择要上传的文件。[ <a href=""reg_upload.asp"">重新上传</a> ]"
			Exit Sub
		End If
	Set Upload = Nothing
End Sub

'删除旧头像
Sub UpUserFace()
	If Dvbbs.UserID=0 Then Exit Sub
	If not IsNumeric(Dvbbs.UserID) Then Exit Sub
	on Error Resume Next
	Dim objFSO,OldUserFace
	OldUserFace = Server.MapPath("UploadFace/"&Dvbbs.UserID&"_")&"*.*"
	Set objFSO = Dvbbs.iCreateObject("Scripting.FileSystemObject")
	'If objFSO.FileExists(OldUserFace) Then
		objFSO.DeleteFile OldUserFace
		If Err<>0 Then Err.Clear
	'End If
	Set objFSO = Nothing
End Sub

'系统设置
Function SysSetting(Setting)
	SysSetting = False
	Select Case Clng(Setting)
		Case 1 : SysSetting = True
		Case 2 :
		If Dvbbs.UserID > 0 Then SysSetting = True
	End Select
End Function
%>