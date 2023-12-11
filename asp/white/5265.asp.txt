<!--#include file="../../inc/AspCms_SettingClass.asp" -->
<%
dim action : action=getForm("action","get")
Select case action		
	case "addg" : addAdminGroup
	case "editg" : editAdminGroup
	case "delg" : delAdminGroup
	case "ong" : onOff "on", "UserGroup", "GroupID", "GroupStatus", "and IsAdmin=1", getPageName()
	case "offg" : onOff "off", "UserGroup", "GroupID", "GroupStatus", "and IsAdmin=1", getPageName()
	
	case "add" : addAdmin
	case "edit" : editAdmin
	case "del" : delAdmin
	case "on" : onOff "on", "User", "UserID", "UserStatus", "", getPageName()
	case "off" : onOff "off", "User", "UserID", "UserStatus", "", getPageName()
	
End Select

dim GroupID, IsAdmin, GroupName, GroupDesc, GroupStatus, GroupMark, GroupMenu, GroupSort, GroupOrder
dim UserID, LanguageID, SceneID, LoginName, Password, PswQuestion, PswAnswer, UserStatus, RegTime, RegIP, LastLoginIP, LastLoginTime, LoginCount, TrueName, Gender, Birthday, Country, Province, City, Address, PostCode, Phone, Mobile, Email, QQ, MSN, Permissions, AdminDesc
dim sql, msg

Sub getAdminGroup
	dim id : id=getForm("id","get")
	if not isnul(ID) then		
		sql ="select * from {prefix}UserGroup where IsAdmin=1 and GroupID="&id
		dim rs : set rs = conn.exec(sql,"r1")
		if rs.eof then 
			alertMsgAndGo "没有这条记录","-1"
		else
			GroupID=rs("GroupID")
			IsAdmin=rs("IsAdmin")
			GroupName=rs("GroupName")
			GroupDesc=rs("GroupDesc")
			GroupStatus=rs("GroupStatus")
			GroupMark=rs("GroupMark")
			GroupMenu=rs("GroupMenu")
			GroupSort=rs("GroupSort")
			GroupOrder=rs("GroupOrder")
		end if
		rs.close : set rs=nothing
	else		
		alertMsgAndGo "没有这条记录","-1"
	end if
End Sub

Sub addAdminGroup	
	IsAdmin=1
	GroupName=getForm("GroupName","post")
	GroupDesc=getForm("GroupDesc","post")
	GroupStatus=getCheck(getForm("GroupStatus","post"))
	GroupMark=getForm("GroupMark","post")
	GroupMenu=getForm("GroupMenu","post")
	GroupSort=""
	GroupOrder=getForm("GroupOrder","post")
	
	if isNul(GroupName) then alertMsgAndGo"请填写组名称","-1"
	if not isNum(GroupMark) then alertMsgAndGo"请正确填写权限值","-1"
	if not isNum(GroupOrder) then alertMsgAndGo"请正确填写排序数字","-1"
		
	sql="insert into {prefix}UserGroup( IsAdmin, GroupName, GroupDesc, GroupStatus, GroupMark, GroupMenu, GroupSort, GroupOrder) values("&IsAdmin&", '"&GroupName&"', '"&GroupDesc&"', "&GroupStatus&", "&GroupMark&", '"&GroupMenu&"', '"&GroupSort&"', "&GroupOrder&")"
	msg="添加会员组成功"
	conn.exec sql,"exe"	
	alertMsgAndGo msg,"AspCms_AdminGroupList.asp"
End Sub

Sub editAdminGroup
	GroupID=getForm("GroupID","post")
	IsAdmin=1
	GroupName=getForm("GroupName","post")
	GroupDesc=getForm("GroupDesc","post")
	GroupStatus=getCheck(getForm("GroupStatus","post"))
	GroupMark=getForm("GroupMark","post")
	GroupMenu=getForm("GroupMenu","post")
	GroupSort=""
	GroupOrder=getForm("GroupOrder","post")
	
	if not isNum(GroupID) then alertMsgAndGo "组ID不正确！","-1"	
	if isNul(GroupName) then alertMsgAndGo"请填写组名称","-1"
	if not isNum(GroupMark) then alertMsgAndGo"请正确填写权限值","-1"
	if not isNum(GroupOrder) then alertMsgAndGo"请正确填写排序数字","-1"
	
	sql="update {prefix}UserGroup set GroupName='"&GroupName&"', GroupDesc='"&GroupDesc&"', GroupMenu='"&GroupMenu&"', GroupOrder="&GroupOrder&", GroupMark="&GroupMark&", GroupStatus="&GroupStatus&" where GroupID="&GroupID
	msg="修改会员组成功"
	conn.exec sql,"exe"	
	alertMsgAndGo msg,"AspCms_AdminGroupList.asp"
