<!--#include file="../../inc/AspCms_SettingClass.asp" -->
<!--#include file="../../editor/fckeditor.asp" -->
<%
dim action : action=getForm("action","get")
dim ContentID, LanguageID, SortID, GroupID, Exclusive, Title, Title2, TitleColor, IsOutLink, OutLink, Author, ContentSource, ContentTag, Content, ContentStatus, IsTop, IsRecommend, IsImageNews, IsHeadline, IsFeatured, ContentOrder, IsGenerated, Visits, AddTime, ImagePath, IndexImage, DownURL, PageTitle, PageKeywords, PageDesc, PageFileName, spec, EditTime,IsNoComment,Star
dim sortType, keyword, page, psize, order, ordsc, sortTypeName
sortType=getForm("sortType","get")
if isnul(sortType) then sortType=0	
sortid=getForm("sortid","post")	
if isnul(sortid) then sortid=getForm("sortid","get")
keyword=getForm("keyword","post")
if isnul(keyword) then keyword=getForm("keyword","get")
page=getForm("page","get")
psize=getForm("psize","get")
order=getForm("order","get")
ordsc=getForm("ordsc","get")
	
select case sortType
	case "2"
		sortTypeName ="文章"
	case "3"
		sortTypeName ="产品"
	case "4"
		sortTypeName ="下载"
	case "5"
		sortTypeName ="招聘"
	case "6"
		sortTypeName ="相册"	
end select 
'单篇1,文章2,产品3,下载4,招聘5,相册6,链接7
	
Select  case action
	case "add" : addContent	
	case "edit" : editContent	
	case "move" : moveContent	
	case "del" : delContent	
	case "recovery" : Recovery	
	case "tdel" : trueDelContent
	case "on" : onOff "on", "Content", "ContentID", "ContentStatus", "", getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc
	case "off" : onOff "off", "Content", "ContentID", "ContentStatus", "", getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc
	
	case "order" : UpdateOrder
	
End Select


