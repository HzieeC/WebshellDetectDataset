<!--#Include File="Conn.asp" -->
<!--#Include File="Inc/Const.asp" -->
<!--#Include File="Inc/dv_classpageac.asp" -->
<!--#Include File="Inc/dv_pageclass.asp" -->
<%
'最后修改：2010-3-19by 小易

'勋章图片数目，默认是10，如果想增加直接把该数字改大并在Dv_plus/medal/images/下添加对应编号的图片。
Const MedalPicNum = 10 

If Dvbbs.UserId = 0 Then 
	Response.redirect "showerr.asp?ErrCodes=<li>游客没有查看勋章插件的权限，请先登陆。&action=OtherErr"
	Reseponse.end
end if
If Cint(dvbbs.Forum_Setting(104))=0 Then
	Response.redirect "showerr.asp?ErrCodes=<li>勋章功能已被管理员关闭。&action=OtherErr"
	Reseponse.end
end if

Dvbbs.LoadTemplates("")
Dvbbs.Stats = "荣誉勋章"
Dvbbs.Nav
Dvbbs.Head_Var 0,0,"荣誉勋章","medal_index.asp"
Dvbbs.Name = "Medal"

Medal_Nav()

Select Case Request("action")
	Case "users" :
		Call Users()
	Case "medal" :
		Call Medal()
	Case "award" :
		Call Award()
	Case "saveaward" :
		Call SaveAward()
	Case "remedal" :
		Call ReAward()
	Case "awardlog" : 
		Call AwardLog()
	Case "savemedal" :
		Call SaveMedal()
	Case "delmedal" :
		Call DelMedal()
	Case Else :
		Call Main()
End Select
Dvbbs.Footer()
Dvbbs.PageEnd()

'功能菜单
Sub Medal_Nav()
	Dim Html
	Html = "<div style=""width:97%;margin:0 auto;border: #DBE1E9 1px solid; background:#fff;""><h3 style=""margin:0;padding:0 10px""><a href='medal_index.asp'>我的勋章</a>&nbsp;&nbsp;|&nbsp;&nbsp;<a href='medal_index.asp?action=users'>获奖名单</a>"
	If Dvbbs.Master Then
		Html = Html & "&nbsp;&nbsp;|&nbsp;&nbsp;<a href='medal_index.asp?action=medal'>勋章管理</a> | <a href='medal_index.asp?action=award'>颁发勋章</a>&nbsp;&nbsp;|&nbsp;&nbsp;<a href='medal_index.asp?action=awardlog'>颁发记录</a>"
	End If
	Html = Html & "</h3></div><br />"
	Response.Write Html
End Sub

'我的勋章
Sub Main()
	Dim Html,Rs,Data,i,MedalData
	If Dvbbs.ObjIsEmpty() Then ReloadCache()
	MedalData = Dvbbs.Value

	Html = "<table class='tableborder1' cellspacing='1'><tr><th colspan='4'>我的勋章</th></tr><tr class='tabletitle2'><td>勋章名称</td><td>颁奖人</td><td>获奖原因</td><td>获奖时间</td></tr>"
	Set Rs = Dvbbs.Execute("SELECT M.MedalName,M.MedalPic,L.AwardUser,L.AwardDesc,L.AddTime FROM Dv_Medal M,Dv_MedalLog L WHERE M.id = L.MedalId AND L.UserId ="&Dvbbs.UserId)
	If Not Rs.Eof Then
		Data = Rs.GetRows(-1)
		For i = 0 To Ubound(Data,2)
			Html = Html & "<tr class='tablebody1'><td><img src='Dv_plus/medal/images/"&Data(1,i)&"' /> "&Data(0,i)&"</td><td>"&Data(2,i)&"</td><td>"&Data(3,i)&"</td><td>"&Data(4,i)&"</td></tr>"
		Next
	Else
		Html = Html & "<tr class='tablebody1'><td colspan='4'>暂无勋章...</td></tr>"
	End If
	Rs.Close : Set Rs = Nothing
	Html = Html & "<table><br />"

	Html = Html & "<table class='tableborder1' cellspacing='1'><tr><th colspan='3'>勋章列表</th></tr><tr class='tabletitle2'><td>图标</td><td>名称</td><td>说明</td></tr>"
	If IsArray(MedalData) Then
		For i = 0 To UBound(MedalData,2)
			Html = Html & "<tr class='tablebody1'><td><img src='Dv_plus/medal/images/"&MedalData(2,i)&"' /></td><td>"&MedalData(1,i)&"</td><td>"&MedalData(3,i)&"</td></tr>"
		Next
	End If
	Html = Html & "</table>"

	Response.write Html
