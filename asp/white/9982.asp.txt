<%@language=vbscript codepage=936 %>
<!--#include file="Conn.asp"-->
<!--#include file="Admin.asp"-->
<!--#include file="Inc/Function.asp"-->
<%
dim Action,BigClassName,EnBigClassName,rs,FoundErr,ErrMsg
Action=trim(Request("Action"))
BigClassName=trim(request("BigClassName"))
EnBigClassName=trim(request("EnBigClassName"))
if Action="Add" then
	if BigClassName="" then
		FoundErr=True
		ErrMsg=ErrMsg & "<br><li>产品大类名不能为空！</li>"
	end if
	if FoundErr<>True then
		Set rs=Server.CreateObject("Adodb.RecordSet")
		rs.open "Select * From BigClass Where BigClassName='" & BigClassName & "'",conn,1,3
		if not (rs.bof and rs.EOF) then
			FoundErr=True
			ErrMsg=ErrMsg & "<br><li>产品大类“" & BigClassName & "”已经存在！</li>"
		else
    	 	rs.addnew
     		rs("BigClassName")=BigClassName
			rs("EnBigClassName")=EnBigClassName			
    	 	rs.update
     		rs.Close
	     	set rs=Nothing
    	 	call CloseConn()
			Response.Redirect "ClassManage.asp"  
		end if
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
    <td align="center" valign="top"> <br> <br>
      <strong>产 品 类 别 设 置</strong> <br> <br> <br>
      <form name="form1" method="post" action="ClassAddBig.asp" onsubmit="return checkBig()">
        <table width="386" border="0" align="center" cellpadding="0" cellspacing="2" class="border">
          <tr bgcolor="#A4B6D7" class="title"> 
            <td height="25" colspan="2" align="center"><strong>添加大类</strong></td>
          </tr>
          <tr bgcolor="#E3E3E3" class="tdbg"> 
            <td width="126" height="22" bgcolor="#A4B6D7"> <div align="right"><strong>大类名称：</strong></div></td>
            <td width="254" bgcolor="#ECF5FF"> <input name="BigClassName" type="text" size="30" maxlength="50"> 
            </td>
          </tr>
          <tr bgcolor="#C0C0C0" class="tdbg">
            <td height="22" align="center" bgcolor="#A4B6D7"><div align="right"><strong>英文大类名称：</strong></div></td>
            <td height="22" align="center" bgcolor="#ECF5FF"><div align="left">
                <input name="EnBigClassName" type="text" id="EnBigClassName" size="30" maxlength="50">
              </div></td>
          </tr>
          <tr bgcolor="#C0C0C0" class="tdbg"> 
            <td height="22" align="center" bgcolor="#A4B6D7">&nbsp; </td>
            <td height="22" align="center" bgcolor="#ECF5FF"> <div align="left"> 
                <input name="Action" type="hidden" id="Action" value="Add">
                <input name="Add" type="submit" value=" 添 加 ">
              </div></td>
          </tr>
        </table>
      </form></td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->
<%
end if
call CloseConn()
%>