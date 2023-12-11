<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="page_next.asp" -->




<% '搜索模块
act=request.querystring("act")
keywords=trim(request.form("keywords"))
if act="search" then
if keywords<>"" then
s_sql="select * from web_article_comment  where [name] like '%"&keywords&"%' and article_id=0 order by time desc"
else
s_sql="select * from web_article_comment where  article_id=0 order by time desc"
end if
else
s_sql="select * from web_article_comment where  article_id=0 order by time desc"
end if

%>

<script type="text/javascript">
var prox;
var proy;
var proxc;
var proyc;
function show(id){/*--打开--*/
clearInterval(prox);
clearInterval(proy);
clearInterval(proxc);
clearInterval(proyc);
var o = document.getElementById(id);
o.style.display = "block";
o.style.width = "1px";
o.style.height = "1px"; 
prox = setInterval(function(){openx(o,700)},10);
} 
function openx(o,x){/*--打开x--*/
var cx = parseInt(o.style.width);
if(cx < x)
{
o.style.width = (cx + Math.ceil((x-cx)/5)) +"px";
}
else
{
clearInterval(prox);
proy = setInterval(function(){openy(o,500)},10);
}
} 
function openy(o,y){/*--打开y--*/ 
var cy = parseInt(o.style.height);
if(cy < y)
{
o.style.height = (cy + Math.ceil((y-cy)/5)) +"px";
}
else
{
clearInterval(proy); 
}
} 
function closeed(id){/*--关闭--*/
clearInterval(prox);
clearInterval(proy);
clearInterval(proxc);
clearInterval(proyc); 
var o = document.getElementById(id);
if(o.style.display == "block")
{
proyc = setInterval(function(){closey(o)},10); 
} 
} 
function closey(o){/*--打开y--*/ 
var cy = parseInt(o.style.height);
if(cy > 0)
{
o.style.height = (cy - Math.ceil(cy/5)) +"px";
}
else
{
clearInterval(proyc); 
proxc = setInterval(function(){closex(o)},10);
}
} 
function closex(o){/*--打开x--*/
var cx = parseInt(o.style.width);
if(cx > 0)
{
o.style.width = (cx - Math.ceil(cx/5)) +"px";
}
else
{
clearInterval(proxc);
o.style.display = "none";
}
} 

//function readyMove(e){ 
od.onmousedown = function(e){
odrag = this;
var e = e ? e : event;
if(e.button == (document.all ? 1 : 0))
{
mx = e.clientX;
my = e.clientY;
od.style.left = od.offsetLeft + "px";
od.style.top = od.offsetTop + "px";
if(isIE)
{
od.setCapture(); 
od.filters.alpha.opacity = 50;
}
else
{
window.captureEvents(Event.MOUSEMOVE);
od.style.opacity = 0.5;
}

//alert(mx);
//alert(my);

} 
}
document.onmousemove = function(e){
var e = e ? e : event;

//alert(mrx);
//alert(e.button); 
if(mouseD==true && odrag)
{ 
var mrx = e.clientX - mx;
var mry = e.clientY - my; 
od.style.left = parseInt(od.style.left) +mrx + "px";
od.style.top = parseInt(od.style.top) + mry + "px"; 
mx = e.clientX;
my = e.clientY;

}
}
</script>
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
	  <th width="100%" height=25 class='tableHeaderText'>留言列表</th>
	
	<tr><td height="400" valign="top"  class='forumRow'><br>
 <form name="form2" method="post" action="Message_Del.asp?action=AllDel&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">

	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="2">
          <tr>
            <td width="2%" height="30" class="TitleHighlight">&nbsp;</td>
            <td width="4%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">编号</div></td>
            <td width="32%" height="30" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">留言人</div></td>
            <td width="24%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">留言内容</div></td>
            <td width="8%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">显示/隐藏</div></td>
            <td width="17%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">留言时间</div></td>
            <td width="13%" class="TitleHighlight"><div align="center" style="font-weight: bold;color:#ffffff">留言操作</div></td>
          </tr>
