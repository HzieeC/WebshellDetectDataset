<%
if session("AdminName") = "" then
    response.Redirect "Login.asp"
end if
%>
<%

%>
<html>
<head>
<meta http-equiv=Content-Type content=text/html;charset=gb2312>
<title>企业网站管理系统</title>
<style type="text/css">
.navPoint {COLOR: white; CURSOR: hand; FONT-FAMILY: Webdings; FONT-SIZE: 9pt}
.a2{BACKGROUND-COLOR: A4B6D7;}
</style>
</head>
<%
select case Request("menu")
case ""
main
case "top"
top
end select
%>
<% sub top %>
<BODY leftMargin=0 topMargin=0 rightMargin=0>
<CENTER>
<TABLE style="BORDER-COLLAPSE: collapse" cellSpacing=0 cellPadding=0 width="100%" border=0>
<TR>
<TD align=center width="100%" height=25 style="BACKGROUND-IMAGE: url(image/titlebg.gif); COLOR: #330099; font: 10.5pt"
><B>企业网站管理系统</B></TD>
</TR>
</TABLE>
</CENTER>
</body>
<% end sub %>

<% sub main %>
<body style="MARGIN: 0px" scroll=no onResize=javascript:parent.carnoc.location.reload()>
<script>
if(self!=top){top.location=self.location;}
function switchSysBar(){
if (switchPoint.innerText==3){
switchPoint.innerText=4
document.all("frmTitle").style.display="none"
}else{
switchPoint.innerText=3
document.all("frmTitle").style.display=""
}}
</script>

<table border="0" cellPadding="0" cellSpacing="0" height="100%" width="100%">
  <tr>
    <td align="middle" noWrap vAlign="center" id="frmTitle">
    <iframe frameBorder="0" id="carnoc" name="carnoc" scrolling=no src="LeftTree.asp" style="HEIGHT: 100%; VISIBILITY: inherit; WIDTH: 170px; Z-INDEX: 2">
    </iframe>
    </td>
    <td bgcolor="A4B6D7" style="WIDTH: 9pt">
    <table border="0" cellPadding="0" cellSpacing="0" height="100%">
      <tr>
        <td style="HEIGHT: 100%" onClick="switchSysBar()">
        <font style="FONT-SIZE: 9pt; CURSOR: default; COLOR: #ffffff">
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <span class="navPoint" id="switchPoint" title="关闭/打开左栏">3</span><br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        屏幕切换 </font></td>
      </tr>
    </table>
    </td>
		<td style="WIDTH: 100%">
		<iframe frameborder="0" id="main" name="top" scrolling="no" scrolling="yes" src="?menu=top" style="HEIGHT: 4%; VISIBILITY: inherit; WIDTH: 100%; Z-INDEX: 1">
		</iframe>
		<iframe frameborder="0" id="main" name="main" scrolling="yes" src="sysadmin_view.asp" style="HEIGHT: 96%; VISIBILITY: inherit; WIDTH: 100%; Z-INDEX: 1"></iframe></td>
  </tr>
</table>
<script>
  if(window.screen.width<'1024'){switchSysBar()}
</script>
</body>
<%
end sub

Function IsObjInstalled(strClassString)
On Error Resume Next
IsObjInstalled = False
Err = 0
Dim xTestObj
Set xTestObj = Server.CreateObject(strClassString)
If 0 = Err Then IsObjInstalled = True
Set xTestObj = Nothing
Err = 0
End Function
%>
</html>