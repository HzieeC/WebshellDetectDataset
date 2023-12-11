<!--#include file="AspCms_ApplyFun.asp" -->
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
<form action="?<%="sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""%>" method="post" name="form">
<DIV class=searchzone>

<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>申请人或联系电话：
      <input type="text" name="keyword" value="<%=keyword%>" class="input"/>&nbsp;&nbsp;  
      <INPUT class="button" type="submit" value="搜索" name="Submit" onClick="form.action='?<%="sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page=&psize="&psize&"&order="&order&"&ordsc="&ordsc&""%>';">
      <INPUT onClick="location.href='<%=getPageName()%>?<%="sortType="&sortType%>'" type="button" value="全部" class="button" /></TD>
    <TD align=right colSpan=2>
    <INPUT onClick="location.href='<%=getPageName()%>?<%="sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""%>'" type="button" value="刷新" class="button" /> 
</TD></TR></TBODY></TABLE></DIV>
<DIV class=listzone>
<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD width=37 align="center" class=biaoti>选择</TD>
    <TD width=36 align="center" class=biaoti>编号</TD>
    <TD width="174" height=28 align="center" class=biaoti><span class="searchzone">应聘职位</span></TD>
    <TD width=112 align="center" class=biaoti><span class="searchzone">申请人</span></TD>
    <TD width=49 align="center" class=biaoti><span class="searchzone">年龄</span></TD>
    <TD width=49 align="center" class=biaoti><span class="searchzone">性别</span></TD>
    <TD width=199 align="center" class=biaoti><span class="searchzone">联系电话</span></TD>
    <TD width=148 align="center" class=biaoti><span class="searchzone">申请时间</span></TD>
    <TD width=39 align="center" class=biaoti><span class="searchzone">状态</span></TD>
    <TD width=80 align="center" class=biaoti><span class="searchzone">操作</span></TD>
    </TR>
	<%ApplyList%>
    </TBODY></TABLE>
</DIV>
<DIV class="piliang">
<INPUT onClick="SelAll(this.form)" type="checkbox" value="1" name="SELALL"> 全选&nbsp;
<INPUT class="button" type="submit" value="删除" onClick="if(confirm('确定要彻底删除吗?\n\n此操作不可逆!')){form.action='?action=del<%="&sortType="&sortType&"&sortid="&sortid&"&keyword="&keyword&"&page="&page&"&psize="&psize&"&order="&order&"&ordsc="&ordsc&""%>';}else{return false};"/>  



</DIV>
</FORM>
</BODY></HTML>
