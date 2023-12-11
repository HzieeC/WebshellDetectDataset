<!--#include file="AspCms_FileMangerFun.asp" -->

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
<FORM name="" action="" method="post">
<DIV class=searchzone>

<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>&nbsp;</TD>
    <TD align=right colSpan=2>
      <INPUT onClick="location.href='?action=begin'" type="button" value="开始检测" class="button" >
      <INPUT onClick="location.href='<%=getPageName()%>'" type="button" value="刷新" class="button" /> 
</TD></TR></TBODY></TABLE>
</DIV>
<DIV class=listzone>
<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD width=47 align="center" class=biaoti>选择</TD>
    <TD width=58 align="center" class=biaoti>编号</TD> 	 	
    <TD width="210" align="center" class=biaoti><span class="searchzone">文件名称</span></TD>
    <TD width="154" height=28 align="center" class=biaoti><span class="searchzone">大小</span></TD>
    <TD width=157 align="center" class=biaoti><span class="searchzone">类型</span></TD>
    <TD width=153 align="center" class=biaoti><span class="searchzone">修改日期</span></TD>
    <TD width=141 align="center" class=biaoti><span class="searchzone">操作</span></TD>
    </TR>
<%
if action ="begin" then 
	Dim path
	path =sitePath&"/upLoad"	
	getAllFileList path,countnum
	if countnum=1 then echo "<tr bgcolor=""#ffffff"" align=""center"">"&vbcrlf& _
				"<td colspan=7 height=30>没有检测到冗余文件!</td>"&vbcrlf& _
				"</tr>"&vbcrlf		
end if

%>
    </TBODY></TABLE>
</DIV>
<DIV class=piliang>
<INPUT onClick="SelAll(this.form)" type="checkbox" value="1" name="SELALL"> 全选&nbsp;
<INPUT class="button" type="submit" value="批量删除" onClick="if(confirm('确定要删除吗')){form.action='?action=delallfile&dirpath=<%=dirpath%>';}else{return false};"/>
      <INPUT onClick="location.href='?action=begin'" type="button" value="开始检测" class="button" ></DIV>
</FORM>
</BODY></HTML>


