<!--#include file="../../inc/AspCms_SettingClass.asp" -->
<!--#include file="../../editor/fckeditor.asp" -->
<%
CheckAdmin("AspCms_Label.asp")
dim action : action=getForm("action","get")
Select case action	
	case "del" : delLabel
	case "add" : addLabel
	case "edit" : editLabel
	case "save" :saveLabel	
End Select

dim LabelID, LabelName, LabelDesc, LabelContent, Content

dim sql, msg

Sub getLabel
	dim id : id=getForm("id","get")
	if not isnul(ID) then		
		sql ="select * from {prefix}Labels where LabelID="&id
		dim rs : set rs = conn.exec(sql,"r1")
		if rs.eof then 
			alertMsgAndGo "没有这条记录","-1"
		else
			LabelID=rs("LabelID")
			LabelName=rs("LabelName")
			LabelDesc=rs("LabelDesc")
			LabelContent=rs("LabelContent")
			Content=rs("LabelContent")
		end if
		rs.close : set rs=nothing
	else		
		alertMsgAndGo "没有这条记录","-1"
	end if 
End Sub


Sub addLabel 	
	LabelName=getForm("LabelName","post")
	LabelDesc=getForm("LabelDesc","post")	
	LabelContent=getForm("content","post")	
	
	if isnul(LabelName) then alertMsgAndGo "标签名称不能为空，请修改","-1"
	
	Dim rsObj	:	Set rsObj=conn.Exec("select count(*) from {prefix}Labels where LabelName='"&LabelName&"'","r1")
	if rsObj(0) >0 then alertMsgAndGo "标签名称已存在，请修改","-1"	
	rsObj.close	:	Set rsObj = nothing
	

	conn.Exec "insert into {prefix}Labels(LabelName,LabelDesc,LabelContent) values('"&LabelName&"','"&LabelDesc&"','"&LabelContent&"')","exe"		
	alertMsgAndGo "修改成功","AspCms_Label.asp"
End Sub	

Sub editLabel
	LabelID=getForm("LabelID","post")
	LabelName=getForm("LabelName","post")
	LabelDesc=getForm("LabelDesc","post")	
	LabelContent=getForm("content","post")		
	
	if isnul(LabelName) then alertMsgAndGo "标签名称不能为空，请修改","-1"
	
	Dim rsObj	:	Set rsObj=conn.Exec("select count(*) from {prefix}Labels where LabelName='"&LabelName&"' and LabelID<>"&LabelID,"r1")
	if rsObj(0) >0 then alertMsgAndGo "标签名称已存在，请修改","-1"	
	rsObj.close	:	Set rsObj = nothing
	
	sql="update {prefix}Labels set LabelName='"&LabelName&"', LabelDesc='"&LabelDesc&"', LabelContent='"&LabelContent&"' where LabelID="&LabelID
	conn.Exec sql,"exe"		
	alertMsgAndGo "修改成功","AspCms_Label.asp"
End Sub

Sub LabelList
	Dim rsObj	:	Set rsObj=conn.Exec("select LabelID,LabelName,LabelDesc from {prefix}Labels Order by LabelID","r1")
	If rsObj.Eof Then 
		echo"<tr bgcolor=""#FFFFFF"" align=""center"">"&vbcrlf& _
			"<td colspan=""5"">没有数据</td>"&vbcrlf& _
		  "</tr>"&vbcrlf
	Else
		Do while not rsObj.Eof 
		 echo"<tr bgcolor=""#FFFFFF"" align=""center"" onMouseOver=""this.bgColor='#CDE6FF'"" onMouseOut=""this.bgColor='#FFFFFF'"">"&vbcrlf& _
		 	"<td height=""28""><input type=""checkbox"" name=""id"" value="""&rsObj(0)&""" class=""checkbox"" /><input type=""hidden"" name=""LabelIDs"" value="""&rsObj(0)&""" /></td>"&vbcrlf& _
			"<td>"&rsObj(0)&"</td>"&vbcrlf& _
			"<td>"&rsObj(1)&"</td>"&vbcrlf& _
			"<td> {label:"&lcase(rsObj(1))&"}</td>"&vbcrlf& _
			"<td>"&rsObj(2)&"</td>"&vbcrlf& _
			"<td><a href=""AspCms_LabelEdit.asp?id="&rsObj(0)&""">修改</a> <a href=""?action=del&id="&rsObj(0)&"""  onClick=""return confirm('确定要删除吗')"">删除</a></td>"&vbcrlf& _
		  "</tr>"&vbcrlf
		  rsObj.MoveNext
		Loop
	End If
	rsObj.close	:	Set rsObj = nothing
End Sub

Sub delLabel 
	dim ID : ID = getForm("id","both")
	conn.Exec "Delete * from {prefix}Labels where LabelID in("&ID&")","exe"
	alertMsgAndGo "删除成功","AspCms_Label.asp"	
End Sub 

%>