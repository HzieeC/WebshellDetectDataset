<%@ Language=VBScript %>
<html>
<!-- #include file="Inc/Head.asp" -->
<p>&nbsp;</p><TABLE width=308 height="150" border="0" align="center" cellPadding=0 cellSpacing=2 class="HeaderTdStyle">
  <TBODY>
      <TR vAlign=top> 
        <TD  width="292" height="53" valign="middle"> 
          <p align="center"><br>
            <%=Request.QueryString("msg")%></p></TD>
      </TR>
      <TR> 
        <TD  width="292" height="27"> 
          <DIV align=center> 
            <INPUT  type=submit size=3 value=·µ»Ø name=Submit2 onclick="window.location.href='Admin_Order.asp';">
          </DIV></TD>
      </TR>
    </TBODY>
  </TABLE>
</body>
</html>