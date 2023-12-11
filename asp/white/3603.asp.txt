<%@language=vbscript codepage=936 %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<%
dim ID,Title,VoteTime,IsSelected
dim rs,sql
ID=request.QueryString("id")   ' 也可用ID=request("id")

if Request.QueryString("mark")="southidc" then
 ID=request("id")
 Title=trim(request.form("Title"))
 VoteTime=now()
 IsSelected=trim(request("IsSelected"))
 if IsSelected="True" then
	conn.execute "Update Vote set IsSelected=False where IsSelected=True"
 end if
 dim i

 sql="select * from Vote where ID="&ID
 Set rs_sql= Server.CreateObject("ADODB.Recordset")
 rs_sql.open sql,conn,1,3
 if not rs_sql.eof then
	 if Title<>"" then
		rs_sql("Title")=Title
		for i=1 to 8
			if trim(request("select"&i))<>"" then
				rs_sql("select"&i)=trim(request("select"&i))
				if request("answer"&i)="" then
					rs_sql("answer"&i)=0
				else
					rs_sql("answer"&i)=clng(request("answer"&i))
				end if
			else
				rs_sql("select"&i)=""
				rs_sql("answer"&i)=0
			end if
		next
		rs_sql("VoteTime")=VoteTime
		rs_sql("VoteType")=request("VoteType")
		if IsSelected="True" then
		  rs_sql("IsSelected")=True
	    else
		  rs_sql("IsSelected")=False
	    end if		
		rs_sql.update
		rs_sql.close
		Response.Redirect "VoteManage.asp"
	end if
  end if
end if  
%>

<%
sql="select * from Vote where ID="&ID
Set rs= Server.CreateObject("ADODB.Recordset")
rs.open sql,conn,1,1	
%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td width="862" align="center" valign="top"><br>
      <strong><b>修 改</b> 调 查</strong> <br> <form method="POST" action="VoteModify.asp?mark=southidc">
        <table width="560" border="0" align="center" cellpadding="2" cellspacing="1" bgcolor="#000000" Class="border">
          <tr class="tdbg"> 
            <td width="15%" align="right" bgcolor="#A4B6D7">主题：</td>
            <td width="85%" colspan="3" bgcolor="#E3E3E3"> 
              <input name="Title" type="text" value="<%=rs("Title")%>" size="52"></td>
          </tr>
          <%
for i=1 to 8%>
          <tr class="tdbg"> 
            <td align="right" bgcolor="#A4B6D7">选项<%=i%>：</td>
            <td bgcolor="#E3E3E3"> 
              <input name="select<%=i%>" type="text" value="<%=rs("select"& i)%>" size="36"> 
            </td>
            <td align="right" bgcolor="#E3E3E3">票数：</td>
            <td width="80" bgcolor="#E3E3E3"> 
              <input name="answer<%=i%>" type="text" value="<%=rs("answer"&i)%>" size="5"></td>
          </tr>
          <%next%>
          <tr class="tdbg"> 
            <td align="right" bgcolor="#A4B6D7">调查类型：</td>
            <td colspan="3" bgcolor="#E3E3E3"> 
              <select name="VoteType" id="VoteType">
                <option value="Single" <% if rs("VoteType")="Single" then %> selected <% end if%>>单选</option>
                <option value="Multi" <% if rs("VoteType")="Multi" then %> selected <% end if%>>多选</option>
              </select></td>
          </tr>
          <tr class="tdbg"> 
            <td align="right" bgcolor="#A4B6D7">&nbsp;</td>
            <td colspan="3" bgcolor="#E3E3E3"> 
              <input name="IsSelected" type="checkbox" id="IsSelected" value="True" <% if rs("IsSelected")=true then response.write "checked"%>>
              设为最新调查</td>
          </tr>
          <tr class="tdbg"> 
            <td colspan=4 align=center bgcolor="#A4B6D7"> 
              <input name="ID" type="hidden" id="ID" value="<%=rs("ID")%>"> 
              <input name="Submit" type="submit" id="Submit" value="保存修改结果"> </td>
          </tr>
        </table>
      </form></strong></p>
      </td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->
<%
rs.close
set rs=nothing
call CloseConn()
%>
