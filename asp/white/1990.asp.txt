<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/dv_clsother.asp"-->
<%
Session("GetCode")=empty	'清空验证码
Dim ErrString,action,i,showstr,ShowErrType,ComeUrl,tmpstr
Dim Template_html_0,Template_html_1,Template_html_2,Template_html_3
action = Request("action")
'过滤HTML标签 2005-12-29 Dv.Yz
action = Dvbbs.Replacehtml(action)
ShowErrType = Trim(Request("ShowErrType"))	'Dvbbs.ErrType '
If ShowErrType = "" Then ShowErrType = "0"
Dvbbs.ErrType = ShowErrType
Dim ErrCodes

If Request.ServerVariables("HTTP_REFERER")<>"" Then 
	tmpstr=Split(Request.ServerVariables("HTTP_REFERER"),"/")
	Comeurl=tmpstr(UBound(tmpstr))
Else
	If isUrlreWrite = 1 Then
		Comeurl="index.html"
	Else
		Comeurl="index.asp"
	End If
End If
Dvbbs.LoadTemplates("showerr")
Template_html_0=Template.html(0)
Template_html_1=Template.html(1)
Template_html_2=Template.html(2)
Template_html_3=Template.html(3)
If Dvbbs.forum_setting(79)="0" Then
	Template_html_1 = Replace(Template_html_1,"{$getcode}","")
Else
	Template_html_3=Replace(Template_html_3,"{$codestr}",Dvbbs.GetCode())
	Template_html_1 = Replace(Template_html_1,"{$getcode}",Template_html_3)
	Session("xcount")=4'o(验证码)
End If

