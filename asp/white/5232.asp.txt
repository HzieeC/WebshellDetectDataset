<!--#include file="../inc/AspCms_SettingClass.asp" -->
<%
dim action : action=getForm("action","get")
dim acttype : acttype=getform("acttype", "get")
dim filename,path,filetext,nametype
if acttype="css" then 
	path=sitePath&"/Templates/"&setting.defaultTemplate&"/"&"css/"	
	nametype="CSS"
else
	nametype="模板"
	path=sitePath&"/Templates/"&setting.defaultTemplate&"/"&setting.htmlFilePath&"/"
end if

select case action
	case "add" : addFile
	case "edit" : editFile
	case "del" : delhtmlFile
	
	case "select" : selectStyle
	
	case "edithtmlfilepath" : editHtmlFilePath
end select

Sub delAllCss
	dim ids,idsArray,arrayLen,i 	
	ids=replace(getForm("cssname","both")," ","")	
	idsArray = split(ids,",") : arrayLen=ubound(idsArray)	
	for i=0 to arrayLen
		if isExistFile(sitePath&"Templates/"&setting.defaultTemplate&"/css/"&idsArray(i)) then delFile "/"&sitePath&"Templates/"&defaultTemplate&"/css/"&idsArray(i)	
	next
	alertMsgAndGo "删除成功","AspCms_CssManger.asp"
End Sub

Sub delhtmlFile
	dim ids,idsArray,arrayLen,i 	
	ids=replace(getForm("filename","both")," ","")	
	idsArray = split(ids,",") : arrayLen=ubound(idsArray)	
	for i=0 to arrayLen
		if isExistFile(path&idsArray(i)) then delFile path&idsArray(i)	
	next
	alertMsgAndGo "删除成功","AspCms_Template.asp?acttype="&acttype
End Sub

Sub selectStyle
	dim defaultTemplate : defaultTemplate=getForm("style","get")
	conn.exec"update {prefix}Language set defaultTemplate='"&defaultTemplate&"' where LanguageID="&rCookie("LanguageID"),"exe"
	alertMsgAndGo "选择模板成功","AspCms_Style.asp"
End Sub

Sub getFile()
	filename=getForm("filename","get")	
	filetext=loadFile(path&filename)
End Sub

Sub editFile()
	filename=getForm("filename","post")
	filetext=decodeHtml(getForm("filetext","post"))
	createTextFile filetext,path&filename,""	
	alertMsgAndGo "保存成功","AspCms_Template.asp?acttype="&acttype
End Sub

Sub addFile()
	filename=getForm("filename","post")
	filetext=decodeHtml(getForm("filetext","post"))
	if isExistFile(path&filename) then alertMsgAndGo "该文件已存在！","-1"
	createTextFile filetext,path&filename,""	
	alertMsgAndGo "添加成功","AspCms_Template.asp?acttype="&acttype
End Sub

Sub editHtmlFilePath
	dim templateobj,configPath,configStr,tempHtmlFilePath
	tempHtmlFilePath=getForm("htmlFilePath","post")
	if isNul(tempHtmlFilePath) then alertMsgAndGo "模板文件所在文件夹不能为空","-1"
	if tempHtmlFilePath<>setting.htmlFilePath then moveFolder "../../templates/"&setting.defaultTemplate&"/"&setting.htmlFilePath&"/","../../templates/"&setting.defaultTemplate&"/"&tempHtmlFilePath&"/"
	conn.exec"update {prefix}Language set htmlFilePath='"&tempHtmlFilePath&"' where LanguageID="&rCookie("LanguageID"),"exe"
	alertMsgAndGo "修改成功","AspCms_Style.asp"
End Sub


%>