<!--#include file="../inc/AspCms_SettingClass.asp" -->
<%
dim action : action=getForm("action","get")
if action = "buy" then
	addOrder()
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
	templatePath=sitePath&"/"&"templates/"&setting.defaultTemplate&"/"&setting.htmlFilePath&"/productbuy.html"
	if not CheckTemplateFile(templatePath) then echo "productbuy.html"&err_16
	
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
		
	dim selectproduct	
	set rsObj=conn.Exec("select title from {prefix}Content where ContentID="&id,"r1")
	selectproduct=rsObj(0)
	
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
		
		.content=replaceStr(.content,"{aspcms:selectproduct}",selectproduct)
		.content=replaceStr(.content,"[aspcms:linkman]",linkman)		
		.content=replaceStr(.content,"[aspcms:gender]",gender)		
		.content=replaceStr(.content,"[aspcms:phone]",phone)		
		.content=replaceStr(.content,"[aspcms:mobile]",mobile)		
		.content=replaceStr(.content,"[aspcms:email]",email)			
		.content=replaceStr(.content,"[aspcms:qq]",qq)			
		.content=replaceStr(.content,"[aspcms:address]",address)			
		.content=replaceStr(.content,"[aspcms:postcode]",postcode)	
		.parseCommon() 
		echo .content 
	end with
	set templateobj =nothing : terminateAllObjects
End Sub



Sub addOrder
	dim OrderID,UserID,OrderName,Linkman,Gender,Address,PostCode,Phone,Mobile,Email,QQ,AddTime,Remark,OrderStatus,num
	if getForm("code","post")<>Session("Code") then alertMsgAndGo "验证码不正确","-1"


	OrderID=getForm("OrderID","post")	

	OrderName=filterPara(getForm("OrderName","post"))
	Linkman=filterPara(getForm("Linkman","post"))
	Gender=filterPara(getForm("Gender","post"))
	Address=filterPara(getForm("Address","post"))
	PostCode=filterPara(getForm("PostCode","post"))
	Phone=filterPara(getForm("Phone","post"))
	Mobile=filterPara(getForm("Mobile","post"))	
	Email=filterPara(getForm("Email","post"))
	QQ=filterPara(getForm("QQ","post"))	
	Remark=filterPara(getForm("Remark","post"))	
	num=filterPara(getForm("num","post"))	
	AddTime=now()
	OrderStatus=0

	
	if isnul(OrderName) then alertMsgAndGo "订单名称不能为空","-1"
	if isnul(Linkman) then alertMsgAndGo "联系人不能为空","-1"
	if isnul(Phone) then alertMsgAndGo "联系电话不能为空","-1"
	if isnul(Mobile) then alertMsgAndGo "手机不能为空","-1"
	if not isnum(num) then alertMsgAndGo "订购数量不正确","-1"

	Conn.Exec"insert into {prefix}Order(OrderName,Linkman,Gender,Address,PostCode,Phone,Mobile,Email,QQ,AddTime,Remark,OrderStatus,num) values('"&OrderName&"','"&Linkman&"',"&Gender&",'"&Address&"','"&PostCode&"','"&Phone&"','"&Mobile&"','"&Email&"','"&QQ&"','"&AddTime&"','"&Remark&"',"&OrderStatus&","&num&")","exe"
	
	
	if orderReminded then sendMail messageAlertsEmail,setting.sitetitle,setting.siteTitle&setting.siteUrl&"--订单信息提醒邮件！","您的网站<a href=""http://"&setting.siteUrl&""">"&setting.siteTitle&"</a>有新的订单信息！<br>订单名称："&OrderName&"<br>订购数量："&num&"<br>联系人："&Linkman&"<br>性别："&getstr(Gender,"男","女")&"<br>联系地址："&Address&"<br>邮政编码："&PostCode&"<br>电话号码："&phone&"<br>手机号码："&Mobile&"<br>电子信箱："&Email&"<br>QQ："&QQ&"<br>备注："&Remark&"<br>申请时间："&AddTime
	alertMsgAndGo "产品订购成功！",sitepath&setting.languagePath
End Sub
%>