End Sub

'获奖名单
Sub Users()
	Dim Html,Users,UserMedal(1),Medals,Rs,Rs2,i,j,k
	Dim Page,Url,TotalCount,PageSize,TotalPage,StarNum,EndNum
	Page = 1 : TotalCount = 1 : PageSize = 1 : Url = "action=users"
	Html = "<script language=""javascript"" type=""text/javascript"" src=""inc/Pagination.js"" /></script>"
	Html = Html & "<table class='tableborder1' cellspacing='1'><tr><th colspan='4'>获奖名单</th></tr><tr class='tabletitle2'><td width='100'>用户</td><td>勋章</td></tr>"

	Rem 分页
	Dim mypage,truekeyword
	truekeyword=" TyMedaled=1"

	PageSize=10	'定义分页每一页的记录数

    If Not isobject(conn) then ConnectionDatabase
	
	If IsSqlDataBase =1 Then
		Set mypage=new Pager
	else
		Set mypage=new Pager2
	end if

	mypage.getconn=conn '得到数据库连接
	mypage.pagesize=PageSize 
	mypage.TableName="Dv_User" '要查询的表名
	mypage.Tablezd=" UserId,UserName,UserMedal"
	mypage.KeyName="UserId"
	mypage.OrderType=1
	mypage.PageWhere=truekeyword
	mypage.GetStyle =1
	Set Rs=mypage.getrs()
	TotalCount = mypage.int_totalRecord

	Set mypage = nothing

	If Not (rs.bof And rs.eof) Then
		Users=rs.getrows(-1):rs.close:Set rs=Nothing

		Set Rs2 = Dvbbs.Execute("SELECT id,MedalName,MedalDesc,MedalPic FROM Dv_Medal")
		Medals = Rs2.GetRows(-1)
		Rs2.Close : Set Rs2 = Nothing
		
		If IsArray(Users) Then
			for i=0 to ubound(Users,2)
				UserMedal(0) = Split(Users(2,i),",")
				UserMedal(1) = ""
				For j = 0 To Ubound(UserMedal(0))
					For k = 0 To Ubound(Medals,2)
						If Clng(UserMedal(0)(j)) = Clng(Medals(0,k)) Then
							UserMedal(1) = UserMedal(1) & "<img src='dv_plus/medal/images/"&Medals(3,k)&"' alt='"&Medals(1,k)&"' /> "
							Exit For
						End If
					Next
				Next
				Html = Html & "<tr class='tablebody1'><td><strong>"&Users(1,i)&"</strong></td><td>"&UserMedal(1)&"</td></tr>"
			Next
		end if

	Else
		rs.close:Set rs=Nothing
		Html = Html & "<tr class='tablebody1'><td colspan='2'>暂无...</td></tr>"
	End If
	

	Page=Cint(Request("Page"))
	if page<1 then Page=1

	Html = Html & "</table></table><table width='100%'><tr><td><script language=""javascript"">PageList("&Page&",10,"&PageSize&","&TotalCount&",'"&Url&"',4);</script></table>"
	Response.write Html
End Sub

