<!--#include file="inc/AspCms_SettingClass.asp" -->
<%
dim page
page=filterPara(getForm("page","get"))
if isnul(page) or not isnum(page) then page=1

echo tags(cint(page))
Function tags(Byval page)
	dim templateFile
	dim templateobj,TemplatePath : set templateobj=new TemplateClass
	dim rsObj,rsObjSmalltype,rsObjBigtype,channelTemplateName,tempStr,tempArr,pageStr,sql,sperStr,sperStrs,content,contentLink
	
	templatePath=sitePath&"/"&"templates/"&setting.defaultTemplate&"/"&setting.htmlFilePath&"/tags.html"	
	'die templatePath
	if not CheckTemplateFile(templatePath) then echo templateFile&err_16 : exit function

	'开始解析标签
	templateObj.load(templatePath)
	templateObj.parseHtml()	
	templateObj.parseList 0,page,"taglist","","tags"		
	templateObj.parseCommon		
	tags=templateObj.content 	
	set templateobj=nothing
End Function
%>