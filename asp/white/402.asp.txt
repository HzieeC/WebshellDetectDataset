<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<!-- #include file="../inc/x_to_html/index_to_html.asp" -->
	<%
Call header()
%>


	<table cellpadding='3' cellspacing='1' border='0' class='tableBorder' align=center>
	<tr>
	  <th width="100%" height=25 class='tableHeaderText'>生成首页</th>
	
	<tr><td height="400" valign="top"  class='forumRow'><br>
	    <table width="90%" border="0" align="center" cellpadding="0" cellspacing="0">
          <tr>
            <td height="25" class="TitleHighlight3"></td>
          </tr>
          <tr>
            <td height="100"><div align="center">
			<%
			call index_to_html()
			response.Write "<span style='color:#ff0000'>首页生成成功！&nbsp;<a href='/index.html' target='_blank'>点击预览>>></a></span>"
			%></div></td>
          </tr>
        </table>
	    </td>
	</tr>
	</table>


<%
Call DbconnEnd()
 %>