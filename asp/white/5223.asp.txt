<!--#include file="../inc/AspCms_SettingClass.asp" -->
<%
dim action : action=getform("action","get")
if action = "reg" then
	addUser()
elseif action = "editpass" then
	editUser()
else
	echoContent()
end if

Sub editPass
	dim LoginName,userPass,reuserPass
	LoginName=trim(rCookie("loginName"))	
	userPass=getForm("userPass","post")	
	reuserPass=getForm("reuserPass","post")		
	
	if userPass<>reuserPass then alertMsgAndGo "两次输入密码不相同","-1" 		
	'die  "update {prefix}User set [Password]='"&md5(userPass,16)&"' where LoginName='"&LoginName&"'"
	conn.Exec "update {prefix}User set [Password]='"&md5(userPass,16)&"' where LoginName='"&LoginName&"'","exe"	
	alertMsgAndGo "密码修改成功","editPass.asp"
End Sub

Sub editUser
	dim LoginName,userPass,reuserPass,Email,Mobile,Address,PostCode,Gender,QQ,TrueName,Phone
	LoginName=trim(rCookie("loginName"))	
	userPass=getForm("userPass","post")	
	reuserPass=getForm("reuserPass","post")		
	
	Email=filterPara(getForm("Email","post"))
	Mobile=filterPara(getForm("Mobile","post"))
	Address=filterPara(getForm("Address","post"))
	PostCode=filterPara(getForm("PostCode","post"))
	Gender=filterPara(getForm("Gender","post"))
	QQ=filterPara(getForm("QQ","post"))
	TrueName=filterPara(getForm("TrueName","post"))
	Phone=filterPara(getForm("Phone","post"))

	
	if userPass<>reuserPass then alertMsgAndGo "两次输入密码不相同","-1" 	
	
	dim passStr
	if not isnul(userPass) then passStr="[Password]='"&md5(userPass,16)&"',"	

	Conn.Exec"update {prefix}User set "&passStr&" Email='"&Email&"',QQ='"&QQ&"',Mobile='"&Mobile&"',Address='"&Address&"',PostCode='"&PostCode&"',Gender="&Gender&",Phone='"&Phone&"',TrueName='"&TrueName&"' where LoginName='"&LoginName&"'","exe"	
	alertMsgAndGo "修改成功","editPass.asp"	
End Sub

Sub echoContent()
	dim templateobj,templatePath : set templateobj = new TemplateClass	
	templatePath=sitePath&"/"&"templates/"&setting.defaultTemplate&"/"&setting.htmlFilePath&"/reg.html"	
	'die templatePath
	if not CheckTemplateFile(templatePath) then echo "reg.html"&err_16
	
	
	with templateObj 
		.content=loadFile(templatePath) 
		.parseHtml()		
		.parseCommon
		echo .content 
	end with	
	set templateobj =nothing : terminateAllObjects
End Sub



Sub addUser
	'dim UserID,GroupID,LanguageID,SceneID,LoginName,Password,PswQuestion,PswAnswer,UserStatus,RegTime,RegIP,LastLoginIP,LastLoginTime,LoginCount,TrueName,Gender,Birthday,Country,Province,City,Address,PostCode,Phone,Mobile,Email,QQ,MSN,Permissions,AdminDesc

	Dim LoginName,Password,verifyPass,Email,Mobile,Address,PostCode,Gender,QQ,UserStatus,RegTime,RegIP,LastLoginIP,LastLoginTime,Birthday,Exp1,Exp2,Exp3,GroupID,TrueName,Phone
	if getForm("code","post")<>Session("Code") then alertMsgAndGo "验证码不正确","-1"

	LoginName=filterPara(getForm("LoginName","post"))
	Password=filterPara(getForm("userPass","post"))
	verifyPass=filterPara(getForm("verifyPass","post"))
	Email=filterPara(getForm("Email","post"))
	Mobile=filterPara(getForm("Mobile","post"))
	Address=filterPara(getForm("Address","post"))
	PostCode=filterPara(getForm("PostCode","post"))
	Gender=1
	Gender=filterPara(getForm("Gender","post"))
	QQ=filterPara(getForm("QQ","post"))
	Phone=filterPara(getForm("Phone","post"))
	TrueName=filterPara(getForm("TrueName","post"))
	UserStatus=1
	RegTime=now()
	RegIP=getip()
	GroupID=3

	
	if isnul(LoginName) then alertMsgAndGo "用户名不能为空","-1"
	if Conn.Exec("select count(*) from {prefix}User where LoginName='"&LoginName&"'","r1")(0) >0 then alertMsgAndGo "该用户名已被注册","-1"
	
	if isnul(Password) then alertMsgAndGo "密码不能为空","-1"
	if isnul(verifyPass) then alertMsgAndGo "确认密码不能为空","-1"
	if Password<>verifyPass then alertMsgAndGo "两次输入密码不相同","-1"
	
	Password=md5(Password,16)
	Conn.Exec"insert into {prefix}User(LoginName,[Password],Email,Mobile,Address,PostCode,Gender,QQ,UserStatus,RegIP,RegTime,GroupID,TrueName,Phone) values('"&LoginName&"','"&Password&"','"&Email&"','"&Mobile&"','"&Address&"','"&PostCode&"',"&Gender&",'"&QQ&"',"&UserStatus&",'"&RegIP&"','"&RegTime&"',"&GroupID&",'"&TrueName&"','"&Phone&"')","exe"
	
	alertMsgAndGo "注册成功！",sitePath&setting.languagepath&"member/login.asp"
End Sub
%>