End Sub

Sub onAdminGroup

End Sub

Sub offAdminGroup

End Sub

Sub delAdminGroup	
	dim id	:	id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"
	dim ids,i
	ids=split(id,",")
	for i=0 to ubound(ids)
		if ids(i)>=4 then conn.exec "delete * from {prefix}UserGroup where IsAdmin=1 and GroupID="&ids(i),"exe"
	next
	alertMsgAndGo "删除成功",getPageName()		
End Sub



Sub adminGroupList
	sql="select * from {prefix}UserGroup where IsAdmin=1 order by GroupOrder ,GroupID"
	dim rs
	set rs=conn.exec(sql,"r1")
	if rs.eof then 
		echo "<TR class=list>"&vbcrlf& _
			"<TD colspan=""8"" align=""center"">没有记录</TD>"&vbcrlf& _
			"</TR>"&vbcrlf
	else
		do while not rs.eof 
			echo "<tr bgcolor=""#ffffff"" onMouseOver=""this.bgColor='#CDE6FF'"" onMouseOut=""this.bgColor='#FFFFFF'"">"&vbcrlf& _
			"<TD align=middle width=29 height=26><INPUT type=checkbox value="""&rs("GroupID")&""" name=""id""></TD>"&vbcrlf& _
			"<TD width=38 height=26>"&rs("GroupID")&"</TD>"&vbcrlf& _
			"<TD>"&rs("GroupName")&"</TD>"&vbcrlf& _
			"<TD width=371>"&rs("GroupDesc")&"</TD>"&vbcrlf& _
			"<TD width=88>"&getStr(rs("GroupStatus"),"<a href=""?action=offg&id="&rs("GroupID")&""" title=""启用""><IMG src=""../../images/toolbar_ok.gif""></a>","<a href=""?action=ong&id="&rs("GroupID")&""" title=""禁用"" ><IMG src=""../../images/toolbar_no.gif""></a>")&"</TD>"&vbcrlf& _
			"<TD width=73>"&rs("GroupMark")&"</TD>"&vbcrlf& _
			"<TD width=50>"&rs("GroupOrder")&"</TD>"&vbcrlf& _
			"<TD width=82><a href=""AspCms_AdminGroupedit.asp?id="&rs("GroupID")&""">修改</a> <a href=""?action=delg&id="&rs("GroupID")&""" onClick=""return confirm('确定要删除吗')"">删除</a></TD>"&vbcrlf& _
			"</TR>"&vbcrlf
			rs.moveNext
		loop
	end if	
	rs.close : set rs=nothing
End Sub

Sub getAdmin
	dim id : id=getForm("id","get")
	if not isnul(ID) then		
		sql ="select * from {prefix}User where UserID="&id
		dim rs : set rs = conn.exec(sql,"r1")
		if rs.eof then 
			alertMsgAndGo "没有这条记录","-1"
		else
			UserID=rs("UserID")
			GroupID=rs("GroupID")
			LoginName=rs("LoginName")
			UserStatus=rs("UserStatus")
			AdminDesc=rs("AdminDesc")
		end if
		rs.close : set rs=nothing
	else		
		alertMsgAndGo "没有这条记录","-1"
	end if
End Sub


