<!--#include file="../../inc/AspCms_MainClass.asp" -->
<%
dim action : action=getForm("action","get")
dim advname,advclass,ImagePath,LinkPath,imgw,imgh,stime,etime,AdvStatus,advcontent,LanguageID,AddTime,orderStr,whereStr,sqlStr,rsObj,allPage,allRecordset,numPerPage,page, psize, order, ordsc,sortID,keyword,sortType,AdvArray,LinkPathr,ImagePathr,fLinkPath,fImagePath,fLinkPathr,fImagePathr,fAdvID,fAdvName,fAdvClass,fimgw,fimgh,fstime,fetime,fAdvStatus,fadvcontent,fAdvStyle,AdvStyle,faore 
	advname=getForm("advname","post")
	advclass=getForm("advclass","post")
	ImagePath=getForm("ImagePath","post")
	LinkPath=getform("LinkPath","post")
	ImagePathr=getForm("ImagePathr","post")
	LinkPathr=getform("LinkPathr","post")
	imgw=getForm("imgw","post")
	imgh=getForm("imgh","post")
	stime=getForm("stime","post")
	etime=getForm("etime","post")
	AdvStatus=getForm("AdvStatus","post")
	advcontent=getForm("advcontent","post")
	advstyle=getForm("advstyle","post")
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
	faore=getForm("faore","get")
	
Select  case action
	case "add" : advadd
	case "del" : AdvDel
	case "edit" : editform
	case "editsave": editsave
	case "editjs"
		if faore=0 then
			call advadd 
		else 
			call editsave 
		end if
	case "savejs":savejs
	case "on" : onOff "on", "Adv", "AdvId", "AdvStatus", "", getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc
	case "off" : onOff "off", "Adv", "AdvId", "AdvStatus", "", getPageName()&"?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc
End Select

Sub editsave
	dim whereStr
	dim AdvID : AdvID=getForm("id","both")
	whereStr="where AdvID="& AdvID
	if isnul(advname) then alertMsgAndGo"请填写广告名称","-1"
	if not ISDATE(stime) then alertMsgAndGo"时间格式错误","-1" 
	if not ISDATE(etime) then alertMsgAndGo"时间格式错误","-1" 
	if datediff("s",stime,etime)<0 then alertMsgAndGo"开始时间不能大于结束时间","-1"

	select case advclass
		case 5
		ImagePath=ImagePath&"$img$"&ImagePathr
		LinkPath=LinkPath&"$link$"&	LinkPathr
	end select
	if advstyle="" then advstyle=1
	conn.exec "update {prefix}Adv set AdvName='"&advname&"', AdvClass="&advclass&", AdvImg='"&ImagePath&"', AdvLink='"&LinkPath&"', AdvWidth='"&imgw&"', AdvHeight='"&imgh&"', AdvStime='"&stime&"', AdvEtime='"&etime&"', AdvStatus="&AdvStatus&", AdvContent= '"&advcontent&"',AdvStyle="&advstyle&" "& whereStr &"","exe"
			if advclass=4 then
	alertMsgAndGo"保存成功", "AspCms_AdvEditPF.asp?action=savejs&id=4"
	elseif advclass=5 then
	alertMsgAndGo"保存成功", "AspCms_AdvEditDl.asp?action=savejs&id=5"
	elseif advclass=6 then
	alertMsgAndGo"保存成功", "AspCms_AdvEditTC.asp?action=savejs&id=6"
	else
	alertMsgAndGo"保存成功", "AspCms_Advlist.asp"
	end if
End Sub

Sub savejs
	dim whereStr,arrl,arri
	dim ClassID : ClassID=getForm("id","both")
	whereStr="where AdvClass="& ClassID
	AdvArray=conn.Exec("select AdvID,AdvName,AdvClass,AdvImg,AdvLink,AdvWidth,AdvHeight,AdvStime,AdvEtime,AdvStatus,AdvContent,AdvStyle from {prefix}Adv "&whereStr&"","arr")
	if not isarray(AdvArray) then
			
			faore=0
			fImagePath=""
			fLinkPath=""
			fImagePathr=""
			fLinkPathr=""
			fAdvID=""
			fAdvName=""
			fAdvClass=""
			fimgw=""
			fimgh=""
			fstime=now()
			fetime=now()
			fAdvStatus=1
			fAdvContent=""
			fAdvStyle=1
	else
			faore=1
			fAdvID=AdvArray(0,0)
			fAdvName=AdvArray(1,0)
			fAdvClass=AdvArray(2,0)
			fimgw=AdvArray(5,0)
			fimgh=AdvArray(6,0)
			fstime=AdvArray(7,0)
			fetime=AdvArray(8,0)
			fAdvStatus=AdvArray(9,0)
			fAdvContent=AdvArray(10,0)
			fAdvStyle=AdvArray(11,0)
		if AdvArray(2,0)=5 then
			arri=split(AdvArray(3,0),"$img$")
			arrl=split(AdvArray(4,0),"$link$")
			fImagePath=arri(0)
			fLinkPath=arrl(0)
			fImagePathr=arri(1)
			fLinkPathr=arrl(1)
		else
			fImagePath=AdvArray(3,0)
			fLinkPath=AdvArray(4,0)
			fImagePathr=""
			fLinkPathr=""
		end if
		
	end if
