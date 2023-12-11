<!--#include file="AspCms_AdminGroupFun.asp" -->
<%CheckAdmin("AspCms_AdminGroupList.asp")%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=addg" method="post" >
<DIV class=formzone>
<DIV class=namezone>添加管理员组</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
  <TR>
    <TD align=middle width=100 height=30>组名称</TD>
    <TD height=30><INPUT class="input" style="FONT-SIZE: 12px; WIDTH: 300px" maxLength="200" name="GroupName"/> <FONT color=#ff0000>*</FONT> </TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>组描述</TD>
    <TD height=30><TEXTAREA class="textarea" style="FONT-SIZE: 12px; WIDTH: 450px" name="GroupDesc" rows="3"></TEXTAREA> 
</TD>
</TR>
  <TR>
    <TD align=middle width=100 height=30>组状态</TD>
    <TD height=30><INPUT class="checkbox" type="checkbox" checked="checked" name="GroupStatus" value="1"/></TD></TR>
  <TR>
    <TD align=middle width=100 height=30>组权限值</TD>
    <TD height=30><input class="input" size="11"  value="99" name="GroupMark"/>    
    </TD></TR>
  <TR>
    <TD align=middle width=100 height=30>排序</TD>
    <TD height=30><input class="input" size="11" value="9" name="GroupOrder" /></TD></TR>
  <TR>
    <TD align=middle height=30>组权限</TD>
    <TD height=30>
<%
dim rs,subrs
set rs=conn.exec("select MenuID, MenuName, (select Count(*) from {prefix}Menu as b where MenuStatus=1 and b.ParentID=a.MenuID ) from {prefix}Menu as a where MenuStatus=1 and ParentID=0 order by MenuOrder","r1")


do while not rs.eof 
	echo "<INPUT class=""checkbox"" type=""checkbox"" name=""GroupMenu"" value="""&rs(0)&"""/><strong>"&rs(1)
	echo "</strong><br/>"
'	echo rs(0)&""& rs(1)&"<Br>&nbsp;&nbsp;"
	set subrs=conn.exec("select MenuID, MenuName from {prefix}Menu where MenuStatus=1 and ParentID="&rs(0)&" order by MenuOrder","r1")
		do while not subrs.eof
			echo "<INPUT class=""checkbox"" type=""checkbox"" name=""GroupMenu"" value="""&subrs(0)&"""/>"&subrs(1)
			
			subrs.moveNext
		loop	
	rs.moveNext
	echo "<br/>"
loop
%>
    
    </TD></TR></TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
<INPUT class="button" type="submit" value="添加" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
</DIV></DIV></FORM>

</BODY></HTML>