Sub getSpec(Contentid)
	if sortType="3" then
		Dim rsObj,specFields,rsObj1,i,SpecNames
		specFields=""		
		Set rsObj=Conn.Exec("select SpecName,SpecField from {prefix}SpecSet order by SpecOrder,SpecID","r1")
		Do While not rsObj.Eof 
			if Contentid=0 then
				echo "<tr>"&vbcrlf& _	
				"<td align=middle height=30>"&rsObj(0)&" </td>"&vbcrlf& _	
				"<td> <input type=""text"" class=""input"" maxlength=""100"" name=""spec"" style=""width:200px"" />"&vbcrlf& _	
				"</td>"&vbcrlf& _	
				"</tr>"&vbcrlf
			end if
			specFields=specFields&rsObj(1)&","
			SpecNames=SpecNames&rsObj(0)&","
			rsObj.MoveNext
		Loop
		
		if not rsObj.eof then rsObj.MoveFirst
		if Contentid<>0 and not isnul(specFields) then 
			Set rsObj1=Conn.Exec("select "&specFields&"Contentid from {prefix}Content where Contentid="&Contentid,"r1")	
			SpecNames=split(SpecNames,",")
			Do While not rsObj1.Eof 
				for i=0 to ubound(SpecNames)-1
					echo "<tr>"&vbcrlf& _	
					"<td align=middle width=100 height=30>"&trim(SpecNames(i))&"：</td>"&vbcrlf& _	
					"<td> <input type=""text"" class=""input"" maxlength=""100"" value="""&trim(rsObj1(i))&"""  name=""spec"" style=""width:200px"" />"&vbcrlf& _	
					"</td>"&vbcrlf& _	
					"</tr>"&vbcrlf
				next
				rsObj1.MoveNext
			Loop
			rsObj1.Close	: Set rsObj1=Nothing
		end if
		rsObj.Close	: Set rsObj=Nothing
	end if
End Sub



Sub getContent
	dim id : id=getForm("id","get")
	if not isnul(ID) then		
		Dim rs : Set rs = Conn.Exec("select * from {prefix}Content where ContentID="&ID,"r1")
		if not rs.eof then	
			ContentID=rs("ContentID")
			LanguageID=rs("LanguageID")
			SortID=rs("SortID")
			GroupID=rs("GroupID")
			Exclusive=rs("Exclusive")
			Title=rs("Title")
			Title2=rs("Title2")
			TitleColor=rs("TitleColor")
			IsOutLink=rs("IsOutLink")
			OutLink=rs("OutLink")
			Author=rs("Author")
			ContentSource=rs("ContentSource")
			'ContentTag=replace(replace(rs("ContentTag")," ",","),"，",",")
			ContentTag=getTags(rs("ContentTag"))
			Content=rs("Content")		
			ContentStatus=rs("ContentStatus")		
			IsTop=rs("IsTop")		
			IsRecommend=rs("IsRecommend")		
			IsImageNews=rs("IsImageNews")		
			IsHeadline=rs("IsHeadline")		
			IsFeatured=rs("IsFeatured")		
			ContentOrder=rs("ContentOrder")		
			IsGenerated=rs("IsGenerated")		
			Visits=rs("Visits")		
			AddTime=rs("AddTime")		
			ImagePath=rs("ImagePath")		
			IndexImage=rs("IndexImage")		
			DownURL=rs("DownURL")		
			PageTitle=rs("PageTitle")
			PageKeywords=rs("PageKeywords")		
			PageDesc=rs("PageDesc")		
			PageFileName=rs("PageFileName")		
			IsNoComment=rs("IsNoComment")			
			Star=rs("Star")							
		end if
	else		
		alertMsgAndGo "没有这条记录","-1"
	end if
End Sub

Sub addContent
	'ContentID=getForm("ContentID", "post")
	LanguageID=cint(rCookie("languageID"))
	SortID=getForm("SortID", "post")
	GroupID=getForm("GroupID", "post")
	Exclusive=getForm("Exclusive", "post")
	
	TitleColor=getForm("TitleColor", "post")
	Title=getForm("Title", "post")
	Title2=getForm("Title2", "post")
	Author=getForm("Author", "post")
	ContentSource=getForm("ContentSource", "post")
	Content=getForm("Content", "post")
	PageTitle=getForm("PageTitle", "post")
	PageKeywords=getForm("PageKeywords", "post")
	PageDesc=getForm("PageDesc", "post")	
	
	OutLink=getForm("OutLink", "post")
	IsOutLink=getCheck(getForm("IsOutLink", "post"))
	ContentStatus=getCheck(getForm("ContentStatus", "post"))
	IsTop=getCheck(getForm("IsTop", "post"))
	IsRecommend=getCheck(getForm("IsRecommend", "post"))
	IsImageNews=getCheck(getForm("IsImageNews", "post"))
	IsHeadline=getCheck(getForm("IsHeadline", "post"))
	IsFeatured=getCheck(getForm("IsFeatured", "post"))
	IsGenerated=getCheck(getForm("IsGenerated", "post"))
	IsNoComment=getCheck(getForm("IsNoComment", "post"))
	
	Star=getForm("Star", "post")
	ContentOrder=getForm("ContentOrder", "post")
	Visits=getForm("Visits", "post")
	AddTime=getForm("AddTime", "post")
	EditTime=now()
	ImagePath=getForm("ImagePath", "post")
	IndexImage=getForm("IndexImage", "post")
	DownURL=getForm("DownURL", "post")
	PageFileName=getForm("PageFileName", "post")
	spec=split(getForm("spec","post"),",")	
	
	dim specStr	: specStr=""
	dim valueStr	: valueStr=""
	
	if sortType="3" then
		dim i	:i=0
		dim rsObj	:	Set rsObj=Conn.Exec("select SpecField from {prefix}SpecSet order by SpecOrder,SpecID","r1")
		Do While not rsObj.Eof 		
			specStr = specStr&","&rsObj(0)
			valueStr = valueStr&",'"&trim(spec(i))&"'"
			i=i+1
			rsObj.MoveNext
		Loop		
		rsObj.Close	:	set rsObj=Nothing
	end if

	if not isnum(LanguageID) then alertMsgAndGo"当前操作语言错误，请重新登陆","-1"
	if isnul(Title) then alertMsgAndGo"请填写标题","-1"
	if not isnum(SortID) or SortID="0" then alertMsgAndGo"请选择分类","-1"
	if not isnum(ContentOrder) then ContentOrder=99
	if not isnum(Visits) then Visits=0
	if not isdate(AddTime) then AddTime=now()

	ContentTag=addTags(replace(replace(getForm("ContentTag", "post")," ",","),"，",","))
	
	conn.exec "insert into {prefix}Content(LanguageID, SortID, GroupID, Exclusive, TitleColor, Title, Title2, Author, ContentSource, [Content], ContentTag, PageTitle, PageKeywords, PageDesc, OutLink, IsOutLink, ContentStatus, IsTop, IsRecommend, IsImageNews, IsHeadline, IsFeatured, IsGenerated, ContentOrder, Visits, AddTime, EditTime, ImagePath, IndexImage, DownURL"&specStr&", PageFileName, IsNoComment, Star) values("&LanguageID&", "&SortID&", "&GroupID&", '"&Exclusive&"', '"&TitleColor&"', '"&Title&"', '"&Title2&"', '"&Author&"', '"&ContentSource&"', '"&Content&"', '"&ContentTag&"', '"&PageTitle&"', '"&PageKeywords&"', '"&PageDesc&"', '"&OutLink&"', "&IsOutLink&", "&ContentStatus&", "&IsTop&", "&IsRecommend&", "&IsImageNews&", "&IsFeatured&", "&IsFeatured&", "&IsGenerated&", "&ContentOrder&", "&Visits&", '"&AddTime&"', '"&EditTime&"', '"&ImagePath&"',  '"&IndexImage&"',  '"&DownURL&"' "&valueStr&", '"&PageFileName&"', "&IsNoComment&", "&Star&")","exe"
	sortType=getForm("sortType", "post")
	alertMsgAndGo"添加成功", "AspCms_ContentList.asp"&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc	
End Sub

Sub editContent
	ContentID=getForm("ContentID", "post")
	LanguageID=cint(rCookie("languageID"))
	SortID=getForm("SortID", "post")
	GroupID=getForm("GroupID", "post")
	Exclusive=getForm("Exclusive", "post")
	
	TitleColor=getForm("TitleColor", "post")
	Title=getForm("Title", "post")
	Title2=getForm("Title2", "post")
	Author=getForm("Author", "post")
	ContentSource=getForm("ContentSource", "post")
	Content=getForm("Content", "post")
	PageTitle=getForm("PageTitle", "post")
	PageKeywords=getForm("PageKeywords", "post")
	PageDesc=getForm("PageDesc", "post")
	
	OutLink=getForm("OutLink", "post")
	IsOutLink=getCheck(getForm("IsOutLink", "post"))
	ContentStatus=getCheck(getForm("ContentStatus", "post"))
	IsTop=getCheck(getForm("IsTop", "post"))
	IsRecommend=getCheck(getForm("IsRecommend", "post"))
	IsImageNews=getCheck(getForm("IsImageNews", "post"))
	IsHeadline=getCheck(getForm("IsHeadline", "post"))
	IsFeatured=getCheck(getForm("IsFeatured", "post"))
	IsGenerated=getCheck(getForm("IsGenerated", "post"))
	IsNoComment=getCheck(getForm("IsNoComment", "post"))
	
	Star=getForm("Star", "post")	
	ContentOrder=getForm("ContentOrder", "post")
	Visits=getForm("Visits", "post")
	AddTime=getForm("AddTime", "post")
	EditTime=now()
	ImagePath=getForm("ImagePath", "post")
	IndexImage=getForm("IndexImage", "post")
	DownURL=getForm("DownURL", "post")
	PageFileName=getForm("PageFileName", "post")
	if isnul(getForm("spec","post")) then 
		spec=split(",",",")
	else
		spec=split(getForm("spec","post"),",")
	end if
	dim specStr	: specStr=""
	
	if sortType="3" then
		dim i	:i=0
		dim rsObj	:	Set rsObj=Conn.Exec("select SpecField from {prefix}SpecSet order by SpecOrder,SpecID","r1")
		Do While not rsObj.Eof 		
			specStr = specStr&","&rsObj(0)&"='"&trim(spec(i))&"'"
			i=i+1
			rsObj.MoveNext
		Loop		
		rsObj.Close	:	set rsObj=Nothing
	end if
	
	if not isnum(LanguageID) then alertMsgAndGo"当前操作语言错误，请重新登陆","-1"
	if isnul(Title) then alertMsgAndGo"请填写标题","-1"
	if not isnum(SortID) or SortID="0" then alertMsgAndGo"请选择分类","-1"
	if not isnum(ContentOrder) then ContentOrder=99
	if not isnum(Visits) then Visits=0		
	ContentTag=addTags(replace(replace(getForm("ContentTag", "post")," ",","),"，",","))	
	if not isdate(AddTime) then AddTime=now()
	
	conn.exec "update {prefix}Content  set SortID="&SortID&", GroupID="&GroupID&", Exclusive='"&Exclusive&"', TitleColor='"&TitleColor&"', Title='"&Title&"', Title2='"&Title2&"', Author='"&Author&"', ContentSource='"&ContentSource&"', [Content]='"&Content&"', ContentTag='"&ContentTag&"', PageTitle='"&PageTitle&"', PageKeywords='"&PageKeywords&"', PageDesc='"&PageDesc&"', OutLink='"&OutLink&"', IsOutLink="&IsOutLink&", ContentStatus="&ContentStatus&", IsTop="&IsTop&", IsRecommend="&IsRecommend&", IsImageNews="&IsImageNews&", IsHeadline="&IsHeadline&", IsFeatured="&IsFeatured&", IsGenerated="&IsGenerated&", ContentOrder="&ContentOrder&", Visits="&Visits&", AddTime='"&AddTime&"', EditTime='"&EditTime&"', ImagePath='"&ImagePath&"', IndexImage='"&IndexImage&"', DownURL='"&DownURL&"'"&specStr&", PageFileName='"&PageFileName&"', IsNoComment="&IsNoComment&", Star="&Star&" where ContentID="&ContentID,"exe"
	sortType=getForm("sortType", "post")
	alertMsgAndGo"修改成功", "AspCms_ContentList.asp"&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc	
End Sub

Sub moveContent
	dim id : id=getForm("id","post")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"
	dim moveSortID 
	moveSortID=getForm("moveSortID","post")
	conn.exec "update {prefix}Content set SortID="&moveSortID&" where LanguageID="&rCookie("languageID")&" and ContentID in("&id&")", "exe"	
	alertMsgAndGo "移动成功！", getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc	
End Sub


Sub delContent
	dim id : id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"
	conn.exec "update {prefix}Content set ContentStatus=2 where ContentID in("&id&")","exe"
	alertMsgAndGo "删除成功",getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc		
End Sub

Sub Recovery
	dim id : id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"
	conn.exec "update {prefix}Content set ContentStatus=1 where ContentID in("&id&")","exe"
	alertMsgAndGo "恢复成功",getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc		
End Sub
	
Sub trueDelContent
	dim id : id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"
	if runmode=1 then
		dim rs, sql, filepath
		dim templateobj : set templateobj=new TemplateClass
		sql="select ContentID,Title,sortType,SortFolder,a.GroupID,ContentFolder,ContentFileName,a.AddTime,PageFileName,a.SortID,b.GroupID from {prefix}Content as a, {prefix}Sort as b where a.LanguageID="&rCookie("languageID")&" and a.SortID=b.SortID and ContentStatus=2 and ContentID in("&id&")"
		set rs=conn.exec(sql,"r1")		
		do while not rs.eof
			
			filepath=templateobj.getContentLink(rs("SortID"),rs("ContentID"),rs("SortFolder"),rs("a.GroupID"),rs("ContentFolder"),rs("ContentFileName"),rs("AddTime"),rs("PageFileName"),rs("b.GroupID"))
			if isExistFile(filepath) then delFile filepath		
			'echo filepath&"<br>"
			rs.movenext
		loop
	end if
	conn.exec "delete * from {prefix}Content where ContentStatus=2 and ContentID in("&id&")","exe"
	alertMsgAndGo "彻底删除成功",getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc
End Sub
		
Sub listContent
	dim i,orderStr,whereStr,sqlStr,rsObj,allPage,allRecordset,numPerPage
	numPerPage=psize
	if not isnum(numPerPage) then numPerPage=10	
	if isnul(order) then order="ContentID"
	if isnul(ordsc) then ordsc="desc"

	if isNul(page) then page=1 else page=clng(page)
	if page=0 then page=1
	
	orderStr=" order by Contentorder asc,"&order&" "&ordsc &""
	whereStr=" where a.LanguageID="&rCookie("languageID")
	
	if not isNul(sortid) and sortid<>"0" then  whereStr=whereStr&" and a.SortID in("&getSubSort(sortID, 1)&")"
	if not isNul(keyword) then whereStr = whereStr&" and Title like '%"&keyword&"%'"

	sqlStr = "select ContentID,Title,visits,ContentStatus,a.SortID,a.AddTime,IsTop,IndexImage,ContentOrder,SortName,IsRecommend,IsImageNews,IsHeadline,IsFeatured,IsOutLink,OutLink,TitleColor from {prefix}Content as a, {prefix}Sort as b"&whereStr&" and a.SortID=b.SortID and ContentStatus<>2 and b.SortType="&sortType&" "&orderStr	


	set rsObj = conn.Exec(sqlStr,"r1")
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
				"<td><input type=""checkbox"" name=""id"" value="""&rsObj(0)&""" class=""checkbox"" /></td>"&vbcrlf& _
				"<td>"&rsObj(0)&"</td>"&vbcrlf& _
				"<td align=""left""><font color="""&rsObj("TitleColor")&""">"&rsObj(1)&"</font>"&getStr(rsObj("IsOutLink"),"<a href="""&rsObj("OutLink")&""" target=""_blank"" title=""查看""><img src=""../../images/view.gif"" border=0 /></a>","<a href="""&sitePath&setting.languagepath&"content/?"&rsObj(0)&".html"" target=""_blank"" title=""查看""><img src=""../../images/view.gif"" border=0 /></a>")&getStr(rsObj("IsHeadline"),"[<font color=""red"">头</font>]","")&getStr(rsObj("IsFeatured"),"[<font color=""red"">特</font>]","")&getStr(rsObj("IsTop"),"[<font color=""red"">顶</font>]","")&getStr(rsObj("IsRecommend"),"[<font color=""red"">推</font>]","")&getStr(rsObj("IsImageNews"),"[<font color=""red"">图</font>]","")&getStr(rsObj("IsOutLink"),"[<font color=""red"">链</font>]","")&"</td>"&vbcrlf& _	
				"<td>"&rsObj("SortName")&"</td>"&vbcrlf& _			
				"<td>"&rsObj(5)&"</td>"&vbcrlf& _
				"<td><input type=""hidden"" name=""nid"" value="""&rsObj(0)&""" style=""width:40px;""><input type=""text"" name=""order"" value="""&rsObj(8)&""" style=""width:40px; text-align:center;""/></td>"&vbcrlf& _
				"<td>"&getStr(rsObj(3),"<a href=""?action=off&id="&rsObj(0)&"&sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""" title=""启用"" ><IMG src=""../../images/toolbar_ok.gif""></a>","<a href=""?action=on&id="&rsObj(0)&"&sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""" title=""禁用"" ><IMG src=""../../images/toolbar_no.gif""></a>")&"</td>"&vbcrlf& _
				"<td><a href=""AspCms_ContentEdit.asp?id="&rsObj(0)&"&sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""" >修改</a> | <a href=""?action=del&id="&rsObj(0)&"&sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&"""  onClick=""return confirm('确定要删除吗')"">删除</a></td>"&vbcrlf& _
			  "</tr>"&vbcrlf
			rsObj.movenext
			if rsObj.eof then exit for
		next
		echo"<tr bgcolor=""#FFFFFF"" class=""pagenavi"">"&vbcrlf& _
			"<td colspan=""8"" height=""28"" style=""padding-left:20px;"">"&vbcrlf& _			
			"页数："&page&"/"&allPage&"  每页"&numPerPage &" 总记录数"&allRecordset&"条 <a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page=1&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">首页</a> <a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&(page-1)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">上一页</a> "&vbcrlf
		dim pageNumber
		pageNumber=makePageNumber_(page, 10, allPage, "newslist",sortID, order,keyword)
		echo pageNumber
		echo"<a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&(page+1)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">下一页</a> <a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&allPage&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">尾页</a>"&vbcrlf
			
		echo"每页显示"&getPageSize("10",psize)&getPageSize("20",psize)&getPageSize("30",psize)&getPageSize("50",psize)
				
		echo"条"&vbcrlf& _	
		"</td>"&vbcrlf& _
		"</tr>"&vbcrlf
	end if
	rsObj.close : set rsObj = nothing	
End Sub

Sub listContent_

	dim i,orderStr,whereStr,sqlStr,rsObj,allPage,allRecordset,numPerPage
	numPerPage=psize
	if not isnum(numPerPage) then numPerPage=10	
	if isnul(order) then order="ContentID"
	if isnul(ordsc) then ordsc="desc"

	if isNul(page) then page=1 else page=clng(page)
	if page=0 then page=1
	
	orderStr=" order by "&order&" "&ordsc
	whereStr=" where a.LanguageID="&rCookie("languageID")
	
	if not isNul(sortid) and sortid<>"0" then  whereStr=whereStr&" and a.SortID="&SortID
	if not isNul(keyword) then whereStr = whereStr&" and Title like '%"&keyword&"%'"

	sqlStr = "select ContentID,Title,visits,ContentStatus,a.SortID,a.AddTime,IsTop,IndexImage,ContentOrder,SortName,IsRecommend,TitleColor from {prefix}Content as a, {prefix}Sort as b"&whereStr&" and a.SortID=b.SortID and ContentStatus=2 "&orderStr	

	set rsObj = conn.Exec(sqlStr,"r1")
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
				"<td><input type=""checkbox"" name=""id"" value="""&rsObj(0)&""" class=""checkbox"" /></td>"&vbcrlf& _
				"<td>"&rsObj(0)&"</td>"&vbcrlf& _
				"<td><font color="""&rsObj("TitleColor")&""">"&rsObj(1)&"</font>"&"<a href=""/"&sitePath&"/?"&rsObj(4)&"_"&rsObj(0)&".html"" target=""_blank"" title=""查看""><img src=""../../images/view.gif"" border=0 /></a>"&"</td>"&vbcrlf& _	
				"<td>"&rsObj("SortName")&"</td>"&vbcrlf& _			
				"<td>"&rsObj(5)&"</td>"&vbcrlf& _
				"<td><input type=""hidden"" name=""nid"" value="""&rsObj(0)&""" style=""width:40px;""><input type=""text"" name=""order"" value="""&rsObj(8)&""" style=""width:40px; text-align:center;""/></td>"&vbcrlf& _
				"<td>"&"<IMG src=""../../images/toolbar_del.gif"">"&"</td>"&vbcrlf& _
				"<td> <a href=""?action=recovery&id="&rsObj(0)&"&sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""" >恢复</a> <a href=""?action=tdel&id="&rsObj(0)&"&sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&"""  onClick=""return confirm('确定要删除吗')"">删除</a></td>"&vbcrlf& _
			  "</tr>"&vbcrlf
			rsObj.movenext
			if rsObj.eof then exit for
		next
		echo"<tr bgcolor=""#FFFFFF"" class=""pagenavi"">"&vbcrlf& _
			"<td colspan=""8"" height=""28"" style=""padding-left:20px;"">"&vbcrlf& _			
			"页数："&page&"/"&allPage&"  每页"&numPerPage &" 总记录数"&allRecordset&"条 <a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page=1&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">首页</a> <a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&(page-1)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">上一页</a> "&vbcrlf
		dim pageNumber
		pageNumber=makePageNumber_(page, 10, allPage, "newslist",sortID, order,keyword)
		echo pageNumber
		echo"<a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&(page+1)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">下一页</a> <a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&allPage&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">尾页</a>"&vbcrlf
			
		echo"每页显示"&getPageSize("10",psize)&getPageSize("20",psize)&getPageSize("30",psize)&getPageSize("50",psize)
				
		echo"条"&vbcrlf& _	
		"</td>"&vbcrlf& _
		"</tr>"&vbcrlf
	end if
	rsObj.close : set rsObj = nothing	
End Sub
'"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""

Function getPageSize(ps,nps)
	if isnul(nps) then nps="10"
	if ps=nps then 
		getPageSize="<span>"&ps&"</span>"
	else
		getPageSize="<a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&ps&"&order="&order&"&ordsc="&ordsc&""">"&ps&"</a>"
	end if
