<!--#include file="../inc/AspCms_SettingClass.asp" -->

<%CheckAdmin("AspCms_plug.asp")%>
<%
dim action : action=getForm("action","get")
dim acttype : acttype=getform("acttype", "get")
dim filename,path,filetext,nametype

path=sitePath&"/plug"

select case action
	case "add" : plugadd
	case "del" : plugDel
end select

Sub getFile()
	filename=getForm("filename","get")	
	filetext=loadFile(path&filename)
End Sub

Sub plugadd
	dim plugkey,Menuname,MenuLink,plugArray,sql,Menunames,MenuLinks,i,plugpath
	plugkey=getForm("plugkey","get")
	Menuname=getForm("Menuname","get")
	MenuLink=getForm("MenuLink","get")
	plugpath=getForm("plugpath","get")
	if isnul(Menuname) then alertMsgAndGo"安装出错","-1"
	if isnul(plugkey) then alertMsgAndGo"安装出错","-1"
	if isnul(MenuLink) then alertMsgAndGo"安装出错","-1"
	plugArray=conn.Exec("select * from {prefix}Menu where MenuKey='"&plugkey&"'","arr")
	if isarray(plugArray) then
		if isnul(plugname) then alertMsgAndGo"已安装的模块或重复的插件标识","-1"
	else
	Menunames=split(Menuname,"|")
	MenuLinks=split(MenuLink,"|")
	for i=0 to ubound(Menunames)
	sql="insert into {prefix}Menu(ParentID, TopID, MenuName, MenuLink, MenuOrder, MenuLevel, MenuStatus,Menukey) values(67, 67, '"& Menunames(i)&"', '"&path & "/"& plugpath&"/"&MenuLinks(i)&"', 1, 2, 1,'"&plugkey&"')"
	conn.exec sql,"exe"
	next
	if rCookie("groupMenu")<>"all" then
	Dim rs :set rs =Conn.Exec ("select Menuid from {prefix}Menu where menukey='"& plugkey &"'","r1")
	IF rs.eof or rs.bof Then
		alertMsgAndGo"安装出错","-1"
	else
		Do While not rs.eof 
			sql="update {prefix}UserGroup set GroupMenu=GroupMenu & ', "& rs("Menuid") &"' where Groupid in (select Groupid from {prefix}user where loginname='"& rCookie("adminName") &"')"
			conn.exec sql,"exe"
			wCookie"groupMenu",rCookie("groupMenu")&", "&rs("Menuid")
		rs.MoveNext
		Loop
	end if
	end if
	echo "<script language='javascript'>alert('安装成功');window.location='AspCms_Plug.asp';if(window.navigator.userAgent.indexOf(""MSIE"")>=1){top.document.frames.menu.location = '../menu.asp?id=67';}else if(window.navigator.userAgent.indexOf(""Firefox"")>=1){top.document.getElementById('menu').src ='menu.asp?id=67';}else{top.document.getElementById('menu').src ='menu.asp?id=67';}</script>"
	end if
End Sub

Sub plugDel
	dim sql,delid,oldid,newid
	delid=""
	dim plugkey : plugkey=getForm("plugkey","both")
	if isnul(plugkey) then alertMsgAndGo "卸载出错","-1"
	if rCookie("groupMenu")<>"all" then
	Dim rs :set rs =Conn.Exec ("select GroupMenu from {prefix}UserGroup where Groupid in (select Groupid from {prefix}user where loginname='"& rCookie("adminName") &"')","r1")
		
		IF rs.eof or rs.bof Then
			alertMsgAndGo"卸载出错","-1"	
		else
			Do While not rs.eof 
				oldid=rs("GroupMenu")	
				rs.MoveNext		
			Loop
		end if		
	
	set rs =Conn.Exec ("select Menuid from {prefix}Menu where menukey='"& plugkey &"'","r1")
	IF rs.eof or rs.bof Then
		alertMsgAndGo"卸载出错","-1"
	else	
		Do While not rs.eof 
		 	oldid=replace(oldid,", "&rs("Menuid"),"")
			rs.MoveNext
		Loop
	end if
		  
		
	sql="update {prefix}UserGroup set GroupMenu='"&oldid&"' where Groupid in (select Groupid from {prefix}user where loginname='"& rCookie("adminName") &"')"
	conn.exec sql,"exe"
	wCookie"groupMenu",oldid
	end if	
	conn.exec "delete from {prefix}menu where MenuKey ='"&plugkey&"'","exe"	
	echo "<script language='javascript'>alert('卸载成功');window.location='AspCms_Plug.asp';if(window.navigator.userAgent.indexOf(""MSIE"")>=1){top.document.frames.menu.location = '../menu.asp?id=67';}else if(window.navigator.userAgent.indexOf(""Firefox"")>=1){top.document.getElementById('menu').src ='menu.asp?id=67';}else{top.document.getElementById('menu').src ='menu.asp?id=67';}</script>"
End Sub	


%>