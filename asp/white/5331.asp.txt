<!--#include file="AspCms_SpecFun.asp" -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<script type="text/javascript">
function checkAll(checkbox,name){
	var checkboxs = document.getElementsByName(name);
	for(var i=0;i<checkboxs.length;i++)checkboxs[i].checked = checkbox.checked;		
}
function silbingCheck(checkbox){
	checkbox.nextSibling.checked = checkbox.checked;
}
</script>
<BODY>
<FORM name="" action="" method="post">
<DIV class=searchzone>

<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>&nbsp;</TD>
    <TD align=right colSpan=2>
    <INPUT onClick="location.href='AspCms_SpecAdd.asp'" type="button" value="添加参数" class="button" >
    <INPUT onClick="location.href='../_Content/AspCms_ContentList.asp?sortType=3'" type="button" value="产品列表" class="button" > 
    <INPUT onClick="location.href='<%=getPageName()%>'" type="button" value="刷新" class="button" /> 
</TD></TR></TBODY></TABLE>
</DIV>
<DIV class=listzone>
<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD width=47 align="center" class=biaoti>选择</TD>
    <TD width=46 align="center" class=biaoti>编号</TD> 	 	
    <TD width="227" height=28 align="center" class=biaoti><span class="searchzone">参数名</span></TD>
    <TD width=97 align="center" class=biaoti><span class="searchzone">字段名称</span></TD>
    <TD width=78 align="center" class=biaoti><span class="searchzone">排序</span></TD>
    <TD width=185 align="center" class=biaoti><span class="searchzone">操作</span></TD>
    </TR>
	<%specList%>
    </TBODY></TABLE>
</DIV>
<DIV class=piliang>
<INPUT onClick="checkAll(this,'id'),checkAll(this,'SpecField')" type="checkbox" value="1" name="SELALL"> 全选&nbsp;
<INPUT class="button" type="submit" value="删除" onClick="if(confirm('确定要删除吗')){form.action='?action=del';}else{return false};"/> 
<INPUT class="button" type="submit" value="更新排序" onClick="form.action='?action=order';"/>

</DIV>
</FORM>
</BODY></HTML>
