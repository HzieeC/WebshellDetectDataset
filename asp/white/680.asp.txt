<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<script language=javascript>
function ConfirmDel()
{
   if(confirm("确定要删除此记录吗？"))
     return true;
   else
     return false;
}
</script>

<%if Request.QueryString("mark")="southidc" then
dim SQL, Rs, ID,CurrentPage
CurrentPage = request("Page")
ID=request("ID")

set rs=server.createobject("adodb.recordset")
sql="delete from HrDemand where Id="&ID
rs.open sql,conn,1,3
set rs=nothing
conn.close
response.redirect "Admin_HrDemand.asp"
end if
%>
<%
set rs=server.createobject("adodb.recordset")
sql="select * from HrDemand order by id desc"
rs.open sql,conn,1,1

dim MaxPerPage
MaxPerPage=10

dim text,checkpage
text="0123456789"
Rs.PageSize=MaxPerPage
for i=1 to len(request("page"))
checkpage=instr(1,text,mid(request("page"),i,1))
if checkpage=0 then
exit for
end if
next

If checkpage<>0 then
If NOT IsEmpty(request("page")) Then
CurrentPage=Cint(request("page"))
If CurrentPage < 1 Then CurrentPage = 1
If CurrentPage > Rs.PageCount Then CurrentPage = Rs.PageCount
Else
CurrentPage= 1
End If
If not Rs.eof Then Rs.AbsolutePage = CurrentPage end if
Else
CurrentPage=1
End if

call list

'显示帖子的子程序
Sub list()%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> <br> <table width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td height="25" class="back_southidc"> 
            <div align="center"><strong>招 聘 管 理 <br>
              </strong></div></td>
        </tr>
        <tr> 
          <td><div align="center"> 
              <table width="100%" border="0" cellspacing="1" cellpadding="0">
                <tr bgcolor="#ECF5FF"> 
                  <td width="8%" height="25"> <div align="center">ID</div></td>
                  <td width="35%"> <div align="center">招聘对象</div></td>
                  <td width="11%"> <div align="center">招聘人数</div></td>
                  <td width="11%"> <div align="center">发布时间</div></td>
                  <td width="11%"> <div align="center">有效期限</div></td>
                  <td colspan="2"> <div align="center"></div>
                    <div align="center">操　作</div></td>
                </tr>
                <%
if not rs.eof then
i=0
do while not rs.eof
%>
                <tr bgcolor="#ECF5FF"> 
                  <td height="22"> <div align="center"><%=rs("id")%></div></td>
                  <td bgcolor="#ECF5FF">&nbsp;&nbsp;<%=rs("HrName")%></td>
                  <td> <div align="center"><%=rs("HrRequireNum")%>人</div></td>
                  <td> <div align="center"><%=rs("HrAddress")%></div></td>
                  <td> <div align="center"><%=rs("HrValidDate")%>天</div></td>
                  <td width="11%"> <div align="center"><a href="Admin_HrDemandEdit.asp?id=<%=rs("id")%>">修改</a></div></td>
                  <td width="13%"> <div align="center"><a href="Admin_HrDemand.asp?id=<%=rs("id")%>&amp;mark=southidc" onClick="return ConfirmDel();">删除</a></div></td>
                </tr>
                <% 
i=i+1 
if i >= MaxPerpage then exit do 
rs.movenext 
loop 
end if 
%>
                <tr bgcolor="#A4B6D7"> 
                  <td height="25" colspan="7"> <div align="right">
                      <%
Response.write "全部-"
Response.write "共" & Cstr(Rs.RecordCount) & "条招聘信息&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"
Response.write "第" & Cstr(CurrentPage) &  "/" & Cstr(rs.pagecount) & "&nbsp;"
If currentpage > 1 Then
response.write "<a href='Admin_HrDemand.asp?&page="+cstr(1)+"'>&nbsp;首页&nbsp;</a>"
Response.write "<a href='Admin_HrDemand.asp?page="+Cstr(currentpage-1)+"'>&nbsp;上一页&nbsp;</a>"
Else
Response.write "&nbsp;上一页&nbsp;"
End if
If currentpage < Rs.PageCount Then
Response.write "<a href='Admin_HrDemand.asp?page="+Cstr(currentPage+1)+"'>&nbsp;下一页&nbsp;</a>"
Response.write "<a href='Admin_HrDemand.asp?page="+Cstr(Rs.PageCount)+"'>&nbsp;尾页&nbsp;</a>"
Else
Response.write ""
Response.write "&nbsp;下一页&nbsp;"
End if
Response.write "转到第"
response.write"<select name='sel_page' onChange='javascript:location=this.options[this.selectedIndex].value;'>"
    for i = 1 to Rs.PageCount
       if i = currentpage then 
          response.write"<option value='Admin_HrDemand.asp?page="&i&"&id="&id&"' selected>"&i&"</option>"
       else
          response.write"<option value='Admin_HrDemand.asp?page="&i&"&id="&id&"'>"&i&"</option>"
       end if
    next
response.write"</select>页"
%>
                    </div></td></tr>
</table>
<%
End sub
rs.close
%>
            </div></td>
        </tr>
      </table>
      <br> <br> </td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->