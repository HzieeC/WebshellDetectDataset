<%@language=vbscript codepage=936 %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%
dim Title,VoteTime,Action,IsSelected
dim rs,sql
Title=trim(request.form("Title"))
VoteTime=trim(request.form("VoteTime"))
if VoteTime="" then VoteTime=now()
Action=trim(request("Action"))
IsSelected=trim(request("IsSelected"))

dim i
if Title<>"" then
	sql="select  * from Vote where (id is null)"
	Set rs= Server.CreateObject("ADODB.Recordset")
	rs.open sql,conn,1,3
	rs.addnew
	rs("Title")=Title
	for i=1 to 8
		if trim(request("select"&i))<>"" then
			rs("select"&i)=trim(request("select"&i))
			if request("answer"&i)="" then
				rs("answer"&i)=0
			else
				rs("answer"&i)=clng(request("answer"&i))
			end if
		end if
	next
	rs("VoteTime")=VoteTime
	rs("VoteType")=request("VoteType")
	if IsSelected="" then IsSelected=false
	if IsSelected="True" then conn.execute "Update Vote set IsSelected=False where IsSelected=True"
	rs("IsSelected")=IsSelected
	rs.update
	rs.close
	set rs=nothing
	call CloseConn()
	Response.Redirect "VoteManage.asp"
end if
%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td width="862" align="center" valign="top"><br>
      <strong>添 加 调 查</strong> <br> <form method="POST" action="VoteAdd.asp">
        <table width="560" border="0" align="center" cellpadding="0" cellspacing="1" bgcolor="#000000" Class="border">
          <tr class="tdbg"> 
            <td width="81" height="22" align="right" bgcolor="#A4B6D7">主题：</td>
            <td colspan="3" bgcolor="#E3E3E3"> <input name="Title" type="text" size="45" maxlength="50"></td>
          </tr>
          <%
	for i=1 to 8%>
          <tr class="tdbg"> 
            <td height="22" align="right" bgcolor="#A4B6D7">选项<%=i%>：</td>
            <td width="281" bgcolor="#E3E3E3"> <input type="text" name="select<%=i%>" size="36"> 
            </td>
            <td width="41" align="right" bgcolor="#E3E3E3">票数：</td>
            <td width="87" bgcolor="#E3E3E3"> <input type="text" name="answer<%=i%>" size="5"></td>
          </tr>
          <%next%>
          <tr class="tdbg"> 
            <td height="22" align="right" bgcolor="#A4B6D7">调查类型：</td>
            <td colspan="3" bgcolor="#E3E3E3"> <select name="VoteType" id="VoteType">
                <option value="Single" selected>单选</option>
                <option value="Multi">多选</option>
              </select></td>
          </tr>
          <tr class="tdbg"> 
            <td height="22" align="right" bgcolor="#A4B6D7">&nbsp;</td>
            <td colspan="3" bgcolor="#E3E3E3"> <input name="IsSelected" type="checkbox" id="IsSelected" value="True" checked>
              设为最新调查</td>
          </tr>
          <tr bgcolor="#FFFFFF" class="tdbg"> 
            <td height="25" colspan=4 align=center> 
              <input name="Submit" type="submit" id="Submit" value=" 添 加 "> 
              &nbsp; <input  name="Reset2" type="reset" id="Reset2" value=" 清 除 "> 
            </td>
          </tr>
        </table>
      </form></strong></p>
      </td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->