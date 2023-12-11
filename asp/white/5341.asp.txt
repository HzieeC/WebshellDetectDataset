<!--#include file="../../inc/AspCms_SettingClass.asp" -->
<%

dim action : action=getForm("action","get")
if action="add" then addComment

Sub addComment	
	Dim CommentsID,contentID,Commentator,CommentContent,AddTime,CommentIP,CommentStatus
	if getForm("code","post")<>Session("Code") then alertMsgAndGo "验证码不正确","-1"
	contentID=filterPara(getForm("contentID","post"))
	Commentator=filterPara(getForm("Commentator","post"))
	CommentContent=encode(filterPara(getForm("CommentContent","post")))	
	CommentIP=getIP()
	CommentStatus=0
	
	if isnul(Commentator) then alertMsgAndGo "评论者不能为空","-1"
	if isnul(CommentContent) then alertMsgAndGo "评论内容不能为空","-1"
	
	Conn.exec "insert into Aspcms_Comments(contentID,Commentator,CommentContent,AddTime,CommentIP,CommentStatus) values("&contentID&",'"&Commentator&"','"&CommentContent&"','"&now()&"','"&CommentIP&"',"&CommentStatus&")","exe"	
	
	
	if commentReminded then sendMail messageAlertsEmail,setting.sitetitle,setting.siteTitle&setting.siteUrl&"--评论信息提醒邮件！","您的网站<a href=""http://"&setting.siteUrl&""">"&setting.siteTitle&"</a>有新的评论信息！<br>评论者："&Commentator&"<br>评论内容："&CommentContent&"<br>评论时间："&now()
	
	if SwitchFaqStatus=0 then
		alertMsgAndGo "评论成功！",getRefer	
	else	
		alertMsgAndGo "评论成功，请等待审核中！",getRefer	
	end if
End Sub

%>