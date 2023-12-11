<%
'///////////////////////////////////////////////////////////////////////////////
'// 插件应用:    Z-Blog 1.7
'// 插件制作:    
'// 备    注:    
'// 最后修改：   
'// 最后版本:    
'///////////////////////////////////////////////////////////////////////////////



Const TOTORO_SV_THRESHOLD = 50

Const TOTORO_TB_PAGE_VALUE = 0
Const TOTORO_LEVEL_VALUE = 100
Const TOTORO_NAME_VALUE = 45
Const TOTORO_HYPERLINK_VALUE = 10
Const TOTORO_BADWORD_VALUE = 50
Const TOTORO_BADWORD_LIST = "虚拟主机|域名注册|服务器托管|hosting|poker|免费铃声|免费彩信|铃声下载|搜索引擎营销|数据恢复|彩票软件|手机图片|魔兽金币|交友中心|成人用品|私服|企业黄页|出租|显示屏|投影仪|群发|翻译公司|留学咨询|外挂下载|硬盘录像机|google排名|注册香港公司|婚庆公司|投影幕|培养箱|花店|一号通|印刷公司|打包机|封口机|管件|砂机|打标机|升降机"
Const TOTORO_INTERVAL_VALUE = 25
Const TOTORO_INTERVAL_HOUR = 1
Const TOTORO_DEL_DIRECTLY = False

Dim Totoro_SV
Totoro_SV=0

Dim Totoro_SpamCount_Comment
Dim Totoro_SpamCount_TrackBack

'注册插件
Call RegisterPlugin("Totoro","ActivePlugin_Totoro")


'具体的接口挂接
Function ActivePlugin_Totoro() 

	'挂上接口
	'Filter_Plugin_PostComment_Core
	Call Add_Filter_Plugin("Filter_Plugin_PostComment_Core","Totoro_chkComment")
	'Filter_Plugin_PostTrackBack_Core
	Call Add_Filter_Plugin("Filter_Plugin_PostTrackBack_Core","Totoro_chkTrackBack")


	'Action_Plugin_Admin_Begin
	Call Add_Action_Plugin("Action_Plugin_Admin_Begin","If Request.QueryString(""act"")=""CommentMng"" Then Call Totoro_GetSpamCount_Comment() End If")
	Call Add_Action_Plugin("Action_Plugin_Admin_Begin","If Request.QueryString(""act"")=""TrackBackMng"" Then Call Totoro_GetSpamCount_TrackBack() End If")


	'网站管理加上二级菜单项
	Call Add_Response_Plugin("Response_Plugin_SettingMng_SubMenu",MakeSubMenu("TotoroⅡ设置","../plugin/totoro/setting.asp","m-left",False))

End Function



'*********************************************************
' 目的：    检查评论
'*********************************************************
Function Totoro_chkComment(ByRef objComment)

	Call Totoro_checkLevel(BlogUser.Level)
	Call Totoro_checkName(objComment.Author)
	Call Totoro_checkHyperLink(objComment.Content)
	Call Totoro_checkBadWord(objComment.Content & "&" & objComment.Author & "&" & objComment.HomePage & "&" & objComment.IP & "&" & objComment.Email)
	Call Totoro_checkInterval(Request.ServerVariables("REMOTE_ADDR"),now,true)

	If Totoro_SV>=TOTORO_SV_THRESHOLD Then

		Dim strKey
		strKey=Request.QueryString("key")

		Dim objArticle
		If objComment.AuthorID>0 Then
			objComment.Author=Users(objComment.AuthorID).Name
		End If

		If objComment.log_ID>0 Then
			Set objArticle=New TArticle
			If objArticle.LoadInfoByID(1-objComment.log_ID) Then
				If Not (strKey=objArticle.CommentKey) Then Call ShowError(43)
				If objArticle.Level<4 Then Call ShowError(44)
			End If
			Set objArticle=Nothing
		Else
			If Not (strKey=Left(MD5(ZC_BLOG_HOST & ZC_BLOG_CLSID & CStr(0) & CStr(Day(Now))),8)) Then Call ShowError(43)
		End If

		Dim objUser
		For Each objUser in Users
			If IsObject(objUser) Then
				If (UCase(objUser.Name)=UCase(objComment.Author)) And (objUser.ID<>objComment.AuthorID) Then Call ShowError(31)
			End If
		Next

		objComment.log_ID=-1-objComment.log_ID
		If objComment.Post Then
		End if

		If IsEmpty(Request.Form("inpAjax"))=False Then
			objComment.Content="你的评论已进入审核过程，请勿再次提交。"
			Call ReturnAjaxComment(objComment)
			Response.End
		End If

		Call Totoro_ExitError("你的评论已进入审核过程，请勿再次提交。")
	End If

