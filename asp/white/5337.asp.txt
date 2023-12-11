<!--#include file="../../inc/AspCms_MainClass.asp" -->
<%
'CheckAdmin()

dim action : action=getForm("action","get")
dim todo :todo=getForm("todo","get")

dim urlb,pagenum,urla,pagelinkcount,listpageareab,listpageareaa,listtitleb,listtitlea,pagelinknum,relink,purl,articletitb,articletita,articleconb,articlecona,contentnum,rulesname,CollectID,sortType, keyword, page, psize, order, ordsc,sortid,LanguageID,numPerPage,orderStr,whereStr,sqlStr,rsObj,allRecordset,allPage
dim sql, msg

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
	
Server.ScriptTimeOut = 500
Select case action
	case "getlinklist" : getlinklist :session("relink")=relink
	'case "delarrlink" : delarrlink:session("relink")=relink
	case "Scollect":scollect
	case "save":collectsave
	case "edit":collectsave
	case "del":collectdel
	case "reset":collectreset
	case "savecontent":contentsave
	case "contentdel":contentdel
	case "addContent":addContent
	case "gettodo":getlinklist
End Select

Sub addContent
	dim id : id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"
	dim SortID: SortID=getForm("moveSortID","both")
	if not isnum(SortID) or SortID="0" then alertMsgAndGo"请选择分类","-1"
	LanguageID=cint(rCookie("languageID"))	
	dim isdel:isdel=getForm("isdel","both")
	sql="insert into {prefix}Content(Title,[Content],LanguageID, SortID, GroupID, Exclusive, TitleColor, ContentStatus, IsTop, IsRecommend, IsImageNews, IsHeadline, IsFeatured, IsGenerated, ContentOrder, Visits,AddTime,IsOutlink) select C_Title,C_Content,"&LanguageID&","& SortID &",2,'>=','#000000',1,0,0,0,0,0,0,0,0,'"& now() &"',0 from {prefix}Collect_Content where C_ContentID in("&id&",)"
	conn.exec sql,"exe"
	if isdel="del" then
		sql="delete from {prefix}Collect_Content where C_ContentID in("&id&")"
		conn.exec sql,"exe"
	end if
	sortType=getForm("sortType", "post")
	alertMsgAndGo"添加成功", getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc
End Sub

Sub contentdel
	dim id : id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"
	sql="delete from {prefix}Collect_Content where C_ContentID in("&id&")"
	conn.exec sql,"exe"
	alertMsgAndGo "删除成功",getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc
End Sub

Sub CollecttodoList		
	dim i
	numPerPage=psize
	if not isnum(numPerPage) then numPerPage=10
	if isnul(order) then order="CollectID"
	if isnul(ordsc) then ordsc="desc"

	if isNul(page) then page=1 else page=clng(page)
	if page=0 then page=1
	
	orderStr=" order by "&order&" "&ordsc
	'whereStr=" where LanguageID="&rCookie("languageID")
	whereStr=" where 1=1"
	sqlStr = "select CollectID,CollectName from {prefix}Collect"&whereStr & orderStr	
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
				"<td><input type=""checkbox"" name=""CollectID"" value="""&rsObj(0)&""" class=""checkbox"" /></td>"&vbcrlf& _
				"<td>"&rsObj(0)&"</td>"&vbcrlf& _
				"<td>"&rsObj(1)&"</td>"&vbcrlf& _	
				"<td><a href=""AspCms_Collectlist.asp?action=gettodo&todo=this&CollectID="&rsObj(0)&""">开始采集</a> | <a href=""AspCms_Collectlist.asp?action=Scollect&CollectID="&rsObj(0)&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""" >修改</a> | <a href=""?action=del&CollectID="&rsObj(0)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&"""  onClick=""return confirm('确定要删除吗')"">删除</a></td>"&vbcrlf& _
			  "</tr>"&vbcrlf
			rsObj.movenext
			if rsObj.eof then exit for
		next
		echo"<tr bgcolor=""#FFFFFF"" class=""pagenavi"">"&vbcrlf& _
			"<td colspan=""8"" height=""28"" style=""padding-left:20px;"">"&vbcrlf& _			
			"页数："&page&"/"&allPage&"  每页"&numPerPage &" 总记录数"&allRecordset&"条 <a href=""?action=Ctoo&keyword="&keyword&"&page=1&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">首页</a> <a href=""?action=Ctoo&keyword="&keyword&"&page="&(page-1)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">上一页</a> "&vbcrlf
		dim pageNumber
		pageNumber=makePageNumber_(page, 10, allPage, "newslist",sortID, order,keyword)
		echo pageNumber
		echo"<a href=""?action=Ctoo&keyword="&keyword&"&page="&(page+1)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">下一页</a> <a href=""?action=Ctoo&keyword="&keyword&"&page="&allPage&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">尾页</a>"
		echo"每页显示"&getPageSize("10",psize)&getPageSize("20",psize)&getPageSize("30",psize)&getPageSize("50",psize)
				
		echo"条"&vbcrlf& _	
		"</td>"&vbcrlf& _
		"</tr>"&vbcrlf
	end if
	rsObj.close : set rsObj = nothing	
