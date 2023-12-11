<!--#include file="AspCms_SceneFun.asp" -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=add" method="post" >
<DIV class=formzone>
<DIV class=namezone>添加新场景</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
  <TR>						
    <TD align=middle width=100 height=30>场景名称</TD>
    <TD><INPUT class="input" style="WIDTH: 150px" maxLength="200" name="SceneName"/> <FONT color=#ff0000>*</FONT> </TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>描述</TD>
    <TD><TEXTAREA class="textarea" style="FONT-SIZE: 12px; WIDTH: 450px" name="SceneDesc" rows="3"></TEXTAREA>
</TD></tr>

  <TR>
    <TD align=middle width=100 height=30>排序</TD>
    <TD><input class="input" size="11"  value="9" name="SceneOrder"/> </TD></TR>
  <TR>
  <TR>
    <TD align=middle height=30>状态</TD>
    <TD> <INPUT class="checkbox" type="checkbox" checked="checked" name="SceneStatus" value="1"/>  
    </TD>
    </TR>
  <TR>
    <TD align=middle height=30>场景菜单</TD>
    <TD>
    <%
dim rs,subrs
set rs=conn.exec("select MenuID, MenuName, (select Count(*) from {prefix}Menu as b where MenuStatus=1 and b.ParentID=a.MenuID ) from {prefix}Menu as a where MenuStatus=1 and ParentID=0 order by MenuOrder","r1")


do while not rs.eof 
	echo "<INPUT class=""checkbox"" type=""checkbox"" name=""SceneMenu"" value="""&rs(0)&"""/><strong>"&rs(1)
	echo "</strong><br/>"
'	echo rs(0)&""& rs(1)&"<Br>&nbsp;&nbsp;"
	set subrs=conn.exec("select MenuID, MenuName from {prefix}Menu where MenuStatus=1 and ParentID="&rs(0)&" order by MenuOrder","r1")
		do while not subrs.eof
			'echo subrs(0)&""& subrs(1)&"  "
			echo "<INPUT class=""checkbox"" type=""checkbox"" name=""SceneMenu"" value="""&subrs(0)&"""/>"&subrs(1)
			
			subrs.moveNext
		loop	
	rs.moveNext
	echo "<br/>"
loop
%>

    
    </TD>
    </TR>
    
    </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
<INPUT class="button" type="submit" value="添加" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
</DIV></DIV></FORM>

</BODY></HTML>