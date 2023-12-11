<%@ Language=VBScript %>
<html>
<head>
<title>信息提示</title>
<meta name="Author" content="heweiqun">
<meta name="Contact" content="hdz2008@163.com">
<meta name="Copyright" content="southidc.net">
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<link href="Inc/ManageMent.css" rel="stylesheet" type="text/css">
</head>

<body topmargin="2" marginheight="0" marginwidth="0" leftmargin="0" class=clblue  bgcolor="#ffffff">
<br>
<br><br><br>

<p>
<br>
<center>

<p align="center">
<br>
  <TABLE width=308 height="150" border="0" cellPadding=0 cellSpacing=2 class="HeaderTdStyle">
    <TBODY>
      <TR vAlign=top> 
        <TD  width="292" height="53" valign="middle"> 
          <p align="center"><br>
            <%=Request.QueryString("msg")%></p></TD>
      </TR>
      <TR> 
        <TD  width="292" height="27"> 
          <DIV align=center> 
            <INPUT  type=submit size=3 value=返回 name=Submit2 onclick="window.location.href='Manage_Eshop.asp';">
          </DIV></TD>
      </TR>
    </TBODY>
  </TABLE>
  <p align="center"><br>
</p>
<div align=center >
</div>
</center>
<p>

<br>

</p>

</body>
</html>