<% '文章列表模块
strFileName="message_list.asp" 
pageno=20
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
          <tr >
            <td   height="30" class='<%=class_style%>'><div align="center"><input type="checkbox" name="Selectitem" id="Selectitem" value="<%=rs("id")%>"></div></td>
            <td   height="30" class='<%=class_style%>'><div align="center"><%=rs("id")%></div></td>
            <td class='<%=class_style%>' >&nbsp;<a href="#"><%=left(rs("name"),20)%></a><%if rs("recontent")<>"" then%>&nbsp;[已回复]<%else%>&nbsp;[<span style="color: #FF0000">未回复</span>]<% end if%></td>
            <td class='<%=class_style%>' >&nbsp;<%=left(rs("content"),15)%>...</td>
            <td class='<%=class_style%>' ><div align="center"><a href="message_view_yes.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>"><%if rs("view_yes")=1 then%>已显示<%else%><span style="color: #FF0000">已隐藏</span><% end if%></a></div></td>
            <td class='<%=class_style%>' ><div align="center"><%=rs("time")%></div></td>
            <td class='<%=class_style%>' >
            <div align="center"><a  href="#"   onclick = "show('fd_<%=rs("id")%>');return false;">查看</a> | <a href="message_reply.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">回复</a> | <a href="javascript:if(ask('警告：删除后将不可恢复，可能造成意想不到后果？')) location.href='message_del.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>';">删除</a></div>
			
				<div class="fd" id="fd_<%=rs("id")%>" style="display:none;filter:alpha(opacity=100);opacity:1;">
<div class="content_div1"><div class="div_close"><a href="#"  onclick = "closeed('fd_<%=rs("id")%>');return false;">
[关闭]
</a></div><table width="680" border="0" align="center" cellpadding="0" cellspacing="2">

  <tr>
    <td class='forumRowhighlight'  height="25" ><span style="font-weight: bold">留言人</span></td>
    <td class='forumRowhighlight' >&nbsp;<%=rs("name")%>&nbsp;(留言时间：<%=rs("time")%>)</td>
  </tr>

  <tr>
    <td class='forumRowhighlight'  height="25" ><span style="font-weight: bold">电子邮件</span></td>
    <td class='forumRowhighlight' >&nbsp;<%=rs("email")%></td>
  </tr>
   <tr>
    <td class='forumRowhighlight'  height="25"><span style="font-weight: bold">QQ</span></td>
    <td class='forumRowhighlight' >&nbsp;<%=rs("qq")%></td>
  </tr> 
    <tr>
    <td class='forumRow'>&nbsp;</td>
    <td class='forumRow'></td>
  </tr>
  <tr>
    <td valign="top"><span style="font-weight: bold">留言内容</span></td>
    <td valign="top"><label>
<div class="PostContent"><%=rs("content")%></div>
</label></td>
  </tr>
    <tr>
    <td class='forumRow'  >&nbsp;</td>
    <td class='forumRow'></td>
  </tr>
  <tr>
    <td  valign="top" class='forumRowhighlight' height="25"><span style="font-weight: bold">回复内容</span></td>
    <td  valign="top" class='forumRowhighlight'><%if rs("recontent")<>"" then%><label>
<div class="PostReply"><%=rs("recontent")%></div>
</label>
<%else%>
<span style='color: #FF0000'>暂无回复！</span>&nbsp;&nbsp;<a href="message_reply.asp?id=<%=rs("id")%>&page=<%=page%>&act=<%=act%>&keywords=<%=keywords%>">立即回复>>></a>
<%end if%></td>
  </tr>
  <tr>
    <td>&nbsp;</td>
    <td>&nbsp;</td>
  </tr>
</table></div> 
</div>		</td>
          </tr>
		  
		  		  <%
		  rs.movenext
		  next
		  else
response.write "<div align='center'><span style='color: #FF0000'>暂无留言！</span></div>"
		  end if 
		  rs.close
		  set rs=nothing
		  %>
		          <tr  >
		            <td height="35"  colspan="9" >&nbsp;<input name='chkAll' type='checkbox' id='chkAll' onclick='CheckAll(this.form)' value='checkbox'>
                    全选/全不选&nbsp;<input type="submit" name="Submit" value="删除选中"></td>
          </tr>

		    <tr  >
              <td height="35"  colspan="7" ><div align="center">
                <%call showpage(strFileName,rscount,pageno,false,true,"")%>
           </div></td>
		    </tr>
      </table> </form>
	    <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="20" class='forumRow'>&nbsp;</td>
          </tr>
          <tr>
            <td height="25" class='forumRowHighLight'>&nbsp;| 留言搜索</td>
          </tr>
          <tr>
            <td height="70"><form name="form1" method="post" action="?act=search">
              <div align="center"><label>
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