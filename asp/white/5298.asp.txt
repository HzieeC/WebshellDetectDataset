<!--#include file="AspCms_SpecFun.asp" -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=add" method="post" >
<DIV class=formzone>
<DIV class=namezone>添加参数</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
  <TR>						
    <TD align=middle width=100 height=30>参数名称</TD>
    <TD><INPUT class="input" style="WIDTH: 200px" maxLength="200" name="SpecName"/> <FONT color=#ff0000>*</FONT> </TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>字段名称</TD>    
    <TD><INPUT class="input" style="WIDTH: 200px" maxLength="200" name="SpecField"/> <FONT color=#ff0000>*</FONT></TD>
</tr>

  <TR>
    <TD align=middle width=100 height=30>排序</TD>
    <TD><input class="input" maxlength="6" value="9" style="WIDTH: 60px" name="SpecOrder"/> </TD></TR>
  <TR>
  
    </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
<INPUT class="button" type="submit" value="添加" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
<INPUT onClick="location.href='<%=getPageName()%>'" type="button" value="刷新" class="button" /> 
</DIV></DIV></FORM>

</BODY></HTML>