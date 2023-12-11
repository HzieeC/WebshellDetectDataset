<%@ CODEPAGE=65001 %>
<%
'///////////////////////////////////////////////////////////////////////////////
'// 插件应用:    Z-Blog 1.7
'// 插件制作:    
'// 备    注:    
'// 最后修改：   
'// 最后版本:    
'///////////////////////////////////////////////////////////////////////////////
%>
<% Option Explicit %>
<%
On Error Resume Next
 %>
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

BlogTitle="TotoroⅡ（基于Totoro的Z-Blog的评论及引用管理审核系统增强版）"

%><!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="<%=ZC_BLOG_LANGUAGE%>" lang="<%=ZC_BLOG_LANGUAGE%>">
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta http-equiv="Content-Language" content="<%=ZC_BLOG_LANGUAGE%>" />
	<link rel="stylesheet" rev="stylesheet" href="../../CSS/admin.css" type="text/css" media="screen" />
	<script language="JavaScript" src="../../script/common.js" type="text/javascript"></script>
	<title><%=BlogTitle%></title>
</head>
<body>

			<div id="divMain">
<div class="Header"><%=BlogTitle%></div>

<div class="SubMenu"><span class="m-left m-now"><a href="setting.asp">TotoroⅡ设置</a></span><span class="m-left"><a href="setting1.asp">审核评论<%
	Dim objRS1
	Set objRS1=objConn.Execute("SELECT COUNT([comm_ID]) FROM [blog_Comment] WHERE [log_ID]<0")
	If (Not objRS1.bof) And (Not objRS1.eof) Then
		Response.Write "("&objRS1(0)&"条未审核的评论)"
	End If
%></a></span><span class="m-left"><a href="setting2.asp">审核引用<%
	Dim objRS2
	Set objRS2=objConn.Execute("SELECT COUNT([tb_ID]) FROM [blog_TrackBack] WHERE [log_ID]<0")
	If (Not objRS2.bof) And (Not objRS2.eof) Then
		Response.Write "("&objRS2(0)&"条未审核的引用)"
	End If
%></a></span></div>