End Function

Sub updateOrder
	Dim ids				:	ids=split(getForm("nid","post"),",")
	Dim orders		:	orders=split(getForm("order","post"),",")
	If Ubound(ids)=-1 Then 	'防止有值为空时下标越界
		ReDim ids(0)
		ids(0)=""
	End If	
	
	If Ubound(orders)=-1 Then
		ReDim orders(0)
		orders(0)=0
	End If
	Dim i
	
	For i=0 To Ubound(ids)		
		if isnum(trim(orders(i))) then
			Conn.Exec "update {prefix}Content Set ContentOrder="&trim(orders(i))&" Where ContentID="&trim(ids(i)),"exe"	
		else
			Conn.Exec "update {prefix}Content Set ContentOrder=0 Where ContentID="&trim(ids(i)),"exe"	
		end if
	Next
	
	
	alertMsgAndGo "更新排序成功",getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc
End Sub


Function addTags(tags)
	dim sql,rs,tag,tagIDs
	tags=split(tags,",")
	for each tag in tags
		if not isnul(tag) then 	
			sql="select TagID from {prefix}Tag where TagName='"&tag&"'"
			set rs=conn.exec(sql,"r1")
			if not rs.eof then 
				tagIDs=tagIDs&"{"&rs(0)&"}"
			else
				sql="insert into {prefix}Tag(TagName, TagCount, SortType, SortID, TagVisits, LanguageID, AddTime) values('"&tag&"',0,"&SortType&","&SortID&",0,"&LanguageID&",'"&now()&"')"
				conn.exec sql,"exe"
				tagIDs=tagIDs&"{"&conn.exec("select @@identity","r1")(0)&"}"
			end if
		end if		
	next
	addTags=tagIDs	
End Function

%>