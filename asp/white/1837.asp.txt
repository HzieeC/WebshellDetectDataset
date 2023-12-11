<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/dv_clsother.asp" -->
<%
Rem 页面说明,前台的注册设置管理页面,只有超级版主和管理员可以进入
If Not Dvbbs.Master and  Not Dvbbs.superboardmaster Then Response.redirect "showerr.asp?ErrCodes=<li>您没有权限进行用户注册管理。</li>&action=OtherErr"
Dim XMLDom,paramnode,page
Set XMLDom=Dvbbs.CreateXmlDoc("Msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
XMLDom.appendChild(XMLDom.createElement("xml"))
Set paramnode=XMLDom.documentElement.appendChild(XMLDom.createNode(1,"param",""))
paramnode.attributes.setNamedItem(XMLDom.createNode(2,"action","")).text=Request("action")
page=Request("page")
If page="" or Not IsNumeric(Page) Then page=1
paramnode.attributes.setNamedItem(XMLDom.createNode(2,"page","")).text=page
Dvbbs.LoadTemplates("query")
If Not Dvbbs.ChkPost() and Request("action") <> "" Then  Response.Redirect "userregmanager.asp"
Dvbbs.stats="用户注册管理"
Dvbbs.Nav
Dvbbs.Head_var 2,0,"",""
 Select Case Request("action")
 Case "setting"
 	loadsetting()
 Case "reglist"
 	loadreglist()
 Case "settingsave"
 	If Request.form("submit")<>"" Then
 		settingsave
 	Else
 		paramnode.attributes.setNamedItem(XMLDom.createNode(2,"action","")).text="err"
 	End If
 Case Else
 	Loadregpostinfolist()
 End Select
ShowHTML()
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub loadsetting()
	Dim XMLdoc,Rs
	Set XMLDoc=Dvbbs.CreateXmlDoc("Msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	Set Rs=Dvbbs.Execute("select Forum_Boards From Dv_setup")
	If Not XMLDoc.loadxml(Rs(0)) Then
		XMLDoc.LoadXML "<?xml version=""1.0""?><regsetting/>"
	End If
	XMLDom.documentElement.appendChild(XMLDoc.documentElement)
End Sub
Sub loadreglist()
	Dim XMLdoc,Rs
	Set XMLDoc=Dvbbs.CreateXmlDoc("Msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	Set Rs=Dvbbs.Execute("select Forum_Ad From Dv_setup")
	If Not XMLDoc.loadxml(Rs(0)) Then
		XMLDoc.LoadXML "<?xml version=""1.0""?><reglist/>"
	End If
	XMLDom.documentElement.appendChild(XMLDoc.documentElement)
End Sub
Sub Loadregpostinfolist()
	Dim XMLdoc,Rs
	Set XMLDoc=Dvbbs.CreateXmlDoc("Msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	Set Rs=Dvbbs.Execute("select Forum_BirthUser From Dv_setup")
	If Not XMLDoc.loadxml(Rs(0)) Then
		XMLDoc.LoadXML "<?xml version=""1.0""?><regpost/>"
	End If
	XMLDom.documentElement.appendChild(XMLDoc.documentElement)
End Sub
Sub ShowHTML()
	Dim xslt,proc,XMLStyle
	Set XMLStyle=Dvbbs.CreateXmlDoc("Msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XMLStyle.loadxml template.html(3)
	'XMLStyle.load Server.MapPath("inc/userregmanager.xslt")
	Set XSLT=Dvbbs.iCreateObject("Msxml2.XSLTemplate" & MsxmlVersion)
	XSLT.stylesheet=XMLStyle
	Set proc = XSLT.createProcessor()
	proc.input = XMLDom
  proc.transform()
  Response.Write  proc.output
	Set XMLDOM=Nothing
	Set XSLt=Nothing
	Set proc=Nothing	
End Sub
Sub settingsave()
	Dim Node,node1,i,iplist,node2,iplist1,XMLDom1
	Set XMLDom1=Dvbbs.CreateXmlDoc("Msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XMLDom1.appendChild(XMLDom1.createElement("regsetting"))
	Set Node=xmldom1.documentElement.appendChild(XMLDom1.createNode(1,"checkip",""))
	If Request.form("checkip")="1" Then
			Node.attributes.setNamedItem(XMLDom1.createNode(2,"use","")).text="1"
	Else
		Node.attributes.setNamedItem(XMLDom1.createNode(2,"use","")).text="0"
	End If
	Set node1=Node.appendChild(XMLDom1.createElement("iplist1"))
	For each iplist in split(Request.form("iplist1"),vbnewline)
		If iplist<>"" Then
			iplist1=Split(iplist,"=")
			If UBound(iplist1)>0 Then
			Set node2=node1.appendChild(XMLDom1.createNode(1,"ip",""))
			node2.text=Trim(iplist1(0))
			Node2.attributes.setNamedItem(XMLDom1.createNode(2,"description","")).text=Trim(iplist1(1))
			End If
		End If
	Next
	Set node1=Node.appendChild(XMLDom1.createElement("iplist2"))
	For each iplist in split(Request.form("iplist2"),vbnewline)
		If iplist<>"" Then
			iplist1=Split(iplist,"=")
			If UBound(iplist1)>0 Then
			Set node2=node1.appendChild(XMLDom1.createNode(1,"ip",""))
			node2.text=Trim(iplist1(0))
			Node2.attributes.setNamedItem(XMLDom1.createNode(2,"description","")).text=Trim(iplist1(1))
			End If
		End If
	Next
	If Request.form("postipinfo")="1" Then
			XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"postipinfo","")).text="1"
	Else
		XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"postipinfo","")).text="0"
	End If
	'If Request.form("checkproxy")="1" Then
	'		XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"checkproxy","")).text="1"
	'Else
	'	XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"checkproxy","")).text="0"
	'End If
	If Request.form("checknumeric")="1" Then
			XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"checknumeric","")).text="1"
	Else
		XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"checknumeric","")).text="0"
	End If
	If Request.form("checktime")="1" Then
			XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"checktime","")).text="1"
	Else
		XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"checktime","")).text="0"
	End If
	If Request.form("usevarform")="1" Then
			XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"usevarform","")).text="1"
	Else
		XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"usevarform","")).text="0"
	End If
	If Request.form("checkregcount")<>"" and IsNumeric(Request.form("checkregcount")) Then
			XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"checkregcount","")).text=Request.form("checkregcount")
	Else
		XMLDom1.documentElement.attributes.setNamedItem(XMLDom1.createNode(2,"checkregcount","")).text="0"
	End If
	Dvbbs.Execute("update dv_setup set Forum_Boards='"&Dvbbs.checkstr(XMLDom1.XML)&"'")
	Dvbbs.loadSetup()
End Sub
%>