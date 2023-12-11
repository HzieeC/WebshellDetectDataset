<!--#include file="conn.asp"-->
<!-- #include file="Inc/Head.asp" -->
<!--#include file="admin.asp"-->
<%owen=request("id")%>
<SCRIPT language=JavaScript>
var currentpos,timer;

function initialize()
{
timer=setInterval("scrollwindow()",50);
}
function sc(){
clearInterval(timer);
}
function scrollwindow()
{
currentpos=document.body.scrollTop;
window.scroll(0,++currentpos);
if (currentpos != document.body.scrollTop)
sc();
}
document.onmousedown=sc
document.ondblclick=initialize
</SCRIPT>
<% 
id=trim(request("id"))
Set rsnews=Server.CreateObject("ADODB.RecordSet") 
sql="select * from news where id="&owen
rsnews.Open sql,conn,1,1
title=rsnews("title")
if rsnews.eof and rsnews.bof then
response.Write("数据库出错")
else
%>
<table width="760" border="0" align="center" cellpadding="0" cellspacing="0">
  <tr> 
    <td height="72" valign="top" bgcolor="#FFFFFF"><table width="760" border="0" align="center" cellpadding="0" cellspacing="0">
        <tr> 

          <td valign="top"><table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
              <tr> 
                <td  height="206" valign="top"> <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0" bgcolor="#A4B6D7">
                    <tr> 
                      <td  height="26" colspan="2" align="center" class="back_southidc"><%= rsnews("title") %></td>
                    </tr>
                    <tr> 
                      <td width="40%" height="30" style="border-top: 1 solid #666666;border-bottom: 1 solid #666666">&nbsp;</td>
                      <td width="60%" align="center" style="border-top: 1 solid #666666;border-bottom: 1 solid #666666">发布者：<%= rsnews("user") %> 
                        发布时间：<%= rsnews("infotime") %> 阅读：<font color="#FF0000"><%= rsnews("hits") %></font>次</td>
                    </tr>
                    <tr> 
                      <td colspan="2"><br> <div style='font-size:10.5pt'><%=rsnews("content") %></div></td>
                    </tr>
                    <tr align="right"> 
                      <td colspan="2">&nbsp;</td>
                    </tr>
                    <tr align="right"> 
                      <td colspan="2">&nbsp;</td>
                    </tr>
                    <% 
		end if
		rsnews.close
		set rsnews=nothing
		%>
                  </table>
                  <br> <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
                    <tr> 
                      <td height="15" align="right"> <div align="center"><img src="Img/line.gif" width="100%" height="1"></div></td>
                    </tr>
                    <tr> 
                      <td height="15" align="right" valign="bottom">&nbsp;</td>
                    </tr>
                  </table>
                  <table width="95%" border="0" align="center" cellpadding="0" cellspacing="0">
				  <tr> 
                      <td class="back_southidc" height="31" bgcolor="#A4B6D7"><strong>相关评论：</strong></td>
                    </tr>
                    <tr> 
                      <td bgcolor="#A4B6D7"> <br> <%
dim rsComment
sql="select * from Comment where com_typeid=" & owen
Set rsComment= Server.CreateObject("ADODB.Recordset")
rsComment.open sql,conn,1,1
if rsComment.eof then
	response.write "&nbsp;&nbsp;&nbsp;&nbsp;暂时没有任何人发表评论"
else
%> 
                        <table width="100%" border="0" cellspacing="1" cellpadding="2" class="border" style="word-break:break-all">
                          <tr align="center" class="title"> 
                            <td width="30" height="22"><strong>ID</strong></td>
                            <td height="22"><strong>内容</strong></td>
                            <td width="60" height="22"><strong>评论人</strong></td>
                            <td width="120" height="22"><strong>评论时间</strong></td>
                            <td width="100" height="22"><strong>操作</strong></td>
                          </tr>
                          <%
	do while not rsComment.eof
%>
                          <tr class="tdbg"> 
                            <td width="30" align="center"><%= rsComment("com_ID") %></td>
                            <td> <div align="center"> 
                                <% response.write "<a href=# title='" & rsComment("com_content") & "'>" & left(rsComment("com_content"),25) & "</a>" %>
                              </div></td>
                            <td width="60" align="center"><%= rsComment("com_name") %></td>
                            <td width="120" align="center"><%= rsComment("com_date") %></td>
                            <td width="100" align="center"> <%			
			  response.write "<a href='AdminDelComment.asp?com_typeid="&owen&"&CommentID=" & rsComment("com_ID") & "'>删除</a>"
		  %> </td>
                          </tr>
                          <%		
		rsComment.movenext
	loop
%>
                        </table>
                        <%
end if
rsComment.close
set rsComment=nothing
%> </td>
                    </tr>
                    
                  </table>
                  <br>
                </td>
              </tr>
            </table></td>
        </tr>
      </table></td>
  </tr>
</table>
<!--#include file="inc/foot.asp"-->
