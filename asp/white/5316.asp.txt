<!--#include file="AspCms_DataFun.asp" -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
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
<FORM name="" action="?"  method="post">
<DIV class=searchzone>

<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30></TD>
    <TD align=right colSpan=2><INPUT onClick="location.href='<%=getPageName()%>'" type="button" value="刷新" class="button" /> 
</TD></TR></TBODY></TABLE></DIV>
<DIV class=listzone>
<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD width=43  height=28 align="center" class=biaoti>选择</TD>
    <TD width=136 align="left" class=biaoti>编号</TD> 	 	
    <TD width="315"align="left" class=biaoti><span class="searchzone">文件名称</span></TD>
    <TD width="303" align="left" class=biaoti><span class="searchzone">备份时间</span></TD>
    <TD width=135 align="center" class=biaoti><span class="searchzone">操作</span></TD>
    </TR>
     <%databackList%>
    </TBODY></TABLE>
</DIV>
<DIV class=piliang>
<INPUT onClick="SelAll(this.form)" type="checkbox" value="1" name="SELALL"> 全选&nbsp;
<INPUT class="button" type="submit" value="删除" onClick="if(confirm('确定要删除吗')){form.action='?action=delall';}else{return false};"/>
<INPUT class="button" type="submit" value="开始备份" onClick="form.action='?action=bakup';"./>
<INPUT class="button" type="submit" value="压缩优化数据库" onClick="form.action='?action=compress';"./>
</DIV>
</FORM>
</BODY></HTML>









