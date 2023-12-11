<!--#include file="AspCms_MessageFun.asp" -->
<%getContent%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=edit&page=<%=page%>&order=<%=order%>&sort=<%=sortID%>&keyword=<%=keyword%>" method="post" >
<DIV class=formzone>
<DIV class=namezone>回复留言</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
<TBODY>
<TR>						
    <TD align=middle width=100 height=30>留言标题</TD>
    <TD><input class="input" name="FaqTitle" type="text" value="<%=FaqTitle%>" /></TD>
</TR>
<TR>
    <TD align=middle width=100 height=30>留言内容</TD>
    <TD><textarea class="textarea" name="Content" cols="40" rows="6" style="width:500px"><%=Content%></textarea></TD>
</TR>
<TR>
    <TD align=middle width=100 height=30>联系人</TD>
    <TD><input class="input" name="Contact" type="text" style="width:300px" value="<%=Contact%>" /></TD>
</TR>
<TR>
    <TD align=middle width=100 height=30>联系方式</TD>
    <TD> <input class="input" name="ContactWay" type="text" value="<%=ContactWay%>" /></TD>
</TR>
<TR>
    <TD align=middle width=100 height=30>回复</TD>
    <TD><textarea class="textarea" name="Reply" cols="40" rows="6" style="width:500px"><%=Reply%></textarea></TD>
</TR>
<TR>
    <TD align=middle width=100 height=30>公开</TD>
    <TD><input class="input" type="checkbox"  value="1" name="FaqStatus" <% if FaqStatus then echo"checked=""checked"""%>/></TD>
</TR>
  
</TBODY>
</TABLE>
</DIV>
<DIV class=adminsubmit>
<input type="hidden"  name="faqID" value="<%=faqID%>"/>
<INPUT class="button" type="submit" value="修改" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
</DIV></DIV></FORM>

</BODY></HTML>    

            
