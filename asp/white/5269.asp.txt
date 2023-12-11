<!--#include file="AspCms_UserGroupFun.asp" -->
<%CheckAdmin("AspCms_UserList.asp")%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=add" method="post" >
<DIV class=formzone>
<DIV class=namezone>添加用户</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
  <TR>
    <TD align=middle width=100 height=30>用户组</TD>
    <TD height=30><%=userGroupSelect("GroupID","","0 and GroupID<>2")%> <FONT color=#ff0000>*</FONT></TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>用户名称</TD>
    <TD height=30><INPUT class="input" style="FONT-SIZE: 12px; WIDTH: 300px" maxLength="200" name="LoginName"/> <FONT color=#ff0000>*</FONT> </TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>用户密码</TD>
    <TD height=30><INPUT  type="Password" class="input" style="FONT-SIZE: 12px; WIDTH: 300px" maxLength="200" name="Password"/> <FONT color=#ff0000>*</FONT> </TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>状态</TD>
    <TD height=30><INPUT class="checkbox" type="checkbox" name="UserStatus" checked="checked" value="1"/></TD></TR></TBODY></TABLE>
</DIV>
<DIV class=adminsubmit><INPUT class="button" type="submit" value="添加" />  
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
</DIV>
</DIV>
</FORM>
</BODY>
</HTML>