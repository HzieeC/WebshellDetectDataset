<!--#include file="../../../inc/AspCms_MainClass.asp" -->
<%
CheckAdmin("AspCms_Language.asp")
dim action : action=getForm("action","get")
Select case action	
	case "add" : addLanguage
	case "edit" : editLanguage
	case "del" : delLanguage
	case "on" : onOff "on", "Language", "LanguageID", "LanguageStatus", "", getPageName()
	case "off" : onOff "off", "Language", "LanguageID", "LanguageStatus", "", getPageName()
	case "default" : setDefault
End Select

dim LanguageID, LanguageName, LanguagePath, defaultTemplate, Alias, htmlFilePath, LanguageOrder, IsDefault, LanguageStatus
dim sql, msg
dim SiteTitle, AdditionTitle, SiteLogoUrl, SiteUrl, CompanyName, CompanyAddress, CompanyPostCode, CompanyContact, _
CompanyPhone, CompanyMobile, CompanyFax, CompanyEmail, CompanyICP, StatisticalCode, CopyRight, SiteKeywords, SiteDesc

Sub getLanguage
	dim id : id=getForm("id","get")
	if not isnul(ID) then		
		sql ="select * from {prefix}Language where LanguageID="&id
		dim rs : set rs = conn.exec(sql,"r1")
		if rs.eof then 
			alertMsgAndGo "没有这条记录","-1"
		else
			LanguageID=rs("LanguageID")
			LanguageName=rs("LanguageName")
			LanguagePath=rs("LanguagePath")
			defaultTemplate=rs("defaultTemplate")
			Alias=rs("Alias")
			htmlFilePath=rs("htmlFilePath")
			LanguageOrder=rs("LanguageOrder")
			IsDefault=rs("IsDefault")
			LanguageStatus=rs("LanguageStatus")
		end if
		rs.close : set rs=nothing
	else		
		alertMsgAndGo "没有这条记录","-1"
	end if 
End Sub