End Function
'*********************************************************




'*********************************************************
' 目的：    检查引用
'*********************************************************
Function Totoro_chkTrackBack(ByRef objTrackBack)

	Call Totoro_checkHyperLink(objTrackBack.Title  &  "|" &  objTrackBack.Blog &  "|" & objTrackBack.Excerpt)
	Call Totoro_checkBadWord(objTrackBack.Title &  "|" & objTrackBack.Blog &  "|" & objTrackBack.Excerpt &  "|" & objTrackBack.IP &  "|" & objTrackBack.URL)
	Call Totoro_checkInterval(Request.ServerVariables("REMOTE_ADDR"),now,false)
	Call TOTORO_CheckTBSource(objTrackBack.URL)

	If Totoro_SV>=TOTORO_SV_THRESHOLD Then

		objTrackBack.log_ID=-1-objTrackBack.log_ID

		If objTrackBack.Post Then
		End if

		Call ExitError("你的引用已进入审核过程，请勿再次提交。")
	End If

End Function
'*********************************************************


'*********************************************************
' 目的：    检查TB原址页面中是否有本博客链接(by Zx.MYS)
'*********************************************************
Function TOTORO_CheckTBSource(ByVal URL)
If TOTORO_TB_Page_Value=0 or Totoro_SV >= TOTORO_SV_THRESHOLD Then Exit Function
dim TOTORO_xmlHTTP,TOTORO_TbPageContent
Dim TOTORO_lResolve,TOTORO_lConnect,TOTORO_lSend,TOTORO_lReceive '超时设置，单位：秒
TOTORO_lResolve=5 '解析地址（DNS）超时时间
TOTORO_lConnect=5 '链接超时时间
TOTORO_lSend=5 '发送请求时间
TOTORO_lReceive=15 '等待响应时间

dim i,TOTORO_HTTPRETRY    '后加入
TOTORO_HTTPRETRY = 3    '后加入, 重试次数

TOTORO_HTTPRETRY = TOTORO_HTTPRETRY-1    '调整重试次数
'On Error Resume Next    '后加入, 防报错. 相当于连接失败后直接+SV

for i=0 to TOTORO_HTTPRETRY    '重试n次, 在任一次连接成功后即跳出循环, 当然也可以用其它循环改写.
	Set TOTORO_xmlHTTP = Server.CreateObject("MSXML2.ServerXMLHTTP")
	TOTORO_xmlHTTP.setTimeouts TOTORO_lResolve*1000,TOTORO_lConnect*1000,TOTORO_lSend*1000,TOTORO_lReceive*1000
	TOTORO_xmlHTTP.Open "GET",URL,false
	TOTORO_xmlHTTP.SetRequestHeader "Content-type", "text/xml"
	TOTORO_xmlHTTP.Send

	If Err Then    '如果连接失败
	  Err.Clear    '清除本次错误
	  TOTORO_TbPageContent = ""    '链接失败后TOTORO_TbPageContent为空字符串, 可省略
	  set TOTORO_xmlhttp = nothing    '清空内存以便重试
	else    '非失败即成功
	  TOTORO_TbPageContent = TOTORO_xmlHTTP.ResponseText
	  Set TOTORO_xmlHTTP = Nothing
	  exit for    '跳出循环
	end if
next

If InStr(LCase(TOTORO_TbPageContent),LCase(ZC_BLOG_HOST))=0 then Totoro_SV=Totoro_SV + TOTORO_TB_Page_Value
End Function
'*********************************************************

