<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="page_next.asp" -->

	<%
Call header()
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
<% '搜索模块
act=request.querystring("act")
keywords=trim(request.form("keywords"))
cid=request("cid")
set rs_t=server.createobject("adodb.recordset")
sql_t="select Web_ThemeID from web_settings"
rs_t.open(sql_t),cn,1,1
Web_ThemeID=rs_t("Web_ThemeID")
rs_t.close
set rs_t=nothing

if act="search" then
if cid=""  then
s_sql="select * from web_models where [name]  like '%"&keywords&"%' and ModelTheme="&Web_ThemeID&"  order by [time] desc"
else
search_sql="and [ModelType]="&cid&""
s_sql="select * from web_models where [name] like '%"&keywords&"%'"&search_sql&" and ModelTheme="&Web_ThemeID&"  order by [time] desc"
end if

else
s_sql="select * from web_models where ModelTheme="&Web_ThemeID&" order by [time] desc"

end if

%>

	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>网站模板管理</th>
	
	<tr><td height="400" valign="top"  class='forumRow'><br>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" class='TipTitle'>&nbsp;√ 操作提示</td>
          </tr>
          <tr>
            <td height="30" valign="top" class="TipWords"><p>1、模板列表只会显示当前主题的所有模板列表，如果您要修改其它主题的模板，请先更换主题。</p></td>
          </tr>
          <tr>
            <td height="10" ></td>
          </tr>
        </table>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" class='forumRowHighLight'>&nbsp;| <a href="Web_Models_Add.asp">添加新的模板</a></td>
          </tr>
          <tr>
            <td height="30"></td>
          </tr>
        </table>

	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="2">
          <tr>
            <td width="7%" height="30" class="TitleHighlight"><div align="center" style="font-weight: bold; color: #FFFFFF">编号</div></td>
            <td width="18%" class="TitleHighlight"><div align="center" style="font-weight: bold; color: #FFFFFF">模板名称</div></td>
            <td width="23%" class="TitleHighlight"><div align="center" style="font-weight: bold; color: #FFFFFF">模板分类</div></td>
            <td width="19%" class="TitleHighlight"><div align="center" style="font-weight: bold; color: #FFFFFF">模板主题</div></td>
            <td width="18%" class="TitleHighlight"><div align="center" style="font-weight: bold; color: #FFFFFF">添加时间</div></td>
            <td width="15%" class="TitleHighlight"><div align="center" style="font-weight: bold; color: #FFFFFF">操作</div></td>
          </tr>
<% '文章列表模块
strFileName="web_models.asp" 
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
          <tr>
            <td height="28" class='<%=class_style%>'>
            <div align="center"><%=rs("id")%></div></td>
            <td class='<%=class_style%>'><div align="center"><%=rs("name")%></div></td>
            <td class='<%=class_style%>'><div align="center">
                <%
set rs_1=server.createobject("adodb.recordset")
sql="select [name],FileName,[id] from web_Models_type where [id]="&rs("ModelType")
rs_1.open(sql),cn,1,1
if not rs_1.eof then
response.write rs_1("name")&"<br>"&rs_1("FileName")
end if
rs_1.close
set rs_1=nothing
%>
            </div></td>
            <td class='<%=class_style%>'><div align="center">
            <%
set rs_1=server.createobject("adodb.recordset")
sql="select [name],[folder],[id] from web_Theme where [id]="&rs("ModelTheme")
rs_1.open(sql),cn,1,1
if not rs_1.eof then
response.write rs_1("name")&"<br>"&rs_1("folder")
end if
rs_1.close
set rs_1=nothing
%></div></td>
            <td class='<%=class_style%>'><div align="center"><%=rs("time")%></div></td>
            <td class='<%=class_style%>'>
            <div align="center"><a href="web_models_edit.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">修改</a><%
			if rs("id")>0 then%> | <a href="javascript:if(ask('警告：删除后将不可恢复，可能造成意想不到后果？')) location.href='web_models_del.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>';">删除</a>
              <%end if %></div></td>
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
              <td height="35"  colspan="8" ><div align="center">
                <%call showpage(strFileName,rscount,pageno,false,true,"")%>
           </div></td>
		    </tr>
      </table>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="20" class='forumRow'>&nbsp;</td>
          </tr>
          <tr>
            <td height="25" class='forumRowHighLight'>&nbsp;| 模板搜索</td>
          </tr>
          <tr>
            <td height="70"><form action="?act=search" method="post" name="form1" id="form1">
              <div align="center"><%
Dim count1,rsClass1,sqlClass1
set rsClass1=server.createobject("adodb.recordset")
sqlClass1="select id,name from web_models_type  order by time" 
rsClass1.open sqlClass1,cn,1,1
%>
            <select name="cid" id="cid" onChange="changeselect1(this.value)">
              <option value="">选择分类</option>
              <%
count1 = 0
do while not rsClass1.eof
response.write"<option value="&rsClass1("ID")&">"&rsClass1("Name")&"</option>"
count1 = count1 + 1
rsClass1.movenext
loop
rsClass1.close
%>
            </select>&nbsp;
                    <label> </label>
                    <label>
                    <input name="keywords" type="text"  size="35" maxlength="40" />
                    </label>
                    <label> &nbsp;
                    <input type="submit" name="Submit" value="搜 索" />
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