Sub languageList
	sql="select LanguageID, LanguageName, Alias, LanguagePath, defaultTemplate, LanguageOrder, IsDefault, LanguageStatus from {prefix}Language order by LanguageOrder ,LanguageID"
	dim rs
	set rs=conn.exec(sql,"r1")
	if rs.eof then 
		echo "<TR class=list>"&vbcrlf& _
			"<TD colspan=""8"" align=""center"">没有记录</TD>"&vbcrlf& _
			"</TR>"&vbcrlf
	else
		do while not rs.eof 
			echo"<TR class=list>"&vbcrlf& _
			"<TD align=middle height=26><INPUT type=checkbox value="""&rs(0)&""" name=""id""></TD>"&vbcrlf& _
			"<TD>"&rs(0)&"</TD>"&vbcrlf& _
			"<TD>"&rs(1)&"</TD>"&vbcrlf& _
			"<TD>"&rs(2)&"</TD>"&vbcrlf& _
			"<TD>"&rs(3)&"</TD>"&vbcrlf& _
			"<TD>"&rs(4)&"</TD>"&vbcrlf& _
			"<TD>"&rs(5)&"</TD>"&vbcrlf& _
			"<TD>"&getStr(rs(6),"<IMG src=""../../images/toolbar_ok.gif"">","<a href=""?action=default&id="&rs(0)&""" title=""禁用"" ><IMG src=""../../images/toolbar_no.gif""></a>")&"</TD>"&vbcrlf& _
			"<TD>"&getStr(rs(7),"<a href=""?action=off&id="&rs(0)&""" title=""启用""><IMG src=""../../images/toolbar_ok.gif""></a>","<a href=""?action=on&id="&rs(0)&""" title=""禁用"" ><IMG src=""../../images/toolbar_no.gif""></a>")&"</TD>"&vbcrlf& _
			"<TD><a href=""AspCms_LanguageEdit.asp?id="&rs(0)&""">修改</a> <a href=""?action=del&id="&rs(0)&""" onClick=""return confirm('确定要删除吗')"">删除</a></TD>"&vbcrlf& _
			"</TR>"&vbcrlf
			rs.moveNext
		loop
	end if	
	rs.close : set rs=nothing
End Sub

Sub addLanguage
	LanguageID=getForm("LanguageID", "post")
	LanguageName=getForm("LanguageName", "post")
	LanguagePath=getForm("LanguagePath", "post")
	defaultTemplate=getForm("defaultTemplate", "post")
	Alias=getForm("Alias", "post")
	htmlFilePath=getForm("htmlFilePath", "post")
	LanguageOrder=getForm("LanguageOrder", "post")
	IsDefault=getCheck(getForm("IsDefault", "post"))
	LanguageStatus=getCheck(getForm("LanguageStatus", "post"))
	
	if isnul(LanguageName) then alertMsgAndGo "语言名称不能为空","-1"
	if isnul(defaultTemplate) then alertMsgAndGo "模板目录不能为空","-1"
	if isnum (Alias) then alertMsgAndGo "语言别名不能为空","-1"
	if not isnum(LanguageOrder) then LanguageOrder=9
		
	if IsDefault="1" then conn.exec "update {prefix}Language set IsDefault=0","exe"
	
	sql="insert into {prefix}Language(LanguageName, LanguagePath, defaultTemplate, Alias, htmlFilePath, LanguageOrder, IsDefault, LanguageStatus) values('"&LanguageName&"', '"&LanguagePath&"', '"&defaultTemplate&"', '"&Alias&"', '"&htmlFilePath&"', "&LanguageOrder&", "&IsDefault&", "&LanguageStatus&")"
	'echo sql
	conn.exec sql, "exe"
	alertMsgAndGo "添加成功", "AspCms_Language.asp"
End Sub

Sub editLanguage
	LanguageID=getForm("LanguageID", "post")
	LanguageName=getForm("LanguageName", "post")
	LanguagePath=getForm("LanguagePath", "post")
	defaultTemplate=getForm("defaultTemplate", "post")
	Alias=getForm("Alias", "post")
	htmlFilePath=getForm("htmlFilePath", "post")
	LanguageOrder=getForm("LanguageOrder", "post")
	IsDefault=getCheck(getForm("IsDefault", "post"))
	LanguageStatus=getCheck(getForm("LanguageStatus", "post"))
	
	if isnul(LanguageName) then alertMsgAndGo "语言名称不能为空","-1"
	if isnul(defaultTemplate) then alertMsgAndGo "模板目录不能为空","-1"
	if isnum (Alias) then alertMsgAndGo "语言别名不能为空","-1"
	if not isnum(LanguageOrder) then LanguageOrder=9
	
	'sql="insert into {prefix}Language(LanguageName, LanguagePath, defaultTemplate, Alias, htmlFilePath, LanguageOrder, IsDefault, LanguageStatus) values('"&LanguageName&"', '"&LanguagePath&"', '"&defaultTemplate&"', '"&Alias&"', '"&htmlFilePath&"', "&LanguageOrder&", "&IsDefault&", "&LanguageStatus&")"
	
	if IsDefault="1" then conn.exec "update {prefix}Language set IsDefault=0","exe"	
	sql="update {prefix}Language set LanguageName='"&LanguageName&"', LanguagePath='"&LanguagePath&"', defaultTemplate='"&defaultTemplate&"', Alias='"&Alias&"', htmlFilePath='"&htmlFilePath&"', LanguageOrder="&LanguageOrder&", IsDefault="&IsDefault&", LanguageStatus="&LanguageStatus&" where LanguageID="&LanguageID
	'echo sql
	conn.exec sql, "exe"
	alertMsgAndGo "修改成功", "AspCms_Language.asp"
End Sub

Sub setDefault
	dim id	:	id=getForm("id","get")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"	
	conn.exec "update {prefix}Language set IsDefault=0","exe"
	conn.exec "update {prefix}Language set IsDefault=1 where LanguageID="&id,"exe"
	response.Redirect getPageName()		
End Sub

Sub delLanguage
	dim id	:	id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"
	conn.exec "delete * from {prefix}Language where IsDefault=0 and LanguageID in("&id&")","exe"
	alertMsgAndGo "删除成功",getPageName()		
End Sub
%>