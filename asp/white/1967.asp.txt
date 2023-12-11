<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<%
GetRssXml()
Dvbbs.PageEnd()
Sub GetRssXml()
	Dim GetUrl
	Dim XmlHttp,XmlDom
	GetUrl = Request.QueryString("url")
	If GetUrl="" Then
		Exit Sub
	End If
	Set XmlHttp = Dvbbs.iCreateObject("Microsoft.XMLHTTP")
	XmlHttp.Open "get",GetUrl,false
	XmlHttp.setRequestHeader "Content-Type", "application/x-www-form-urlencoded"
	'XmlHttp.SetRequestHeader "content-type", "text/xml"
	XmlHttp.send()

	Set XmlDom = Dvbbs.CreateXmlDoc("msxml2.FreeThreadedDOMDocument"& MsxmlVersion)
	XmlDom.async = True
	If XmlDom.Load(xmlhttp.responseXML) Then
		PrintXmldb(XmlDom)
		'TransformNode(XmlDom)
	End If
	Set XmlHttp = Nothing
	Set XmlDom = Nothing
End Sub

Sub PrintXmldb(XmlDoc)
	Response.ContentType="text/xml"
	Response.Write "<?xml version=""1.0"" encoding=""gb2312""?>"
	Response.Write XmlDoc.documentElement.xml
End Sub
%>