End Sub

Sub editform
	dim whereStr,arrl,arri
	dim AdvID : AdvID=getForm("id","both")
	whereStr="where AdvID="& AdvID
	AdvArray=conn.Exec("select AdvID,AdvName,AdvClass,AdvImg,AdvLink,AdvWidth,AdvHeight,AdvStime,AdvEtime,AdvStatus,AdvContent from {prefix}Adv "&whereStr&"","arr")
	if not isarray(AdvArray) then alertMsgAndGo "未存在的广告！","-1" 
	if AdvArray(2,0)=5 then
		arri=split(AdvArray(3,0),"$img$")
		arrl=split(AdvArray(4,0),"$link$")
		fImagePath=arri(0)
		fLinkPath=arrl(0)
		fImagePathr=arri(1)
		fLinkPathr=arrl(1)
	else
		fImagePath=AdvArray(3,0)
		fLinkPath=AdvArray(4,0)
		fImagePathr=""
		fLinkPathr=""
	end if
End Sub
	
Sub advadd
	'LanguageID=cint(rCookie("languageID"))
	'if not isnum(LanguageID) then alertMsgAndGo"当前操作语言错误，请重新登陆","-1"
	if isnul(advname) then alertMsgAndGo"请填写广告名称","-1"
	
	if not ISDATE(AddTime) then AddTime=now()
	if not ISDATE(stime) then alertMsgAndGo"时间格式错误","-1" 
	if not ISDATE(etime) then alertMsgAndGo"时间格式错误","-1" 
	if datediff("s",stime,etime)<0 then alertMsgAndGo"开始时间不能大于结束时间","-1"
	select case advclass
		case 5
		ImagePath=ImagePath&"$img$"&ImagePathr
		LinkPath=LinkPath&"$link$"&	LinkPathr
	end select
	if advstyle="" then advstyle=1
	conn.exec "insert into {prefix}Adv(AdvName, AdvClass, AdvImg, AdvLink, AdvWidth, AdvHeight, AdvStime, AdvEtime, AdvStatus, AdvContent, AddTime,AdvStyle) values('"&advname&"', "&advclass&", '"&ImagePath&"', '"&LinkPath&"', '"&imgw&"', '"&imgh&"', '"&stime&"', '"&etime&"', "&AdvStatus&", '"&advcontent&"', '"&AddTime&"',"&advstyle&")","exe"
		if advclass=4 then
	alertMsgAndGo"保存成功", "AspCms_AdvEditPF.asp?action=savejs&id=4"
	elseif advclass=5 then
	alertMsgAndGo"保存成功", "AspCms_AdvEditDl.asp?action=savejs&id=5"
	elseif advclass=6 then
	alertMsgAndGo"保存成功", "AspCms_AdvEditTC.asp?action=savejs&id=6"
	else
	alertMsgAndGo"保存成功", "AspCms_Advlist.asp"
	end if
End Sub

Sub AdvList		
	dim i
	numPerPage=psize
	if not isnum(numPerPage) then numPerPage=10
	if isnul(order) then order="AdvID"
	if isnul(ordsc) then ordsc="desc"
	if isNul(page) then page=1 else page=clng(page)
	if page=0 then page=1
	
	orderStr=" order by "&order&" "&ordsc
	'whereStr=" where LanguageID="&rCookie("languageID")
	whereStr=" where AdvClass not in(4,5,6)"
	sqlStr = "select AdvID,AdvName,AdvClass,AdvStime,AdvEtime,AdvStatus from {prefix}Adv"&whereStr & orderStr	
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
				"<td>"&rsObj(3)&"</td>"&vbcrlf& _			
				"<td>"&rsObj(4)&"</td>"&vbcrlf& _
				"<td>{aspcms:adv"&rsObj(0)&"}</td>"&vbcrlf& _
				"<td>"&getStr(rsObj(5),"<a href=""?action=off&id="&rsObj(0)&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""" title=""启用"" ><IMG src=""../images/toolbar_ok.gif""></a>","<a href=""?action=on&id="&rsObj(0)&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""" title=""禁用"" ><IMG src=""../images/toolbar_no.gif""></a>")&"</td>"&vbcrlf& _
				"<td><a href=""AspCms_AdvEdit.asp?action=edit&id="&rsObj(0)&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""" >修改</a> | <a href=""?action=del&id="&rsObj(0)&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&"""  onClick=""return confirm('确定要删除吗')"">删除</a></td>"&vbcrlf& _
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

Sub AdvDel
	dim AdvID : AdvID=getForm("id","both")
	if isnul(AdvID) then alertMsgAndGo "请选择要删除的内容","-1"
	conn.exec "delete from {prefix}Adv where AdvID in("&AdvID&")","exe"	
	alertMsgAndGo "删除成功","AspCms_Advlist.asp"
End Sub

Function getPageSize(ps,nps)
	if isnul(nps) then nps="10"
	if ps=nps then 
		getPageSize="<span>"&ps&"</span>"
	else
		getPageSize="<a href=""?sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&ps&"&order="&order&"&ordsc="&ordsc&""">"&ps&"</a>"
	end if
End Function

%>