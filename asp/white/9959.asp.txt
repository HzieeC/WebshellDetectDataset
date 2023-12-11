<%@language=vbscript codepage=936 %>
<!--#include file="Conn.asp"-->
<!--#include file="Admin.asp"-->
<!--#include file="Inc/Function.asp"-->
<%
dim BigClassID,Action,rs,NewBigClassName,OldEnBigClassName,NewEnBigClassName,OldBigClassName,FoundErr,ErrMsg
BigClassID=trim(Request("BigClassID"))
Action=trim(Request("Action"))
NewBigClassName=trim(Request("NewBigClassName"))
OldBigClassName=trim(Request("OldBigClassName"))
NewEnBigClassName=trim(Request("NewEnBigClassName"))
OldEnBigClassName=trim(Request("OldEnBigClassName"))

if BigClassID="" then
  response.Redirect("ClassManage.asp")
end if
Set rs=Server.CreateObject("Adodb.RecordSet")
rs.Open "Select * from BigClass where BigClassID=" & CLng(BigClassID),conn,1,3
if rs.Bof and rs.EOF then
	FoundErr=True
	ErrMsg=ErrMsg & "<br><li>此产品大类不存在！</li>"
else
	if Action="Modify" then
		if NewBigClassName="" then
			FoundErr=True
			ErrMsg=ErrMsg & "<br><li>产品大类名不能为空！</li>"
		end if
		if FoundErr<>True then
			rs("BigClassName")=NewBigClassName
			rs("EnBigClassName")=NewEnBigClassName			
    	 	rs.update
			rs.Close
	     	set rs=Nothing
			if NewBigClassName<>OldBigClassName then
				conn.execute "Update SmallClass set BigClassName='" & NewBigClassName & "' where BigClassName='" & OldBigClassName & "'"
				conn.execute "Update Product set BigClassName='" & NewBigClassName & "' where BigClassName='" & OldBigClassName & "'"
     		end if
			
			if NewEnBigClassName<>OldEnBigClassName then
				conn.execute "Update SmallClass set EnBigClassName='" & NewEnBigClassName & "' where EnBigClassName='" & OldEnBigClassName & "'"
				conn.execute "Update Product set EnBigClassName='" & NewEnBigClassName & "' where EnBigClassName='" & OldEnBigClassName & "'"
     		end if		
			call CloseConn()
     		Response.Redirect "ClassManage.asp" 
		end if
	end if
	if FoundErr=True then
		call WriteErrMsg()
	else
%>
<script language="JavaScript" type="text/JavaScript">
function checkBig()
{
  if (document.form1.BigClassName.value=="")
  {
    alert("大类名称不能为空！");
    document.form1.BigClassName.focus();
    return false;
  }
}
</script>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> <br> <strong>产 品 类 别 设 置</strong> <br> 
      <br> <br> <form name="form1" method="post" action="ClassModifyBig.asp">
        <table width="375" border="0" align="center" cellpadding="0" cellspacing="2" bgcolor="#ECF5FF" class="border">
          <tr class="title"> 
            <td height="25" colspan="2" align="center" bgcolor="#A4B6D7"><strong>修改大类名称</strong></td>
          </tr>
          <tr class="tdbg"> 
            <td width="131" bgcolor="#A4B6D7"><div align="right"><strong>大类ID：</strong></div></td>
            <td width="238"><%=rs("BigClassID")%> <input name="BigClassID" type="hidden" id="BigClassID" value="<%=rs("BigClassID")%>"> 
              <input name="OldBigClassName" type="hidden" id="OldBigClassName" value="<%=rs("BigClassName")%>"> 
			    <input name="OldEnBigClassName" type="hidden" id="OldEnBigClassName" value="<%=rs("EnBigClassName")%>"> 
            </td>
          </tr>
          <tr class="tdbg"> 
            <td width="131" height="20" bgcolor="#A4B6D7"><div align="right"><strong>大类名称：</strong></div></td>
            <td> <input name="NewBigClassName" type="text" id="NewBigClassName" value="<%=rs("BigClassName")%>" size="30" maxlength="50"> 
            </td>
          </tr>
          <tr class="tdbg">
            <td align="center" bgcolor="#A4B6D7"><div align="right"><strong>英文大类名称：</strong></div></td>
            <td align="center"><div align="left">
                <input name="NewEnBigClassName" type="text" id="NewEnBigClassName" value="<%=rs("EnBigClassName")%>" size="30" maxlength="50">
              </div></td>
          </tr>
          <tr class="tdbg"> 
            <td align="center" bgcolor="#A4B6D7">&nbsp;</td>
            <td align="center"> <div align="left"> 
                <input name="Action" type="hidden" id="Action" value="Modify">
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