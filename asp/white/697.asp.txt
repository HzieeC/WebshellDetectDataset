<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<SCRIPT language=javascript>
function unselectall()
{
    if(document.del.chkAll.checked){
	document.del.chkAll.checked = document.del.chkAll.checked&0;
    } 	
}

function CheckAll(form)
  {
  for (var i=0;i<form.elements.length;i++)
    {
    var e = form.elements[i];
    if (e.Name != "chkAll")
       e.checked = form.chkAll.checked;
    }
  }
function ConfirmDel()
{
   if(confirm("确定要删除选中的项目吗？一旦删除将不能恢复！"))
     return true;
   else
     return false;	 
}

</SCRIPT>

<%if Request.QueryString("mark")="Honor" then
dim SQL, Rs, contentID,CurrentPage
CurrentPage = request("Page")
ID=request("ID")

set rs=server.createobject("adodb.recordset")
sql="delete from CompHonor where Id="&ID
rs.open sql,conn,3,3
set rs=nothing
conn.close
response.redirect "Admin_Honor.asp"
end if
%>

<%
set rs=server.createobject("adodb.recordset")
sql="select * from CompHonor order by id desc"
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
    <td align="center" valign="top"> <br> <table width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td class="back_southidc" height="25"> <div align="center"><strong>企业荣誉管理 <br>
              </strong></div></td>
        </tr>
        <tr> 
          <td><div align="center"> 
              <table width="100%" border="0" cellspacing="1" cellpadding="0">
                <tr> 
                  <td width="8%" height="25" bgcolor="#A4B6D7"> 
                    <div align="center">ID</div></td>
                  <td width="25%" bgcolor="#A4B6D7"> 
                    <div align="center">荣誉名称</div></td>
                  <td width="47%" bgcolor="#A4B6D7"> 
                    <div align="center">荣誉证书</div></td>
                  <td width="10%" bgcolor="#A4B6D7"> 
                    <div align="center">操作</div></td>
                  <td width="10%" bgcolor="#A4B6D7"> 
                    <div align="center">操作</div></td>
                </tr>
                <%
if not rs.eof then
i=0
do while not rs.eof
%>
                <tr bgcolor="#ECF5FF"> 
                  <td height="22"> 
                    <div align="center"><%=rs("id")%></div></td>
                  <td bgcolor="#ECF5FF">&nbsp;&nbsp;<%=rs("explain")%></td>
                  <td> <div align="center">
					<img name="" src="../<%=rs("CompHonor")%>" width="189" height="120" alt=""></div></td>
                  <td> 
                    <div align="center"><a href="Admin_HonorEdit.asp?id=<%=rs("id")%>">修改</a></div></td>
                  <td> 
                    <div align="center"><a href="Admin_Honor.asp?id=<%=rs("id")%>&mark=Honor" onClick="return ConfirmDel();">删除</a></div></td>
                </tr>
                <% 
i=i+1 
if i >= MaxPerpage then exit do 
rs.movenext 
loop 
end if 
%>
                <tr bgcolor="#A4B6D7"> 
                  <td height="25" colspan="5"> <div align="right">
                      <%
Response.write "全部"
Response.write "共" & Cstr(Rs.RecordCount) & "条荣誉&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"
Response.write "第" & Cstr(CurrentPage) &  "/" & Cstr(rs.pagecount) & "&nbsp;"
If currentpage > 1 Then
response.write "<a href='Admin_Honor.asp?&page="+cstr(1)+"'>&nbsp;首页&nbsp;</a>"
Response.write "<a href='Admin_Honor.asp?page="+Cstr(currentpage-1)+"'>&nbsp;上一页&nbsp;</a>"
Else
Response.write "&nbsp;上一页&nbsp;"
End if
If currentpage < Rs.PageCount Then
Response.write "<a href='Admin_Honor.asp?page="+Cstr(currentPage+1)+"'>&nbsp;下一页&nbsp;</a>"
Response.write "<a href='Admin_Honor.asp?page="+Cstr(Rs.PageCount)+"'>&nbsp;尾页&nbsp;</a>"
Else
Response.write ""
Response.write "&nbsp;下一页&nbsp;"
End if
Response.write "转到第"
response.write"<select name='sel_page' onChange='javascript:location=this.options[this.selectedIndex].value;'>"
    for i = 1 to Rs.PageCount
       if i = currentpage then 
          response.write"<option value='Admin_Honor.asp?page="&i&"&id="&id&"' selected>"&i&"</option>"
       else
          response.write"<option value='Admin_Honor.asp?page="&i&"&id="&id&"'>"&i&"</option>"
       end if
    next
response.write"</select>页"
%>
                    </div></td>
                </tr>
              </table>
<div align="right">
                <%
End sub
rs.close
%>
              </div>
            </div></td>
        </tr>
      </table>
      <br> <br> </td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->