'勋章管理
Sub Medal()
	If Not Dvbbs.Master Then ShowMsg "对不起，你不是管理员，不能进入管理页面！" : Exit Sub
	Dim Html,Rs,PicStr,i,Data,TyUsed
	Html = "<form name='m' method='post' action='?action=savemedal&type=edit'><table class='tableborder1' cellspacing='1'><tr><th colspan='6'>勋章管理</th></tr><tr class='tabletitle2'><td>编号</td><td>奖项名称</td><td>相关说明</td><td>勋章图片</td><td>图片演示</td><td>操作</td></tr>"
	Set Rs = Dvbbs.Execute("SELECT Id,MedalName,MedalDesc,MedalPic FROM [Dv_Medal]")
	If Not Rs.Eof Then
		Data = Rs.GetRows(-1)
		For i = 0 To Ubound(Data,2)
			TyUsed=TyUsed&Data(3,i)&","
			Html = Html & "<tr class='tablebody1'><td><input type='text' name='id' readonly='true' size='3' value='"&Data(0,i)&"' /></td><td><input type='text' name='m_name' value='"&Data(1,i)&"' /></td><td><textarea name='m_desc' cols='50' rows='2'>"&Data(2,i)&"</textarea></td><td><input type='text' name='m_pic' value='"&Data(3,i)&"' /></td><td><img src='dv_plus/medal/images/"&Data(3,i)&"' /></td><td><a href='?action=delmedal&id="&Data(0,i)&"' onclick=""if (confirm('本操作将会对所有具有该勋章的用户执行收回操作，确定吗？')){return true}else{return false;}"">删除</a></td></tr>"
		Next
		Html = Html & "<tr class='tablebody1'><td colspan='6' align='center'><input type='submit' value='提 交' /></td></tr>"
	Else
		Html = Html & "<tr class='tablebody1'><td colspan='6'>暂无...</td></tr>"
	End If
	Rs.Close : Set Rs = Nothing
	For i = 1 to MedalPicNum
		PicStr = PicStr & " <input type='radio' name='m_pic' value='"&i&".gif' "
		Rem 防止不同勋章使用同样的图标，小易 2009.9.25
		if instr(TyUsed,i)>0 then PicStr=PicStr & "disabled" 
		PicStr = PicStr & " /><img src='Dv_Plus/medal/images/"&i&".gif' />&nbsp;&nbsp;"
	Next
	Html = Html & "</table></form><br /><form name='add' method='post' action='?action=savemedal&type=add'><table class='tableborder1' cellspacing='1'><tr><th colspan='2'>添加奖项</th></tr><tr class='tablebody1'><td>奖项名称</td><td><input type='text' name='m_name' size='50' /></td></tr><tr class='tablebody1'><td>奖项说明</td><td><textarea name='m_desc' rows='5' cols='45'></textarea></td></tr><tr class='tablebody1'><td>奖项图片</td><td>"&PicStr&"</td></tr><tr class='tablebody1'><td></td><td><input type='submit' value='提交' /></td></tr></table></form>"
	Response.write Html
End Sub

'保存奖项
Sub SaveMedal()
	If Not Dvbbs.Master Then ShowMsg "对不起，你不是管理员，不能进入管理页面！" : Exit Sub
	Dim id,m_name,m_desc,m_pic,i
	id = Dvbbs.Checkstr(Dvbbs.iHtmlEncode(Request.Form("id")))
	m_name = Dvbbs.Checkstr(Dvbbs.iHtmlEncode(Request.Form("m_name")))
	m_desc = Dvbbs.Checkstr(Dvbbs.iHtmlEncode(Request.Form("m_desc")))
	m_pic = Dvbbs.Checkstr(Dvbbs.iHtmlEncode(Request.Form("m_pic")))
	If Request("type")="add" Then
		If m_name <> "" And m_pic <> "" Then
			Dvbbs.Execute("INSERT INTO Dv_Medal (MedalName,MedalDesc,MedalPic) VALUES ('"&m_name&"','"&m_desc&"','"&m_pic&"')")
			ReloadCache()
			Response.Redirect "medal_index.asp?action=medal"
		Else
			ShowMsg "奖项名称和奖项图片不能为空！",""
		End If
	ElseIf Request("type")="edit" Then
		Rem fish 修复编辑出错
		id=Replace(id,"&#44;&nbsp;",", ") 
		m_name=Replace(m_name,"&#44;&nbsp;",", ") 
		m_desc=Replace(m_desc,"&#44;&nbsp;",", ") 
		m_pic=Replace(m_pic,"&#44;&nbsp;",", ") 
		id = Split(id,", ") : m_name = Split(m_name,", ") : m_desc = Split(m_desc,", ") : m_pic = Split(m_pic,", ")
		For i = 0 To Ubound(id)
			Dvbbs.Execute("UPDATE Dv_Medal SET MedalName = '"&m_name(i)&"',MedalDesc = '"&m_desc(i)&"',MedalPic = '"&m_pic(i)&"' WHERE id = "&id(i))
		Next
		ReloadCache()
		Response.Redirect "medal_index.asp?action=medal"
	End If
