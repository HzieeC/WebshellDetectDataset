<%
Dim OK,xiaoweimanage
OK=session("xiaoweiAdmin")
xiaoweimanage=Request.Cookies("xiaoweimanage")("UserName")
if OK="" and xiaoweimanage="" then
	Response.Write("<script language=javascript>this.top.location.href='Admin_Login.asp';</script>")
	Response.end
end if
%>