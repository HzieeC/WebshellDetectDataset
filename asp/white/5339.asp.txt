<!--#include file="AspCms_CollectFun.asp" -->
<%CheckLogin()%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE>数据采集</TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312"><LINK 
href="style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR>
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

function MM_jumpMenu(targ,selObj,restore){ //v3.0
eval(targ+".location='?action=Scollect&Collectid="+selObj.options[selObj.selectedIndex].value+"'");
if (restore) selObj.selectedIndex=0;
}
</SCRIPT></HEAD>
<BODY>

<FORM name="" action="?" method="post">
<DIV class=searchzone>
<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>&nbsp;</TD>
    <TD align=right colSpan=2><span class="piliang">
      <INPUT class=button type="button" value=采集规则管理 name=cc2 onClick="window.location.href='AspCms_Collect.asp'">
    </span></TD>
  </TR></TBODY></TABLE></DIV>
<DIV class=tablezone>
<DIV class=searchzone>
<DIV class=namezone>1.采集规则编写</FONT>  → 2.链接过滤  →  3.文章采集储存  →  <FONT color=#ff0000>4.文章归类发布</FONT></DIV>
</DIV><TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>
      关键词：
        <INPUT class="input" size="16" name="keyword" value="<%=keyword%>">
      <INPUT class="button" type="submit" value="搜索" name="Submit" onClick="form.action='?<%="&keyword="&keyword&"&page=&psize="&psize&"&order="&order&"&ordsc="&ordsc&""%>';">
      <INPUT onClick="location.href='<%=getPageName()%>?action=Ctoo'" type="button" value="全部" class="button" /></TD>
    <TD align=right colSpan=2>&nbsp;</TD>
  </TR></TBODY></TABLE>
<DIV class=listzone>
<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD width=47 align="center" class=biaoti>选择</TD>
    <TD width=46 align="center" class=biaoti>编号</TD>
    <TD height=28 align="center" class=biaoti><span class="searchzone">标题</span></TD>
    <TD width=134 align="center" class=biaoti>采集时间</TD>
    <TD width=134 align="center" class=biaoti><span class="searchzone">操作</span></TD>
    </TR>
	<%Ctoolist%>
    </TBODY></TABLE>
</DIV>
<DIV class="piliang">
<INPUT onClick="SelAll(this.form)" type="checkbox" value="1" name="SELALL"> 全选&nbsp;
<INPUT class="button" type="submit" value="删除" onClick="if(confirm('确定要删除吗')){form.action='?action=contentdel<%="&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""%>';}else{return false};"/>  

 发布到:
 <%LoadSelect "moveSortID","Aspcms_Sort","SortName","SortID",getForm("id","get"), 0," and SortType<>1 and SortType<>7 order by SortOrder","请选择分类"%>
 <label>
 <input type="checkbox" name="isdel" id="isdel" value="del">
 发布完成删除临时内容
 <input class="button" type="submit" value="发布" onClick="form.action='?action=addContent<%="&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""%>';"/>
 </label>
</DIV></DIV>
</FORM>

</BODY></HTML>
