<!--#include file="../../inc/AspCms_SettingClass.asp" -->
<%
CheckAdmin("AspCms_Data.asp")
if dbType=1 then die "SQL数据库不能使用此功能，此功能只支持Access数据库！"
dim action : action=getForm("action","get")
Select  case action
	case "del" : delBackUp
	case "bakup" : backUp
	case "delall" : delAll
	case "restore" : restore
	case "compress" : compress
End Select 



Sub databackList
'die "aa"
	dim path,fileListArray,i,arrayLen,fileAttr
	path = sitePath&"/data/backup"
	if not isExistFolder(path) then createFolder path,"folderdir"
	fileListArray= getFileList(path)
	arrayLen = ubound(fileListArray)

	if instr(fileListArray(0),",")>0 then	
		for  i = 0 to arrayLen
			fileAttr=split(fileListArray(i),",")
			 echo "<tr bgcolor=""#ffffff"" align=""left"" onMouseOver=""this.bgColor='#CDE6FF'"" onMouseOut=""this.bgColor='#FFFFFF'"">"&vbcrlf& _
			"<td align=""center"" ><input type=""checkbox"" name=""id"" value="""&fileAttr(4)&""" class=""checkbox"" /></td>"&vbcrlf& _
			"<td height=""28"">"&i+1&"</td>"&vbcrlf& _
			"<td><a href="""&fileAttr(4)&""" class=""txt_C1"" >"&fileAttr(0)&"</a></td>"&vbcrlf& _
			"<td>"&fileAttr(3)&"</td>"&vbcrlf& _
			"<td><a href=""?action=restore&path="&fileAttr(4)&""" class=""txt_C1"" onClick=""return confirm('确定恢复吗')"">恢复此备份</a> | <a href=""?action=del&path="&fileAttr(4)&""" class=""txt_C1"" onClick=""return confirm('确定要删除吗')"">删除</a></td>"&vbcrlf& _
			"</tr>"&vbcrlf
		next
	else
		echo "<tr bgcolor=""#FFFFFF"" align=""center""><td colspan=""5"" algin=""center"">还没有备份文件， <a href=""?action=bakup"" class=""txt_C1"">现在备份</a></td></tr>"
	end if
End Sub

Sub  delBackUp
	dim path
	path = getForm("path","get")
	delFile path
	alertMsgAndGo "","AspCms_Data.asp"
End Sub

Sub backUp
	dim fileName	: fileName = timeToStr(now())
	moveFile sitePath&"/"&accessFilePath,"../../../data/backup/"&fileName&"_bak.asp",""
	alertMsgAndGo "备份成功","AspCms_Data.asp"
End Sub


Sub restore
	dim path
	path = getForm("path","get")
	moveFile path,"/"&sitePath&accessFilePath,""
	alertMsgAndGo "恢复成功","AspCms_Data.asp"
End Sub


Sub compress
	dim engineObj,tempDbPath
	tempDbPath = sitePath&"/"&"data/tempdata.asp"
	conn.Class_Terminate
	set engineObj = server.CreateObject("JRO.JetEngine")
	engineObj.CompactDatabase "Provider=Microsoft.Jet.OLEDB.4.0;Data Source=" & server.MapPath(sitePath&"/"&accessFilePath),"Provider=Microsoft.Jet.OLEDB.4.0;Data Source="& server.MapPath(tempDbPath)
	moveFile tempDbPath,sitePath&"/"&accessFilePath,"del" 
	set engineObj = Nothing
	alertMsgAndGo "压缩成功","AspCms_Data.asp"
End Sub

Sub delAll
	dim ids,idsArray,arrayLen,i
	ids=replace(getForm("id","post")," ","")
	idsArray = split(ids,",") : arrayLen=ubound(idsArray)
	for i=0 to arrayLen
		delFile idsArray(i)
	next
	alertMsgAndGo "删除成功","AspCms_Data.asp"
End Sub







%>