Function Totoro_checkLevel(ByVal level)

	If TOTORO_LEVEL_VALUE=0 Then Exit Function

	If Level=1 Then
	Totoro_SV=Totoro_SV-TOTORO_LEVEL_VALUE*(4)
	ElseIf  Level=2 Then
	Totoro_SV=Totoro_SV-TOTORO_LEVEL_VALUE*(3)
	ElseIf  Level=3 Then
	Totoro_SV=Totoro_SV-TOTORO_LEVEL_VALUE*(2)
	ElseIf  Level=4 Then
	Totoro_SV=Totoro_SV-TOTORO_LEVEL_VALUE*(1)
	ElseIf  Level=5 Then
	Totoro_SV=Totoro_SV-TOTORO_LEVEL_VALUE*(0)
	End If
End Function


Function Totoro_checkName(ByVal name)

	If TOTORO_NAME_VALUE=0 Then Exit Function

	Dim i,s
	s=FilterSQL(name)

	Dim objRS
	Set objRS=objConn.Execute("SELECT COUNT([comm_ID]) FROM [blog_Comment] WHERE [log_ID]>=0 and [comm_Author] ='" & s & "'")
	If (Not objRS.bof) And (Not objRS.eof) Then
		i=objRS(0)
	End If
	Set objRS=Nothing

	If i>0 And i<=10   Then Totoro_SV=Totoro_SV-10-TOTORO_NAME_VALUE*(0)
	If i>10 And i<=20  Then Totoro_SV=Totoro_SV-10-TOTORO_NAME_VALUE*(1)
	If i>20 And i<=50 Then Totoro_SV=Totoro_SV-10-TOTORO_NAME_VALUE*(2)
	If i>50           Then Totoro_SV=Totoro_SV-10-TOTORO_NAME_VALUE*(3)

End Function


Function Totoro_checkBadWord(ByVal content)

	If Totoro_SV+TOTORO_BADWORD_VALUE=0 Then Exit Function

	Dim i,j
	j=0
    Dim strFilter
    strFilter = Split(TOTORO_BADWORD_LIST, "|")
	For i = 0 To UBound(strFilter)
		If strFilter(i)<>"" Then
			If InStr(LCase(content), LCase(strFilter(i))) > 0 Then
				j=j+1
			End If
		End If
    Next

	Totoro_SV=Totoro_SV+TOTORO_BADWORD_VALUE*j

End Function


Function Totoro_checkHyperLink(ByVal content)

	If TOTORO_HYPERLINK_VALUE=0 Then Exit Function

	Dim SRegExp,Matches
	Set SRegExp=New RegExp
	SRegExp.IgnoreCase =True
	SRegExp.Global=True
	SRegExp.Pattern="https:|http:"
	Set Matches = SRegExp.Execute(content)

	If Matches.count=0 Then
		Totoro_SV=Totoro_SV
	ElseIf  Matches.count=1 Then
		Totoro_SV=Totoro_SV+TOTORO_HYPERLINK_VALUE*(2-1)
	ElseIf  Matches.count=2 Then
		Totoro_SV=Totoro_SV+TOTORO_HYPERLINK_VALUE*(2*2-1)
	ElseIf  Matches.count=3 Then
		Totoro_SV=Totoro_SV+TOTORO_HYPERLINK_VALUE*(2*2*2-1)
	ElseIf  Matches.count=4 Then
		Totoro_SV=Totoro_SV+TOTORO_HYPERLINK_VALUE*(2*2*2*2-1)
	ElseIf  Matches.count=5 Then
		Totoro_SV=Totoro_SV+TOTORO_HYPERLINK_VALUE*(2*2*2*2*2-1)
	Else
		Totoro_SV=Totoro_SV+TOTORO_HYPERLINK_VALUE*(2*2*2*2*2*2-1)
	End If

	Set SRegExp=Nothing

End Function


