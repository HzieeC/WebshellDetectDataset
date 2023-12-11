<!--#include file="inc/AspCms_SettingClass.asp" -->
<%
CheckLogin()
dim action : action=getForm("action","get")
if action="editPass" then editPass
Sub editPass
	dim LoginName,PassWord,rePassword
	LoginName=getForm("LoginName","post")	
	PassWord=getForm("PassWord","post")	
	rePassword=getForm("rePassword","post")		

	if isnul(PassWord) then alertMsgAndGo "密码不能为空","-1"
	if rePassword<>Password then alertMsgAndGo "两次输入密码不相同","-1" 
	'if isnul(RealName) then alertMsgAndGo "管理员姓名不能为空","-1"	
	conn.Exec "update {prefix}User set [Password]='"&md5(PassWord,16)&"' where LoginName='"&LoginName&"'","exe"	
	alertMsgAndGo "修改成功","editPass.asp"
End Sub
%>


<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=editPass" method="post" >
<DIV class=formzone>
<DIV class=namezone>修改密码</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
  <TR>						
    <TD align=middle width=100 height=30>登录名</TD>
    <TD><%=rCookie("adminName")%><INPUT type="hidden" value="<%=rCookie("adminName")%>" name="LoginName"/></TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>新密码</TD>    
    <TD><INPUT type="password" class="input" style="WIDTH: 200px" maxLength="200" name="Password"/> <FONT color=#ff0000>*</FONT></TD>
</tr>

  <TR>
    <TD align=middle width=100 height=30>确认密码</TD>
    <TD><input type="password" class="input" maxlength="200" style="WIDTH: 200px" name="rePassword"/> </TD></TR>
  <TR>
  
    </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
<INPUT class="button" type="submit" value="修改" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
</DIV></DIV></FORM>

</BODY></HTML>
