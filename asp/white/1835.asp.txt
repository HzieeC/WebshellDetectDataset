<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/Dv_ClsOther.asp"-->
<%
PageMain()
Dvbbs.PageEnd()
Sub PageMain()
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="content-type" content="text/html; charset=gb2312" />
<TITLE>左右分栏</TITLE>
</head>
<body>
<script type="text/javascript">
if (document.getElementById){ //DynamicDrive.com change
document.write('<style type="text/css">\n')
document.write('.submenu{display: none;}\n')
document.write('</style>\n')
}

function SwitchMenu(obj){
	if(document.getElementById){
	var el = document.getElementById(obj);
	var ar = document.getElementById("masterdiv").getElementsByTagName("span"); //DynamicDrive.com change
		if(el.style.display != "block"){ //DynamicDrive.com change
			for (var i=0; i<ar.length; i++){
				if (ar[i].className=="submenu") //DynamicDrive.com change
				ar[i].style.display = "none";
			}
			el.style.display = "block";
		}else{
			el.style.display = "none";
		}
	}
}
</script>
<%
	BoardMenu()
%>

</body>
</html>
<%

End Sub

Sub BoardMenu()
	Dim node,BoardNode,XMLDOM
	Set XMLDOM=Application(Dvbbs.CacheName&"_boardlist").cloneNode(True)
	For each node in XMLDOM.documentElement.getElementsByTagName("board[@depth <= 1]")
		If node.attributes.getNamedItem("hidden").text="1" and Dvbbs.GroupSetting(37)="0" Then
			node.parentNode.removeChild(node)
		End If
		node.removeAttribute "indeximg"
		node.removeAttribute "readme"
	Next
	XMLDOM.documentElement.setAttribute "forum_name",Dvbbs.Forum_Info(0)
	TransNode(XMLDOM)
	Set XMLDOM = Nothing

End Sub

Sub TransNode(XmlDoc)
	'XSLT模板转换开始
	Dim Xmlskin,Proc,XmlStyle
	Set Xmlskin = Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	Dvbbs.LoadTemplates("")
	If Not (Xmlskin.loadxml(Dvbbs.mainhtml(23))) Then
		Response.Write "模板数据出错，请与管理员联系！"
		Response.End
	End If
	Set XMLStyle=Dvbbs.iCreateObject("msxml2.XSLTemplate" & MsxmlVersion)
	XMLStyle.stylesheet=Xmlskin
	Set Proc=XMLStyle.createProcessor()
	Proc.input = XmlDoc
  	proc.transform()
  	Response.Write Dvbbs.ArchiveHtml(proc.output)
	Set XmlStyle = Nothing
	Set Xmlskin = Nothing
End Sub
%>