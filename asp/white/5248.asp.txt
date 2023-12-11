<!--#include file="AspCms_SceneFun.asp" -->
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
<DIV class=searchzone>

<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>&nbsp;</TD>
    <TD align=right colSpan=2><INPUT onClick="location.href='AspCms_SceneAdd.asp'" type="button" value="添加更多场景" class="button" > <INPUT onClick="location.href='<%=getPageName()%>'" type="button" value="刷新" class="button" /> 
</TD></TR></TBODY></TABLE></DIV>
<FORM name="" action="" method="post">
<DIV class=listzone>
<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD class=biaoti width=46>选择</TD>
    <TD class=biaoti width=58>编号</TD>
    <TD width="155" height=28 class=biaoti><span class="searchzone">场景名称</span></TD>
    <TD class=biaoti width=374><span class="searchzone">描述</span></TD>
    <TD class=biaoti width=59><span class="searchzone">排序</span></TD>
    <TD class=biaoti width=95><span class="searchzone">状态</span></TD>
    <TD class=biaoti width=140><span class="searchzone">操作</span></TD>
    </TR>
	<%sceneList%>
    </TBODY></TABLE>
</DIV>
<DIV class=piliang>
<INPUT onClick="SelAll(this.form)" type="checkbox" value="1" name="SELALL"> 全选&nbsp;
<INPUT class="button" type="submit" value="删除" onClick="if(confirm('确定要删除吗')){form.action='?action=del';}else{return false};"/>  
<INPUT class="button" type="submit" value="禁用" onClick="form.action='?action=off';"./>
<INPUT class="button" type="submit" value="启用" onClick="form.action='?action=on';"/>
<INPUT class="button" type="submit" value="更新排序" onClick="form.action='?action=order';"/>

</DIV>
</FORM>
</BODY></HTML>
