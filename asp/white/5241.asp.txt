<!--#include file="AspCms_LanguageFun.asp" -->
<%getLanguage%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=edit" method="post" >
<DIV class=formzone>
<DIV class=namezone>修改语言</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
  <TR>						
    <TD align=middle width=100 height=30>语言名称</TD>
    <TD><INPUT class="input" style="WIDTH: 150px" maxLength="200" name="LanguageName" value="<%=LanguageName%>"/> <FONT color=#ff0000>*</FONT> </TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>别名</TD>
    <TD> <INPUT class="input" style="WIDTH: 150px" maxLength="200" name="Alias" value="<%=Alias%>"/> <FONT color=#ff0000>*</FONT>
</TD>
</TR>
  <TR>
    <TD align=middle width=100 height=30>网站目录</TD>
    <TD><INPUT class="input" style="WIDTH: 150px" maxLength="200" name="LanguagePath" value="<%=LanguagePath%>"/> <FONT color=#ff0000>*</FONT></TD></TR>
  <TR>
    <TD align=middle width=100 height=30>模板目录</TD>
    <TD><input class="input" style="WIDTH: 150px" size="200" name="defaultTemplate" value="<%=defaultTemplate%>"/> <FONT color=#ff0000>*</FONT>    
    </TD></TR>
  <TR>
    <TD align=middle width=100 height=30>排序</TD>
    <TD><input class="input" size="11"  name="LanguageOrder" value="<%=LanguageOrder%>"/> </TD></TR>
  <TR>
    <TD align=middle height=30>是否为默认语言</TD>
    <TD> <INPUT class="checkbox" type="checkbox" name="IsDefault" value="1" <% if IsDefault=1 then echo"checked=""checked"""%>/>  
    </TD>
    </TR>
  <TR>
    <TD align=middle height=30>是否启用</TD>
    <TD> <INPUT class="checkbox" type="checkbox" name="LanguageStatus" value="1" <% if LanguageStatus=1 then echo"checked=""checked"""%>/>  
    </TD>
    </TR>
  <TR>
    <TD align=middle height=30>html模板路径</TD>
    <TD><INPUT class="input" style="WIDTH: 150px" maxLength="200" name="htmlFilePath" value="<%=htmlFilePath%>"/> <FONT color=#ff0000>*</FONT>防模板下载
    </TD>
    </TR>
    
    </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
<INPUT type="hidden" name="LanguageID" value="<%=LanguageID%>" />
<INPUT class="button" type="submit" value="修改" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
</DIV></DIV></FORM>
</BODY></HTML>