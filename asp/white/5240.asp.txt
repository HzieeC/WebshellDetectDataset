<!--#include file="../inc/AspCms_SettingClass.asp" -->
<%

dim action : action=getForm("action","get")
Select case action	
	case "savec" : saveComplanySetting
	case "saves" : saveSiteSetting
End Select


dim LanguageID, SiteTitle, AdditionTitle, SiteLogoUrl, SiteUrl, CompanyName, CompanyAddress, CompanyPostCode, CompanyContact, CompanyPhone, CompanyMobile, CompanyFax, CompanyEmail, CompanyICP, StatisticalCode, CopyRight, SiteKeywords, SiteDesc
dim rs, sql


Sub saveSiteSetting	
	if runMode=1 and getForm("runMode","post")<>1 then
		if isExistFile(sitePath&"/"&"index.html") then delFile sitePath&"/"&"index.html"	'删除首页	
		if not isnul(htmlDir) and isExistFolder(sitePath&"/"&htmlDir) then delFolder(sitePath&"/"&htmlDir)
	end if
	dim templateobj,configStr,configPath
	configPath="../../config/AspCms_Config.asp"
	set templateobj=new TemplateClass
	configStr=loadFile(configPath)
	configStr=templateobj.regExpReplace(configStr,"Const runMode=(\d?)","Const runMode="&getForm("runMode","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const siteMode=(\d?)","Const siteMode="&getForm("siteMode","post")&"")
	
	configStr=templateobj.regExpReplace(configStr,"Const siteHelp=""(\S*?)""","Const siteHelp="""&getForm("siteHelp","post")&"""")
	
	configStr=templateobj.regExpReplace(configStr,"Const switchFaq=(\d?)","Const switchFaq="&getForm("switchFaq","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const switchFaqStatus=(\d?)","Const switchFaqStatus="&getForm("switchFaqStatus","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const switchComments=(\d?)","Const switchComments="&getForm("switchComments","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const switchCommentsStatus=(\d?)","Const switchCommentsStatus="&getForm("switchCommentsStatus","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const dirtyStr=""(\S*?)""","Const dirtyStr="""&encode(getForm("dirtyStr","post"))&"""")
	
	configStr=templateobj.regExpReplace(configStr,"Const waterMark=(\d?)","Const waterMark="&getForm("waterMark","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const waterMarkFont=""(\S*?)""","Const waterMarkFont="""&getForm("waterMarkFont","post")&"""")
	configStr=templateobj.regExpReplace(configStr,"Const waterMarkLocation=""(\S*?)""","Const waterMarkLocation="""&getForm("waterMarkLocation","post")&"""")
	
	configStr=templateobj.regExpReplace(configStr,"Const smtp_usermail=""(\S*?)""","Const smtp_usermail="""&getForm("smtp_usermail","post")&"""")
	configStr=templateobj.regExpReplace(configStr,"Const smtp_user=""(\S*?)""","Const smtp_user="""&getForm("smtp_user","post")&"""")
	configStr=templateobj.regExpReplace(configStr,"Const smtp_password=""(\S*?)""","Const smtp_password="""&getForm("smtp_password","post")&"""")
	configStr=templateobj.regExpReplace(configStr,"Const smtp_server=""(\S*?)""","Const smtp_server="""&getForm("smtp_server","post")&"""")
	
	
	configStr=templateobj.regExpReplace(configStr,"Const messageAlertsEmail=""(\S*?)""","Const messageAlertsEmail="""&getForm("messageAlertsEmail","post")&"""")
	configStr=templateobj.regExpReplace(configStr,"Const messageReminded=(\d?)","Const messageReminded="&getForm("messageReminded","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const orderReminded=(\d?)","Const orderReminded="&getForm("orderReminded","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const commentReminded=(\d?)","Const commentReminded="&getForm("commentReminded","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const applyReminded=(\d?)","Const applyReminded="&getForm("applyReminded","post")&"")
	
	
	createTextFile configStr,configPath,""
	set templateobj=nothing
	alertMsgAndGo "修改成功",getPageName()
End Sub


Sub getComplanySetting
	dim ID : ID=rCookie("languageID")
	if not isnul(ID) and isnum(ID) then		
		sql ="select * from {prefix}Language where LanguageID="&id
		dim rs : set rs=conn.exec(sql,"r1")
		if rs.eof then 
			alertMsgAndGo "没有这条记录","-1"
		else
			LanguageID=rs("LanguageID")
			SiteTitle=rs("siteTitle")
			AdditionTitle=rs("additionTitle")
			SiteLogoUrl=rs("siteLogoUrl")
			SiteUrl=rs("siteUrl")
			CompanyName=rs("companyName")
			CompanyAddress=rs("companyAddress")
			CompanyPostCode=rs("companyPostCode")
			CompanyContact=rs("companyContact")
			CompanyPhone=rs("companyPhone")
			CompanyMobile=rs("companyMobile")
			CompanyFax=rs("companyFax")
			CompanyEmail=rs("companyEmail")
			CompanyICP=rs("companyICP")
			StatisticalCode=rs("statisticalCode")
			CopyRight=rs("copyRight")
			SiteKeywords=rs("siteKeywords")
			SiteDesc=rs("siteDesc")
		end if
		rs.close : set rs=nothing
	else		
		alertMsgAndGo "没有这条记录","-1"
	end if 
End Sub

Sub saveComplanySetting
	LanguageID=getForm("LanguageID","post")
	SiteTitle=getForm("siteTitle","post")
	AdditionTitle=getForm("additionTitle","post")
	SiteLogoUrl=getForm("siteLogoUrl","post")
	SiteUrl=getForm("siteUrl","post")
	CompanyName=getForm("companyName","post")
	CompanyAddress=getForm("companyAddress","post")
	CompanyPostCode=getForm("companyPostCode","post")
	CompanyContact=getForm("companyContact","post")
	CompanyPhone=getForm("companyPhone","post")
	CompanyMobile=getForm("companyMobile","post")
	CompanyFax=getForm("companyFax","post")
	CompanyEmail=getForm("companyEmail","post")
	CompanyICP=getForm("companyICP","post")
	StatisticalCode=getForm("statisticalCode","post")
	CopyRight=getForm("copyRight","post")
	SiteKeywords=getForm("siteKeywords","post")
	SiteDesc=getForm("siteDesc","post")

	conn.exec"update {prefix}Language set SiteTitle='"&SiteTitle&"', AdditionTitle='"&AdditionTitle&"', SiteLogoUrl='"&SiteLogoUrl&"', SiteUrl='"&SiteUrl&"', CompanyName='"&CompanyName&"', CompanyAddress='"&CompanyAddress&"', CompanyPostCode='"&CompanyPostCode&"', CompanyContact='"&CompanyContact&"', CompanyPhone='"&CompanyPhone&"', CompanyMobile='"&CompanyMobile&"', CompanyFax='"&CompanyFax&"',CompanyEmail ='"&CompanyEmail&"', CompanyICP='"&CompanyICP&"', StatisticalCode='"&StatisticalCode&"', CopyRight='"&CopyRight&"', SiteKeywords='"&SiteKeywords&"', SiteDesc='"&SiteDesc&"' where LanguageID="&LanguageID, "exe"
	alertMsgAndGo "保存成功！",getPageName()
End Sub
%>