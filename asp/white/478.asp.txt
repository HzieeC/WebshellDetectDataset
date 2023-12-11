<%

Class Cls_Admin

	Dim vID
	Dim vUsername
	Dim vPassword
	Dim vLevels
	Dim vManagePlus
	Dim vManageChannel
	Dim vUploadfileexts
	Dim vUploadfilesize
	Dim LastError

	Private Sub Class_Initialize()
		Call ChkLogin("admin")
		Call Initialize()
	End Sub

	Private Sub Class_Terminate()
		Call Initialize()
	End Sub

	Public Function Initialize()
		vID = 0
		vUsername = ""
		vPassword = ""
		vLevels = ""
		vManagePlus = ""
		vManageChannel = ""
		vUploadfileexts = "gif/jpg/png/bmp/jpeg/mp3/wma/rm/rmvb/ra/asf/avi/wmv/swf/rar/exe/zip/doc/xls"
		vUploadfilesize = 1024
	End Function

	Public Function GetValue()
		vUsername = Request.Form("oUsername")
		vPassword = Request.Form("oPassword")
		vLevels = Replace(Request.Form("oLevels")," ","")
		vManagePlus = Replace(Request.Form("oManagePlus")," ","")
		vManageChannel = Replace(Request.Form("oManageChannel")," ","")
		vUploadfileexts = Request.Form("oUploadfileexts")
		vUploadfilesize = Request.Form("oUploadfilesize")
		If Len(vPassword) <> 32 Then vPassword = MD5(vPassword , 32)
		If Len(vUsername) < 3 Or Len(vUsername) > 20 Then LastError = "管理员帐号的长度请控制在 3 至 20 位" : GetValue = False : Exit Function
		If Len(vPassword) < 3 Or Len(vPassword) > 50 Then LastError = "管理员密码的长度请控制在 3 至 50 位" : GetValue = False : Exit Function
		If Len(vLevels) > 200 Then vLevels = Left(vLevels,200)
		If Len(vManagePlus) > 200 Then vManagePlus = Left(vManagePlus,200)
		If Len(vManageChannel) > 200 Then vManageChannel = Left(vManageChannel,200)
		If LCase(Request.Form("oManageChannelALL")) = "yes" And Len(vManageChannel)=0 Then vManageChannel = 0
		if LCase(getLogin("admin","username")) = LCase(vUsername) Then
			call setLogin("admin","levels",vLevels)
			call setLogin("admin","manageplus",vManagePlus)
			call setLogin("admin","managechannel",vManageChannel)
			call setLogin("admin","uploadfileexts",vUploadfileexts)
			call setLogin("admin","uploadfilesize",vUploadfilesize)
                End If
		vUploadfileexts = Left(vUploadfileexts,100)
		vUploadfilesize = Left(vUploadfilesize,50)
		GetValue = True
	End Function

	Public Function SetValue()
		Dim Rs
		Set Rs = DB("Select * From [{pre}Admin] Where [ID]=" & vID,1)
		If Rs.Eof Then Rs.Close : Set Rs = Nothing : LastError = "你所需要查询的记录 " & vID & " 不存在!" : SetValue = False : Exit Function
		vUsername = Rs("Username")
		vPassword = Rs("Password")
		vLevels = Rs("Levels")
		vManagePlus = Rs("ManagePlus")
		vManageChannel = Rs("ManageChannel")
		vUploadfileexts = Rs("Uploadfileexts")
		vUploadfilesize = Rs("Uploadfilesize")
		Rs.Close
		Set Rs = Nothing
		SetValue = True
	End Function

	Public Function Create()
		Dim Rs
		Set Rs = DB("Select [ID] From [{pre}Admin] Where [Username]='" & vUsername & "'",1)
		If Not Rs.Eof Then Rs.Close : Set Rs = Nothing : LastError = "管理员帐号 的值 " & vUsername & " 已存在!" : Create = False : Exit Function
		Set Rs = DB("Select * From [{pre}Admin]",3)
		Rs.AddNew
		Rs("Username") = vUsername
		Rs("Password") = vPassword
		Rs("Levels") = vLevels
		Rs("ManagePlus") = vManagePlus
		Rs("ManageChannel") = vManageChannel
		Rs("Uploadfileexts") = vUploadfileexts
		Rs("Uploadfilesize") = vUploadfilesize
		Rs.Update
		Rs.Close
		Set Rs = Nothing
		Create = True
	End Function

	Public Function Modify()
		Dim Rs
		Set Rs = DB("Select * From [{pre}Admin] Where [ID]=" & vID,3)
		If Rs.Eof Then Rs.Close : Set Rs = Nothing : LastError = "你所需要更新的记录 " & vID & " 不存在!" : Modify = False : Exit Function
		Rs("Username") = vUsername
		Rs("Password") = vPassword
		Rs("Levels") = vLevels
		Rs("ManagePlus") = vManagePlus
		Rs("ManageChannel") = vManageChannel
		Rs("Uploadfileexts") = vUploadfileexts
		Rs("Uploadfilesize") = vUploadfilesize
		Rs.Update
		Rs.Close
		Set Rs = Nothing
		Modify = True
	End Function

	Public Function Delete()
		Call SetValue()
		DB "Delete From [{pre}Admin] Where [ID]=" & vID ,0
		Delete = True
	End Function

End Class
%>