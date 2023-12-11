<!--#include file="../../inc/AspCms_SettingClass.asp" -->
<%
commentList
Sub commentList
	Response.Buffer=True
	Response.ContentType="text/html;charset=gb2312"
	Dim rsObj,ContentID,page,pCount,pSize,i,pagerStr,pPage,nPage,numPage
	pSize=5		'评论显示数
	numPage=5	'显示页码数
	ContentID=filterPara(getForm("id","get"))
	page=filterPara(getForm("page","get"))
	
	dim sql
	if SwitchCommentsStatus=0 then
		sql="select * from {prefix}Comments where ContentID="&ContentID
	else
		sql="select * from {prefix}Comments where CommentStatus=1 and ContentID="&ContentID	
	end if
	set rsObj=conn.Exec(sql,"r1")	
	
	if not rsObj.eof then
		rsObj.pageSize=pSize
		pCount=rsObj.pageCount
		if isNul(page) or not isnum(page) or page<1 then page=1 else page=clng(page)
		if page>pCount then page=pCount
		rsObj.absolutepage = page
		for i = 1 to pSize		 
			echo"<div class=""clistbox"">" &vbcrlf& _
			"<div class=""line1""><span>发表于："&rsObj("AddTime")&"</span> 评论者："&filterDirty(rsObj("Commentator"))&" IP："&ipHide(rsObj("CommentIP"))&" </div>" &vbcrlf& _
			"<div class=""line2"">"&filterDirty(rsObj("CommentContent"))&"</div>" &vbcrlf& _
			"</div>" &vbcrlf		 
			rsObj.moveNext
			if rsObj.eof then exit for
		next
		
		pagerStr=makePageNumber(page, numPage, pCount, "commentlist",ContentID,"")		
		if page>1 then pPage=page-1 else pPage=1
		if page=pCount then nPage=page else nPage=page+1
		
		echo "<div class=""pages"">"&"<a href=""javascript:pager("&ContentID&",1)"">首页</a><a href=""javascript:pager("&ContentID&","&pPage&")"">上一页</a>"
		echo pagerStr
		echo "<a href=""javascript:pager("&ContentID&","&nPage&")"">下一页</a>"&"<a href=""javascript:pager("&ContentID&","&pCount&")"">末页</a>"&"</div>"
	else
		echo "还没有评论！"
	end if
	rsObj.close()
	set rsObj=nothing
End Sub
%>
