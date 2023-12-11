<!--#include file="../../../inc/AspCms_MainClass.asp" -->
<%
CheckAdmin("AspCms_Scene.asp")
dim action : action=getForm("action","get")
Select case action	
	case "add" : addScene
	case "edit" : editScene
	case "del" : delScene
	case "on" : onOff "on", "Scene", "SceneID", "SceneStatus", "", getPageName()
	case "off" : onOff "off", "Scene", "SceneID", "SceneStatus", "", getPageName()

End Select

dim SceneID, SceneName, SceneMenu, SceneOrder, SceneDesc, SceneStatus
dim sql, msg

Sub getScene
	dim id : id=getForm("id","get")
	if not isnul(ID) then		
		sql ="select * from {prefix}Scene where SceneID="&id
		dim rs : set rs = conn.exec(sql,"r1")
		if rs.eof then 
			alertMsgAndGo "没有这条记录","-1"
		else
			SceneID=rs("SceneID")
			SceneName=rs("SceneName")
			SceneMenu=rs("SceneMenu")
			SceneOrder=rs("SceneOrder")
			SceneDesc=rs("SceneDesc")
			SceneStatus=rs("SceneStatus")
		end if
		rs.close : set rs=nothing
	else		
		alertMsgAndGo "没有这条记录","-1"
	end if 
End Sub

Sub sceneList
	sql="select SceneID, SceneName, SceneDesc, SceneOrder, SceneStatus from {prefix}Scene order by SceneOrder ,SceneID"
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
			"<TD>"&getStr(rs(4),"<a href=""?action=off&id="&rs(0)&""" title=""启用"" ><IMG src=""../../images/toolbar_ok.gif""></a>","<a href=""?action=on&id="&rs(0)&""" title=""禁用"" ><IMG src=""../../images/toolbar_no.gif""></a>")&"</TD>"&vbcrlf& _			
			"<TD><a href=""AspCms_SceneEdit.asp?id="&rs(0)&""">修改</a> <a href=""?action=del&id="&rs(0)&""" onClick=""return confirm('确定要删除吗')"">删除</a></TD>"&vbcrlf& _
			"</TR>"&vbcrlf
			rs.moveNext
		loop
	end if	
	rs.close : set rs=nothing
End Sub

Sub addScene
	SceneID=getForm("SceneID", "post")
	SceneName=getForm("SceneName", "post")
	SceneMenu=getForm("SceneMenu", "post")
	SceneOrder=getForm("SceneOrder", "post")
	SceneDesc=getForm("SceneDesc", "post")
	SceneStatus=getCheck(getForm("SceneStatus", "post"))
	
	if isnul(SceneName) then alertMsgAndGo "场景名称不能为空","-1"
	if not isnum(SceneOrder) then SceneOrder=9
	
	sql="insert into {prefix}Scene(SceneName, SceneMenu, SceneDesc, SceneOrder, SceneStatus) values('"&SceneName&"', '"&SceneMenu&"', '"&SceneDesc&"', "&SceneOrder&", "&SceneStatus&")"
	'echo sql
	conn.exec sql, "exe"
	alertMsgAndGo "添加成功", "AspCms_Scene.asp"
	
End Sub

Sub editScene
	SceneID=getForm("SceneID", "post")
	SceneName=getForm("SceneName", "post")
	SceneMenu=getForm("SceneMenu", "post")
	SceneOrder=getForm("SceneOrder", "post")
	SceneDesc=getForm("SceneDesc", "post")
	SceneStatus=getCheck(getForm("SceneStatus", "post"))
	
	if isnul(SceneName) then alertMsgAndGo "场景名称不能为空","-1"
	if not isnum(SceneOrder) then SceneOrder=9
	
	sql="update {prefix}Scene set SceneName='"&SceneName&"', SceneMenu='"&SceneMenu&"', SceneDesc='"&SceneDesc&"', SceneOrder="&SceneOrder&", SceneStatus="&SceneStatus&" where SceneID="&SceneID
	'echo sql
	conn.exec sql, "exe"
	alertMsgAndGo "修改成功", "AspCms_Scene.asp"
	
End Sub


Sub delScene
	dim id	:	id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"
	conn.exec "delete * from {prefix}Scene where SceneID in("&id&")","exe"
	alertMsgAndGo "删除成功",getPageName()		
End Sub
%>