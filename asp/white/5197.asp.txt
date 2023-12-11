<!--#include file="../inc/AspCms_SettingClass.asp" -->
<%
dim SortID,Page,SortAndID,rs
SortAndID=split(replaceStr(request.QueryString,FileExt,""),"_")
if isnul(request.QueryString) then 
	set rs=conn.exec("select SortID from {prefix}Sort where SortUrl='/gbook/'","r1")
	if not rs.eof then SortID=rs(0) : rs.close : set rs=nothing
	response.Redirect("?"&SortID&"_1.html")
else
	if isNul(replaceStr(request.QueryString,FileExt,"")) then  echoMsgAndGo "页面不存在",3 
	SortID = SortAndID(0)
end if
if not isNul(SortID) and isNum(SortID) then SortID=clng(SortID) else echoMsgAndGo "页面不存在",3 end if
if ubound(SortAndID)=0 then page=1 else page=SortAndID(1) end if 
if not isNul(page) and isNum(page) then page=clng(page) else echoMsgAndGo "页面不存在",3 end if

echoGbook SortID, Page
Sub echoGbook(SortID, Page)
	dim templateobj,channelTemplatePath : set templateobj = new TemplateClass	
	dim typeIds,rsObj,rsObjtid,Tid,rsObjSmalltype,rsObjBigtype
	Dim templatePath,tempStr
	templatePath=sitePath&"/"&"templates/"&setting.defaultTemplate&"/"&setting.htmlFilePath&"/gbook.html"
	if not CheckTemplateFile(templatePath) then echo "gbook.html"&err_16
	
	set rsobj=conn.exec("select * from {prefix}Sort where SortID="&sortID, "exe")							
	if rsObj.eof then echoMsgAndGo "页面不存在",3 : exit sub		
	with templateObj 
		.content=loadFile(templatePath)	
		.parseHtml()
		templateObj.content=replace(templateObj.content,"{aspcms:sortname}",rsObj("SortName"))
		templateObj.content=replace(templateObj.content,"{aspcms:parentsortid}",rsObj("parentid"))		
		templateObj.content=replace(templateObj.content,"{aspcms:sortid}",sortID)	
		templateObj.content=replace(templateObj.content,"{aspcms:topsortid}",rsObj("topsortid"))
		if isnul(rsObj("PageKeywords")) then 
			templateObj.content=replace(templateObj.content,"{aspcms:sortkeyword}",setting.siteKeyWords)	
		else
			templateObj.content=replace(templateObj.content,"{aspcms:sortkeyword}",rsObj("PageKeywords"))	
		end if
		if isnul(rsObj("PageDesc")) then
			templateObj.content=replace(templateObj.content,"{aspcms:sortdesc}",setting.sitedesc)
		else
			templateObj.content=replace(templateObj.content,"{aspcms:sortdesc}",rsObj("PageDesc"))
		end if
		if isnul(rsObj("PageTitle")) then 
			templateObj.content=replace(templateObj.content,"{aspcms:sorttitle}",rsObj("SortName"))	
		else
			templateObj.content=replace(templateObj.content,"{aspcms:sorttitle}",rsObj("PageTitle"))
		end if
		templateObj.parsePosition(sortID)
		.parseList SortID,page,"gbooklist","","gbook"
		.parseCommon() 
		echo .content 
	end with
	set templateobj =nothing : terminateAllObjects
End Sub
%>