End Sub

Sub Ctoolist	
	dim i,orderStr,whereStr,sqlStr,rsObj,allPage,allRecordset,numPerPage
	numPerPage=psize
	if not isnum(numPerPage) then numPerPage=10	
	if isnul(order) then order="C_ContentID"
	if isnul(ordsc) then ordsc="desc"
	if isNul(page) then page=1 else page=clng(page)
	if page=0 then page=1	
	orderStr=" order by "&order&" "&ordsc
	whereStr=" where 1=1"
	if not isNul(keyword) then whereStr = whereStr&" and C_Title like '%"&keyword&"%'"
	sqlStr = "select C_ContentID,C_Title,AddTime from {prefix}Collect_Content "&whereStr&" "&orderStr	
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
				"<td>"&rsObj(1)&"</td>"&vbcrlf& _
				"<td>"&rsObj(2)&"</td>"&vbcrlf& _	
				"<td><a href=""?action=contentdel&id="&rsObj(0)&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&"""  onClick=""return confirm('确定要删除吗')"">删除</a></td>"&vbcrlf& _
			  "</tr>"&vbcrlf
			rsObj.movenext
			if rsObj.eof then exit for
		next
		echo"<tr bgcolor=""#FFFFFF"" class=""pagenavi"">"&vbcrlf& _
			"<td colspan=""8"" height=""28"" style=""padding-left:20px;"">"&vbcrlf& _			
			"页数："&page&"/"&allPage&"  每页"&numPerPage &" 总记录数"&allRecordset&"条 <a href=""?action=Ctoo&keyword="&keyword&"&page=1&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">首页</a> <a href=""?action=Ctoo&keyword="&keyword&"&page="&(page-1)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">上一页</a> "&vbcrlf
		dim pageNumber
		pageNumber=makePageNumber_(page, 10, allPage, "newslist",0, order,keyword)
		echo pageNumber
		echo"<a href=""?action=Ctoo&keyword="&keyword&"&page="&(page+1)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">下一页</a> <a href=""?action=Ctoo&keyword="&keyword&"&page="&allPage&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""">尾页</a>"
		echo"每页显示"&getPageSize("10",psize)&getPageSize("20",psize)&getPageSize("30",psize)&getPageSize("50",psize)
				
		echo"条"&vbcrlf& _	
		"</td>"&vbcrlf& _
		"</tr>"&vbcrlf
	end if
	rsObj.close : set rsObj = nothing	
End Sub

Sub contentsave
	dim id	:id=getForm("id","both")
	dim arrtcs : arrtcs=session("savecontent")
	dim ids :ids=split(id,",")
	dim arrtc :arrtc=split(arrtcs,"$RKfgf$")
	dim i
	dim Tcs
	for i=ubound(arrtc) to 1 step -1
		if issame(i,ids)=true then
			Tcs=split(arrtc(i),"$TITfgf$")
			sql="insert into {prefix}Collect_Content(C_Title,C_Content,AddTime) values('"&Tcs(0)&"', '"&Tcs(1)&"', '"&now()&"')"
			conn.exec sql,"exe"	
		end if
	next
	session("savecontent")=""
	msg="入库成功"
	alertMsgAndGo msg,"AspCms_CollectContentlist.asp"
End Sub

Sub collectreset
	session("formvalue")=""
	alertMsgAndGo msg,"AspCms_Collectlist.asp"
End Sub