Select Case action
	Case "stop"		'论坛暂停
		Dvbbs.Stats=Template.Strings(1)
		Dvbbs.head()
		Template_html_2=Replace(Template_html_2,"{$title}",Template.Strings(2)&Template.Strings(1))
		If Dvbbs.BoardID=0 Then
			Rem 指定用户组关闭 Fish 2010-2-3
			If (Dvbbs.Forum_Setting(69)="0" Or Dvbbs.Forum_Setting(21)="1") and Dvbbs.TyClsGroup=0 Then 
				Template_html_2=Replace(Template_html_2,"{$stopreadme}",Stopreadme)
			Else
				if Dvbbs.TyClsGroup= 1 Then
				Dvbbs.Forum_Setting(70)=Split(Dvbbs.TyClsGroupM,"|")
				showstr="<br><b>&nbsp;&nbsp;您所在的用户组</b>设置了是否定时开放，请按下面的时间访问：<hr size=""1""><ul>"
				else
				Dvbbs.Forum_Setting(70)=Split(Dvbbs.Forum_Setting(70),"|")
				showstr="<br><b>&nbsp;&nbsp;"&Dvbbs.Forum_Info(0)&"</b>设置了是否定时开放，请按下面的时间访问：<hr size=""1""><ul>"
				end if
				For i=0 to UBound(Dvbbs.Forum_Setting(70))
					If i mod 6=0 Then showstr=showstr&"<li>"
					If i<10 Then showstr=showstr&"&nbsp;"
					showstr=showstr&i&"点："
					If Dvbbs.Forum_Setting(70)(i)="1" Then
						showstr=showstr&"<font color="""&Dvbbs.mainsetting(1)&""">是</font>&nbsp;&nbsp;"
					Else
						showstr=showstr&"否&nbsp;&nbsp;"
					End If
				Next
				showstr=showstr&"</ul>"
				Template_html_2=Replace(Template_html_2,"{$stopreadme}",showstr)
			End If
		Else
			Dvbbs.Board_Setting(22)=Split(Dvbbs.Board_Setting(22),"|")
			showstr="<br><b>&nbsp;&nbsp;"&Dvbbs.boardtype&"</b>设置了是否定时开放，请按下面的时间访问：<hr size=""1""><ul>"
			For i=0 to UBound(Dvbbs.Board_Setting(22))
				If i mod 6=0 Then showstr=showstr&"<li>"
				If i<10 Then showstr=showstr&"&nbsp;"
				showstr=showstr&i&"点："
				If Dvbbs.Board_Setting(22)(i)="1" Then
					showstr=showstr&"<font color="""&Dvbbs.mainsetting(1)&""">是</font>&nbsp;&nbsp;"
				Else
					showstr=showstr&"否&nbsp;&nbsp;"
				End If
			Next
			showstr=showstr&"</ul>"
			Template_html_2=Replace(Template_html_2,"{$stopreadme}",showstr)
		End If 
		Response.Write Template_html_2
	Case "iplock"	'IP被限
		Dvbbs.Stats=Template.Strings(4)
		Dvbbs.head()
		Session(Dvbbs.CacheName & "UserID")=Empty 
		Template_html_2=Replace(Template_html_2,"{$title}",Template.Strings(4))

		Dim Template_Strings_5
		Template_Strings_5=Template.Strings(5)
		Template_Strings_5=replace(Template_Strings_5,"{$ip}",Dvbbs.usertrueip)
		Template_Strings_5=replace(Template_Strings_5,"{$email}",Dvbbs.Forum_Info(5))

		Template_html_2=Replace(Template_html_2,"{$stopreadme}",Template_Strings_5)
		Response.Write Template_html_2
	Case "limitedonline"	'在线被限
		Dvbbs.Stats=Template.Strings(4)
		Dvbbs.head()
		Template_html_2=Replace(Template_html_2,"{$title}",Template.Strings(23))
		Template_html_2=Replace(Template_html_2,"{$stopreadme}",Replace(Template.Strings(22),"{$onlinelimited}",Request("lnum")))
		Response.Write Template_html_2
	Case "OtherErr"		'显示顶部及导航的错误信息提示
		ErrCodes=CheckErrCodes(Request("ErrCodes"),0)
		Dvbbs.Stats=action&"-"&Template.Strings(0)
		Dvbbs.head()
		If ShowErrType<>"1" Then
		Dvbbs.showtoptable()
		Dvbbs.Head_var 0,"",Template.Strings(0),""
		End If
		Template_html_0=Replace(Template_html_0,"{$color}",Dvbbs.mainsetting(1))
		Template_html_0=Replace(Template_html_0,"{$errtitle}",Dvbbs.Forum_Info(0)&"-"&Dvbbs.Stats)
		Template_html_0=Replace(Template_html_0,"{$action}","访问论坛")
		Template_html_0=Replace(Template_html_0,"{$ErrCount}",1)
		Template_html_0=Replace(Template_html_0,"{$ErrString}",ErrCodes)		
		If Request("autoreload")=1 Then	
			Response.Write "<meta http-equiv=refresh content=""2;URL="&Request.ServerVariables("HTTP_REFERER")&""">"
		End If
		Response.Write Template_html_0
		If dvbbs.userid=0 and Cstr(Dvbbs.ErrType) <> "1" and Cstr(Request("autoreload"))<>"1" Then 			
			'Response.Write Replace(Template_html_1,"{$comeurl}",ComeUrl)'o 08.01.31 Originally 
			Response.Write Replace(Template_html_1,"{$comeurl}",Request.ServerVariables("HTTP_REFERER"))'o 08.01.31 modify
		End If
		Dvbbs.ActiveOnline()
		Dvbbs.footer()
	Case "iOtherErr"		'显示顶部及导航的错误信息提示，可使用html语法
		ErrCodes=CheckErrCodes(Request("ErrCodes"),1)
		Dvbbs.Stats=action&"-"&Template.Strings(0)
		Dvbbs.head()
		If ShowErrType<>"1" Then
		Dvbbs.showtoptable()
		Dvbbs.Head_var 0,"",Template.Strings(0),""
		End If
		Template_html_0=Replace(Template_html_0,"{$color}",Dvbbs.mainsetting(1))
		Template_html_0=Replace(Template_html_0,"{$errtitle}",Dvbbs.Forum_Info(0)&"-"&Dvbbs.Stats)
		Template_html_0=Replace(Template_html_0,"{$action}","访问论坛")
		Template_html_0=Replace(Template_html_0,"{$ErrCount}",1)
		Template_html_0=Replace(Template_html_0,"{$ErrString}",ErrCodes)
		If Request("autoreload")=1 Then	
			Response.Write "<meta http-equiv=refresh content=""2;URL="&Request.ServerVariables("HTTP_REFERER")&""">"	
		End If
		Response.Write Template_html_0
		If dvbbs.userid=0 and Dvbbs.ErrType <> 1 and Request("autoreload")<>1 Then 
			Response.Write Replace(Template_html_1,"{$comeurl}",ComeUrl)
		End If
		Dvbbs.ActiveOnline()
		Dvbbs.footer()
	Case "NoHeadErr"	'不显示顶部及导航的错误信息提示
		ErrCodes=CheckErrCodes(Request("ErrCodes"),0)
		Dvbbs.Stats=action&"-"&Template.Strings(0)
		Dvbbs.head()
		Template_html_0=Replace(Template_html_0,"{$color}",Dvbbs.mainsetting(1))
		Template_html_0=Replace(Template_html_0,"{$errtitle}",Dvbbs.Forum_Info(0)&"-"&Dvbbs.Stats)
		Template_html_0=Replace(Template_html_0,"{$action}","访问论坛")
		Template_html_0=Replace(Template_html_0,"{$ErrCount}",1)
		Template_html_0=Replace(Template_html_0,"{$ErrString}",ErrCodes)
		If Request("autoreload")=1 Then	
			Response.Write "<meta http-equiv=refresh content=""2;URL="&Request.ServerVariables("HTTP_REFERER")&""">"		
		End If
		Response.Write Template_html_0
		If dvbbs.userid=0 and Dvbbs.ErrType <> 1 and Request("autoreload")<>1 Then
			Response.Write Replace(Template_html_1,"{$comeurl}",ComeUrl)
		End If
		Dvbbs.ActiveOnline()
		Dvbbs.footer()
	Case "readonly"
		Dvbbs.Stats="当前论坛为只读"
		Dvbbs.head()
		If ShowErrType<>"1" Then
			Dvbbs.showtoptable()
			Dvbbs.Head_var 1,Dvbbs.boardtype,"",""
		End If
		Template_html_2=Replace(Template_html_2,"{$title}",Template.Strings(2)&"当前时间论坛为只读")
		If Dvbbs.BoardID>0 Then
			Rem Fish 2010-2-3
			If Dvbbs.Board_Setting(21)="2" or Dvbbs.TyClsGroup=2 Then
				if Dvbbs.TyClsGroup=2 Then
					Dvbbs.Board_Setting(22)=Split(Dvbbs.TyClsGroupM,"|")
					showstr="<br><b>&nbsp;&nbsp;您所在的用户组</b>设置了当前时间为是否只读状态，请在规定的时间内发贴：<hr size=""1""><ul>"
				else
					Dvbbs.Board_Setting(22)=Split(Dvbbs.Board_Setting(22),"|")
					showstr="<br><b>&nbsp;&nbsp;"&Dvbbs.boardtype&"</b>设置了当前时间为是否只读状态，请在规定的时间内发贴：<hr size=""1""><ul>"
				end if
				
				
				For i=0 to UBound(Dvbbs.Board_Setting(22))
					If i mod 6=0 Then showstr=showstr&"<li>"
					If i<10 Then showstr=showstr&"&nbsp;"
					showstr=showstr&i&"点："
					If Dvbbs.Board_Setting(22)(i)="1" Then
						showstr=showstr&"<font color="""&Dvbbs.mainsetting(1)&""">是</font>&nbsp;&nbsp;"
					Else
						showstr=showstr&"否&nbsp;&nbsp;"
					End If
				Next
				showstr=showstr&"</ul>"
			End If
		End If
		If Dvbbs.Forum_Setting(69) ="2" Then 
			Dvbbs.Forum_Setting(70)=Split(Dvbbs.Forum_Setting(70),"|")
				showstr="<br><b>&nbsp;&nbsp;"&Dvbbs.Forum_Info(0)&"</b>设置了当前时间为是否只读状态，请在规定的时间内发贴：<hr size=""1""><ul>"
				For i=0 to UBound(Dvbbs.Forum_Setting(70))
					If i mod 6=0 Then showstr=showstr&"<li>"
					If i<10 Then showstr=showstr&"&nbsp;"
					showstr=showstr&i&"点："
					If Dvbbs.Forum_Setting(70)(i)="1" Then
						showstr=showstr&"<font color="""&Dvbbs.mainsetting(1)&""">是</font>&nbsp;&nbsp;"
					Else
						showstr=showstr&"否&nbsp;&nbsp;"
					End If
				Next
				showstr=showstr&"</ul>"
				Template_html_2=Replace(Template_html_2,"{$stopreadme}",showstr)
			End If
			Template_html_2=Replace(Template_html_2,"{$stopreadme}",showstr)
		Response.Write Template_html_2
		Dvbbs.ActiveOnline()
		Dvbbs.footer()
	Case "lock"
		Dvbbs.Stats="论坛已锁定"
		Dvbbs.head()
		If ShowErrType<>"1" Then
			Dvbbs.showtoptable()
			Dvbbs.Head_var 0,"",Dvbbs.boardtype,""
		End If
		Template_html_2=Replace(Template_html_2,"{$title}",Template.Strings(2)&"论坛已锁定")
		Template_html_2=Replace(Template_html_2,"{$stopreadme}","本论坛已经被锁定，不允许发贴回贴。")
		Response.Write Template_html_2
		Dvbbs.ActiveOnline()
		Dvbbs.footer()
	Case "plus"
		ErrCodes=CheckErrCodes(Request("ErrCodes"),0)
		Dvbbs.Stats=action&"-"&Template.Strings(0)
		Dvbbs.head()
		If ShowErrType<>"1" Then
		Dvbbs.showtoptable()
		Dvbbs.Head_var 0,"",Template.Strings(0),""
		End If
		Template_html_0=Replace(Template_html_0,"{$color}",Dvbbs.mainsetting(1))
		Template_html_0=Replace(Template_html_0,"{$errtitle}",Dvbbs.Forum_Info(0)&"-"&Dvbbs.Stats)
		Template_html_0=Replace(Template_html_0,"{$action}","使用论坛插件")
		Template_html_0=Replace(Template_html_0,"{$ErrCount}",1)
		Template_html_0=Replace(Template_html_0,"{$ErrString}",ErrCodes)
		Response.Write Template_html_0
		If dvbbs.userid=0 and Dvbbs.ErrType <> 1 Then 
			Response.Write Replace(Template_html_1,"{$comeurl}",ComeUrl)
		End If
		Dvbbs.ActiveOnline()
		Dvbbs.footer()
		Dvbbs.PageEnd()
	Case Else
		Dvbbs.Stats = Action & Template.Strings(0)
		Dvbbs.head()
		If ShowErrType<>"1" Then
			Dvbbs.showtoptable()
			Dvbbs.Head_var 0,"",Template.Strings(0),""
		End If
		Template_html_0=Replace(Template_html_0,"{$color}",Dvbbs.mainsetting(1))
		Template_html_0=Replace(Template_html_0,"{$errtitle}",Dvbbs.Forum_Info(0)&"-"&Dvbbs.Stats)
		Template_html_0=Replace(Template_html_0,"{$action}",action)
		Template_html_0=Replace(Template_html_0,"{$ErrCount}",ErrCount)
		Template_html_0=Replace(Template_html_0,"{$ErrString}",ErrString)
		Response.Write Template_html_0
		If dvbbs.userid=0 and Dvbbs.ErrType <> 1 Then 
			Response.Write Replace(Template_html_1,"{$comeurl}",ComeUrl)
		End If
		Dvbbs.ActiveOnline()
		Dvbbs.footer()
		Dvbbs.PageEnd()
End Select

Function Stopreadme()
	Dim Setting
	Setting=Dvbbs.CacheData(1,0)
	Setting=split(Setting,"|||")
	Stopreadme=Setting(5)
End Function 

Function  ErrCount()
	Dim i
	ErrCount=0
	ErrCodes=Request("ErrCodes")
	If ErrCodes<>"" Then
		ErrCodes=Split(ErrCodes,",")
		For i=0 to UBound(ErrCodes)
			If IsNumeric(ErrCodes(i)) Then 
				If i=0 Then
					ErrString=ErrString&"<li>"&Template.Strings(ErrCodes(i))
				Else
					ErrString=ErrString&"<br><li>"&Template.Strings(ErrCodes(i))
				End If
				ErrCount=ErrCount+1
			End If 
		Next 
	End If 
End Function

Function CheckErrCodes(ErrCode,sType)
	Dim Re
	Set re=new RegExp
	re.IgnoreCase =True
	re.Global=True
	re.Pattern="<(a.[^>]*|\/a|li|br|B|\/li|\/B)>"
	ErrCode=re.Replace(ErrCode,"[$1]")
	If sType = 0 Then
		ErrCode=Server.htmlencode(ErrCode)
	End If
	re.Pattern="\[(a.[^\]]*|\/a|li|br|B|\/li|\/B)\]"
	ErrCode=re.Replace(ErrCode,"<$1>")
	Set Re=Nothing
	CheckErrCodes=ErrCode
End Function
%>