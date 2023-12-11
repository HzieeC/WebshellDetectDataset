<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<%
Dvbbs.LoadTemplates("")
Dvbbs.Stats=Dvbbs.Forum_Info(0)&"¹ÜÀí°ïÖú"
Dvbbs.Head
%>
<br>
<p><p>
<table border="0" cellspacing="1" cellpadding="5" class=tableborder1 align=center style="width:90%">
<tr><th height="24"><%=Dvbbs.Stats%></th></tr>
<tr><td width="100%" class=tablebody1>
<script>
document.write(opener.txtRun.value)
</script>
</td></tr>
<tr>
<td height="22" align=center class=tablebody2>
<input type="button" name="close" value="[¹Ø  ±Õ]" onclick="window.close()">
</td>
</tr>
</table>
<br>
</body>
</html>
<%Dvbbs.PageEnd()%>