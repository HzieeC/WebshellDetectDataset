<!--#include file="../../inc/AspCms_SettingClass.asp" -->
<%
dim action : action=getForm("action","get")
Select case action	
	case "del" : DelTag
	case "add" : AddTag
	case "save" :SaveTag	
	case "visits" :updateVisits	
End Select

dim TagID, TagName, TagCount, SortType, SortID, TagVisits, LanguageID, AddTime
dim sql, msg

Sub AddTag 	
	TagName=getForm("TagName","post")
	TagCount=0
	SortType=0
	SortID=0
	TagVisits=getForm("TagVisits","post")	
	LanguageID=cint(rCookie("languageID"))
	AddTime=now()
	
	if isnul(TagName) then alertMsgAndGo "TAG名称不能为空，请修改","-1"
	if not isnum(TagVisits) then alertMsgAndGo "点击量只能为数字，请修改","-1"
	sql="insert into {prefix}Tag(TagName, TagCount, SortType, SortID, TagVisits, LanguageID, AddTime) values('"&TagName&"',"&TagCount&","&SortType&","&SortID&","&TagVisits&","&LanguageID&",'"&AddTime&"')"
	conn.Exec sql,"exe"		
	alertMsgAndGo "添加成功","AspCms_Tag.asp"
End Sub	

dim keyword, page, psize, order, ordsc
keyword=getForm("keyword","post")
if isnul(keyword) then keyword=getForm("keyword","get")
page=getForm("page","get")
psize=getForm("psize","get")
order=getForm("order","get")
ordsc=getForm("ordsc","get")

Sub TagList
	dim i,orderStr,whereStr,sqlStr,rsObj,allPage,allRecordset,numPerPage
	numPerPage=psize
	if not isnum(numPerPage) then numPerPage=10	
	if isnul(order) then order="TagID"
	if isnul(ordsc) then ordsc="desc"

	if isNul(page) then page=1 else page=clng(page)
	if page=0 then page=1
	
	orderStr=" order by "&order&" "&ordsc &""
	whereStr=" where LanguageID="&rCookie("languageID")
	
	if not isNul(sortid) and sortid<>"0" then  whereStr=whereStr&" and a.SortID="&SortID
	if not isNul(keyword) then whereStr = whereStr&" and TagName like '%"&keyword&"%'"



	set rsObj=conn.Exec("select TagID, TagName, TagCount, SortType, SortID, TagVisits, LanguageID, AddTime from {prefix}Tag "&whereStr&orderStr,"r1")
	rsObj.pagesize = numPerPage
	allRecordset = rsObj.recordcount : allPage= rsObj.pagecount
	if page>allPage then page=allPage
	if allRecordset=0 then
		if not isNul(keyword) then
		    echo "<tr align=""center""><td colspan='8' height=""28"">关键字 <font color=red>"""&keyword&"""</font> 没有记录</td></tr>" 
		else
		    echo "<tr align=""center""><td colspan='8' height=""28"">还没有记录!</td></tr>"
		end if
	else  
		rsObj.absolutepage = page
		for i = 1 to numPerPage
			 echo "<tr bgcolor=""#ffffff"" align=""center"" onMouseOver=""this.bgColor='#CDE6FF'"" onMouseOut=""this.bgColor='#FFFFFF'"">"&vbcrlf& _
						 	"<td height=""28""><input type=""checkbox"" name=""id"" value="""&rsObj(0)&""" class=""checkbox"" /><input type=""hidden"" name=""TagIDs"" value="""&rsObj(0)&""" /></td>"&vbcrlf& _
			"<td>"&rsObj(0)&"</td>"&vbcrlf& _
			"<td>"&rsObj(1)&"</td>"&vbcrlf& _
			"<td><input class=""input"" style=""width:30px"" name=""TagVisits"" value="""&rsObj(5)&"""/></td>"&vbcrlf& _
			"<td>"&rsObj(7)&"</td>"&vbcrlf& _
			"<td><a href=""?action=del&id="&rsObj(0)&"&TagField="&rsObj(2)&"""  onClick=""return confirm('确定要删除吗')"">删除</a></td>"&vbcrlf& _
			  "</tr>"&vbcrlf
			rsObj.movenext
			if rsObj.eof then exit for
		next
		echo"<tr bgcolor=""#FFFFFF"" class=""pagenavi"">"&vbcrlf& _
			"<td colspan=""8"" height=""28"" style=""padding-left:20px;"">"&vbcrlf& _			
			"页数："&page&"/"&allPage&"  每页"&numPerPage &" 总记录数"&allRecordset&"条 <a href=""?keyword="&keyword&"&page=1&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">首页</a> <a href=""?keyword="&keyword&"&page="&(page-1)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">上一页</a> "&vbcrlf
		dim pageNumber
		pageNumber=makePageNumber_(page, 10, allPage, "newslist",sortID, order,keyword)
		echo pageNumber
		echo"<a href=""?keyword="&keyword&"&page="&(page+1)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">下一页</a> <a href=""?keyword="&keyword&"&page="&allPage&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">尾页</a>"&vbcrlf
			
		echo"每页显示"&getPageSize("10",psize)&getPageSize("20",psize)&getPageSize("30",psize)&getPageSize("50",psize)
				
		echo"条"&vbcrlf& _	
		"</td>"&vbcrlf& _
		"</tr>"&vbcrlf
	end if
	rsObj.close : set rsObj = nothing	
End Sub

Function getPageSize(ps,nps)
	if isnul(nps) then nps="10"
	if ps=nps then 
		getPageSize="<span>"&ps&"</span>"
	else
		getPageSize="<a href=""?keyword="&keyword&"&page="&page&"&psize="&ps&"&order="&order&"&ordsc="&ordsc&""">"&ps&"</a>"
	end if
End Function









Sub DelTag 
	Dim id	:	id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要删除的内容","-1"
	Conn.Exec "delete * from {prefix}Tag where TagID in("&id&")","exe"	
	alertMsgAndGo "删除成功","AspCms_Tag.asp"	
End Sub 


Sub updateVisits
	Dim ids			:	ids=split(getForm("TagIDs","post"),",")
	Dim TagVisitss		:	TagVisitss=split(getForm("TagVisits","post"),",")
	If Ubound(ids)=-1 Then 	'防止有值为空时下标越界
		ReDim ids(0)
		ids(0)=""
	End If	
	
	If Ubound(TagVisitss)=-1 Then
		ReDim TagVisitss(0)
		TagVisitss(0)=0
	End If
	Dim i
	
	For i=0 To Ubound(ids)		
		if isnum(trim(TagVisitss(i))) then
			Conn.Exec "update {prefix}Tag Set TagVisits="&trim(TagVisitss(i))&" Where TagID="&trim(ids(i)),"exe"	
		else
			Conn.Exec "update {prefix}Tag Set TagVisits=0 Where TagID="&trim(ids(i)),"exe"	
		end if
	Next
	
	
	alertMsgAndGo "更新排序成功",getPageName()
End Sub
%>