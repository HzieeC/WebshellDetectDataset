<%@language=vbscript codepage=936 %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<!--#include file="../Inc/Ubbcode.asp"-->
<!--#include file="Inc/Function.asp"-->
<%
dim strFileName
const MaxPerPage=20
dim totalPut   
dim CurrentPage
dim TotalPages
dim i,j
dim ID
dim Title
dim BigClassName,SmallClassName,SpecialName
dim Passed
dim PurviewChecked
dim strAdmin,arrAdmin
PurviewChecked=false
Passed=trim(request("Passed"))
strFileName="ProductCheck.asp?BigClassName=" & BigClassName & "&SmallClassName=" & SmallClassName

if Passed="" then
	if Session("Passed")="" then
		Passed="False"
	else
		Passed=Session("Passed")
	end if
end if
session("Passed")=Passed
Title=Trim(request("Title"))
ID=Request("ID")
BigClassName=Trim(request("BigClassName"))
SmallClassName=Trim(request("SmallClassName"))
if request("page")<>"" then
    currentPage=cint(request("page"))
else
	currentPage=1
end if
dim sql
dim rs
sql="select * from Product where Passed=" & Passed
if Title<>"" then
	sql=sql & " and title like '%" & Title & "%' "
else
	if BigClassName<>"" then
		sql=sql & " and BigClassName='" & BigClassName & "' "
		if SmallClassName<>"" then
			sql=sql & " and SmallClassName='" & SmallClassName & "' "
		end if	
	end if
