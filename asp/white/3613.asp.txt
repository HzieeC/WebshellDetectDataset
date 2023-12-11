<%@ LANGUAGE="VBScript" %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<!--#include file="Inc/articlechar.inc"-->
<!-- #include file="Inc/Head.asp" -->
<script language=javascript>
function ConfirmDel()
{
   if(confirm("确定要删除此记录吗？"))
     return true;
   else
     return false;
}
</script>
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> 
      <table width="560" border="0" cellpadding="0" cellspacing="1" bgcolor="#CCCCCC" >
        <tr> 
          <td height="28" class="back_southidc"> 
            <div align="center"><strong>留言管理 <br>
              </strong></div></td>
        </tr>
        <tr> 
          <td> 
<div align="center"> 
<% 
const MaxPerPage=5 '分页显示的纪录个数 
dim sql 
dim rs 
dim totalPut 
dim CurrentPage 
dim TotalPages 
dim i,j 
 
 
if not isempty(request("page")) then 
  currentPage=cint(request("page")) 
else 
  currentPage=1 
end if 
set rs=server.createobject("adodb.recordset") 
sql="select * from Feedback order by id desc" 
rs.open sql,conn,1,1 
 
if rs.eof and rs.bof then 
  response.write "<p align='center'>还没有任何留言!</p>" 
else 
  totalPut=rs.recordcount  
if currentpage<1 then 
  currentpage=1 
end if 
if (currentpage-1)*MaxPerPage>totalput then 
  if (totalPut mod MaxPerPage)=0 then 
   currentpage= totalPut \ MaxPerPage 
  else 
   currentpage= totalPut \ MaxPerPage + 1 
  end if 
end if 
if currentPage=1 then 
  showpages 
  showContent 
  showpages 
else 
 if (currentPage-1)*MaxPerPage<totalPut then 
  rs.move (currentPage-1)*MaxPerPage 
  dim bookmark 
  bookmark=rs.bookmark 
  showpages 
  showContent 
  showpages 
 else 
  currentPage=1 
  showContent 
 end if 
end if 
rs.close 
end if 
set rs=nothing 
conn.close 
set conn=nothing 


sub showContent 
dim i 
i=0 
do while not (rs.eof or err) %>
              <table  width="560" border="0" cellpadding="2" cellspacing="1" bgcolor="#000000" >
                <tr bgcolor="#A4B6D7"> 
                  <td width="18%" height="25"> <div align="center"> 
                      <%if rs("Username")="管理员"then%>
                      [管理员公告] 
                      <%else%>
                      用户名 
                      <%end if%>
                    </div></td>
                  <td width="82%">&nbsp;&nbsp;<%=rs("Username")%> &nbsp; <a href="Admin_FeedbackDel.asp?id=<%=rs("ID")%>" onClick="return ConfirmDel();">删除</a>&nbsp;&nbsp;&nbsp;&nbsp; 
                    <a href="Admin_FeedbackRe.asp?id=<%=rs("id")%>"> 
                    <%if rs("Username")<>"管理员" then%>
                    <%
response.write "回复" 
%>
                    </a> <%end if%> </td>
                </tr>
                <%if rs("email")<>"" then%>
                <tr bgcolor="#ECF5FF"> 
                  <td height="10"> <div align="center">公司名称</div></td>
                  <td>&nbsp;&nbsp;<%=rs("CompanyName")%></td>
                </tr>
                <tr bgcolor="#ECF5FF"> 
                  <td height="10"> <div align="center">联系人</div></td>
                  <td>&nbsp;&nbsp;<%=rs("Receiver")%></td>
                </tr>
                <tr bgcolor="#ECF5FF"> 
                  <td height="22"> <div align="center">联系电话</div></td>
                  <td>&nbsp;&nbsp;<%=rs("Phone")%></td>
                </tr>
                <tr bgcolor="#ECF5FF"> 
                  <td height="22"> <div align="center">联系传真</div></td>
                  <td>&nbsp;&nbsp;<%=rs("Fax")%></td>
                </tr>
                <tr bgcolor="#ECF5FF"> 
                  <td height="22" bgcolor="#ECF5FF"> 
                    <div align="center">E-mail</div></td>
                  <td><a href="AdminMaillist.asp?email=<%=rs("email")%>"><%=rs("email")%></a></td>
                </tr>
                <%end if%>
                <tr bgcolor="#A4B6D7">
                  <td height="25" bgcolor="#ECF5FF"><div align="center">手机</div></td>
                  <td height="25" bgcolor="#ECF5FF">&nbsp;&nbsp;<%=rs("Mobile")%></td>
                </tr>
                <tr bgcolor="#A4B6D7"> 
                  <td height="25"> <div align="center">主 题</div></td>
                  <td height="25">&nbsp;&nbsp;<%=rs("title")%>&nbsp;&nbsp;[<%=rs("time")%>]</td>
                </tr>
                <tr bgcolor="#ECF5FF"> 
                  <td height="25"> <div align="center">内 容</div></td>
                  <td height="25">&nbsp;&nbsp;<%=rs("content")%></td>
                </tr>
                <%if rs("ReFeedback")<>""then%>
                <tr bgcolor="#ECF5FF"> 
                  <td height="22"> <div align="center">回复内容</div></td>
                  <td height="22">&nbsp;&nbsp;<%=rs("ReFeedback")%></td>
                </tr>
                <%end if%>
              </table>
            </div></td>
        </tr>
      </table>
      <br> <table width="550" border="0" align="center" cellpadding="0" cellspacing="0">
        <tr> 
          <td><div align="center">
              <% i=i+1 
 if i>=MaxPerPage then exit do 
 rs.movenext 
loop 
end sub 
sub showpages() 
 dim n 
 if (totalPut mod MaxPerPage)=0 then 
   n= totalPut \ MaxPerPage 
 else 
   n= totalPut \ MaxPerPage + 1 
 end if 
 if n=1 then 
  exit sub 
 end if 
 dim k 
 response.write "<p align='left'>&gt;&gt; 留言分页 " 
 for k=1 to n 
 if k=currentPage then 
   response.write "[<b>"+Cstr(k)+"</b>] " 
 else 
   response.write "[<b>"+"<a href='Admin_Feedback.asp?page="+cstr(k)+"'>"+Cstr(k)+"</a></b>] " 
 end if 
 next 
end sub 
%>
            </div></td>
        </tr>
      </table>
      <br> <br> </td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->