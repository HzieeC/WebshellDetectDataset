<%@ Language=VBScript %>
<html>
<head>
<title>信息提示</title>
<meta name="Author" content="heweiqun">
<meta name="Contact" content="hdz2008@163.com">
<meta name="Copyright" content="southidc.net">
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<link href="Inc/Css.css" rel="stylesheet" type="text/css">
</head>
<body topmargin="0" marginheight="0" marginwidth="0" leftmargin="0" class=clblue  bgcolor="#D9D9D9">
<div align="center">
  <table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
    <tr>
      <td><TABLE width=230 align="center" cellPadding=0 cellSpacing=0>
          <TR> 
            <TD align=middle> <TABLE width=308 align="center" cellPadding=0 cellSpacing=1 bgColor=#006699>
                <TBODY>
                  <TR> 
                    <TD height=22></TD>
                  </TR>
                </TBODY>
              </TABLE>
              <div align="center"> 
                <center>
                  <TABLE cellSpacing=1 cellPadding=0 width=308 bgColor=#006699 height="100">
                    <TBODY>
                      <TR vAlign=top bgColor=#eeeeee> 
                        <TD  width="292" height="53"> <p align="center"><br>
                            <%=Request.QueryString("msg")%></p></TD>
                      </TR>
                      <TR bgColor=#eeeeee> 
                        <TD  width="292" height="27"> <DIV align=center> <a href="javascript:window.close()">关闭本窗口</a> 
                          </DIV></TD>
                      </TR>
                    </TBODY>
                  </TABLE>
                </center>
              </div>
              <TABLE cellSpacing=1 cellPadding=0 width=308 bgColor=#006699>
                <TBODY>
                  <TR> 
                    <TD height=22></TD>
                  </TR>
                </TBODY>
              </TABLE></TD>
          </TR>
        </TABLE></td>
    </tr>
  </table>
</div>
</body>
</html>