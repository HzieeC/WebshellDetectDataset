<!--#include file="AspCms_StyleFun.asp" -->
<%CheckAdmin("AspCms_Template.asp")%>
<%getFile%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
	  <form action="?acttype=<%=acttype%>&action=edit" method="post" name="form">
<DIV class=formzone>
<DIV class=namezone>编辑<%=nametype%></DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
  <TR>						
    <TD align=middle width=100 height=30>文件名称</TD>
    <TD><%=filename%><input class="input" type="hidden" name="filename" value="<%=filename%>"/> <FONT color=#ff0000>*</FONT> </TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>文件内容</TD>    
    <TD> <textarea class="textarea" cols="80" rows="3" name="filetext" style=" height:400px; width:98%;" ><%=encodeHtml(filetext)%></textarea></TD>
</tr> 
  
    </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
<INPUT class="button" type="submit" value="修改" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
</DIV></DIV></FORM>
</BODY></HTML>