Sub collectdel
	CollectID=getForm("CollectID","both")
	sql="delete from {prefix}Collect where CollectID in ("&CollectID&")"
	conn.exec sql,"exe"	
	msg="删除成功"
	session("formvalue")=""
	alertMsgAndGo msg,"AspCms_Collect.asp"
End Sub

Sub collectedit
		dim Crules
		urlb=getForm("urlb","post")
		pagenum=getForm("pagenum","post")
		urla=getForm("urla","post")
		pagelinkcount=getform("pagelinkcount","post")
		listpageareab=getForm("listpageareab","post")
		listpageareaa=getForm("listpageareaa","post")
		listtitleb=getForm("listtitleb","post")
		listtitlea=getForm("listtitlea","post")
		pagelinknum=getForm("pagelinknum","post")
		purl=getForm("purl","post")
		articletitb=getForm("articletitb","post")
		articletita=getForm("articletita","post")
		articleconb=getForm("articleconb","post")
		articlecona=getForm("articlecona","post")
		contentnum=getForm("contentnum","post")
		rulesname=getForm("rulesname","post")
		Crules=urlb&"$Cfgf$"&pagenum&"$Cfgf$"&urla&"$Cfgf$"&pagelinkcount&"$Cfgf$"&listpageareab&"$Cfgf$"&listpageareaa&"$Cfgf$"&listtitleb&"$Cfgf$"&listtitlea&"$Cfgf$"&pagelinknum&"$Cfgf$"&purl&"$Cfgf$"&articletitb&"$Cfgf$"&articletita&"$Cfgf$"&articleconb&"$Cfgf$"&articlecona&"$Cfgf$"&contentnum
		CollectID=getForm("DocollectID","post")
	if isNul(rulesname) then alertMsgAndGo"请填写规则名称","-1"
	
	conn.exec sql,"exe"	
	msg="修改规则成功"
	alertMsgAndGo msg,"AspCms_Collectlist.asp?action=Scollect&Collectid="&CollectID
End Sub

Sub scollect
	CollectID=getForm("CollectID","get")
	sql="select * from {prefix}Collect where CollectID="&CollectID
	dim rs
	set rs=conn.exec(sql,"r1")
	if rs.eof then 
		
	else
		do while not rs.eof		
			session("formvalue")=rs("CollectRules")
			CollectID=rs("CollectID")
			rulesname=rs("CollectName")
			rs.moveNext
		loop
	end if	
	rs.close : set rs=nothing
End Sub


Sub collectsave
	dim Crules
		urlb=getForm("urlb","post")
		pagenum=getForm("pagenum","post")
		urla=getForm("urla","post")
		pagelinkcount=getform("pagelinkcount","post")
		listpageareab=getForm("listpageareab","post")
		listpageareaa=getForm("listpageareaa","post")
		listtitleb=getForm("listtitleb","post")
		listtitlea=getForm("listtitlea","post")
		pagelinknum=getForm("pagelinknum","post")
		purl=getForm("purl","post")
		articletitb=getForm("articletitb","post")
		articletita=getForm("articletita","post")
		articleconb=getForm("articleconb","post")
		articlecona=getForm("articlecona","post")
		contentnum=getForm("contentnum","post")
		rulesname=getForm("rulesname","post")
		CollectID=getForm("DocollectID","post")
		
		Crules=urlb&"$Cfgf$"&pagenum&"$Cfgf$"&urla&"$Cfgf$"&pagelinkcount&"$Cfgf$"&listpageareab&"$Cfgf$"&listpageareaa&"$Cfgf$"&listtitleb&"$Cfgf$"&listtitlea&"$Cfgf$"&pagelinknum&"$Cfgf$"&purl&"$Cfgf$"&articletitb&"$Cfgf$"&articletita&"$Cfgf$"&articleconb&"$Cfgf$"&articlecona&"$Cfgf$"&contentnum
		
		
		
	
	if isNul(rulesname) then alertMsgAndGo "请填写规则名称","-1"
	if isNul(CollectID) then 
		sql="insert into {prefix}Collect(CollectName,CollectRules,Addtime) values('"&rulesname&"', '"&Crules&"', '"&now()&"')"
	else
		sql="update {prefix}Collect set CollectName= '"&rulesname&"',CollectRules= '"&Crules&"' where CollectID="&CollectID
	end if
	conn.exec sql,"exe"	
	msg="保存规则成功"
	alertMsgAndGo msg,"AspCms_Collect.asp"
