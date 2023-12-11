<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<%
Dvbbs.LoadTemplates("index")
Dvbbs.Stats="代码演示"
Dvbbs.head()
%>
<CENTER>
<FONT style="FONT-SIZE: 25px;line-height: 30px;" face=Verdana size=4>
<b><%=Dvbbs.Stats%>测试与帮助</b></font>
</CENTER>
<hr class=tableborder1>
<p><p>
<script>
document.write(opener.txtRun.value)
</script>
<p><p><hr class=tableborder1>
<table border="0" cellspacing="1" cellpadding="3" width="100%" class=tableborder2 align=center>
<tr><th height="22">模 板 编 辑 窗 口</th></tr>
<tr><td align=center width="98%" class=tablebody1>
<form name="vieweditTXT" ><TEXTAREA id="edittxt" ROWS="20" COLS="120" style="width:98%;overflow-y:visible; border:0px; overflow:visible; border-width:1px;border-style: dotted;color: #000000;" ></TEXTAREA></td></tr>
<tr>
	<td height="22" align=center class=tablebody2>
	<input type="button" name="editbutton" value="[保存修改]" onclick="selfedit()">
	<input type="button" name="close" value="[关  闭]" onclick="window.close()">
	<!-- <button onclick="edittxt.readOnly=true">ReadOnly</button> -->
	</td>
</tr>
</form>
</table>
<script>
document.vieweditTXT.edittxt.value=self.opener.txtRun.value
function selfedit(){
var n=opener.txtRun
self.opener.txtRun.value=vieweditTXT.edittxt.value;
//alert(""); 
self.close();
//return false;
}
</script>

<%
Dvbbs.PageEnd()
%>