<!--#include file="../../../inc/AspCms_MainClass.asp" -->
<%
CheckAdmin("AspCms_update.asp")
On Error Resume Next
dim action : action=getForm("action","get")
Dim objUpdate,updatelist,newver,info,outmes,vnum,vstr,fs,adminpath
info=""
vstr=split(aspcmsVersion," ")
if ubound(vstr) then vnum=replace(vstr(1),"v","")


Set objUpdate = New Cls_oUpdate
	With objUpdate
	  .action=action
	  .UrlVersion = "http://up.aspcms.com/Update/Version.asp"
	  .UrlUpdate = "http://up.aspcms.com/Update/"
	  .UpdateLocalPath = "/"
	  .LocalVersion =vnum
	  .FileType      = ".asp"
	  .doUpdate
	  outmes=.outmes
	  info= .Info
	  updatelist = .updatelist
	  newver=.newver
	End With
Set objUpdate = Nothing

Class Cls_oUpdate
  Public LocalVersion, LastVersion, FileType,action,newver,updatelist,updatever
  Public UrlVersion, UrlUpdate, UpdateLocalPath, Info,outmes,yesup,newsitepath
  Public UrlHistory
  Private sstrVersionList, sarrVersionList, sintLocalVersion, sstrLocalVersion
  Private sstrLogContent, sstrHistoryContent, sstrUrlUpdate, sstrUrlLocal


  Public function doUpdate()
   doUpdate = False  
   UrlVersion    = Trim(UrlVersion)
   UrlUpdate    = Trim(UrlUpdate)

   If (Left(UrlVersion, 7) <> "http://") Or (Left(UrlUpdate, 7) <> "http://") Then
    alertMsgAndGo"升级地址检测异常", "AspCms_update.asp"
    Exit function
   End If
   
   If Right(UrlUpdate, 1) <> "/" Then 
    sstrUrlUpdate = UrlUpdate & "/"
   Else
    sstrUrlUpdate = UrlUpdate
   End If
   
   If Right(UpdateLocalPath, 1) <> "/" Then 
    sstrUrlLocal = UpdateLocalPath & "/"
   Else
    sstrUrlLocal = UpdateLocalPath
   End If   
   
   sstrLocalVersion = LocalVersion
   sintLocalVersion = Replace(sstrLocalVersion, ".", "")
   sintLocalVersion = toNum(sintLocalVersion, 0)
   
   If IsLastVersion Then Exit function
   doUpdate = NowUpdate()
   LastVersion = sstrLocalVersion
  End function
 
  Private function IsLastVersion()
    If iniVersionList Then
     Dim i,intVerL,newvers
     IsLastVersion = false
	 newvers=split(sarrVersionList(0),"{$}")
	 newver=newvers(0)
	 intVerL = toNum(Replace(newvers(0), ".", ""), 0)
      If intVerL <= LastVersion Then
       IsLastVersion = true
       Info = "已经是最新版本!"
      End If
    Else
     IsLastVersion = True
     Info = "获取版本信息时出错!(#2)"
    End If   
   End function

   Private function iniVersionList()
    iniVersionList = False
    
    Dim strVersion
    strVersion = getVersionList()
    If strVersion = "" Then
     Info = "出错......."
     Exit function
    End If
    
    sarrVersionList = Split(getVersionList, "{$end$}") 
    iniVersionList = True
   End function

   Private function getVersionList()
    getVersionList = GetContent(UrlVersion)
   End function

   Private function NowUpdate()
    Dim i,intVers,versnum
	dim updateid:updateid=getForm("id","get")
    For i = UBound(sarrVersionList) to 0 step -1
		yesup=false
		versnum=split(sarrVersionList(i),"{$}")	
		if ubound(versnum)=3 then    
			intVers = toNum(Replace(versnum(0), ".", ""), 0)
			LastVersion=toNum(Replace(LocalVersion, ".", ""), 0)
			If intVers > sintLocalVersion Then
			if action="update" then
				if cint(updateid)=cint(i) then yesup=true			
				Call doUpdateVersion(versnum(0))
				alertMsgAndGo"版本升级成功！", "AspCms_update.asp"
			end if
			Call doUpdateVersion(versnum(0))
			outmes=outmes&"<TABLE cellSpacing=0 cellPadding=2 width=""100%"" align=center border=1 bordercolor=""#cde6ff"">"&vbcrlf& _
	  "<TBODY>"&vbcrlf& _
	   " <TR>"&vbcrlf& _
		 " <TD align=center width=100 height=30>版本信息</TD>"&vbcrlf& _
		 " <TD width=""266"">"&versnum(1)&"("&versnum(0)&")</TD>"&vbcrlf& _
		  "<TD width=""122"" align=""center"">发布日期</TD>"&vbcrlf& _
		"  <TD width=""391"">"&versnum(2)&"</TD>"&vbcrlf& _
	   " </TR>"&vbcrlf& _
		"<TR>"&vbcrlf& _
		 " <TD align=center width=100 height=30>版本说明</TD>"&vbcrlf& _
		 " <TD colspan=""3"">"&versnum(3)&"</TD>"&vbcrlf& _
		"</tr>"&vbcrlf& _
		"<TR>"&vbcrlf& _
		  "<TD align=""center"" width=100 height=30><br>"&vbcrlf& _

			"更新文件</TD>"&vbcrlf& _
		  "<TD colspan=""3"" valign=""top"" >"&updatelist&"</TD>"&vbcrlf& _
		"</TR>"&vbcrlf& _
		 "<TR>"&vbcrlf& _
		  "<TD align=""center"" valign=""top"" width=100 height=30><br>"&vbcrlf& _
			"</TD>"&vbcrlf& _
		  "<TD colspan=""3"" valign=""top"" ><INPUT class=""button"" type=""submit"" value=""升级此补丁""  onClick=""if(confirm('升级有风险，建议先备份要升级的文件，您确认现在就升级？')){form.action='?action=update&id="&i&"';}else{return false};""/></TD>"&vbcrlf& _
		"</TR>"&vbcrlf& _
	 " </TBODY>"&vbcrlf& _
	"</TABLE><br/>"
	end if
		end if
	Next
   End function

   Private function doUpdateVersion(strVer)
    doUpdateVersion = False
    
    Dim intVer
    intVer = toNum(Replace(strVer, ".", ""), 0)

    If intVer <= sintLocalVersion Then
     Exit function
    End If
    
    Dim strFileListContent, arrFileList, strUrlUpdate   
    strUrlUpdate = sstrUrlUpdate & intVer & FileType
	strFileListContent = GetContent(strUrlUpdate)
    If strFileListContent = "" Then
     Exit function
    End If

    sintLocalVersion = intVer
    sstrLocalVersion = strVer
    Dim i, arrTmp
	
    arrFileList = Split(strFileListContent, vbCrLf)
    sstrLogContent = ""
	if sitePath<>"" then
		newsitepath= replace(sitePath,"/","")& "/"
	end if
		For i = 0 to UBound(arrFileList)
			arrTmp = Split(arrFileList(i), "|")
		set fs=server.CreateObject("scripting.filesystemobject")
		adminpath=fs.getbasename(fs.getparentfoldername(fs.getparentfoldername(fs.getparentfoldername(Request.ServerVariables("PATH_TRANSLATED")))))
 		
			If UBound(arrTmp)=1 Then
				if action="update" and yesup=true then				
					sstrLogContent = sstrLogContent& arrTmp(1) & "<br/>" 
						Call doUpdateFile(intVer & "/" & arrTmp(0), newsitepath &replace(arrTmp(1),"$admin$",adminpath))		  
				else		
						sstrLogContent = sstrLogContent  & newsitepath&replace(arrTmp(1),"$admin$",adminpath) & "<br/>"
						updatelist=sstrLogContent
				end if  
			end if
		Next	
    sstrLogContent = sstrLogContent & Now() & vbCrLf
    'response.Write("<pre>" & sstrLogContent & "</pre>")
   End function

   Private function doUpdateFile(strSourceFile, strTargetFile)
    Dim strContent
    strContent = GetContent(sstrUrlUpdate & strSourceFile)

    If sDoCreateFile(Server.MapPath(sstrUrlLocal &strTargetFile), strContent) Then     
     	sstrLogContent = sstrLogContent & "  更新成功" & vbCrLf
    Else
     	sstrLogContent = sstrLogContent & "  更新失败" & vbCrLf
    End If
   End function

   Private function GetContent(strUrl)
    GetContent = ""
    
    Dim oXhttp, strContent
    Set oXhttp = Server.CreateObject("Microsoft.XMLHTTP")
    'On Error Resume Next 
    With oXhttp
     .Open "GET", strUrl,False, "", ""
     .Send
     If .readystate <> 4 Then Exit function
     strContent = .Responsebody
     strContent = sBytesToBstr(strContent)
    End With
    
    Set oXhttp = Nothing
    If Err.Number <> 0 Then
     response.Write(Err.Description)
     Err.Clear
     Exit function
    End If
    
    GetContent = strContent
   End function

   Private function sBytesToBstr(vIn)
    dim objStream
    set objStream = Server.CreateObject("adodb.stream")
    objStream.Type    = 1
    objStream.Mode    = 3
    objStream.Open
    objStream.Write vIn
    
    objStream.Position  = 0
    objStream.Type    = 2
    objStream.Charset  = "GB2312"
    sBytesToBstr     = objStream.ReadText 
    objStream.Close
    set objStream    = nothing
   End function

   Private function sDoCreateFile(strFileName, ByRef strContent)
    sDoCreateFile = False
    Dim strPath
    strPath = Left(strFileName, InstrRev(strFileName, "\", -1, 1))

   If Not(CreateDir(strPath)) Then Exit function
    'If Not(CheckFileName(strFileName)) Then Exit function
    
    'response.Write(strFileName)
    Const ForReading = 1, ForWriting = 2, ForAppending = 8
    Dim fso, f
    Set fso = CreateObject("Scripting.FileSystemObject")
    Set f = fso.OpenTextFile(strFileName, ForWriting, True)
    f.Write strContent
    f.Close
    Set fso = nothing
    Set f = nothing
    sDoCreateFile = True
   End function

   Private function sDoAppendFile(strFileName, ByRef strContent)
    sDoAppendFile = False
    Dim strPath
    strPath = Left(strFileName, InstrRev(strFileName, "\", -1, 1))

    If Not(CreateDir(strPath)) Then Exit function

    Const ForReading = 1, ForWriting = 2, ForAppending = 8
    Dim fso, f
    Set fso = CreateObject("Scripting.FileSystemObject")
	
	
    Set f = fso.OpenTextFile(strFileName, ForAppending, True)
    f.Write strContent
    f.Close
    Set fso = nothing
    Set f = nothing
    sDoAppendFile = True
   End function

   Private function CreateDir(ByVal strLocalPath)
    Dim i, strPath, objFolder, tmpPath, tmptPath
    Dim arrPathList, intLevel
    
    'On Error Resume Next
    strPath     = Replace(strLocalPath, "\", "/")
    Set objFolder  = server.CreateObject("Scripting.FileSystemObject")
    arrPathList   = Split(strPath, "/")
    intLevel     = UBound(arrPathList)
    
    For I = 0 To intLevel
     If I = 0 Then
      tmptPath = arrPathList(0) & "/"
     Else
      tmptPath = tmptPath & arrPathList(I) & "/"
     End If
     tmpPath = Left(tmptPath, Len(tmptPath) - 1)
     If Not objFolder.FolderExists(tmpPath) Then objFolder.CreateFolder tmpPath
    Next
    
    Set objFolder = Nothing
    If Err.Number <> 0 Then
     CreateDir = False
     Err.Clear
    Else
     CreateDir = True
    End If
   End function

   Private function toNum(s, default)
    If IsNumeric(s) and s <> "" then
     toNum = CLng(s)
    Else
     toNum = default
    End If
   End function
 End Class
  If Err.Number <> 0 Then
     echo err.description
  	Err.Clear
  end if
%>
