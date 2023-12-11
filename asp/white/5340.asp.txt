<!--#include file="AspCms_CollectFun.asp" -->
<%CheckLogin()%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<SCRIPT>
function SelAll(theForm){
		for ( i = 0 ; i < theForm.elements.length ; i ++ )
		{
			if ( theForm.elements[i].type == "checkbox" && theForm.elements[i].name != "SELALL" )
			{
				theForm.elements[i].checked = ! theForm.elements[i].checked ;
			}
		}
}
</SCRIPT>
<BODY>
<FORM name="" action="?" method="post">
<DIV class=searchzone>
<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>&nbsp;</TD>
    <TD align=right colSpan=2><span class="piliang">
      <INPUT class=button type=button value=采集发布管理 name=cc2 onClick="window.location.href='AspCms_CollectContentlist.asp'">
      <INPUT class=button type=submit value=添加新规则 name=cc2 onClick="form.action='AspCms_Collectlist.asp?action=reset';">
    </span></TD>
  </TR></TBODY></TABLE></DIV>
<DIV class=listzone>
<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD width=54 class=biaoti align="center">选择</TD>
    <TD width=61 align="center" class=biaoti>编号</TD>
    <TD width="599" height=28 align="center" class=biaoti>采集规则名称</TD>
    <TD width=245 align="center" class=biaoti>操作</TD>
    </TR>
	<%CollecttodoList%>
    </TBODY></TABLE>
</DIV>
<DIV class="piliang">
<INPUT onClick="SelAll(this.form)" type="checkbox" value="1" name="SELALL"> 全选&nbsp;
<INPUT class="button" type="submit" value="删除" onClick="if(confirm('确定要删除吗')){form.action='?action=del<%="&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""%>';}else{return false};"/>
</DIV>
</FORM>
</BODY></HTML>
