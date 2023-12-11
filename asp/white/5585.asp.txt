<%@ CODEPAGE=65001 %>
<%
'///////////////////////////////////////////////////////////////////////////////
'// 插件应用:    Z-Blog 1.5及以上的版本
'// 插件制作:    williamlong(http://www.williamlong.info)
'// 备    注:    反垃圾留言的插件代码
'// 最后修改：   2006-6-27
'// 最后版本:    1.0.0
'///////////////////////////////////////////////////////////////////////////////
%>
<% Option Explicit %>
<% On Error Resume Next %>
<% Response.Charset="UTF-8" %>
<% Response.Buffer=True %>
<!-- #include file="../../c_option.asp" -->
<!-- #include file="../../function/c_function.asp" -->
<!-- #include file="../../function/c_function_md5.asp" -->
<!-- #include file="../../function/c_system_lib.asp" -->
<!-- #include file="../../function/c_system_base.asp" -->
<!-- #include file="../../function/c_system_event.asp" -->
<!-- #include file="../../function/c_system_plugin.asp" -->
<%

Call System_Initialize()

'检查非法链接
Call CheckReference("")

'检查权限
If BlogUser.Level>1 Then Call ShowError(6) 

If CheckPluginState("Totoro")=False Then Call ShowError(48)

	Dim strContent
	strContent=LoadFromFile(BlogPath & "/PLUGIN/totoro/include.asp","utf-8")

	Dim strZC_TOTORO_TB_Page_Value
	strZC_TOTORO_TB_Page_Value=Request.Form("strZC_TOTORO_TB_Page_Value")
	Call SaveValueForSetting(strContent,True,"Numeric","TOTORO_TB_Page_Value",strZC_TOTORO_TB_Page_Value)
	
	Dim strZC_TOTORO_INTERVAL_VALUE
	strZC_TOTORO_INTERVAL_VALUE=Request.Form("strZC_TOTORO_INTERVAL_VALUE")
	Call SaveValueForSetting(strContent,True,"Numeric","TOTORO_INTERVAL_VALUE",strZC_TOTORO_INTERVAL_VALUE)

	Dim strZC_TOTORO_BADWORD_VALUE
	strZC_TOTORO_BADWORD_VALUE=Request.Form("strZC_TOTORO_BADWORD_VALUE")
	Call SaveValueForSetting(strContent,True,"Numeric","TOTORO_BADWORD_VALUE",strZC_TOTORO_BADWORD_VALUE)

	Dim strZC_TOTORO_HYPERLINK_VALUE
	strZC_TOTORO_HYPERLINK_VALUE=Request.Form("strZC_TOTORO_HYPERLINK_VALUE")
	Call SaveValueForSetting(strContent,True,"Numeric","TOTORO_HYPERLINK_VALUE",strZC_TOTORO_HYPERLINK_VALUE)

	Dim strZC_TOTORO_NAME_VALUE
	strZC_TOTORO_NAME_VALUE=Request.Form("strZC_TOTORO_NAME_VALUE")
	Call SaveValueForSetting(strContent,True,"Numeric","TOTORO_NAME_VALUE",strZC_TOTORO_NAME_VALUE)

	Dim strZC_TOTORO_LEVEL_VALUE
	strZC_TOTORO_LEVEL_VALUE=Request.Form("strZC_TOTORO_LEVEL_VALUE")
	Call SaveValueForSetting(strContent,True,"Numeric","TOTORO_LEVEL_VALUE",strZC_TOTORO_LEVEL_VALUE)
	
	Dim strZC_TOTORO_SV_THRESHOLD
	strZC_TOTORO_SV_THRESHOLD=Request.Form("strZC_TOTORO_SV_THRESHOLD")
	Call SaveValueForSetting(strContent,True,"Numeric","TOTORO_SV_THRESHOLD",strZC_TOTORO_SV_THRESHOLD)
	
	Dim bolTOTORO_DEL_DIRECTLY
	bolTOTORO_DEL_DIRECTLY=Request.Form("bolTOTORO_DEL_DIRECTLY")
	Call SaveValueForSetting(strContent,True,"Boolean","TOTORO_DEL_DIRECTLY",bolTOTORO_DEL_DIRECTLY)

	Dim strZC_TOTORO_BADWORD_LIST
	strZC_TOTORO_BADWORD_LIST=Replace(Replace(Request.Form("strZC_TOTORO_BADWORD_LIST"),vbCrlf,""),vbLf,"")
	Call SaveValueForSetting(strContent,True,"String","TOTORO_BADWORD_LIST",strZC_TOTORO_BADWORD_LIST)

	Call SaveToFile(BlogPath & "/PLUGIN/totoro/include.asp",strContent,"utf-8",False)


Call System_Terminate()

If Err.Number<>0 then
  Call ShowError(0)
End If
%>
<script type="text/javascript">window.location="setting.asp"</script>
