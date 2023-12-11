<!--#include file="../../../inc/AspCms_MainClass.asp" -->
<%
CheckAdmin("AspCms_qqkf.asp")
dim action : action=getForm("action", "get")
Select Case action
	Case "save"	 : Call saveService()
End Select 

Sub saveService
	dim templateobj,configStr,configPath
	configPath="../../../config/AspCms_Config.asp"
	set templateobj=new TemplateClass
	configStr=loadFile(configPath)
	configStr=templateobj.regExpReplace(configStr,"Const serviceStatus=(\d?)","Const serviceStatus="&getForm("serviceStatus","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const serviceStyle=(\d?)","Const serviceStyle="&getForm("serviceStyle","post")&"")
	
	configStr=templateobj.regExpReplace(configStr,"Const serviceLocation=""(\S*?)""","Const serviceLocation="""&getForm("serviceLocation","post")&"""")
	
	configStr=templateobj.regExpReplace(configStr,"Const serviceQQ=""(\S*?\s?)*""","Const serviceQQ="""&getForm("serviceQQ","post")&"""")
	configStr=templateobj.regExpReplace(configStr,"Const serviceWangWang=""(\S*?\s?)*""","Const serviceWangWang="""&getForm("serviceWangWang","post")&"""")
	configStr=templateobj.regExpReplace(configStr,"Const serviceContact=""(\S*?)""","Const serviceContact="""&getForm("serviceContact","post")&"""")
	configStr=templateobj.regExpReplace(configStr,"Const servicekfStatus=(\d?)","Const servicekfStatus="&getForm("servicekfStatus","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const servicekf=""(\S*?\s?)*""","Const servicekf="""&encode(getForm("servicekf","post"))&"""")
	
	createTextFile configStr,configPath,""
	set templateobj=nothing
	alertMsgAndGo "ÐÞ¸Ä³É¹¦",getPageName()
End Sub

%>
