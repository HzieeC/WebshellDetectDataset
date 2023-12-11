<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="page_next.asp" -->
<%
'文章文件夹获取
set rs_1=server.createobject("adodb.recordset")
sql="select FolderName from web_Models_type where [id]=9"
rs_1.open(sql),cn,1,1
if not rs_1.eof then
Article_FolderName=rs_1("FolderName")
end if
rs_1.close
set rs_1=nothing%>

<% '搜索模块
act=request.querystring("act")
keywords=trim(request.form("keywords"))
if act="search" then
if keywords<>"" then
s_sql="select * from web_article_comment where [content] like '%"&keywords&"%'  and article_id<>0 order by [time] desc "
else
s_sql="select * from web_article_comment where [content] like '%"&keywords&"%' and article_id<>0 order by [time] desc"
end if
else
s_sql="select * from web_article_comment where [content] like '%"&keywords&"%' and article_id<>0 order by [time] desc "

end if 
%>
<script language="javascript">

//全选JS
function unselectall(){
if(document.form2.chkAll.checked){
document.form2.chkAll.checked = document.form2.chkAll.checked&0;
}
}
function CheckAll(form){
for (var i=0;i<form.elements.length;i++){
var e = form.elements[i];
if (e.Name != 'chkAll'&&e.disabled==false)
e.checked = form.chkAll.checked;
}
}
</script>
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
	  <th width="100%" height=25 class='tableHeaderText'>文章评论列表</th>
	
	<tr><td height="400" valign="top"  class='forumRow'><br>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" class='TipTitle'>&nbsp;√ 操作提示</td>
          </tr>
          <tr>
            <td height="30" valign="top" class="TipWords"><p>1、评论列表显示所有文章的评论，可对评论进行回复。</p>
              <p>2、批量删除评论后建议生成文章页和评论页才能看到更新。</p></td>
          </tr>
          <tr>
            <td height="10" ></td>
          </tr>
        </table>
	 <form name="form2" method="post" action="Comment_Del.asp?action=AllDel&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">
	
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="2">
          <tr>
                        <td width="2%" height="30" class="TitleHighlight">&nbsp;</td>
            <td width="3%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">编号</div></td>
            <td width="44%" height="30" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">评论内容</div></td>
            <td width="9%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">评论者</div></td>
            <td width="7%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">审核</div></td>
            <td width="20%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">评论时间</div></td>
            <td width="15%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">问题操作</div></td>
          </tr>
<% '文章列表模块
strFileName="comment_list.asp" 
pageno=20
set rs = server.CreateObject("adodb.recordset")
rs.Open (s_sql),cn,1,1
rscount=rs.recordcount
if not rs.eof and not rs.bof then
call showsql(pageno)
rs.move(rsno)
for p_i=1 to loopno
%>

          <tr >
            <td rowspan="2" class='forumRowHighLight'><div align="center"><input type="checkbox" name="Selectitem" id="Selectitem" value="<%=rs("id")%>"></div></td>		  
            <td   height="30" class='forumRowHighLight'><div align="center"><%=rs("id")%></div></td>
            <td class='forumRowHighLight' ><span style="color: #FF0000">文章：</span>
			
			<%
			set rst=server.createobject("adodb.recordset")
			sql="select [title],file_path from [article] where id="&rs("article_id")&""
			rst.open(sql),cn,1,1
			if not rst.eof and not rst.bof then
			response.write "<a href='/"&Article_FolderName&"/"&rst("file_path")&"' target='_blank'>"&rst("title")&"</a>"
			end if
			rst.close
			set rst=nothing
			%>&nbsp;<%if rs("recontent")<>"" then%>&nbsp;[已回复]<%else%>&nbsp;[<span style="color: #FF0000">未回复</span>]<% end if%></td>
            <td class='forumRowHighLight' ><div align="center"><%=rs("name")%>
            </div></td>
            <td class='forumRowHighLight'><div align="center"><a href="comment_view_yes.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>"><%if rs("view_yes")=1 then%>已审核<%else%><span style="color: #FF0000">未审核</span><% end if%></a></div></td>
            <td class='forumRowHighLight' ><div align="center"><%=rs("time")%></div></td>
            <td class='forumRowHighLight' >
            <div align="center"><a href="comment_reply.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">回复</a> | <a href="javascript:if(ask('警告：删除后将不可恢复，可能造成意想不到后果？')) location.href='comment_del.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>';">删除</a></div>				</td>
          </tr>
		            <tr >
            <td   height="50" class='forumRow'>&nbsp;</td>
            <td colspan="5" valign="top" class='forumRow'  style="line-height:180%"><span style="color: #FF0000">评论：</span><%=rs("content")%></td>
          </tr>

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
		          <tr  >
		            <td height="35"  colspan="9" >&nbsp;<input name='chkAll' type='checkbox' id='chkAll' onclick='CheckAll(this.form)' value='checkbox'>
                    全选/全不选&nbsp;<input type="submit" name="Submit" value="删除选中"></td>
          </tr>
		    <tr  >
		
              <td height="35"  colspan="6" ><div align="center">
                <%call showpage(strFileName,rscount,pageno,false,true,"")%>
           </div></td>
		    </tr>
      </table></form>	
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="20" class='forumRow'>&nbsp;</td>
          </tr>
          <tr>
            <td height="25" class='forumRowHighLight'>&nbsp;| 评论内容搜索</td>
          </tr>
          <tr>
            <td height="70"><form name="form1" method="post" action="?act=search">
                <div align="center">
               
                  <label>
                    <input name="keywords" type="text"  size="35" maxlength="40">
                  </label>
                  <label> &nbsp;
                    <input type="submit" name="Submit" value="搜 索">
                  </label>
                </div>
            </form></td>
          </tr>
        </table>
	    <br></td>
	</tr>
	</table>

<%
Call DbconnEnd()
 %>