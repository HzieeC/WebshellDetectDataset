<!--#include file="../inc/AspCms_SettingClass.asp" -->
<%
dim action : action=getForm("action","get")
if action = "apply" then
	addApply()
else
	echoContent()
end if

Sub echoContent()
	dim id, sortID,SortAndID
	SortAndID=split(replaceStr(request.QueryString,FileExt,""),"_")
	if isNul(replaceStr(request.QueryString,FileExt,"")) then  echoMsgAndGo "页面不存在",3 
	SortID = SortAndID(0)
	if not isNul(SortID) and isNum(SortID) then SortID=clng(SortID) else echoMsgAndGo "页面不存在",3 end if
	id=SortAndID(1)
	if not isNul(id) and isNum(id) then id=clng(id) else echoMsgAndGo "页面不存在",3 end if
	
	if isnul(id) or not isnum(id) then alertMsgAndGo "请选择产品！","-1" 
	if isnul(sortID) or not isnum(sortID) then alertMsgAndGo "请选择产品！","-1" 
	
	dim templateobj,channelTemplatePath : set templateobj = new TemplateClass	
	dim typeIds,rsObj,rsObjtid,Tid,rsObjSmalltype,rsObjBigtype
	Dim templatePath,tempStr
	templatePath=sitePath&"/"&"templates/"&setting.defaultTemplate&"/"&setting.htmlFilePath&"/apply.html"
	if not CheckTemplateFile(templatePath) then echo "apply.html"&err_16
	
	set rsobj=conn.exec("select * from {prefix}Sort where SortID="&sortID, "exe")							
	if rsObj.eof then echoMsgAndGo "页面不存在",3 : exit sub		
	with templateObj 
		.content=loadFile(templatePath)	
		.parseHtml()
		templateObj.content=replace(templateObj.content,"{aspcms:sortname}",rsObj("SortName"))
		templateObj.content=replace(templateObj.content,"{aspcms:parentsortid}",rsObj("parentid"))		
		templateObj.content=replace(templateObj.content,"{aspcms:sortid}",sortID)	
		templateObj.content=replace(templateObj.content,"{aspcms:topsortid}",rsObj("topsortid"))
		if isnul(rsObj("PageKeywords")) then 
			templateObj.content=replace(templateObj.content,"{aspcms:sortkeyword}",setting.siteKeyWords)	
		else
			templateObj.content=replace(templateObj.content,"{aspcms:sortkeyword}",rsObj("PageKeywords"))	
		end if
		if isnul(rsObj("PageDesc")) then
			templateObj.content=replace(templateObj.content,"{aspcms:sortdesc}",setting.sitedesc)
		else
			templateObj.content=replace(templateObj.content,"{aspcms:sortdesc}",rsObj("PageDesc"))
		end if
		if isnul(rsObj("PageTitle")) then 
			templateObj.content=replace(templateObj.content,"{aspcms:sorttitle}",rsObj("SortName"))	
		else
			templateObj.content=replace(templateObj.content,"{aspcms:sorttitle}",rsObj("PageTitle"))
		end if
		templateObj.parsePosition(sortID)
		
	dim jobs	
	set rsObj=conn.Exec("select title from {prefix}Content where ContentID="&id,"r1")
	jobs=rsObj(0)
	
	Dim linkman,gender,phone,mobile,email,qq,address,postcode
	if isnul(rCookie("loginstatus")) then  wCookie"loginstatus",0
	if rCookie("loginstatus")=1 then  
		set rsObj=conn.Exec("select *  from {prefix}User where UserID="&trim(rCookie("userID")),"r1")
		linkman=rsObj("truename")
		gender=rsObj("gender")
		phone=rsObj("phone")
		mobile=rsObj("mobile")
		email=rsObj("email")
		qq=rsObj("qq")
		address=rsObj("address")
		postcode=rsObj("postcode")
	else 
		gender=1
	end if
	rsObj.close()
		
		.content=replaceStr(.content,"[aspcms:jobs]",jobs)
		.content=replaceStr(.content,"[aspcms:id]",id)
		.content=replaceStr(.content,"[aspcms:linkman]",linkman)		
		.content=replaceStr(.content,"[aspcms:gender]",gender)				
		.content=replaceStr(.content,"[aspcms:age]",gender)		
		.content=replaceStr(.content,"[aspcms:phone]",phone)		
		.content=replaceStr(.content,"[aspcms:mobile]",mobile)		
		.content=replaceStr(.content,"[aspcms:email]",email)			
		.content=replaceStr(.content,"[aspcms:qq]",qq)			
		.content=replaceStr(.content,"[aspcms:address]",address)			
		.content=replaceStr(.content,"[aspcms:postcode]",postcode)			
		.content=replaceStr(.content,"[aspcms:jobresume]",postcode)			
		.content=replaceStr(.content,"[aspcms:eduresume]",postcode)			
		.content=replaceStr(.content,"[aspcms:regresidence]",postcode)			
		.content=replaceStr(.content,"[aspcms:education]",postcode)		
		.parseCommon() 
		echo .content 
	end with
	set templateobj =nothing : terminateAllObjects
