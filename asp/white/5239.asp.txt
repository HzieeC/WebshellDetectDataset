<!--#include file="inc/AspCms_SettingClass.asp" -->
<%CheckLogin()%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=GB2312" />
<title>Home</title>
<link href="css/css_body.css" rel="stylesheet" type="text/css" />
</head>

<body>
<div class="bodytitle">
	<div class="bodytitleleft"></div>
	<div class="bodytitletxt">后台导航</div>
</div>
<table width="100%" border="0" align="center" cellpadding="10" cellspacing="1" bgcolor="#cad9ea" >
		
<%
dim rs,subrs,sql

if rCookie("GroupMenu")="all" then
	sql="select MenuID, MenuName, MenuLink, (select Count(*) from {prefix}Menu as b where MenuStatus=1 and b.ParentID=a.MenuID ) from {prefix}Menu as a where MenuStatus=1 and ParentID=0 order by MenuOrder"
else
	sql="select MenuID, MenuName, MenuLink, (select Count(*) from {prefix}Menu as b where MenuStatus=1 and b.ParentID=a.MenuID ) from {prefix}Menu as a where MenuStatus=1 and ParentID=0 and MenuID in("&rCookie("GroupMenu")&") order by MenuOrder"
end if

set rs=conn.exec(sql,"r1")
do while not rs.eof 
%>
<tr>
		<td align="right" bgcolor="#f5fafe" class="main_bleft" width="150"><%=rs(1)%>：</td>
		<td bgcolor="#FFFFFF" class="main_link">
		<p><big><a href="menu.asp?id=<%=rs(0)%>" target="menu">[<%=rs(1)%>]</a></big>
        <%
if rCookie("GroupMenu")="all" then
	sql="select MenuID, MenuName, MenuLink from {prefix}Menu where MenuStatus=1 and ParentID="&rs(0)&" order by MenuOrder"
else
	sql="select MenuID, MenuName, MenuLink from {prefix}Menu where MenuStatus=1 and ParentID="&rs(0)&" and MenuID in("&rCookie("GroupMenu")&")"&" order by MenuOrder"
end if
		set subrs=conn.exec(sql,"r1")
		do while not subrs.eof	
			%>
			<a href="<%=subrs(2)%>"><%=subrs(1)%></a><small>|</small>
			<%
			subrs.moveNext
		loop	
		%>
		</p>
		</td>
	</tr>

<%
	rs.moveNext
loop

%>
	
</table>
</body>
</html>