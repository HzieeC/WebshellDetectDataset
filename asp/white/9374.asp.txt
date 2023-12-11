<%
' ================================================

' ================================================== 
Class LeadWit_FSO	
	Public LZFSO,FSOInstall
	Private FSObject			'FSO组件名称
	
	Private Sub Class_Initialize()
		FSObject	= "Scripting.FileSystemObject"
		On Error Resume Next
		Set LZFSO = Server.CreateObject(FSObject)
		If Err Then
			err.Clear
'			Response.write "您的服务器不支持FSO"
'			Response.end
		End If

		FSOInstall = IsObjInstalled(FSObject) 
	End Sub	
	
	Private Sub class_terminate()
		If Isobject(LZFSO) Then
			Set LZFSO = Nothing
		End If
	End Sub

	Private Sub FSOMessage()
		If Not FSOInstall Then
			Response.write "您的服务器不支持FSO"
			Response.end
		End If
	End Sub

	'=========================================================
	'◆是否支持组件
	'=========================================================
	Public Function IsObjInstalled(strClassString)
		On Error Resume Next
		IsObjInstalled = True
		Dim xTestObj
		Set xTestObj = Server.CreateObject(strClassString)
		If Err Then
			err.Clear
			IsObjInstalled = False
		End If
		Set xTestObj = Nothing
	End Function

	'=========================================================
	'◆检查某一目录是否存在
	'=========================================================
	Public Function CheckDir(FolderPath)
		FSOMessage()
		folderpath	= Server.MapPath(".")&"\"&folderpath
		If LZFSO.FolderExists(FolderPath) then
			CheckDir = True
		Else
			CheckDir = False
		End if
	End Function
	
	'=========================================================
	'◆ 根据指定名称生成目录
	'=========================================================
	Public Function CreateDir(strPath)
		FSOMessage()
		Dim sTmp,i,Fso,sPath
		sPath = replace(strPath,"/","\")
		sPath = Split(sPath,"\")
		If Not IsObject(LZFSO) Then Set LZFSO = Server.CreateObject(FSObject)
		For I=0 To Ubound(sPath)
			sTmp = Replace(sTmp&sPath(I)&"\","\\","\")
			If Not LZFSO.FolderExists(server.Mappath(sTmp)) Then
				LZFSO.CreateFolder server.Mappath(sTmp)			
			End if
		Next
	End Function
	
	
	'=========================================================
	'◆              删除文件夹            ◆
	'=========================================================
	Public Function DeleDir(strPath)
		FSOMessage()
		Dim SystemFolder
		SystemFolder = ",admin,inc,skins,data,User,"
		if LZFSO.FolderExists(Server.MapPath(strPath)) then
			LZFSO.DeleteFolder(Server.MapPath(strPath))
		End if
	End Function

	'=========================================================
	'◆            更改文件夹名称          ◆
	'=========================================================
	Public Function MoveFolder(foldername,newfoldername)
		FSOMessage()
		LZFSO.MoveFolder request.ServerVariables("APPL_PHYSICAL_PATH")&"\"&foldername&"" ,""&request.ServerVariables("APPL_PHYSICAL_PATH")&"\"&newfoldername&""
	End Function

	'=========================================================
	'◆            删除指定文件            ◆
	'=========================================================
	Public Function DelFile(sFile)
		FSOMessage()
		On Error Resume Next
		If LZFSO.FileExists(server.Mappath(sFile)) Then LZFSO.DeleteFile(server.Mappath(sFile))
	End Function

	'=========================================================
	'◆            备份指定文件            ◆
	'=========================================================
	Public Function CopyFile(oldfile,newfile)
		FSOMessage()
		oldfile = Server.MapPath(oldfile)
		If LZFSO.FileExists(oldfile) Then
			Response.write "原路径错误!"
			REsponse.end
		End If
		newfile = Server.MapPath(newfile)
		LZFSO.CopyFile oldfile,newfile		'覆盖原来的文件
		if Err.Number>0 Then
			Response.write Err.Description
			REsponse.end
		End IF
	End Function
'	Sub CopyFile(sFile,tPath)
'		If LZFSO.FileExists(server.Mappath(sFile)) Then LZFSO.CopyFile server.Mappath(sFile), server.Mappath(tPath)
'	End Sub
	
	
	'=========================================================
	'◆            复制指定目录            ◆
	'=========================================================
	Public Function CopyDir(oldPath,NewPath)
		FSOMessage()
		If Not LZFSO.FolderExists(Server.MapPath(oldPath)) Then Response.write "源文件夹不存在，操作停止！":Response.end
		LZFSO.CopyFolder Server.MapPath(oldPath),Server.MapPath(NewPath)
	End Function

	'=========================================================
	'◆            转移指定文件            ◆
	'=========================================================
	Public Function MoveFile(oldfile,newfile)
		FSOMessage()
		On Error Resume Next 
		oldfile = Server.MapPath(oldfile)
		If Not LZFSO.FileExists(oldfile) Then
			Response.write "原路径错误!"
			REsponse.end
		End If
		
		newfile=Server.MapPath(newfile)
		
		If Not LZFSO.FileExists(newfile) Then
			Response.write "新路径错误或文件已存在!"
			REsponse.end
		End IF
		
		LZFSO.MoveFile oldfile,newfile		'不能覆盖原来的文件
		if Err.Number>0 Then
			Response.write Err.Description
			REsponse.end
		End IF
	End Function
	
	'重命名文件
	Public Function ReFileName(P_FilePath,NewFileName)
		FSOMessage()
		Dim f
		On Error Resume Next 
		Set f = LZFSO.GetFile(server.mappath(P_FilePath))
		If Not LZFSO.FileExists(oldfile) Then
			Err.Clear
			ReFileName = False
		End If
		f.name = NewFileName
		Set f=Nothing
		ReFileName = True
	End Function

	'=========================================================
	'◆             读取文件代码           ◆
	'=========================================================
	Public Function loadfile(file)		'读取文件
		FSOMessage()
		dim ftemp
		Set ftemp	= LZFSO.OpenTextFile(Server.MapPath(""&file&""), 1)
		loadfile	= ftemp.ReadAll
		ftemp.Close
	End Function
	
	'=========================================================
	'◆         根据代码生成文件           ◆
	'=========================================================
	'========================================
	'■strPathName生成文件名
	'■Content文件的代码
	'========================================
	Public Function CreateFile(Content,strPathName)
		FSOMessage()
		On Error Resume Next 
		Dim File
		Set File = LZFSO.CreateTextFile(Server.MapPath(strPathName), True)
		File.Write Content
		Set File = nothing
		if Err Then
			err.Clear
			Response.write Err.Description
			REsponse.end
		End IF
	End Function

	'===========================================================
	'将Rs对象转换为XML文件
	'ObjName：对象名
	'===========================================================
	Public Function GetXMLStr(ObjName)
		FSOMessage()
		Dim varItem,strXML
		strXML = "<?xml version=""1.0"" encoding=""gb2312""?><xml>" & vbnewline
		With ObjName
			Do While NOT ObjName.EOF 
				strXML = strXML & Chr(9) &"<row>"  & vbnewline
				For Each varItem In ObjName.Fields 
					strXML = strXML & Chr(9) & Chr(9) & "<" & varItem.name & ">"
					If Not IsNumeric(varItem.value) Then
						strXML = strXML &"<![CDATA["& varItem.value & "]]>"
					Else
						strXML = strXML & varItem.value
					End IF
					strXML = strXML &"</" & varItem.name & ">"  & vbnewline
				Next 
				strXML = strXML & Chr(9) & "</row>"  & vbnewline
				ObjName.MoveNext 
			Loop
		End With
		strXML = strXML & "</xml>" 
		'Set ObjName = Nothing
		GetXMLStr = strXML
	End Function
	
	
	'私有接口,取得请求后返回的html
	Public Function getHTTPURL(strPath)
        Dim strTemp
		strTemp			= GetBody(strPath)
        getHTTPURL		= BytesToBstr(strTemp,"GB2312")
	End function

	'私有接口,取得请求后返回的html Stream
	Private Function GetBody(strURL)
		Dim Retrieval
		On Error Resume Next
'        Set Retrieval	= Server.CreateObject("Microsoft.XMLHTTP") 
		Set Retrieval	= Server.CreateObject("Msxml2.ServerXMLHTTP.3.0")		
        With Retrieval 
			.Open "GET", strURL, False, "", "" 
			.Async = False
			.Send 
			GetBody	= .ResponseBody
        End With 
        Set Retrieval	= Nothing
	End Function

	'POST方法，私有接口,取得请求后返回的html
	Public Function postHTTPURL(strPath,PostData)
        Dim strTemp
		strTemp			= PostBody(strPath,PostData)
        postHTTPURL		= BytesToBstr(strTemp,"GB2312")
	End function

	'POST方法，私有接口,取得请求后返回的html Stream
	Private Function PostBody(strURL,PostData)
		Dim Retrieval
		On Error Resume Next
'        Set Retrieval	= Server.CreateObject("Microsoft.XMLHTTP") 
		Set Retrieval	= Server.CreateObject("Msxml2.ServerXMLHTTP.3.0")		
        With Retrieval 
			.Open "POST", strURL, False, "", ""
			.setRequestHeader "CONTENT-TYPE","application/x-www-form-urlencoded"
			.Send PostData
			PostBody	= .ResponseBody
        End With 
        Set Retrieval	= Nothing
	End Function

	'私有接口,转换Stream-->String
	Private Function BytesToBstr(strBody,strCharset)
		If IsNull(strBody) Then Exit Function
        Dim streamObj
        set streamObj	= Server.CreateObject("ADODB.Stream")
	On Error Resume Next
        With streamObj 
			.Type		= 1
			.Mode		= 3
			.Open
			.Write strBody
			.Position	= 0
			.Type		= 2
			.Charset	= strCharset
			BytesToBstr	= .ReadText 
			.Close
        End With 
	if err Then
		err.Clear
		BytesToBstr = "NG"
	End IF
        Set streamObj	= Nothing
	End Function
	
End Class
%>