Sub addAdmin	
	GroupID=getForm("GroupID","post")
	LoginName=getForm("LoginName","post")
	Password=getForm("Password","post")
	UserStatus=getCheck(getForm("UserStatus","post"))
	AdminDesc=getForm("AdminDesc","post")
	RegTime=now()
	RegIP=getIP()
	LoginCount=0
	Gender=1
		
	if isNul(LoginName) then alertMsgAndGo"请填写管理员名称","-1"
	if isNul(Password) then alertMsgAndGo"请填写管理员密码","-1"
	if conn.Exec("select count(*) from {prefix}User where LoginName='"&LoginName&"'","r1")(0) >0 then alertMsgAndGo "该用户名已存在","-1"
		
	sql="insert into {prefix}User( GroupID, LoginName, [Password], UserStatus, RegTime, RegIP, LoginCount, Gender, AdminDesc) values("&GroupID&", '"&LoginName&"', '"&md5(Password,16)&"', "&UserStatus&", '"&RegTime&"', '"&RegIP&"', "&LoginCount&", "&Gender&", '"&AdminDesc&"')"
	msg="添加管理成功"
	conn.exec sql,"exe"	
	alertMsgAndGo msg,"AspCms_AdminList.asp"
End Sub

Sub editAdmin
	UserID=getForm("UserID","post")
	GroupID=getForm("GroupID","post")
	LoginName=getForm("LoginName","post")
	Password=getForm("Password","post")
	UserStatus=getCheck(getForm("UserStatus","post"))
	AdminDesc=getForm("AdminDesc","post")
	RegTime=now()
	RegIP=getIP()
	LoginCount=0
	Gender=1	
	
	if not isNum(GroupID) then alertMsgAndGo "组ID不正确！","-1"	
	Dim passStr
	if isNul(Password) then 
		passStr=""
	else
		passStr=" , [Password]='"&md5(Password, 16)&"'"
	end if
	
	sql="update {prefix}User set GroupID="&GroupID&passStr&" where UserID="&UserID
	'die sql
	msg="修改成功"
	conn.exec sql,"exe"	
	alertMsgAndGo msg,"AspCms_AdminList.asp"
End Sub

Sub delAdmin	
	dim id	:	id=getForm("id","both")
	if isnul(id) then alertMsgAndGo "请选择要操作的内容","-1"
	dim ids,i
	ids=split(id,",")
	for i=0 to ubound(ids)
		if ids(i)>1 then conn.exec "delete * from {prefix}User where UserID="&ids(i),"exe"
	next	
	alertMsgAndGo "删除成功",getPageName()
End Sub

Sub adminList
	sql="select * from {prefix}User as a, {prefix}UserGroup as b where IsAdmin=1 and a.GroupID=b.GroupID order by a.UserID"
	dim rs
	set rs=conn.exec(sql,"r1")
	if rs.eof then 
		echo "<TR class=list>"&vbcrlf& _
			"<TD colspan=""8"" align=""center"">没有记录</TD>"&vbcrlf& _
			"</TR>"&vbcrlf
	else
		do while not rs.eof 
			echo "<tr bgcolor=""#ffffff"" onMouseOver=""this.bgColor='#CDE6FF'"" onMouseOut=""this.bgColor='#FFFFFF'"">"&vbcrlf& _
			"<TD align=middle height=26><INPUT type=checkbox value="""&rs("UserID")&""" name=""id""></TD>"&vbcrlf& _
			"<TD>"&rs("UserID")&"</TD>"&vbcrlf& _
			"<TD>"&rs("LoginName")&"</TD>"&vbcrlf& _
			"<TD>"&rs("GroupName")&"</TD>"&vbcrlf& _
			"<TD>"&rs("LastLoginTime")&"</TD>"&vbcrlf& _
			"<TD>"&rs("LastLoginIP")&"</TD>"&vbcrlf& _
			"<TD>"&getStr(rs("UserStatus"),"<a href=""?action=off&id="&rs("UserID")&""" title=""启用""><IMG src=""../../images/toolbar_ok.gif""></a>","<a href=""?action=on&id="&rs("UserID")&""" title=""禁用"" ><IMG src=""../../images/toolbar_no.gif""></a>")&"</TD>"&vbcrlf& _
			"<TD><a href=""AspCms_AdminEdit.asp?id="&rs("UserID")&""">修改</a> <a href=""?action=del&id="&rs("UserID")&""" onClick=""return confirm('确定要删除吗')"">删除</a></TD>"&vbcrlf& _
			"</TR>"&vbcrlf
			rs.moveNext
		loop
	end if	
	rs.close : set rs=nothing
End Sub


%>