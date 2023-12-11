<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<%
'//*****************************************************************************//
''@	动网论坛首页调用
''@ 程序适用版本：DVBBS 8.3.0 
''@ 官方论坛地址：http://bbs.dvbbs.net http://www.aspsky.net
''@ 程序作者：	Written by Fssunwin
''@ 更新日期：	2005-04-16
'//*****************************************************************************//
Dim NewsConfigFile,XmlDoc
NewsConfigFile = "Dv_ForumNews/Dv_NewsSetting.config"
Dvbbs.LoadTemplates("")
Dvbbs.Stats = "DVBBS8.3.0 首页调用演示"
Dvbbs.Head()
DemoMain()
Dvbbs.Footer
Dvbbs.PageEnd()
Sub DemoMain()
	Dim BbsUrl,ScriptCode
	Dim Node,NewsCode,Childs
	BbsUrl = Dvbbs.Get_ScriptNameUrl
	LoadXml()
	Set NewsCode = XmlDoc.DocumentElement.SelectNodes("NewsCode")
	Childs = NewsCode.Length	'列表数
	%>
	<script language="JavaScript">
	<!--
	function oCopy(obj){ 
		obj.select(); 
		var js=obj.createTextRange(); 
		js.execCommand("Copy");
	}
	//-->
	</script>
	<table width="100%" border="0" cellpadding="0" cellspacing="0" background="admin/images/top_bg.gif" style="BORDER-BOTTOM: #135093 1px solid">
	<tr>
	<td width="145" background="admin/images/top_logo.gif" height="32"></td>
	<td width="*" align="center">
	<span style="float:right;border-bottom : 1px dotted buttonface;"><font style="color:#CCCCCC ;Font-size:8pt;">Copyright By Fssunwin &copy; 2005-4-18</font></span>
	<font style="FONT-SIZE: 18px;color:white;"><b><%=Dvbbs.Stats%></b></font>
	</td>
	</tr>
	</table>
	<BR>
	<%
	For Each Node in NewsCode
	ScriptCode = "<script src="""&BbsUrl&"Dv_News.asp?GetName="&Node.getAttribute("NewsName")&"""></script>"
	%>
	<table cellpadding="5" cellspacing="1" border="0" class="tableBorder2" align="center" style="width:500px;height:250px">
	<tr><th height="23"><%=Node.getAttribute("Intro")%>效果测试</th></tr>
	<tr>
	<td height="*" class="tablebody1" valign="top">
	<div class="quote">
	调用代码：
	<input name="NewScript" id="<%=Node.getAttribute("NewsName")%>" value="<%=Server.Htmlencode(ScriptCode)%>" size="100">
	<input type="button" value="拷贝调用代码" onclick="oCopy(document.getElementById('<%=Node.getAttribute("NewsName")%>'));">
	</div>
	<font color="gray">以下为查看的调用效果：</font>
	<hr size=1>
	<%=ScriptCode%>
	</td>
	</tr>
	</table>
	<p></p>
	<%
	Next
	Set XmlDoc = Nothing
End Sub

Sub LoadXml()
	NewsConfigFile = Server.MapPath(NewsConfigFile)
	Set XmlDoc = Dvbbs.iCreateObject("MSXML.DOMDocument")
	XmlDoc.Async = False
	If Not XmlDoc.load(NewsConfigFile) Then
		XmlDoc.loadxml "<?xml version=""1.0"" encoding=""gb2312""?><NewscodeInfo/>"
	End If
End Sub
%>


