<!--#include file="AspCms_SortFun.asp" -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=quick" method="post" >
<DIV class=formzone>
<DIV class=namezone>批量添加分类</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=0 width="100%" align=center border=0>
  <TBODY>
  <TR id="hid7">
    <TD align=middle height=30> 浏览权限 </TD>
    <TD colspan="3">
    <%=userGroupSelect("GroupID", 0, 0)%>
        <input type="radio" name="Exclusive" value=">=" checked="checked" />
        隶属
        <input type="radio" name="Exclusive" value="=" /> 
        专属（隶属：权限值≥可查看，专属：权限值＝可查看）    </TD>
        </TR>	
  <TR class=list>			
    <TD align=middle width=81 height=30 class=biaoti>类型<font color=#ff0000>&nbsp;</font> </TD>
    <TD width="78" class=biaoti>排序</TD>
    <TD width="160" class=biaoti>顶级分类名称</TD>
    <TD width="571"class=biaoti>子分类(用&quot;分类1,分类2...&quot;这样表示多个分类)</TD>
  </TR>
<%
dim i 
for i=1 to 10
%>
  <TR>
    <TD align=middle width=81 height=30><%=makeSortTypeSelect("sortType"&i, 2, "onChange=""setInput(sortType.value)""")%></TD>  
    <TD><input class="input" maxlength="200" value="<%=i%>" style="WIDTH:60px" name="SortOrder<%=i%>"/></TD>
    <TD><input class="input" maxlength="200" style="WIDTH: 120px" name="TopSortName<%=i%>"/></TD>
    <TD><input class="input" maxlength="200" style="WIDTH: 90%" name="SubSortName<%=i%>"/></TD>
  </TR>
<%
next

%>
    </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
<INPUT class="button" type="submit" value="添加" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/>
</DIV>
</DIV></FORM>

</BODY></HTML>