<div id="divMain2">
<form id="edit" name="edit" method="post">
<%

	Dim tmpSng

	tmpSng=LoadFromFile(BlogPath & "/PLUGIN/totoro/include.asp","utf-8")

	Response.Write "<p><b>关于TotoroⅡ</b></p>"
	Response.Write "<p>Totoro是个采用评分机制的防止垃圾留言及引用的插件，原作<a href=""http://www.rainbowsoft.org/zblog/"" target=""_blank"">zx.asd</a>。<br/>TotoroⅡ是<a href=""http://ZxMYS.COM"" target=""_blank"">Zx.MYS</a>在Totoro的基础上修改而成的增强版，加入了诸多新特性，同时修正一些问题。</p>"
	Response.Write "<p>Spam Value(SV)初始值为0，经过相关运算后的SV分值越高Spam嫌疑越大，超过设定的阈值这条评论或Trackback就进入审核状态。</p>"

	Response.Write "<p></p>"
	Response.Write "<p><b>加分减分细则：</b></p>"
	
	Dim strZC_TOTORO_HYPERLINK_VALUE
	Call LoadValueForSetting(tmpSng,True,"Numeric","TOTORO_HYPERLINK_VALUE",strZC_TOTORO_HYPERLINK_VALUE)
	strZC_TOTORO_HYPERLINK_VALUE=TransferHTML(strZC_TOTORO_HYPERLINK_VALUE,"[html-format]")
	Response.Write "<p>1.评论和引用里有链接就加<input name=""strZC_TOTORO_HYPERLINK_VALUE"" style=""width:25px"" type=""text"" value=""" & strZC_TOTORO_HYPERLINK_VALUE & """/>分(默认：10)，每多一个链接SV翻倍加分</p>"
	
	Dim strTOTORO_INTERVAL_VALUE
	Call LoadValueForSetting(tmpSng,True,"Numeric","TOTORO_INTERVAL_VALUE",strTOTORO_INTERVAL_VALUE)
	strTOTORO_INTERVAL_VALUE=TransferHTML(strTOTORO_INTERVAL_VALUE,"[html-format]")
	Response.Write "<p>2.提交频率评分:基数为<input name=""strZC_TOTORO_INTERVAL_VALUE"" style=""width:25px"" type=""text"" value=""" & strTOTORO_INTERVAL_VALUE & """/>分(默认：25)，根据1小时内同一IP的评论和引用数量加分。(每条评论或引用最多加基数的五分之六，最少加基数的五分之一，按时间间隔递减。)</p>"
	
	Dim strTOTORO_BADWORD_VALUE
	Call LoadValueForSetting(tmpSng,True,"Numeric","TOTORO_BADWORD_VALUE",strTOTORO_BADWORD_VALUE)
	strTOTORO_BADWORD_VALUE=TransferHTML(strTOTORO_BADWORD_VALUE,"[html-format]")
	Response.Write "<p>3.评论和引用里的每一个黑词都加<input name=""strZC_TOTORO_BADWORD_VALUE"" style=""width:25px"" type=""text"" value=""" & strTOTORO_BADWORD_VALUE & """/>分(默认：50)</p>"
	
	Dim strTOTORO_LEVEL_VALUE
	Call LoadValueForSetting(tmpSng,True,"Numeric","TOTORO_LEVEL_VALUE",strTOTORO_LEVEL_VALUE)
	strTOTORO_LEVEL_VALUE=TransferHTML(strTOTORO_LEVEL_VALUE,"[html-format]")
	Response.Write "<p>4.用户信任度评分:基数为<input name=""strZC_TOTORO_LEVEL_VALUE"" style=""width:25px"" type=""text"" value=""" & strTOTORO_LEVEL_VALUE & """/>分(默认：100)，初级用户评论时SV减基数×1，中级用户SV减基数×2，高级用户减SV减基数×3，管理员SV减基数×4</p>"

	Dim strTOTORO_NAME_VALUE
	Call LoadValueForSetting(tmpSng,True,"Numeric","TOTORO_NAME_VALUE",strTOTORO_NAME_VALUE)
	strTOTORO_NAME_VALUE=TransferHTML(strTOTORO_NAME_VALUE,"[html-format]")
	Response.Write "<p>5.访客熟悉度评分:基数为<input name=""strZC_TOTORO_NAME_VALUE"" style=""width:25px"" type=""text"" value=""" & strTOTORO_NAME_VALUE & """/>分(默认：45)，同一访客在BLOG留言1-10条内的SV减10分,10-20条的SV减10分再减基数×1，20-50条的SV减10分再减基数×2，大于50条的SV减10分再减基数×3</p>"
	
	Dim strTOTORO_TB_Page_Value
	Call LoadValueForSetting(tmpSng,True,"Numeric","TOTORO_TB_Page_Value",strTOTORO_TB_Page_Value)
	strTOTORO_TB_Page_Value=TransferHTML(strTOTORO_TB_Page_Value,"[html-format]")
	Response.Write "<p>6.Trackback反向检查评分:收到Trackback后自动访问来源页面，如果来源页面没有指向本Blog的链接则SV加<input name=""strZC_TOTORO_TB_Page_Value"" style=""width:25px"" type=""text"" value=""" & strTOTORO_TB_Page_Value & """/>分(默认：0[即关闭此功能]，如果要开启推荐和阈值相同)。（此功能可能会耗费一定服务器带宽流量，慎用。）</p>"
	Response.Write "<p></p>"
	Response.Write "<p><b>相关设置</b></p>"
	Response.Write "<hr/>"
	
	Dim strZC_TOTORO_SV_THRESHOLD
	If LoadValueForSetting(tmpSng,True,"Numeric","TOTORO_SV_THRESHOLD",strZC_TOTORO_SV_THRESHOLD) Then
		strZC_TOTORO_SV_THRESHOLD=TransferHTML(strZC_TOTORO_SV_THRESHOLD,"[html-format]")
		Response.Write "<p>·设置系统审核阈值(默认50，阈值越小越严格，低于0则使游客的评论全进入审核):</p><p><input name=""strZC_TOTORO_SV_THRESHOLD"" style=""width:99%"" type=""text"" value=""" & strZC_TOTORO_SV_THRESHOLD & """/></p><p></p>"
	End If

	Dim strZC_TOTORO_BADWORD_LIST
	If LoadValueForSetting(tmpSng,True,"String","TOTORO_BADWORD_LIST",strZC_TOTORO_BADWORD_LIST) Then
		strZC_TOTORO_BADWORD_LIST=TransferHTML(strZC_TOTORO_BADWORD_LIST,"[html-format]")
		Response.Write "<p>·黑词列表(分隔符'|'):</p><p><textarea rows=""6"" name=""strZC_TOTORO_BADWORD_LIST"" style=""width:99%"" >"& strZC_TOTORO_BADWORD_LIST &"</textarea></p>"
	End If
	
	Response.Write "<p>·"
	Dim bolTOTORO_DEL_DIRECTLY
	Call LoadValueForSetting(tmpSng,True,"Boolean","TOTORO_DEL_DIRECTLY",bolTOTORO_DEL_DIRECTLY)
	Response.Write "<input name=""bolTOTORO_DEL_DIRECTLY"" id=""bolTOTORO_DEL_DIRECTLY"" type=""checkbox"" value=""True"""
	If bolTOTORO_DEL_DIRECTLY then
		Response.Write " checked=""checked"">"
	else
		Response.Write ">"
	End if
	
	Response.Write "<label for=""bolTOTORO_DEL_DIRECTLY"">点击[这是SPAM]按钮提取域名后直接删除评论/引用（若不删除则进入审核）</label></p><p></p>"
	Response.Write "<hr/>"
	Response.Write "<p><input type=""submit"" class=""button"" value="""& ZC_MSG087 &""" id=""btnPost"" onclick='document.getElementById(""edit"").action=""savesetting.asp"";' /></p>"

	
	'Response.Write "<br/><p><a target='_blank' href='http://bbs.rainbowsoft.org/viewthread.php?tid=11849'>Totoro的相关说明文档</a></p><br/>"


%>
</form>
</div>
</body>
</html>
<%
Call System_Terminate()

If Err.Number<>0 then
  Call ShowError(0)
End If
%>