end if
sql=sql & " order by ID desc"
Set rs= Server.CreateObject("ADODB.Recordset")
rs.open sql,conn,1,1
%>
<SCRIPT language=javascript>
function unselectall()
{
    if(document.Check.chkAll.checked){
	document.Check.chkAll.checked = document.Check.chkAll.checked&0;
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
</SCRIPT>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td width="862" align="center" valign="top"> <br> <p align="center"><strong>产 
        品 审 核</strong></p>
      <form name="Passed" method="Get" action="ProductCheck.asp">
        <div align="center"><strong>产品选项：</strong> 
          <input name="Passed" type="radio" value="False" onclick="submit();" <%if Passed="False" then response.write " checked"%>>
          未审核的产品&nbsp;&nbsp;&nbsp;&nbsp; 
          <input name="Passed" type="radio" value="True" onclick="submit();" <%if Passed="True" then response.write " checked"%>>
          已审核的产品 
          <input name="BigClassName" type="hidden" id="BigClassName" value="<%=BigClassName%>">
          <input name="SmallClassName" type="hidden" id="SmallClassName" value="<%=SmallClassName%>">         
        </div>
      </form>
      <table width="620" border="0" align="center" cellpadding="5" cellspacing="2" bgcolor="#000000" class="border">
        <tr class="title"> 
          <td height="25" bgcolor="A4B6D7">|&nbsp; 
            <%
dim sqlBigClass,sqlSmallClass,rsBigClass,rsSmallClass,sqlSpecial,rsSpecial
sqlBigClass="select * from BigClass"
Set rsBigClass= Server.CreateObject("ADODB.Recordset")
rsBigClass.open sqlBigClass,conn,1,1
if rsBigClass.eof then 
	response.Write("还没有任何类别，请首先添加类别。")
end if
do while not rsBigClass.eof
	if rsBigClass("BigClassName")=BigClassName then
		response.Write("<a href='ProductCheck.asp?BigClassName=" & rsBigClass("BigClassName") & "'><font color='red'>" & rsBigClass("BigClassName") & "</font></a> | ")	
	else
		response.Write("<a href='ProductCheck.asp?BigClassName=" & rsBigClass("BigClassName") & "'>" & rsBigClass("BigClassName") & "</a> | ")
	end if
	rsBigClass.movenext
loop
rsBigClass.close
set rsBigClass=nothing
%>
          </td>
        </tr>
        <%
if BigClassName<>"" then
	sqlSmallClass="select * from SmallClass where BigClassName='" & BigClassName & "'"
	Set rsSmallClass= Server.CreateObject("ADODB.Recordset")
	rsSmallClass.open sqlSmallClass,conn,1,1
	if not (rsSmallClass.bof and rsSmallClass.eof) then
		response.write "<tr class='tdbg'><td bgcolor='#CCCCCC'>"
		do while not rsSmallClass.eof
			if rsSmallClass("SmallClassName")=SmallClassName then
				response.Write("&nbsp;<a href='ProductCheck.asp?BigClassName=" & rsSmallClass("BigClassName") & "&SmallClassName=" & rsSmallClass("SmallClassName") & "'><font color='red'>" & rsSmallClass("SmallClassName") & "</font></a>&nbsp;&nbsp;")				
			else
				response.Write("&nbsp;<a href='ProductCheck.asp?BigClassName=" & rsSmallClass("BigClassName") & "&SmallClassName=" & rsSmallClass("SmallClassName") & "'>" & rsSmallClass("SmallClassName") & "</a>&nbsp;&nbsp;")
			end if
			rsSmallClass.movenext
		loop
		response.write "</td></tr>"
	end if
	rsSmallClass.close
	set rsSmallClass=nothing
end if
%>
      </table>
      <form action="ProductCheckSet.asp" method="Post" name="Check" id="Check">
        <table width="620" height="22" border="0" cellpadding="0" cellspacing="1" bgcolor="#000000">
          <tr> 
            <td width="453" height="22" bgcolor="A4B6D7"><a href="ProductCheck.asp">&nbsp;产品审核</a>&nbsp;&gt;&gt;&nbsp; 
              <%
if Title="" and BigClassName="" and SmallClassName="" and SpecialName="" then
	if Passed="False" then
		response.write "所有未审核的产品"
	else
		response.write "所有已审核的产品"
	end if
else
	if request("Query")<>"" then
		if Title<>"" then
			response.write "标题中含有“<font color=blue>" & Title & "</font>”并且未审核的产品"
		else
			response.Write("所有未审核的产品")
		end if
 	else
		if BigClassName<>"" then
			response.write "<a href='ProductCheck.asp?BigClassName=" & BigClassName & "'>" & BigClassName & "</a>&nbsp;&gt;&gt;&nbsp;"
			if SmallClassName<>"" then
				response.write "<a href='ProductCheck.asp?BigClassName=" & BigClassName & "&SmallClassName=" & SmallClassName & "'>" & SmallClassName & "</a>"
			else
				response.write "所有小类"
			end if
		end if
		if SpecialName<>"" then
			response.write "<font color=red>[专题]</font> " & SpecialName
		end if
	end if
end if
%>
            </td>
            <td width="161" bgcolor="#A4B6D7"> &nbsp; 
              <%
  	if rs.eof and rs.bof then
		response.write "共找到 0 个产品</td></tr></table>"
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
		response.Write "共找到 " & totalPut & " 个产品"
%>
            </td>
          </tr>
        </table>
        <%		
	    if currentPage=1 then
        	showContent
        	showpage strFileName,totalput,MaxPerPage,true,false," 个产品"
   	 	else
   	     	if (currentPage-1)*MaxPerPage<totalPut then
         	   	rs.move  (currentPage-1)*MaxPerPage
         		dim bookmark
           		bookmark=rs.bookmark
            	showContent
            	showpage strFileName,totalput,MaxPerPage,true,false," 个产品"
        	else
	        	currentPage=1
           		showContent
           		showpage strFileName,totalput,MaxPerPage,true,false," 个产品"
	    	end if
		end if
	end if
%>
        <%  
sub showContent
   	dim i
    i=0
%>
        <table width="620" border="0" cellpadding="0" cellspacing="1" bgcolor="#000000" class="border" style="word-break:break-all">
          <tr bgcolor="#A4B6D7" class="title"> 
            <td width="30" height="25" align="center"><strong>选择</strong></td>
            <td width="37"  height="20" align="center"><strong>ID</strong></td>
            <td width="317" align="center" bgcolor="#A4B6D7" ><strong>产品名称</strong></td>
            <td width="63" align="center" ><strong>加入时间</strong></td>
            <td width="57" align="center" ><strong>点击数</strong></td>
            <td width="40" align="center" ><strong>已审核</strong></td>
            <td width="60" align="center" ><strong>操作</strong></td>
          </tr>
          <%do while not rs.eof%>
          <tr class="tdbg"> 
            <td width="30" height="22" align="center" bgcolor="#A4B6D7"> 
              <input name='ID' type='checkbox' onclick="unselectall()" id="ID" value='<%=cstr(rs("ID"))%>'>
            </td>
            <td width="37" align="center" bgcolor="#ECF5FF"><%=rs("ID")%></td>
            <td bgcolor="#ECF5FF"> <a href="../ProductShow.asp?ID=<%=rs("ID")%>" title="<%=replace(left(nohtml(rs("Content")),200),chr(34),"") & "……"%>">&nbsp;<%=rs("title")%></a></td>
            <td width="63" align="center" bgcolor="#ECF5FF"><%= FormatDateTime(rs("UpdateTime"),2) %></td>
            <td width="57" align="center" bgcolor="#ECF5FF"><%=rs("Hits")%> </td>
            <td width="40" align="center" bgcolor="#ECF5FF"> 
              <%if rs("Passed")=true then response.write "是" else response.write "否" end if%> </td>
            <td width="60" align="center" bgcolor="#ECF5FF"> 
              <%
	  if PurviewChecked=True then
	  	If rs("Passed")=False Then %> <a href="ProductCheckSet.asp?Action=Check&ID=<%=rs("ID")%>">通过审核</a> 
              <% Else %> <a href="ProductCheckSet.asp?Action=CancelCheck&ID=<%=rs("ID")%>">取消通过</a> 
              <%
	    End If
	  end if %> </td>
          </tr>
          <%
	i=i+1
	      if i>=MaxPerPage then exit do
	      rs.movenext
	loop
%>
        </table>
        <table width="620" border="0" cellpadding="0" cellspacing="0">
          <tr> 
            <td width="250" height="30"><input name="chkAll" type="checkbox" id="chkAll" onclick=CheckAll(this.form) value="checkbox">
              选中本页显示的所有产品</td>
            <td><input name="submit" type='submit' value="<%if Passed="True" then response.write "取消"%>审核选定的产品"> 
              <input name="Action" type="hidden" id="Action" value="<% if Passed="False" then
			  	response.write "Check"
			  else
			    response.write "CancelCheck"
			  end if%>"></td>
          </tr>
        </table>
        <%
   end sub 
%>
      </form>
      <table width="620" class="border">
        <tr class="tdbg"> 
          <form name="searchsoft" method="get" action="ProductCheck.asp">
            <td height="30"> <strong>查找产品：</strong> <input name="Title" type="text" class=smallInput id="Title3" size="20"> 
              <input name="Query" type="submit" id="Query" value="查 询"> &nbsp;&nbsp;请输入产品名称。如果为空，则查找所有产品。 
            </td>
          </form>
        </tr>
      </table></td>
  </tr>
</table>
<!--#include file="Inc/Foot.asp" -->
<%
rs.close
set rs=nothing  
call CloseConn()
%>