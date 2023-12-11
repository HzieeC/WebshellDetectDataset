<!--#include file="../../inc/AspCms_SettingClass.asp" -->
<!--#include file="../../editor/fckeditor.asp" -->
<%
dim action : action=getForm("action","get")
Select case action	
	case "edit" :editSort	
End Select

dim SortID, LanguageID, ParentID, SortOrder, SortType, SortName, SortURL, SortLevel, AddTime, PageTitle, PageKeywords, PageDesc, SortPath, SortTemplate, ContentTemplate, SortFolder, ContentFolder, SortFileName, ContentFileName, SortStatus, TopSortID, GroupID, Exclusive, Content
dim sql, msg

Sub getSort
	dim id : id=getForm("id","get")
	if not isnul(ID) then		
		sql ="select * from {prefix}Sort where SortID="&id
		dim rs : set rs = conn.exec(sql,"r1")
		if rs.eof then 
			alertMsgAndGo "没有这条记录","-1"
		else
			SortID=rs("SortID")
			LanguageID=rs("LanguageID")
			ParentID=rs("ParentID")
			SortOrder=rs("SortOrder")
			SortType=rs("SortType")
			SortName=rs("SortName")
			SortURL=rs("SortURL")
			SortLevel=rs("SortLevel")
			AddTime=rs("AddTime")
			PageTitle=rs("PageTitle")
			PageKeywords=rs("PageKeywords")
			PageDesc=rs("PageDesc")
			SortPath=rs("SortPath")
			SortTemplate=rs("SortTemplate")
			ContentTemplate=rs("ContentTemplate")
			SortFolder=rs("SortFolder")
			ContentFolder=rs("ContentFolder")
			SortFileName=rs("SortFileName")
			ContentFileName=rs("ContentFileName")
			SortStatus=rs("SortStatus")
			TopSortID=rs("TopSortID")
			GroupID=rs("GroupID")
			Exclusive=rs("Exclusive")
			Content=rs("SortContent")
		end if
		rs.close : set rs=nothing
	else		
		alertMsgAndGo "没有这条记录","-1"
	end if 
End Sub

Sub aboutList
	Dim rsObj	:	Set rsObj=conn.Exec("select * from {prefix}Sort where SortType=1 Order by SortOrder Asc,SortID","r1")
	If rsObj.Eof Then 
		echo"<tr bgcolor=""#FFFFFF"" align=""center"">"&vbcrlf& _
			"<td colspan=""5"">没有数据</td>"&vbcrlf& _
		  "</tr>"&vbcrlf
	Else
		Do while not rsObj.Eof 
		 echo"<tr bgcolor=""#FFFFFF"" align=""center"">"&vbcrlf& _
		 	"<td height=""28""><input type=""checkbox"" name=""id"" value="""&rsObj(0)&""" class=""checkbox"" /><input type=""hidden"" name=""SortIDs"" value="""&rsObj(0)&""" /></td>"&vbcrlf& _
			"<td>"&rsObj(0)&"</td>"&vbcrlf& _
			"<td>"&rsObj("sortName")&"<a href=""../../../about/?"&rsObj(0)&".html"" target=""_blank"" title=""查看""><img src=""../../images/view.gif"" border=0 /></a></td>"&vbcrlf& _
			"<td><a href=""AspCms_AboutEdit.asp?id="&rsObj(0)&""" >修改</a></td>"&vbcrlf& _
		  "</tr>"&vbcrlf
		  rsObj.MoveNext
		Loop
	End If
	rsObj.close	:	Set rsObj = nothing
End Sub

Sub editSort	
	SortID=getForm("SortID", "post")
	SortName=getForm("SortName", "post")
	PageTitle=getForm("PageTitle", "post")
	PageKeywords=getForm("PageKeywords", "post")
	PageDesc=getForm("PageDesc", "post")	
	SortStatus=getForm("SortStatus", "post")
	Content=getForm("Content", "post")	
	LanguageID=cint(rCookie("languageID"))		
	GroupID=getForm("GroupID", "post")	
	Exclusive=getForm("Exclusive", "post")	
	if isnul(SortName) then alertMsgAndGo "分类名称不能为空","-1"	
	conn.exec "update {prefix}Sort set GroupID="&GroupID&", SortName='"&SortName&"', PageTitle='"&PageTitle&"', PageKeywords='"&PageKeywords&"', PageDesc='"&PageDesc&"', SortContent='"&Content&"', Exclusive='"&Exclusive&"' where SortID="&SortID, "exe"	
	alertMsgAndGo "修改成功","AspCms_About.asp"
End Sub
%>