End Sub

'删除奖项
Sub DelMedal()
	If Not Dvbbs.Master Then ShowMsg "对不起，你不是管理员，不能进入管理页面！" : Exit Sub
	Dim Id,Rs,Data,i,j,UserMedal(1),TyMedaled
	Id = Trim(Request.QueryString("id"))
	If isNumeric(Id) Then
		Set Rs = Dvbbs.Execute("SELECT UserId,UserName,UserMedal FROM Dv_User WHERE TyMedaled=1")
		If Not Rs.Eof Then
			Data = Rs.GetRows(-1)
			For i = 0 To Ubound(Data,2)
				UserMedal(0) = Split(Data(2,i),",")
				For j = 0 To Ubound(UserMedal(0))
					If UserMedal(0)(j) <> Id Then
						If UserMedal(1) = "" Then
							UserMedal(1) = UserMedal(0)(j)
						Else
							UserMedal(1) = UserMedal(1) & "," & UserMedal(0)(j)
						End If
					End If
				Next
				TyMedaled=1
				if UserMedal(1)="" then TyMedaled=0
				Dvbbs.Execute("UPDATE Dv_User SET UserMedal = '"&UserMedal(1)&"',TyMedaled="&TyMedaled&" WHERE UserId = "&Data(0,i))
			Next
		End If
		Rs.Close : Set Rs = Nothing
		Dvbbs.Execute("DELETE FROM Dv_Medal WHERE id = "&Id)
		Dvbbs.Execute("DELETE FROM Dv_MedalLog WHERE MedalId = "&Id)
		ReloadCache()
		ShowMsg "操作成功！","medal_index.asp?action=medal"
	Else
		ShowMsg "请不要传递非法参数！","medal_index.asp?action=medal"
	End If
End Sub

'颁发页面
Sub Award()
	If Not Dvbbs.Master Then ShowMsg "对不起，你不是管理员，不能进入管理页面！" : Exit Sub
	Dim Html,Options,Rs
	Set Rs = Dvbbs.Execute("SELECT id,MedalName FROM Dv_Medal")
	Do While Not Rs.Eof
		Options = Options & "<option value='"&Rs(0)&"'>"&Rs(1)&"</option>"
		Rs.MoveNext
	Loop
	Rs.Close : Set Rs = Nothing
	Html = "<form name='award' method='post' action='medal_index.asp?action=saveaward'><table class='tableborder1' cellspacing='1'><tr><th colspan='2'>颁发勋章</th></tr><tr class='tablebody1'><td>获奖者：</td><td><input type='text' name='UserName' /></td></tr><tr class='tablebody1'><td>颁发勋章：</td><td><select name='MedalId'>"&Options&"</select></td></tr><tr class='tablebody1'><td>颁发原因：</td><td><textarea name='Desc' rows='3' cols='50'></textarea></td></tr><tr class='tablebody1'><td></td><td><input type='submit' value='提 交' /></td></tr></table></form>"
	
	Response.Write Html
End Sub

