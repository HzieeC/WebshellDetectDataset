<%

'注册插件
Call RegisterPlugin("pingtool","ActivePlugin_pingtool")

Dim PingTool_objArticle

Dim PingTool_PingEnable

Dim PingTool_TBContent


'具体的接口挂接
Function ActivePlugin_PingTool() 

	'挂上接口
	Call Add_Action_Plugin("Action_Plugin_ArticlePst_Begin","PingTool_PingEnable=Request.Form(""PingTool_PingEnable""):PingTool_TBContent=Request.Form(""PingTool_TBContent""):Call PingTool_Main()")

	Call Add_Action_Plugin("Action_Plugin_Edit_Begin","Call PingTool_addForm()")
	Call Add_Action_Plugin("Action_Plugin_Edit_Fckeditor_Begin","Call PingTool_addForm()")


End Function


Function PingTool_addForm()

Call Add_Response_Plugin("Response_Plugin_Edit_Form2","<div class=""anti_normal"" style=""width:725px""><p><a href=""#"" onclick=""this.style.display='none';document.getElementById('divPingTool').style.display='block';"">[PING中心和引用通告发送]</a></p></div><div id=""divPingTool"" class=""normal"" style=""display:none;width:725px""><p>发送引用通告:输入引用通告的URL地址,每行表示一个地址.<br/><textarea style=""width:100%"" rows=""3""  name=""PingTool_TBContent"" id=""PingTool_TBContent""></textarea></p><p><input type=""checkbox""  name=""PingTool_PingEnable"" id=""PingTool_PingEnable"" onclick="""" value=""True""/><label for=""PingTool_PingEnable"">发布文章同时通知Ping中心.</label></p></div>")


End Function


Function PingTool_getArticle(ByRef objArticle) 

	Set PingTool_objArticle=objArticle

End Function


Function PingTool_gotoPingTB() 

	If PingTool_objArticle.ID>0 Then

		If Replace(Replace(Replace(PingTool_TBContent,vbCr,""),vbLf,"")," ","")<>"" Or PingTool_PingEnable=True Then

		Response.Redirect "plugin/PingTool/send.asp?id=" & PingTool_objArticle.ID & "&ping=" & Server.URLEncode(PingTool_PingEnable) & "&tbs=" & Server.URLEncode(PingTool_TBContent)

		End If

	End If

End Function


Function PingTool_Main()

	If IsEmpty(PingTool_PingEnable)=True Then
		PingTool_PingEnable=False
	Else
		PingTool_PingEnable=True
	End If

	Call Add_Filter_Plugin("Filter_Plugin_PostArticle_Core","PingTool_getArticle")

	Call Add_Action_Plugin("Action_Plugin_ArticlePst_Succeed","Call PingTool_gotoPingTB()")

End Function

%>