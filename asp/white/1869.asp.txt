<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<%
Dvbbs.LoadTemplates("")
Dvbbs.mainsetting(0)="98%"
Dvbbs.Head()
ViewMain()
Response.Write "</body></html>"
Dvbbs.PageEnd()
Sub ViewMain()
%>
<br>
<table cellpadding="5" cellspacing="1" border="0" class="tableBorder2" align="center" height="85%">
<tr><th height="23">首页调用效果测试</th></tr>
<tr>
<td height="*" class="tablebody1" valign="top">
<div class="quote">
调用代码： <font class="redfont" id="scriptcode"></font>
<input type="hidden" name="NewScript" id="NewScript">
<input type="button" value="拷贝调用代码" onclick="oCopy(document.getElementById('NewScript'));">
</div>
<font color="gray">以下为查看的调用效果：</font>
<hr size=1>
<script language="JavaScript">
<!--
var Script = opener.txtRun.value;
document.getElementById("scriptcode").innerText = Script;
document.getElementById("NewScript").value = Script;
document.write(Script);
function oCopy(obj){ 
	obj.select(); 
	var js=obj.createTextRange(); 
	js.execCommand("Copy");
}
//-->
</script>
</td>
</tr>
</table>
<p></p>
<center><button onclick="window.close();">关闭窗口</button></center>
<%
End Sub
%>