End Sub



Sub addApply
	dim ApplyID,ContentID,Jobs,Linkman,Gender,Age,Marriage,Education,RegResidence,EduResume,JobResume,Address,PostCode,phone,Mobile,Email,QQ,AddTime,ApplyStatus
	if getForm("code","post")<>Session("Code") then alertMsgAndGo "验证码不正确","-1"

	ContentID=filterPara(getForm("ContentID","post"))
	Jobs=filterPara(getForm("Jobs","post"))	
	Linkman=filterPara(getForm("Linkman","post"))
	Gender=filterPara(getForm("Gender","post"))
	Age=filterPara(getForm("Age","post"))
	Marriage=filterPara(getForm("Marriage","post"))
	Education=filterPara(getForm("Education","post"))
	RegResidence=filterPara(getForm("RegResidence","post"))
	EduResume=filterPara(getForm("EduResume","post"))	
	JobResume=filterPara(getForm("JobResume","post"))	
	Address=filterPara(getForm("Address","post"))
	PostCode=filterPara(getForm("PostCode","post"))
	phone=filterPara(getForm("phone","post"))
	Mobile=filterPara(getForm("Mobile","post"))	
	Email=filterPara(getForm("Email","post"))
	QQ=filterPara(getForm("QQ","post"))
	AddTime=now()
	ApplyStatus=0
	

	
	if isnul(Jobs) then alertMsgAndGo "职位名称不能为空","-1"
	if isnul(Linkman) then alertMsgAndGo "姓名不能为空","-1"
	if isnul(Age) then alertMsgAndGo "年龄不能为空","-1"
	if not isnum(Age) then alertMsgAndGo "年龄只能为数字","-1"
	if isnul(phone) then alertMsgAndGo "联系电话不能为空","-1"
	if isnul(Mobile) then alertMsgAndGo "手机不能为空","-1"

	Conn.Exec"insert into {prefix}Apply(ContentID,Jobs,Linkman,Gender,Age,Marriage,Education,RegResidence,EduResume,JobResume,Address,PostCode,phone,Mobile,Email,QQ,AddTime,ApplyStatus) values('"&ContentID&"','"&Jobs&"','"&Linkman&"',"&Gender&","&Age&",'"&Marriage&"','"&Education&"','"&RegResidence&"','"&EduResume&"','"&JobResume&"','"&Address&"','"&PostCode&"','"&phone&"','"&Mobile&"','"&Email&"','"&QQ&"','"&AddTime&"',"&ApplyStatus&")","exe"
	
	if applyReminded then sendMail messageAlertsEmail,setting.sitetitle,setting.siteTitle&setting.siteUrl&"--应聘信息提醒邮件！","您的网站<a href=""http://"&setting.siteUrl&""">"&setting.siteTitle&"</a>有新的应聘信息！<br>应聘职位："&Jobs&"<br>姓名："&Linkman&"<br>年龄："&Age&"<br>性别："&getstr(Gender,"男","女")&"<br>婚姻状况："&Marriage&"<br>户籍："&Education&"<br>学历文凭："&RegResidence&"<br>教育经历："&EduResume&"<br>工作经历："&JobResume&"<br>联系地址："&Address&"<br>邮政编码："&PostCode&"<br>电话号码："&phone&"<br>手机号码："&Mobile&"<br>电子信箱："&Email&"<br>QQ："&QQ&"<br>申请时间："&AddTime

	alertMsgAndGo "申请成功！",sitepath&setting.languagePath
End Sub
%>
