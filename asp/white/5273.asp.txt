<!--#include file="AspCms_UserGroupFun.asp" -->
<%CheckAdmin("AspCms_UserList.asp")%>
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

<FORM name="" action="?page=<%=page%>&order=<%=order%>&sort=<%=sortID%>&keyword=<%=keyword%>" method="post">
<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>用户名：<input type="text" name="keyword" value="<%=keyword%>" class="input" />&nbsp;&nbsp;<input type="submit" name="selectBtn" value="搜索" class="button"/> <input class="button" type="button" name="selectBtn" value="全部"  onclick="location.href='?'" /></TD>
    <TD align=right colSpan=2><INPUT onClick="location.href='AspCms_UserAdd.asp'" type="button" value="添加用户" class="button" > <INPUT onClick="location.href='<%=getPageName()%>'" type="button" value="刷新" class="button" /> 
</TD></TR></TBODY></TABLE></DIV>
<DIV class=listzone>
<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD class=biaoti width=29>选择</TD>
    <TD class=biaoti width=38>编号</TD>
    <TD width="150" 
    height=28 class=biaoti><span class="searchzone">登录名</span></TD>
    <TD class=biaoti width=162><span class="searchzone">用户组</span></TD>
    <TD class=biaoti width=238 height=28><span class="searchzone">最后登录时间</span></TD>
    <TD class=biaoti width=91><span class="searchzone">最后登录IP</span></TD>
    <TD class=biaoti width=93><span class="searchzone">状态</span></TD>
    <TD class=biaoti width=141><span class="searchzone">操作</span></TD>
    </TR>
	<%userList%>
    </TBODY></TABLE>
</DIV>
<DIV class=piliang>
<INPUT onClick="SelAll(this.form)" type="checkbox" value="1" name="SELALL"> 全选&nbsp;
<INPUT class="button" type="submit" value="删除" onClick="if(confirm('确定要删除吗')){form.action='?action=del';}else{return false};"/>  
<INPUT class="button" type="submit" value="禁用" onClick="form.action='?action=off';"./>
<INPUT class="button" type="submit" value="启用" onClick="form.action='?action=on';"/>
</DIV>
</FORM>
</BODY></HTML>