Function Totoro_checkInterval(ByVal ip,ByVal posttime,ByVal iscomment)

	If TOTORO_INTERVAL_VALUE=0 Then Exit Function

	Dim i,j,t,s,m,n
	Dim objRS
	If iscomment Then
		m="SELECT COUNT([comm_ID]) FROM [blog_Comment] WHERE [comm_IP] ='" & ip & "'"
		n="SELECT [comm_PostTime] FROM [blog_Comment] WHERE [comm_IP] ='" & ip & "'"
	Else
		m="SELECT COUNT([tb_ID]) FROM [blog_TrackBack] WHERE [tb_IP] ='" & ip & "'"
		n="SELECT [tb_PostTime] FROM [blog_TrackBack] WHERE [tb_IP] ='" & ip & "'"
	End If

	Set objRS=objConn.Execute(m)
	If (Not objRS.bof) And (Not objRS.eof) Then
		i=objRS(0)
	End If
	Set objRS=Nothing
	s=0
	If i>0 Then
		Set objRS=objConn.Execute(n)
			If (Not objRS.bof) And (Not objRS.eof) Then
				Do While Not objRS.eof
					t=objRS("comm_PostTime")
					If DateDiff("h",t,posttime)<TOTORO_INTERVAL_HOUR Then
						j=j+1
						If     DateDiff("n",t,posttime)>((TOTORO_INTERVAL_HOUR*60)\5)*4 Then
							s=s+(TOTORO_INTERVAL_VALUE\5)*1
						ElseIf DateDiff("n",t,posttime)>((TOTORO_INTERVAL_HOUR*60)\5)*3 Then
							s=s+(TOTORO_INTERVAL_VALUE\5)*2
						ElseIf DateDiff("n",t,posttime)>((TOTORO_INTERVAL_HOUR*60)\5)*2 Then
							s=s+(TOTORO_INTERVAL_VALUE\5)*3
						ElseIf DateDiff("n",t,posttime)>((TOTORO_INTERVAL_HOUR*60)\5)*1 Then
							s=s+(TOTORO_INTERVAL_VALUE\5)*4
						ElseIf DateDiff("n",t,posttime)>((TOTORO_INTERVAL_HOUR*60)\5)*0 Then
							s=s+(TOTORO_INTERVAL_VALUE\5)*5
						Else
							s=s+(TOTORO_INTERVAL_VALUE\5)*6
						End If
					End If
					objRS.MoveNext
				Loop
			End If
		Set objRS=Nothing
	End If

	Totoro_SV=Totoro_SV+s

End Function




'*********************************************************
' 目的：    错误退出
'*********************************************************
Function Totoro_ExitError(strInput)
	If IsEmpty(Request.Form("inpAjax"))=False Then
		Call RespondError(vbObjectError+1,strInput)
		Response.End
	End If
	Response.Redirect ZC_BLOG_HOST & "function/c_error.asp?errorid=" & 0 & "&number=" & (vbObjectError+1) & "&description=" & Server.URLEncode(strInput) & "&source="
End Function
'*********************************************************


'*********************************************************
' 目的：    
'*********************************************************
Function Totoro_GetSpamCount_Comment()
	If IsEmpty(objConn)=True Then Exit Function
	Dim objRS1
	Set objRS1=objConn.Execute("SELECT COUNT([comm_ID]) FROM [blog_Comment] WHERE [log_ID]<0")
	If (Not objRS1.bof) And (Not objRS1.eof) Then
		Totoro_SpamCount_Comment="("&objRS1(0)&"条未审核的评论)"
	End If

	'评论管理加上二级菜单项
	Call Add_Response_Plugin("Response_Plugin_CommentMng_SubMenu",MakeSubMenu("审核评论" & Totoro_SpamCount_Comment,"../plugin/totoro/setting1.asp","m-left",False) & "<scr" & "ipt src=""../plugin/totoro/common.js"" type=""text/javascript""></scr" & "ipt><scr" & "ipt src=""../plugin/totoro/cmmng.js"" type=""text/javascript""></scr" & "ipt>")

End Function
'*********************************************************



'*********************************************************
' 目的：    
'*********************************************************
Function Totoro_GetSpamCount_TrackBack()
	If IsEmpty(objConn)=True Then Exit Function
	Dim objRS2
	Set objRS2=objConn.Execute("SELECT COUNT([tb_ID]) FROM [blog_TrackBack] WHERE [log_ID]<0")
	If (Not objRS2.bof) And (Not objRS2.eof) Then
		Totoro_SpamCount_TrackBack="("&objRS2(0)&"条未审核的引用)"
	End If
	'引用管理加上二级菜单项
	Call Add_Response_Plugin("Response_Plugin_TrackBackMng_SubMenu",MakeSubMenu("审核引用" & Totoro_SpamCount_TrackBack,"../plugin/totoro/setting2.asp","m-left",False) & "<scr" & "ipt src=""../plugin/totoro/common.js""  type=""text/javascript""></scr" & "ipt><scr" & "ipt src=""../plugin/totoro/tbmng.js""  type=""text/javascript""></scr" & "ipt>")
End Function
'*********************************************************
%>