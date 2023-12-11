<!--#include file="inc/AspCms_SettingClass.asp" -->
<%
dim tag,page
tag=filterPara(getForm("tag","get"))
page=filterPara(getForm("page","get"))
if isnul(page) or not isnum(page) then page=1
echo tagList(tag,cint(page))

Function tagList(Byval tag, Byval page)
	dim templateFile
	dim templateobj,TemplatePath : set templateobj=new TemplateClass
	dim rsObj,rsObjSmalltype,rsObjBigtype,channelTemplateName,tempStr,tempArr,pageStr,sql,sperStr,sperStrs,content,contentLink
	
	templatePath=sitePath&"/"&"templates/"&setting.defaultTemplate&"/"&setting.htmlFilePath&"/taglist.html"	
	'die templatePath
	if not CheckTemplateFile(templatePath) then echo templateFile&err_16 : exit function

	'开始解析标签
	templateObj.load(templatePath)
	templateObj.parseHtml()
	conn.exec"update {prefix}Tag set TagVisits=TagVisits+1  where TagName='"&tag&"'","exe"
	templateObj.content =replace(templateObj.content , "[tag]",tag)			
	
	templateObj.parseList "",page,"list","","taglist"	
		
	'templateObj.parseList sortID,page,"list","","ff"
	templateObj.parseCommon		
	tagList=templateObj.content 	
	set templateobj=nothing
End Function
%>