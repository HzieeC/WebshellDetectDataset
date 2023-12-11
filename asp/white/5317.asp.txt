<!--#include file="../../../inc/AspCms_MainClass.asp" -->
<%
CheckAdmin("AspCms_Links.asp")

dim action : action=getForm("action", "get")
dim LinkType,LinkText,LinkDesc,ImageURL,LinkOrder,LinkStatus,LinkURL,ID


Select case action
	case "del" : delLinks
	case "add" : addLinks
	case "edit" : editLinks
	case "on" : onOff "on", "Links", "LinkID", "LinkStatus", "", getPageName()
	case "off" : onOff "off", "Links", "LinkID", "LinkStatus", "", getPageName()
	
End Select

Sub getContent
	ID = getForm("id","get")
	Dim rsObj 	: Set rsObj=Conn.Exec("select LinkType,LinkText,LinkDesc,ImageURL,LinkOrder,LinkStatus,LinkURL from {prefix}Links where LinkID="&ID,"r1")
	if isnul(ID) then alertMsgAndGo "ID不能为空","-1"	
	LinkType=rsObj(0)
	LinkText=rsObj(1)
	LinkDesc=rsObj(2)
	ImageURL=rsObj(3)
	LinkOrder=rsObj(4)
	LinkStatus=rsObj(5)
	LinkURL=rsObj(6)
	rsObj.close	:	Set rsObj = nothing
End Sub

Sub editLinks
	ID=getForm("LinkID","post")
	LinkType=getForm("LinkType","post")	
	LinkText=getForm("LinkText","post")	
	LinkDesc=getForm("LinkDesc","post")	
	ImageURL =getForm("ImageURL","post")	
	LinkOrder=getForm("LinkOrder","post")
	LinkStatus=getCheck(getForm("LinkStatus","post"))
	LinkURL=getForm("LinkURL","post")	

	if isnul(LinkText) then alertMsgAndGo "网站名称不能为空","-1"
	if isnul(LinkURL) then alertMsgAndGo "链接地址不能为空","-1"		
	if not isurl(LinkURL) then alertMsgAndGo "链接地址不正确","-1"	
	if isnul(LinkStatus) then LinkStatus=0
	conn.Exec "update {prefix}Links set LinkType="&LinkType&",LinkText='"&LinkText&"',LinkDesc='"&LinkDesc&"',ImageURL='"&ImageURL&"',LinkOrder="&LinkOrder&",LinkStatus="&LinkStatus&",LinkURL='"&LinkURL&"' where LinkID="&ID,"exe"
	
	alertMsgAndGo "修改成功","AspCms_Links.asp"
End Sub

Sub addLinks 	
	LinkType=getForm("LinkType","post")	
	LinkText=getForm("LinkText","post")	
	LinkDesc=getForm("LinkDesc","post")	
	ImageURL =getForm("ImageURL","post")	
	LinkOrder=getForm("LinkOrder","post")
	LinkStatus=getCheck(getForm("LinkStatus","post"))
	LinkURL=getForm("LinkURL","post")	

	if isnul(LinkText) then alertMsgAndGo "网站名称不能为空","-1"
	if isnul(LinkURL) then alertMsgAndGo "链接地址不能为空","-1"		
	if not isurl(LinkURL) then alertMsgAndGo "链接地址不正确","-1"
	'if LinkType and isnul(ImageURL) then alertMsgAndGo "图片地址不能为空","-1"
	conn.Exec "insert into {prefix}Links(LinkType,LinkText,LinkDesc,ImageURL,LinkOrder,LinkStatus,LinkURL) values('"&LinkType&"','"&LinkText&"','"&LinkDesc&"','"&ImageURL&"','"&LinkOrder&"','"&LinkStatus&"','"&LinkURL&"')","exe"		
	alertMsgAndGo "添加成功","AspCms_Links.asp"
End Sub	

Sub linksList
	Dim rsObj	:	Set rsObj=conn.Exec("select LinkID,LinkText,LinkURL,ImageURL,LinkOrder,LinkStatus,LinkType from {prefix}Links Order by LinkOrder Asc,LinkID","r1")
	If rsObj.Eof Then 
		echo"<tr bgcolor=""#FFFFFF"" align=""center"">"&vbcrlf& _
			"<td colspan=""7"">没有数据</td>"&vbcrlf& _
		  "</tr>"&vbcrlf
	Else
		Do while not rsObj.Eof 
		Dim linkDisplay,linktype
		if rsObj(6) ="0" then linkDisplay=rsObj(1) : linktype ="文字链接"
		if rsObj(6) ="1" then linkDisplay="<img height=""30"" title="""&rsObj(1)&""" src="""&rsObj(3)&""" />" : linktype ="图片链接"
		 echo"<tr class=list>"&vbcrlf& _
			"<TD align=middle height=26><INPUT type=checkbox value="""&rsObj(0)&""" name=""id""></TD>"&vbcrlf& _
			"<td>"&rsObj(0)&"</td>"&vbcrlf& _
			"<td>"&linkDisplay&"</td>"&vbcrlf& _
			"<td>"&rsObj(2)&"</td>"&vbcrlf& _
			"<td>"&rsObj(4)&"</td>"&vbcrlf& _
			"<td>"&linktype&"</td>"&vbcrlf& _
			"<TD>"&getStr(rsObj(5),"<a href=""?action=off&id="&rsObj(0)&""" title=""启用""><IMG src=""../../images/toolbar_ok.gif""></a>","<a href=""?action=on&id="&rsObj(0)&""" title=""禁用"" ><IMG src=""../../images/toolbar_no.gif""></a>")&"</TD>"&vbcrlf& _
			"<td><a href=""AspCms_LinksEdit.asp?id="&rsObj(0)&""" class=""txt_C1"">修改</a> | <a href=""?action=del&id="&rsObj(0)&""" class=""txt_C1"" onClick=""return confirm('确定要删除吗')"">删除</a></td>"&vbcrlf& _
		  "</tr>"&vbcrlf
		  rsObj.MoveNext
		Loop
	End If
	rsObj.close	:	Set rsObj = nothing
End Sub

Sub delLinks 
	id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要删除的内容","-1"

	conn.Exec "Delete * from {prefix}Links where LinkID in("&id&")" ,"exe"
'	alertMsgAndGo "删除成功","AspCms_Links.asp"	
	response.Redirect("AspCms_Links.asp")
End Sub 

%>