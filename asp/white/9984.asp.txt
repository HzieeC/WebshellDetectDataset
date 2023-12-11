<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<!-- #include file="Inc/Head.asp" -->
<SCRIPT language=javascript>
function ConfirmDel()
{
   if(confirm("确定要删除当前的产品？一旦删除将不能恢复！"))
     return true;
   else
     return false;	 
}
</SCRIPT>
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> <strong><br>
      </strong> <table width="560" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td class="back_southidc" height="25"><div align="center"><strong>询价管理 
              <br>
              </strong></div></td>
        </tr>
        <tr> 
          <td height="16"> 
            <%
flag="尚未处理"
set rs=server.createobject("adodb.recordset")
sqltext="select * from OrderList  order by OrderTime desc"
rs.open sqltext,conn,1,1

dim MaxPerPage
MaxPerPage=20

'取得页数,并判断用户输入的是否数字类型的数据，如不是将以第一页显示
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
            <table width="100%" border="0" cellspacing="1" cellpadding="4">
              <tr bgcolor="#A4B6D7" class="tr_southidc"> 
                <td width="18%" height="25"> 
                  <div align="center">订单编号</div></td>
                <td width="20%"> 
                  <div align="center">客户姓名</div></td>
                <td width="22%"> 
                  <div align="center">联系电话</div></td>
                <td width="15%"> 
                  <div align="center">处理情况</div></td>
                <td width="15%"> 
                  <div align="center">询价详情</div></td>
                <td width="10%"> 
                  <div align="center">操作</div></td>
              </tr>
              <%
if not rs.eof then
i=0
do while not rs.eof
%>
              <tr bgcolor="#ECF5FF"> 
                <td> 
                  <div align="center"><%=rs("OrderNum")%></div></td>
                <td> 
                  <div align="center"><%=rs("UserName")%></div></td>
                <td> 
                  <div align="center"><%=rs("Phone")%></div></td>
                <td> 
                  <div align="center"> 
                    <%If rs("Flag")="Yes" Then%>
                    已处理
                    <%else%>
                    <b><font color="#FF0000">未处理</font></b> 
                    <%End If%>
                  </div></td>
                <td> 
                  <div align="center"> 
                    <%response.write "<a href='Admin_OrderDetail.asp?OrderNum="&rs("OrderNum")&"&page="&CurrentPage&"'  >详细资料</a>"
%>
                  </div></td>
                <td> 
                  <div align="center"> 
                    <a href="Admin_OrderDel.asp?OrderNum=<%=rs("OrderNum")%>&page=<%=CurrentPage%>&Action=Del" onClick="return ConfirmDel();">删除</a>
                  </div></td>
              </tr>
              <%
i=i+1
if i >= MaxPerpage then exit do
rs.movenext
loop
end if
%>
              <tr bgcolor="#A4B6D7"> 
                <td height="25" colspan="6"> 
                  <div align="right"> 
                    <%
Response.write "全部-"
Response.write "共" & Cstr(Rs.RecordCount) & "条询价信息&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"
Response.write "第" & Cstr(CurrentPage) &  "/" & Cstr(rs.pagecount) & "&nbsp;"
If currentpage > 1 Then
response.write "<a href='Admin_Order.asp?&page="+cstr(1)+"'>&nbsp;首页&nbsp;</a>"
Response.write "<a href='Admin_Order.asp?page="+Cstr(currentpage-1)+"'>&nbsp;上一页&nbsp;</a>"
Else
Response.write "&nbsp;上一页&nbsp;"
End if
If currentpage < Rs.PageCount Then
Response.write "<a href='Admin_Order.asp?page="+Cstr(currentPage+1)+"'>&nbsp;下一页&nbsp;</a>"
Response.write "<a href='Admin_Order.asp?page="+Cstr(Rs.PageCount)+"'>&nbsp;尾页&nbsp;</a>"
Else
Response.write ""
Response.write "&nbsp;下一页&nbsp;"
End if
Response.write "转到第"
response.write"<select name='sel_page' onChange='javascript:location=this.options[this.selectedIndex].value;'>"
    for i = 1 to Rs.PageCount
       if i = currentpage then 
          response.write"<option value='Admin_Order.asp?page="&i&"&id="&id&"' selected>"&i&"</option>"
       else
          response.write"<option value='Admin_Order.asp?page="&i&"&id="&id&"'>"&i&"</option>"
       end if
    next
response.write"</select>页"
%>
                  </div></td>
              </tr>
            </table>
            <%
End sub
rs.close
conn.close
%> </td>
        </tr>
      </table></td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->