'保存颁发
Sub SaveAward()
	If Not Dvbbs.Master Then ShowMsg "对不起，你不是管理员，不能进入管理页面！" : Exit Sub
	Dim Rs,UserName,MedalId,Desc,UserInfo,UserMedal(1),i,MedalName
	UserName = Dvbbs.Checkstr(Dvbbs.iHtmlEncode(Request.Form("UserName")))
	MedalId = Dvbbs.CheckNumeric(Request.Form("MedalId"))
	Desc = Dvbbs.Checkstr(Dvbbs.iHtmlEncode(Request.Form("Desc")))
	If UserName = "" Or MedalId = "" Or Desc = "" Then
		ShowMsg "每一项都必须填写，请返回填写完整！","" : Exit Sub
	End If
	If isNumeric(MedalId) Then
		MedalName = Dvbbs.Execute("SELECT MedalName FROM Dv_Medal WHERE id = "&MedalId)(0)
		If MedalName = "" Or isNull(MedalName) Then
			ShowMsg "不存在该奖项，请不要传递非法参数！","" : Exit Sub
		End If
		Set Rs = Dvbbs.Execute("SELECT UserId,UserName,UserMedal FROM Dv_User WHERE UserName = '"&UserName&"'")		
		If Not Rs.Eof Then
			UserInfo = Rs.GetRows(1)
			If UserInfo(2,0) <> "" Then
				UserMedal(0) = Split(UserInfo(2,0),",")
				UserMedal(1) = UserInfo(2,0)
				For i = 0 To Ubound(UserMedal(0))
					If CLng(UserMedal(0)(i)) = CLng(MedalId) Then
						ShowMsg "<strong>"&UserName&"</strong> 已经拥有该勋章，不能重复颁发！","" : Exit Sub
					End If
				Next
				UserMedal(1) = UserMedal(1) & "," & MedalId
			Else
				UserMedal(1) = MedalId
			End If
			Dvbbs.Execute("UPDATE Dv_User Set UserMedal = '"& UserMedal(1) &"',TyMedaled=1 WHERE UserId = "&UserInfo(0,0))
			Dvbbs.Execute("INSERT INTO Dv_MedalLog (UserId,UserName,MedalId,AwardUser,AwardDesc) VALUES ("&UserInfo(0,0)&",'"&UserInfo(1,0)&"',"&MedalId&",'"&Dvbbs.MemberName&"','"&Desc&"')")

			ShowMsg "勋章 <strong>"&MedalName&"</strong> 已经成功颁发给 <strong>"&UserName&"</strong>","medal_index.asp?action=awardlog"
		Else
			ShowMsg "不存在该用户，请返回检查您的输入是否正确！",""
		End If
	End If
End Sub

'颁发日志
Sub AwardLog()
	If Not Dvbbs.Master Then ShowMsg "对不起，你不是管理员，不能进入管理页面！" : Exit Sub
	Dim Html,Rs,Data,i,UserName,Sql
	Dim Page,Url,TotalCount,PageSize,TotalPage,StarNum,EndNum
	Page = 1 : TotalCount = 1 : PageSize = 1
	UserName = Dvbbs.Checkstr(Dvbbs.iHtmlEncode(Request.QueryString("u")))
	Html = "<script language=""javascript"" type=""text/javascript"" src=""inc/Pagination.js"" /></script>"
	Html = Html & "<table class='tableborder1' cellspacing='1'><tr><th colspan='8'>颁发记录</th>"
	hTML = Html & "</tr><tr class='tabletitle2'><td>获奖者</td><td>勋章名称</td><td>颁发者</td><td>颁发原因</td><td>颁发时间</td><td>操作</td></tr>"
	If UserName <> "" Then
		Url = "action=awardlog&u="&UserName
		Sql = "SELECT L.*,M.MedalName,M.MedalPic,M.id FROM Dv_MedalLog L,Dv_Medal M  WHERE L.MedalId = M.Id AND L.UserName = '"&UserName&"' ORDER BY L.id DESC"
	Else
		Url = "action=awardlog"
		Sql = "SELECT L.*,M.MedalName,M.MedalPic,M.id FROM Dv_MedalLog L,Dv_Medal M  WHERE L.MedalId = M.Id ORDER BY L.id DESC"
	End If
	Set Rs = Dvbbs.Execute(Sql)
	If Not Rs.Eof Then
		Data = Rs.GetRows(-1)
		'---分页设置---
		TotalCount = Ubound(Data,2) + 1
		PageSize = 10
		TotalPage = Int(TotalCount / PageSize)
		Page = Clng(Request.QueryString("page"))
		If TotalPage = 0 Then TotalPage = 1
		If PageSize * TotalPage < TotalCount Then TotalPage = TotalPage + 1
		If Not isNumeric(Page) Or Page="" Or Page < 1 Then Page = 1
		If Page > TotalPage Then Page = TotalPage
		StarNum = (Page-1) * PageSize
		EndNum = Page * PageSize
		If EndNum > TotalCount Then EndNum = TotalCount
		For i = StarNum To EndNum - 1
			Html = Html & "<tr class='tablebody1'><td><strong>"&Data(2,i)&"</strong></td><td><img src='dv_plus/medal/images/"&Data(8,i)&"' /> "&Data(7,i)&"</td><td><u>"&Data(4,i)&"</u></td><td>"&Data(5,i)&"</td><td>"&Data(6,i)&"</td><td><a href='?action=remedal&uid="&Data(1,i)&"&mid="&Data(9,i)&"&lid="&Data(0,i)&"' onclick=""if (confirm('确定收回该用户的勋章吗？')){return true}else{return false;}"">收回勋章</a></td></tr>"
		Next

	End If
	Html = Html & "</table><table width='100%'><tr><td colspan='8'><span style=""float:right"">用户名：<input type='text' id='UserName' name='UserName' /> <input type='button' onclick=""window.location='medal_index.asp?action=awardlog&u='+document.getElementById('UserName').value"" value='查找' /></span><script language=""javascript"">PageList("&Page&",10,"&PageSize&","&TotalCount&",'"&Url&"',4);</script></table>"
	Response.write Html
