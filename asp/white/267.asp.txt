<% Option Explicit %>
<!--#include file = "../asp/config.asp"-->

<%
'######################################
' eWebEditor v5.5 - Advanced online web based WYSIWYG HTML editor.
' Copyright (c) 2003-2008 eWebSoft.com
'
' For further information go to http://www.ewebsoft.com/
' This copyright notice MUST stay intact for use.
'######################################
%>

<%

Dim sAction, sErrMsg
Dim s_Usr, s_Pwd
sAction = UCase(Trim(Request("action")))
sErrMsg = ""

Select Case sAction
Case "LOGIN"
	s_Usr = Trim(Request("usr"))
	s_Pwd = Trim(Request("pwd"))
	If s_Usr = sUsername And s_Pwd = sPassword Then
		Session("eWebEditor_User") = s_Usr
		Response.Redirect "default.asp"
		Response.End
	End If
	sErrMsg = "提示：用户名或密码错误！"
Case "OUT"
	Session("eWebEditor_User") = ""
End Select

%>

<HTML>
<HEAD>
<TITLE>eWebEditor在线编辑器 - 后台管理</TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<style>
body,td,a,p,input{font-size:9pt}
body {margin:0px;background-color:#d9ddf7}
.c92 {FONT-SIZE: 9pt; COLOR: #003366; LINE-HEIGHT: 150%}
A:hover {COLOR: #ff9900}
A:link {COLOR: #003366}
.input {BORDER-RIGHT: #000000 1px solid; BORDER-TOP: #000000 1px solid; BORDER-LEFT: #000000 1px solid; BORDER-BOTTOM: #000000 1px solid}
</style>

<SCRIPT language=JavaScript>
function checkForm(){
	var frm = document.loginform
	if(frm.usr.value == ""){
		alert('用户名不允许为空');
		frm.usr.focus();
		return false;
	}
	if(frm.pwd.value == ""){
		alert('用户密码不允许为空');
		frm.pwd.focus();
		return false;
	}
	frm.submit()
}
</SCRIPT>

</head>
<BODY onload=document.loginform.usr.focus()>
<BR><BR>
<TABLE cellSpacing=0 cellPadding=0 width=500 align=center border=0>
  <TBODY>
  <TR>
    <TD height=60></TD></TR></TBODY></TABLE>
<TABLE cellSpacing=0 cellPadding=0 width=732 align=center border=0>
  <TBODY>
  <TR>
    <TD colSpan=7><IMG height=1 alt="" src="images/spacer.gif" width=718></TD>
    <TD rowSpan=6>&nbsp; </TD>
    <TD><IMG height=1 alt="" src="images/spacer.gif" width=1></TD></TR>
  <TR>
    <TD vAlign=bottom colSpan=3 rowSpan=2><IMG height=201 alt="" src="images/2_10.gif" width=341></TD>
    <TD vAlign=bottom colSpan=2><IMG height=108 alt="" src="images/2_11.gif" width=295></TD>
    <TD colSpan=2>&nbsp; </TD>
    <TD><IMG height=110 alt="" src="images/spacer.gif" width=1></TD></TR>
  <TR>
    <TD background=images/1_12.gif colSpan=4>
      <TABLE cellSpacing=0 cellPadding=3 width="50%" border=0>
        <FORM onkeydown="if(event.keyCode==13) return checkForm()" name=loginform action="?action=login" method=post>
        <TBODY>
        <TR>
          <TD class=c92 width="24%">用户名</TD>
          <TD width="76%"><INPUT class=input size=16 name=usr> </TD></TR>
        <TR>
          <TD class=c92 width="24%">密　码</TD>
          <TD width="76%"><INPUT class=input type=password size=16 name=pwd> </TD></TR>
        <TR>
          <TD width="24%">&nbsp;</TD>
          <TD width="76%">
            <DIV align=left>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<IMG style="CURSOR: hand" onclick="return checkForm()" src="images/login.gif" border=0> 
        </DIV></TD></TR></FORM></TBODY></TABLE></TD>
    <TD><IMG height=93 alt="" src="images/spacer.gif" width=1></TD></TR>
  <TR>
    <TD width=254 background=images/1_13.gif colSpan=2 height=161><TABLE cellSpacing=0 cellPadding=0 width="100%" border=0>
        <TBODY>
        <TR>
          <TD height=50></TD></TR>
        <TR>
          <TD align=right></TD></TR>
        <TR>
          <TD height=20></TD></TR>
        <TR>
          <TD align=right></TD></TR></TBODY></TABLE></TD>
    <TD width=387 background=images/1_14.gif colSpan=4 height=212 
    rowSpan=2></TD>
    <TD rowSpan=3><IMG height=242 alt="" src="images/spacer.gif" width=77></TD>
    <TD><IMG height=161 alt="" src="images/spacer.gif" width=1></TD></TR>
  <TR>
    <TD colSpan=2 rowSpan=2><IMG height=81 alt="" src="images/spacer.gif" width=254></TD>
    <TD><IMG height=51 alt="" src="images/spacer.gif" width=1></TD></TR>
  <TR>
    <TD colSpan=4><IMG height=30 alt="" src="images/spacer.gif" width=387></TD>
    <TD><IMG height=30 alt="" src="images/spacer.gif" width=1></TD></TR>
  <TR>
    <TD><IMG height=1 alt="" src="images/spacer.gif" width=195></TD>
    <TD><IMG height=1 alt="" src="images/spacer.gif" width=59></TD>
    <TD><IMG height=1 alt="" src="images/spacer.gif" width=87></TD>
    <TD><IMG height=1 alt="" src="images/spacer.gif" width=214></TD>
    <TD><IMG height=1 alt="" src="images/spacer.gif" width=81></TD>
    <TD><IMG height=1 alt="" src="images/spacer.gif" width=5></TD>
    <TD><IMG height=1 alt="" src="images/spacer.gif" width=77></TD>
    <TD><IMG height=1 alt="" src="images/spacer.gif" width=14></TD>
    <TD></TD></TR></TBODY></TABLE>
</BODY></HTML>