End Sub

Sub collectlist
	sql="select * from {prefix}Collect order by CollectID"
	dim rs
	set rs=conn.exec(sql,"r1")
	if rs.eof then 
		
	else
		do while not rs.eof		
			echo "<option value="& rs("CollectID") &">"& rs("CollectName") &"</option>"
			rs.moveNext
		loop
	end if	
	rs.close : set rs=nothing
End Sub


Sub getcontenttl
	dim Arrlink,i,Ctitle,Ccontent,Cnum,savecontent,ischecked
	dim id	:id=getForm("id","both")
	dim arrlinkvalues : arrlinkvalues=session("relink")
	dim ids :ids=split(id,",")
	dim arrlinkvalue :arrlinkvalue=split(arrlinkvalues,"$Lfgf$")
	if isNul(formvalue(14)) then Cnum=0 else Cnum=formvalue(14)
	for i=1 to ubound(arrlinkvalue)
		if issame(i,ids)=true or todo="this" then
			Ctitle=replace(replace(dropHtml(RegExpTest(formvalue(10)&"([\s\S]*?)"&formvalue(11),getHTTPPage(arrlinkvalue(i)),"",0,0)),formvalue(10),""),formvalue(11),"")
			Ccontent=ImgNoHtml(RegExpTest(formvalue(12)&"([\s\S]*?)"&formvalue(13),getHTTPPage(arrlinkvalue(i)),"",0,Cnum))
			Ccontent=replace(replace(Ccontent,formvalue(12),""),formvalue(13),"")
			Ccontent=replace(Ccontent,"src=""","src="""&formvalue(9)&"")
			savecontent=savecontent&"$RKfgf$"& Ctitle &	"$TITfgf$"& Ccontent
			if isnul(Ctitle) or isnul(Ccontent) then
				ischecked=""
			else
				ischecked="checked"
			end if
		   echo "<TR class=list>"&vbcrlf& _
				"<TD align=middle height=26><INPUT type=checkbox value="""&i&""" name=""id"" "& ischecked &"></TD>"&vbcrlf& _
				"<TD>"&i&"</TD>"&vbcrlf& _
				"<TD><a href="""&arrlinkvalue(i)&"""  target=""_blank"">"&Ctitle&"</a></TD>"&vbcrlf& _	
				"</TR>"&vbcrlf& _
				"<TR class=list>"&vbcrlf& _
				"<TD colspan=3>"&getStrByLen(Ccontent,500)&"</TD></TR>"
		end if
	next
	session("savecontent")=savecontent
End Sub

Sub getlinklist
dim Cpagenum,Cpagelinkcount,Cpagelinknum,urlfull,pgcount
if todo<>"del" then
	if todo="this" then
		CollectID=getForm("CollectID","both")
		sql="select CollectRules from {prefix}Collect where CollectID ="&CollectID
		dim rs
		set rs=conn.exec(sql,"r1")
		if rs.eof then 
			
		else
			do while not rs.eof		
				session("formvalue")=rs("CollectRules")
				rs.moveNext
			loop
		end if			
		rs.close : set rs=nothing
		
		if isNul(formvalue(3)) then Cpagelinkcount=1 else Cpagelinkcount=formvalue(3)
		if isNul(formvalue(1)) then Cpagenum=1 else Cpagenum=formvalue(1)
		if isNul(formvalue(8)) then Cpagelinknum=0 else Cpagelinknum=formvalue(8)

		for pgcount=cint(Cpagenum) to (cint(Cpagenum)+cint(Cpagelinkcount))-1 
			urlfull=formvalue(0) & pgcount & formvalue(2)
			relink=relink&RegExpTest(formvalue(6)&"([\s\S]*?)"&formvalue(7),RegExpTest(formvalue(4)&"([\s\S]*?)"&formvalue(5),getHTTPPage(urlfull),"",0,0),"li",Cpagelinknum,0)
		next
		session("relink")=relink
		'*********************************************************
		
		'***********************************************************
		else
		urlb=getForm("urlb","post")
		pagenum=getForm("pagenum","post")
		urla=getForm("urla","post")
		pagelinkcount=getform("pagelinkcount","post")
		listpageareab=getForm("listpageareab","post")
		listpageareaa=getForm("listpageareaa","post")
		listtitleb=getForm("listtitleb","post")
		listtitlea=getForm("listtitlea","post")
		pagelinknum=getForm("pagelinknum","post")
		purl=getForm("purl","post")
		articletitb=getForm("articletitb","post")
		articletita=getForm("articletita","post")
		articleconb=getForm("articleconb","post")
		articlecona=getForm("articlecona","post")
		contentnum=getForm("contentnum","post")
		session("formvalue")=urlb&"$Cfgf$"&pagenum&"$Cfgf$"&urla&"$Cfgf$"&pagelinkcount&"$Cfgf$"&listpageareab&"$Cfgf$"&listpageareaa&"$Cfgf$"&listtitleb&"$Cfgf$"&listtitlea&"$Cfgf$"&pagelinknum&"$Cfgf$"&purl&"$Cfgf$"&articletitb&"$Cfgf$"&articletita&"$Cfgf$"&articleconb&"$Cfgf$"&articlecona&"$Cfgf$"&contentnum
		
		if isNul(formvalue(3)) then Cpagelinkcount=1 else Cpagelinkcount=formvalue(3)
		if isNul(formvalue(1)) then Cpagenum=1 else Cpagenum=formvalue(1)
		if isNul(formvalue(8)) then Cpagelinknum=0 else Cpagelinknum=formvalue(8)
	
		for pgcount=cint(Cpagenum) to (cint(Cpagenum)+cint(Cpagelinkcount))-1 
			urlfull=formvalue(0) & pgcount & formvalue(2)
			relink=relink&RegExpTest(formvalue(6)&"([\s\S]*?)"&formvalue(7),RegExpTest(formvalue(4)&"([\s\S]*?)"&formvalue(5),getHTTPPage(urlfull),"",0,0),"li",Cpagelinknum,0)
		next
		end if
	
	end if
End Sub
'*********************************************
Sub linklist
	dim Arrlink,i
	Arrlink=split(session("relink"),"$Lfgf$")
	for i = 1 to ubound(Arrlink)
	echo "<TR class=list>"&vbcrlf& _
			"<TD align=middle height=26><INPUT type=checkbox value="""&i&""" name=""id"" checked></TD>"&vbcrlf& _
			"<TD>"&i&"</TD>"&vbcrlf& _
			"<TD><a href="""&Arrlink(i)&"""  target=""_blank"">"&Arrlink(i)&"</a></TD></TR>"	
	next
End Sub
'************************************************
Sub delarrlink
	dim id	:id=getForm("id","both")
	dim arrlinkvalues : arrlinkvalues=session("relink")
	dim ids :ids=split(id,",")
	dim arrlinkvalue :arrlinkvalue=split(arrlinkvalues,"$Lfgf$")
	dim i
	for i=1 to ubound(arrlinkvalue)
		if issame(i,ids)=false then
			relink=relink&"$Lfgf$"&arrlinkvalue(i)
		end if
	next
End Sub

Function issame(t,arrt)
	dim i
	For i=0 to ubound(arrt)
	if t=cint(arrt(i)) then
		issame=true
		exit for
	else
		issame=false
	end if
	next
end Function

Function BytesToBstr(body,Cset)
	dim objstream
	set objstream = Server.CreateObject("adodb.stream")
	objstream.Type = 1
	objstream.Mode =3
	objstream.Open
	objstream.Write body
	objstream.Position = 0
	objstream.Type = 2
	objstream.Charset = Cset
	BytesToBstr = objstream.ReadText
	objstream.Close
	set objstream = nothing
End Function

Function getHTTPPage(url) '获取采集页面源码
	On Error Resume Next
	dim http
	set http=Server.createobject("Microsoft.XMLHTTP")
	Http.open "GET",url,false
	Http.send()
	if Http.readystate<>4 then
		exit function
	end if
	getHTTPPage=bytesToBSTR(Http.responseBody,"GB2312")
	set http=nothing
	If Err.number<>0 then
		echo "<p align='center'><font color='red'><b>服务器获取文件内容出错</b></font></p>"
		Err.Clear
	End If
End Function

Function RegExpTest(patrn,strng,area,pnum,cnum)
	Dim regEx,Matches,RetStr,Match
	Set regEx = New RegExp
	regEx.Pattern = patrn
	regEx.IgnoreCase = True
	regEx.Global = True
	Set Matches = regEx.Execute(strng)
	if area="li" then
		For Each Match in Matches
			RetStr=RetStr&RegExplink(Match.Value,pnum)
		Next
	else
		if cnum=0 then
			For Each Match in Matches
				RetStr=RetStr&Match.Value
			Next
		else
			if Matches.count-cnum>=0 then	
				RetStr =RetStr&Matches(cnum-1) 
			end if
		end if
	end if
	RegExpTest = RetStr
End Function 

Function RegExplink(strng,pnum)
	Dim regEx,Matches,RetStr,Match,PageArticle
	Set regEx = New RegExp
	regEx.Pattern = "<a.+?href=""(.+?)""[^\>]*>.+?</a>"
	regEx.IgnoreCase = True
	regEx.Global = True
	Set Matches = regEx.Execute(strng)
	if pnum=0 then
		For Each Match in Matches
			RetStr=RetStr&"$Lfgf$"& formvalue(9) &regEx.replace(Match.Value,"$1") 
		Next
	else
		if Matches.count-pnum>=0 then	
			RetStr =RetStr&"$Lfgf$"& formvalue(9)& regEx.replace(Matches(pnum-1),"$1") 
		end if
	end if
	RegExplink = RetStr
End Function
	
Function formvalue(i)
	dim formvalues
	if session("formvalue")="" then
		session("formvalue")="$Cfgf$$Cfgf$$Cfgf$$Cfgf$$Cfgf$$Cfgf$$Cfgf$$Cfgf$$Cfgf$$Cfgf$$Cfgf$$Cfgf$$Cfgf$$Cfgf$"
	end if
	formvalues=split(session("formvalue"),"$Cfgf$")
	formvalue=decodeHtml(formvalues(i))
End Function

Function ImgNoHtml(IString)
       IString = RemoveByRegExp("< *iframe(.|\r|\n)+?/iframe *>", IString)
	   IString = RemoveByRegExp("< *script(.|\r|\n)+?/ *script *>", IString)
	'  IString=RemoveByRegExp("&#[^>]*;",IString)   
	'  IString=RemoveByRegExp("</?marquee[^>]*>",IString)   
	  IString=RemoveByRegExp("</?object[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?param[^>]*>",IString)   
	  IString=RemoveByRegExp("</?embed[^>]*>",IString)   
	   IString=RemoveByRegExp("</?table[^>]*>",IString)   
	'  IString=RemoveByRegExp("&nbsp;",IString)   
	'  IString=RemoveByRegExp("</?tr[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?th[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?p[^>]*>",IString)   
	   IString=RemoveByRegExp("</?a[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?img[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?tbody[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?li[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?span[^>]*>",IString)   
	   IString=RemoveByRegExp("</?div[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?th[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?td[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?script[^>]*>",IString)   
	'  IString=RemoveByRegExp("(javascript|jscript|vbscript|vbs):",IString)   
	'  IString=RemoveByRegExp("on(mouse|exit|error|click|key)",IString)   
	'  IString=RemoveByRegExp("<\\?xml[^>]*>",IString)   
	'  IString=RemoveByRegExp("<\/?[a-z]+:[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?font[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?b[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?u[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?i[^>]*>",IString)   
	'  IString=RemoveByRegExp("</?strong[^>]*>",IString)  
	   IString=RemoveByRegExp("\<style\>([\s\S]*?)\<\/style\>",IString)  
	
        ImgNoHtml = IString
End Function

Function RemoveByRegExp(RegExpPattern, sInputStr)   
Dim objRegExp   
Set objRegExp = New Regexp   
objRegExp.IgnoreCase = True  
objRegExp.Global = True  
objRegExp.Pattern = RegExpPattern   
sInputStr = objRegExp.Replace(sInputStr, "")   
Set objRegExp = Nothing    
RemoveByRegExp = sInputStr   
End Function  

Function getPageSize(ps,nps)
	if isnul(nps) then nps="10"
	if ps=nps then 
		getPageSize="<span>"&ps&"</span>"
	else
		getPageSize="<a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&ps&"&order="&order&"&ordsc="&ordsc&""">"&ps&"</a>"
	end if
End Function
%>