End Sub

'收回勋章
Sub ReAward()
	If Not Dvbbs.Master Then ShowMsg "对不起，你不是管理员，不能进入管理页面！" : Exit Sub
	Dim UserId,MedalId,LogId,Rs,UserInfo,UserMedal(1),i,TyMedaled
	UserId = Request("uid") : MedalId = Request("mid") : LogId = Request("lid")
	If Not (isNumeric(UserId) And isNumeric(MedalId) And isNumeric(LogId)) Then
		ShowMsg "参数错误，请不要尝试传递非法参数！","" : Exit Sub
	End If
	Set Rs = Dvbbs.Execute("SELECT UserId,UserName,UserMedal,TyMedaled from Dv_User WHERE UserId = "&Clng(UserId))
	If Not Rs.Eof Then
		UserInfo = Rs.GetRows(1)
		UserMedal(0) = Split(UserInfo(2,0),",")
		TyMedaled=0
		For i = 0 To Ubound(UserMedal(0))
			If UserMedal(0)(i) <> MedalId Then
				If UserMedal(1) = "" Then
					UserMedal(1) = UserMedal(0)(i)
				Else
					UserMedal(1) = UserMedal(1) & "," & UserMedal(0)(i)
				End If
			End If
		Next
		if UserMedal(1)<>"" then TyMedaled=1
		Dvbbs.Execute("UPDATE Dv_User SET UserMedal = '"&UserMedal(1)&"',TyMedaled="&TyMedaled&" WHERE UserId = "&Clng(UserId))
		Dvbbs.Execute("DELETE FROM Dv_MedalLog WHERE id = "&Clng(LogId))
		ShowMsg "成功收回 <strong>"&UserInfo(1,0)&"</strong>的勋章！","medal_index.asp?action=awardlog"
	Else
		ShowMsg "用户不存在！",""
	End If
End Sub

Sub ReloadCache()
	Dim Rs
	Set Rs = Dvbbs.Execute("SELECT id,MedalName,MedalPic,MedalDesc FROM Dv_Medal")
	Dvbbs.RemoveCache()
	If Not Rs.Eof Then
		Dvbbs.Value = Rs.GetRows(-1)
	Else
		Dvbbs.Value = ""
	End If
	Rs.Close : Set Rs = Nothing
End Sub

Sub ShowMsg(Msg,url)
	Dim Html
	Html = "<table class='tableborder1' cellspacing='1'><tr><th>提示信息</th></tr><tr class='tablebody1'><td height='50'>"&Msg&"</td></tr><tr class='tabletitle1'><td align='center'><input type='button' onclick="""
	If Url = "" Then
		Html = Html & "javascript:history.back()"" value='返回上一页' /></td></tr></table>"
	Else
		Html = Html & "javascript:window.location='"&url&"'"" value='点击返回' /></td></tr></table>"
	End If
	Response.Write Html
End Sub
%>