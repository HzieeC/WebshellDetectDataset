<%@language=vbscript codepage=936 %>
<!--#include file="Conn.asp"-->
<!--#include file="Admin.asp"-->
<!--#include file="Inc/Function.asp"-->
<%
dim SmallClassID,Action,BigClassName, SmallClassName, OldSmallClassName,rs,FoundErr,ErrMsg
SmallClassID=trim(Request("SmallClassID"))
Action=trim(Request("Action"))
BigClassName=trim(Request.form("BigClassName"))
SmallClassName=trim(Request.form("SmallClassName"))
OldSmallClassName=trim(request.form("OldSmallClassName"))
EnSmallClassName=trim(Request.form("EnSmallClassName"))
OldEnSmallClassName=trim(request.form("OldEnSmallClassName"))
if SmallClassID="" then
	response.Redirect("ClassManage.asp")
end if
Set rs=Server.CreateObject("Adodb.RecordSet")
rs.Open "Select * from SmallClass where SmallClassID="&SmallClassID&"",conn,1,3
if rs.Bof or rs.EOF then
	FoundErr=True
	ErrMsg=ErrMsg & "<br><li>此产品小类不存在！</li>"
else
	if Action="Modify" then
		if SmallClassName="" then
			FoundErr=True
			ErrMsg=ErrMsg & "<br><li>产品小类名不能为空！</li>"
		end if
		if FoundErr<>True then
			rs("SmallClassName")=SmallClassName
			rs("EnSmallClassName")=EnSmallClassName
     		rs.update
			rs.Close
    	 	set rs=Nothing
			if SmallClassName<>OldSmallClassName then
				conn.execute "Update Product set SmallClassName='" & SmallClassName & "' where BigClassName='" & BigClassName & "' and SmallClassName='" & OldSmallClassName & "'"
	     	end if
			if EnSmallClassName<>OldEnSmallClassName then
				conn.execute "Update Product set EnSmallClassName='" & EnSmallClassName & "' where BigClassName='" & BigClassName & "' and SmallClassName='" & OldEnSmallClassName & "'"
	     	end if	
			call CloseConn()
    	 	Response.Redirect "ClassManage.asp"
		end if
	end if
	if FoundErr=True then
		call WriteErrMsg()
	else
%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> <br>
      <strong>产 品 类 别 设 置</strong> <br> <br> <br> <form name="form1" method="post" action="ClassModifySmall.asp">
        <p>&nbsp;</p>
        <table width="395" border="0" align="center" cellpadding="0" cellspacing="2" bgcolor="#ECF5FF" class="border">
          <tr bgcolor="#A4B6D7" class="title"> 
            <td height="25" colspan="2" align="center"><strong>修改小类名称</strong></td>
          </tr>
          <tr class="tdbg"> 
            <td width="131" height="22" bgcolor="#A4B6D7"><div align="right"><strong>所属大类：</strong></div></td>
            <td width="258"><%=rs("BigClassName")%> <input name="SmallClassID" type="hidden" id="SmallClassID" value="<%=rs("SmallClassID")%>"> 
              <input name="OldSmallClassName" type="hidden" id="OldSmallClassName" value="<%=rs("SmallClassName")%>"> 
              <input name="BigClassName" type="hidden" id="BigClassName" value="<%=rs("BigClassName")%>"> 
            </td>
          </tr>
          <tr class="tdbg"> 
            <td height="22" bgcolor="#A4B6D7"><div align="right"><strong>小类名称：</strong></div></td>
            <td> <input name="SmallClassName" type="text" id="SmallClassName" value="<%=rs("SmallClassName")%>" size="30" maxlength="50"> 
            </td>
          </tr>
          <tr class="tdbg">
            <td height="22" align="center" bgcolor="#A4B6D7"><div align="right"><strong>英文小类名称：</strong></div></td>
            <td align="center"><div align="left">
                <input name="EnSmallClassName" type="text" id="EnSmallClassName" value="<%=rs("EnSmallClassName")%>" size="30" maxlength="50">
              </div></td>
          </tr>
          <tr class="tdbg"> 
            <td height="22" align="center" bgcolor="#A4B6D7">&nbsp;</td>
            <td align="center"> <div align="left"> 
                <input name="Action" type="hidden" id="Action4" value="Modify">
                <input name="Save" type="submit" id="Save" value=" 修 改 ">
              </div></td>
          </tr>
        </table>
      </form></td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->
<%
	end if
end if
rs.close
set rs=nothing
call CloseConn()
%>