<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="page_next.asp" -->

<% '搜索模块
act=request.querystring("act")
keywords=trim(request.form("keywords"))
if act="search" then
if keywords<>"" then
s_sql="select * from web_models_type where [name] like '%"&keywords&"%' order by [time] desc"
else
s_sql="select * from web_models_type where [name] like '%"&keywords&"%' order by [time] desc"
end if
else
s_sql="select * from web_models_type where [name] like '%"&keywords&"%' order by [time] desc"

end if 
%>


<script language="JavaScript">
<!--
function ask(msg) {
	if( msg=='' ) {
		msg='警告：删除后将不可恢复，可能造成意想不到后果？';
	}
	if (confirm(msg)) {
		return true;
	} else {
		return false;
	}
}
//-->
</script>
	<%
Call header()
%>

	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>模板分类列表</th>
	
	<tr><td height="400" valign="top"  class='forumRow'><br>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" class='TipTitle'>&nbsp;√ 操作提示</td>
          </tr>
          <tr>
            <td height="30" valign="top" class="TipWords"><p>1、“模板分类”的意思：网站的每一个页面都是由模板生成的，首页有首页的模板，相册有相册页面模板，留言有留言页面的模板，这就构成了模板的分类。</p>
                <p>2、网站的所有模板都是放在根目录下的Templates文件夹内的，此文件夹内的每个一个文件夹即是一个网站主题存放模板的地方。</p>
                <p>3、“目标文件夹”的意思：每个模板都生成一个页面，这些页面放在哪呢，就是目标文件夹了，目标文件夹均是建立在根目录下的。 举例，www.hitux.com/blog/是博客页面，blog即是目标文件夹。</p>
                <p>4、首页面一般是放在根目录下，不需要目标文件夹，因此保持为空。</p>
                <p>5、网站根目录下会有许多系统文件夹，您命名的目标文件夹最好不要与这些文件夹重复，否则会产生很严重后果。</p>
                <p>6、为避免频繁的文件操作，已经命名的“模板文件名”是不能修改的，命名前请慎重，如果实在需要修改，只能打开数据库进行手动修改了。</p>
                <p>7、系统默认的8个模板分类都是网站必须的，非专业编程人员，删除请慎重，切记啊。</p>
                <p>8、目标文件夹做了修改后，之前的文件夹将会删除，需要手动生成才会生成新的页面到新文件夹。</p></td>
          </tr>
          <tr>
            <td height="10" ></td>
          </tr>
        </table>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" class='forumRowHighLight'>&nbsp;| <a href="Models_Type_Add.asp">添加新的模板分类</a></td>
          </tr>
          <tr>
            <td height="30"></td>
          </tr>
      </table>
	   
	  
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="2">
          <tr>
            <td width="7%" height="30" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">编号</div></td>
            <td width="15%" height="30" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">模板分类名称</div></td>
            <td width="14%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">目标文件夹</div></td>
            <td width="18%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">模板文件名</div></td>
            <td width="12%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">是否可用</div></td>
            <td width="20%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">添加时间</div></td>
            <td width="14%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">操作</div></td>
          </tr>
<% '文章列表模块
strFileName="Models_Type_list.asp" 
pageno=15
set rs = server.CreateObject("adodb.recordset")
rs.Open (s_sql),cn,1,1
rscount=rs.recordcount
if not rs.eof and not rs.bof then
call showsql(pageno)
rs.move(rsno)
for p_i=1 to loopno
%>
<% if p_i mod 2 =0 then
class_style="forumRow"
else
class_style="forumRowHighLight"
end if%>
            <form name="form1" method="post" action="?action=edit&id=<%=rs("id")%>">
          <tr >
            <td   height="40" class='<%=class_style%>'><div align="center"><%=rs("id")%></div></td>
           <td class='<%=class_style%>' ><div align="center"><%=rs("name")%></div></td>
            <td class='<%=class_style%>' ><div align="center"><%=rs("FolderName")%></div></td>
            <td class='<%=class_style%>' ><div align="center"><%=rs("FileName")%></div></td>
            <td class='<%=class_style%>' ><div align="center"><a href="Models_Type_view_yes.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>"><%if rs("view_yes")=1 then%>可用<%else%><span style="color: #FF0000">不可用</span><% end if%></a></div></td>
            <td class='<%=class_style%>' ><div align="center"><%=rs("time")%></div></td>
            <td class='<%=class_style%>' >
            <div align="center"><a href="Models_Type_edit.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">修改</a> | <a href="javascript:if(ask('警告：删除后将不可恢复，可能造成意想不到后果？')) location.href='Models_type_del.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>';">删除</a>            </div></td>
          </tr></form>
		  		  <%
		  rs.movenext
		  next
		  else
response.write "<div align='center'><span style='color: #FF0000'>暂无数据！</span></div>"
		  end if 
		  rs.close
		  set rs=nothing
		  %>
		    <tr  >
              <td height="35"  colspan="9" ><div align="center">
                <%call showpage(strFileName,rscount,pageno,false,true,"")%>
           </div></td>
		    </tr>
      </table>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="20" class='forumRow'>&nbsp;</td>
          </tr>
          <tr>
            <td height="25" class='forumRowHighLight'>&nbsp;| 模板分类搜索</td>
          </tr>
          <tr>
            <td height="70"><form name="form1" method="post" action="?act=search"><div align="center">&nbsp;
              <label>
                </label>
            <label>
<input name="keywords" type="text"  size="35" maxlength="40">
              </label>
                <label>
                       &nbsp;
                       <input type="submit" name="Submit" value="搜 索">
                </label>
              </div>
            </form>
            </td>
          </tr>
      </table>
	    <br></td>
	</tr>
	</table>

<%
Call DbconnEnd()
 %>