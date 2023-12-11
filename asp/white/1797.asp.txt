<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/dv_clsother.asp" -->
<%
Dvbbs.stats="版主管理页面"
Dvbbs.LoadTemplates("")
Dvbbs.Nav()
Dim sql1,rs1,sql,Rs,i
If Dvbbs.UserID=0 Then Response.redirect "showerr.asp?ErrCodes=<li>请登录后进行操作。&action=OtherErr"

If DVbbs.BoardID=0 then
	Dvbbs.Head_var 2,0,"",""
Else
	Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
	GetBoardPermission
End If

If Not(Dvbbs.boardmaster or Dvbbs.master or Dvbbs.superboardmaster) Then Response.redirect "showerr.asp?ErrCodes=<li>只有该版版主级别以上的管理员才能登录。&action=OtherErr"
If Not Dvbbs.ChkPost() Then Dvbbs.AddErrCode(16):Dvbbs.Showerr()
Main()
Dvbbs.Footer()
Dvbbs.PageEnd()
Sub Main()
%>
<TABLE cellpadding=0 cellspacing=1 class=tableborder1 align=center > 
        <tr >
          <th height=24 align=center colspan="2">欢迎 <%=Dvbbs.htmlencode(Dvbbs.membername)%>进入版主管理页面</th>
        </tr>
        <tr >
          <td height=24 align=center colspan="2" class=tablebody1>
        <b>管理选项：<a href="announcements.asp?boardid=<%=Dvbbs.BoardID%>">公告发布和管理</a> | 
		<a href="admin_boardset.asp?action=editbminfo&boardid=<%=Dvbbs.BoardID%>">基本信息管理</a> |  
				<a href="admin_boardset.asp?action=editbmads&boardid=<%=Dvbbs.BoardID%>">分版广告管理</a> |  
				<a href="index.asp?action=batch&boardid=<%=Dvbbs.BoardID%>">批量管理帖子</a> |  
				<a href="infolist.asp?t=paper&boardid=<%=Dvbbs.BoardID%>">分版小字报管理</a>
		</b></td>
        </tr>
</table>
<BR>
<table cellpadding=0 cellspacing=0 width="<%=Dvbbs.mainsetting(0)%>" align=center style="word-break:break-all;">
		<tr>
              <td width="30%" valign=top align=left>
		<table cellpadding=3 cellspacing=1 class=tableborder1 style="width:100%;word-break:break-all;">
		<tr>
			<th width="100%" height=24  colspan="2">《 本版信息栏 》
			</th>
		</tr>
		<tr>
			<td  height=24 class=tablebody2 colspan="2" align=center ><%=Dvbbs.BoardType%>
			</td>
		</tr>
		<tr>
			<td width="60%" height=24 class=tablebody1 >今日新帖：
			</td>
			<td width="40%" height=24 class=tablebody1 ><font color="RED"><%=Application(Dvbbs.CacheName &"_information_" & Dvbbs.boardid).documentElement.selectSingleNode("information/@todaynum").text%></FONT>
			</td>
		</tr>
		<tr>
			<td  height=24 class=tablebody2 >主题帖子：
			</td>
			<td  height=24 class=tablebody2 ><%=Application(Dvbbs.CacheName &"_information_" & Dvbbs.boardid).documentElement.selectSingleNode("information/@topicnum").text%>
			</td>
		</tr>
		<tr>
			<td  height=24 class=tablebody1 >本版帖子：
			</td>
			<td  height=24 class=tablebody1 ><%=Application(Dvbbs.CacheName &"_information_" & Dvbbs.boardid).documentElement.selectSingleNode("information/@postnum").text%>
			</td>
		</tr>
		<tr>
			<td width="30%" height=24 class=tablebody2 colspan="2">管理成员：
			<%
			Dim master
			For Each master in Application(Dvbbs.CacheName&"_boardmaster").documentElement.selectNodes("boardmaster[@boardid='"&Dvbbs.boardid&"']/master")
				Response.Write master.text &" "
			Next
			%>
			</td>
		</tr>
		<tr>
			<th width="100%" height=24  colspan="2">《 管理权限 》
			</th>
		</tr>
		<tr>
			<td  height=24 class=tablebody1 >主版主可增删副版主：
			</td>
			<td  height=24 class=tablebody1 ><%if Dvbbs.Board_Setting(33)=1 then%>打开<%else%><FONT COLOR=RED>关闭</FONT><%end if%>
			</td>
		</tr>
		<tr>
			<td  height=24 class=tablebody2 >主版主可修改广告配置：
			</td>
			<td  height=24 class=tablebody2 ><%if Dvbbs.Board_Setting(34)=1 then%>打开<%else%><FONT COLOR=RED>关闭</FONT><%end if%>
			</td>
		</tr>
		<tr>
			<td  height=24 class=tablebody1 >所有版主均可修改广告配置：
			</td>
			<td  height=24 class=tablebody1 ><%if Dvbbs.Board_Setting(35)=1 then%>打开<%else%><FONT COLOR=RED>关闭</FONT><%end if%>
			</td>
		</tr>
		<tr>
			<td width="100%" height=24  colspan="2" class=tablebody2>
		<b>注意：</b>各个版面版主可以在自己版面自由发布公告和版面设置，管理员可以在所有版面发布，并对信息进行管理操作。
			</td>
		</tr>
		</table>
	      </td>
		  <td width="2%" valign=top align=center></td>
              <td width="70%" valign=top align=center>
      		<table cellpadding=3 cellspacing=1 class=tableborder1 style="width:100%;word-break:break-all;">
		  <tr>
			<td width="100%" height=24 class=tablebody1>
<B>注意</B>：<BR>本页面为版主专用，使用前请看左边相对应的功能是否打开，在进行管理设置的时候，不要随意更改设置，如需更改，必须填写完整或者正确的填写。
		  </td></tr>
		</table>
<%
Select Case request("action")
	Case "new"
		'Call savenews()
	Case "manage"
		Call manage()
	Case "updat"
		'Call Update()
	Case "del"
		'Call del()
	Case "editbminfo"
		Call editbminfo()
	Case "saveditbm"
		Call savebminfo()
	Case "editbmads"
		Call editbmads()
	Case "savebmads"
		Call savebmads()
	Case  Else 
End Select
%>
        </td>
    </tr>
</table>
<%
End Sub 

Sub editbmads()
	Dim master_1,chkedit
	If Not ChkBoardEditor(1) Then
		Response.Redirect "showerr.asp?ErrCodes=<li>你的权限不足，不能进行该项管理设置。&action=OtherErr"
	End If

	Set Rs=Dvbbs.Execute("select boardmaster,Board_Ads from dv_board where boardid="&Dvbbs.BoardID)
	If rs.eof and rs.bof Then Response.redirect "showerr.asp?ErrCodes=<li>您没有指定相应论坛ID，不能进行管理。&action=OtherErr"
	Dvbbs.Forum_Ads = Split(Rs(1),"$")
%>
<script language = "javaScript" src = "inc/toxhtml.js" type="text/javascript"></script>
<div style="display : none;" id="hiddenhtml"></div>
<form method="POST" action="?action=savebmads&boardid=<%=Dvbbs.BoardID%>">
<TABLE cellPadding=1 cellSpacing=1 class=tableborder1 align=center style="width:100%;word-break:break-all;">
<tr> 
<th height="23" colspan="2" class="tableHeaderText"><b>论坛广告设置</b>（如为设置分论坛，就是分论坛首页广告，下属页面为帖子显示页面）</th>
</tr>
<tr> 
<td colspan="2" class="tablebody1">注意事项：为防止管理帐号被盗用导致的挂马问题，如果您提交的广告数据含前台不接受的带object,iframe,script,link,meta等等有可能危害客户端的内容，请到后台广告管理中操作。</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>首页顶部广告代码</B></font></td>
<td width="60%" class="tablebody1"> 
<textarea name="Forum_ads(0)" cols="50" rows="3" onblur="fixtoxhtml(this)"><%=(Dvbbs.Forum_ads(0))%></textarea>
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>首页尾部广告代码</B></font></td>
<td width="60%" class="tablebody1"> 
<textarea name="Forum_ads(1)" cols="50" rows="3" onblur="fixtoxhtml(this)"><%=(Dvbbs.Forum_ads(1))%></textarea>
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>开启首页浮动广告</B></font></td>
<td width="60%" class="tablebody1"> 
<input type=radio name="Forum_ads(2)" value=0 <%if Dvbbs.Forum_ads(2)=0 then%>checked<%end if%>>关闭&nbsp;
<input type=radio name="Forum_ads(2)" value=1 <%if Dvbbs.Forum_ads(2)=1 then%>checked<%end if%>>打开&nbsp;
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>论坛首页浮动广告图片地址</B></font></td>
<td width="60%" class="tablebody1"> 
<input type="text" name="Forum_ads(3)" size="35" value="<%=Dvbbs.Forum_ads(3)%>" onblur="fixtoxhtml(this)">
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>论坛首页浮动广告连接地址</B></font></td>
<td width="60%" class="tablebody1"> 
<input type="text" name="Forum_ads(4)" size="35" value="<%=Dvbbs.Forum_ads(4)%>" onblur="fixtoxhtml(this)">
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>论坛首页浮动广告图片宽度</B></font></td>
<td width="60%" class="tablebody1"> 
<input type="text" name="Forum_ads(5)" size="3" value="<%=Dvbbs.Forum_ads(5)%>" onblur="fixtoxhtml(this)">&nbsp;象素
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>论坛首页浮动广告图片高度</B></font></td>
<td width="60%" class="tablebody1"> 
<input type="text" name="Forum_ads(6)" size="3" value="<%=Dvbbs.Forum_ads(6)%>" onblur="fixtoxhtml(this)">&nbsp;象素
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>开启首页右下固定广告</B></font></td>
<td width="60%" class="tablebody1"> 
<input type=radio name="Forum_ads(13)" value=0 <%if Dvbbs.Forum_ads(13)=0 then%>checked<%end if%>>关闭&nbsp;
<input type=radio name="Forum_ads(13)" value=1 <%if Dvbbs.Forum_ads(13)=1 then%>checked<%end if%>>打开&nbsp;
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>论坛首页右下固定广告图片地址</B></font></td>
<td width="60%" class="tablebody1"> 
<input type="text" name="Forum_ads(8)" size="35" value="<%=Dvbbs.Forum_ads(8)%>" onblur="fixtoxhtml(this)">
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>论坛首页右下固定广告连接地址</B></font></td>
<td width="60%" class="tablebody1"> 
<input type="text" name="Forum_ads(9)" size="35" value="<%=Dvbbs.Forum_ads(9)%>" onblur="fixtoxhtml(this)">
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>论坛首页右下固定广告图片宽度</B></font></td>
<td width="60%" class="tablebody1"> 
<input type="text" name="Forum_ads(10)" size="3" value="<%=Dvbbs.Forum_ads(10)%>" onblur="fixtoxhtml(this)">&nbsp;象素
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1"><B>论坛首页右下固定广告图片高度</B></font></td>
<td width="60%" class="tablebody1"> 
<input type="text" name="Forum_ads(11)" size="3" value="<%=Dvbbs.Forum_ads(11)%>" onblur="fixtoxhtml(this)">&nbsp;象素
</td>
</tr>

<tr> 
<td width="200" class="tablebody1"><B>是否开启帖间随机广告</B></td>
<td width="*" class="tablebody1"> 
<input type=radio name="Forum_ads(7)" value=0 <%if Dvbbs.Forum_ads(7)="0" then%>checked<%end if%>>关闭&nbsp;
<input type=radio name="Forum_ads(7)" value=1 <%if Dvbbs.Forum_ads(7)="1" then%>checked<%end if%>>打开&nbsp;
</td>
</tr>
<tr> 
<td width="*" class="tablebody1" valign="top" colspan=2><B>论坛帖间随机广告代码</B> <br>支持HTML语法，每条随机广告一行，用回车分开。</td>
</tr>
<tr>
<td width="*" class="tablebody1" colspan=2> 
<textarea name="Forum_ads(14)" style="width:100%" rows="10" onblur="fixtoxhtml(this,1)"><%=Dvbbs.Forum_ads(14)%></textarea>
</td>
</tr>
<tr> 
<td width="200" class="tablebody1"><B>是否开启页面文字广告位</B></td>
<td width="*" class="tablebody1"> 
<input type=radio name="Forum_ads(12)" value=0 <%if Dvbbs.Forum_ads(12)="0" then%>checked<%end if%>>关闭&nbsp;
<input type=radio name="Forum_ads(12)" value=1 <%if Dvbbs.Forum_ads(12)="1" then%>checked<%end if%>>打开&nbsp;
</td>
</tr>
<tr> 
<td width="200" class="tablebody1"><B>页面文字广告位设置(版面)</B><BR>请确认已打开了页面文字广告位功能<BR></td>
<td width="*" class="tablebody1"> 
<input type=radio name="Forum_ads(15)" value=0 <%if Dvbbs.Forum_ads(15)="0" then%>checked<%end if%>>帖子列表&nbsp;
<input type=radio name="Forum_ads(15)" value=1 <%if Dvbbs.Forum_ads(15)="1" then%>checked<%end if%>>帖子内容&nbsp;
<input type=radio name="Forum_ads(15)" value=2 <%if Dvbbs.Forum_ads(15)="2" then%>checked<%end if%>>两者都显示&nbsp;
<input type=radio name="Forum_ads(15)" value=3 <%if Dvbbs.Forum_ads(15)="3" then%>checked<%end if%>>两者都不显示&nbsp;
<input type="hidden" name="Forum_ads(17)" size="3" value="<%=Dvbbs.Forum_ads(17)%>">
</td>
</tr>

<tr> 
<td width="*" class="tablebody1" valign="top" colspan=2><B>页面文字广告位内容</B> <br>支持HTML语法，每条广告一行，用回车分开。</td>
</tr>
<tr>
<td width="*" class="tablebody1" colspan=2> 
<textarea name="Forum_ads(16)" style="width:100%" rows="10" onblur="fixtoxhtml(this,1)"><%=Dvbbs.Forum_ads(16)%></textarea>
</td>
</tr>
<tr> 
<td width="40%" class="tablebody1">&nbsp;</td>
<td width="60%" class="tablebody1"> 
<div align="center"> 
<input type="submit" name="Submit" value="提 交">
</div>
</td>
</tr>
</table>
</form>
<%
End Sub

Sub savebmads()
	If Not ChkBoardEditor(1) Then
		Response.Redirect "showerr.asp?ErrCodes=<li>你的权限不足，不能进行该项管理设置。&action=OtherErr"
	End If
	Dim Forum_adsinfo
	Dim iSetting
	For i = 0 To 30
		If Trim(Request.Form("Forum_ads("&i&")"))="" Then
			iSetting=0
		Else
			iSetting=Replace(Trim(Request.Form("Forum_ads("&i&")")),"$","")
		End If

		If i = 0 Then
			Forum_adsinfo = iSetting
		Else
			Forum_adsinfo = Forum_adsinfo & "$" & iSetting
		End If
	Next
	Dim checkinfo
	checkinfo=checkXHTML(Forum_adsinfo)
	If checkinfo="" Then
		sql = "update dv_board set board_ads='"&Replace(Forum_adsinfo,"'","''")&"' where boardid="&Dvbbs.boardid
		Dvbbs.Execute(sql)
		Dvbbs.LoadBoardData Dvbbs.BoardID
		LoardTextAd()
		Response.Write Dvbbs.BoardType&"广告设置成功。"
		Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,"&Dvbbs.BoardID&",'广告管理','" & Dvbbs.MemberName & "','设置 "&Dvbbs.boardtype&"广告','" & Dvbbs.userTrueIP & "',3)")
	Else
		Response.Write Dvbbs.BoardType&"广告设置失败，原因："&checkinfo&"。<br />如需要设置包含脚本或其他危险标签的广告，请到后台操作。"
		Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,"&Dvbbs.BoardID&",'广告管理','" & Dvbbs.MemberName & "','设置 "&Dvbbs.boardtype&"广告失败','" & Dvbbs.userTrueIP & "',3)")
	End If
End Sub

'版主设置权限	Bloon
'Act 0=修改基本设置，1=修改广告
Function ChkBoardEditor(Act)
	Dim Master,IsMaster
	IsMaster = False
	ChkBoardEditor = False
	'修正超版进入分版基本设置出错 2005-6-1 Dv.Yz
	If Dvbbs.Master Or Dvbbs.SuperBoardMaster Then
		ChkBoardEditor = True
		Exit Function
	End If

	Dim XpathSQL
	XpathSQL="boardmaster[@boardid = "& Dvbbs.Boardid &" and master='"& Dvbbs.UserSession.documentElement.selectSingleNode("userinfo/@username").text &"']"
	IsMaster=Not Application(Dvbbs.CacheName&"_boardmaster").documentElement.selectSingleNode(XpathSQL) Is Nothing
	
	Select Case Act
		Case 0
			If Dvbbs.Board_Setting(33) = "1" and IsMaster Then
				ChkBoardEditor = True
			End If
		Case 1
			If Dvbbs.Board_Setting(35) = "1" Then
				ChkBoardEditor = True
			Else
				If Dvbbs.Board_Setting(34) = "1" and IsMaster Then
					ChkBoardEditor = True
				End If
			End If
	End Select
End Function

Sub Editbminfo()
	If Not IsObject(Conn) Then ConnectionDatabase
	Dim Master_1
	Response.Write "<form action =""admin_boardset.asp?action=saveditbm&boardid="
	Response.Write Dvbbs.BoardID
	Response.Write """ method=post>"
	Set Rs = Dvbbs.iCreateObject("Adodb.Recordset")
	Sql = "SELECT * FROM Dv_Board WHERE Boardid = " & Dvbbs.Boardid
	Rs.Open Sql,Conn,1,1
	If Rs.Eof And Rs.bof Then Response.Redirect "showerr.asp?ErrCodes=<li>您没有指定相应论坛ID，不能进行管理。&action=OtherErr"
	If Not ChkBoardEditor(0) Then
		Response.Redirect "showerr.asp?ErrCodes=<li>你的权限不足，不能进行该项管理设置。&action=OtherErr"
	End If
%>
<script language = "javaScript" src = "inc/toxhtml.js" type="text/javascript"></script>
<div style="display : none;" id="hiddenhtml"></div>
<Input type='hidden' name=editid value='<%=Dvbbs.BoardID%>'>
<TABLE cellPadding=1 cellSpacing=1 class=tableborder1 align=center style="width:100%;word-break:break-all;">
<tr> 
<td colspan="2" class="tablebody1">注意事项：为防止管理帐号被盗用导致的挂马问题，如果您的版面说明，或版面规则包含带object,iframe,script,link,meta等等有可能危害客户端的内容，请到后台论坛管理中操作。</td>
</tr>
    <tr> 
    <th colspan="3" height=22 class=tablebody2><b>基本信息管理 </b> 
 
  <tr> 
      <td height=22 class=tablebody1  align="center">论坛名称：</td>
      <td  class=tablebody1>
	  <input type="text" name="BoardType" size="30" value="<%=server.HTMLEncode(rs("BoardType"))%>" readonly />
	  </td>
    </tr>
    <tr> 
      <td height=22 class=tablebody2  align="center">版面说明：</td>
      <td  class=tablebody1>
      <textarea name="Readme" cols="80" rows="5" onblur="fixtoxhtml(this)"><%=server.HTMLEncode(rs("readme"))%></textarea>
      </td>
    </tr>
    <tr> 
      <td height=22 class=tablebody2  align="center">版面规则：</td>
      <td  class=tablebody1>
      <textarea name="Rules" cols="80" rows="5" onblur="fixtoxhtml(this)"><%=Server.Htmlencode(rs("Rules")&"")%></textarea>
      </td>
    </tr>
    <tr> 
      <td height=22 class=tablebody1  align="center">版主修改：</td>
      <td  class=tablebody1> 
        <input type="text" name="boardmaster" size="50" value="<%=server.HTMLEncode(rs("boardmaster")&"")%>"><BR>(多版主添加请用|分隔，如：沙滩小子|wodeail)
      </td>
    </tr>
    <%If Cint(Dvbbs.Board_Setting(2))=1 Then%>
    <tr> 
      <td height=22 class=tablebody1  align="center">认证用户：</td>
      <td  class=tablebody1> 
      <textarea name="boarduser" cols="80" rows="3"><%=replace(rs("boarduser")&"",",",chr(13)&chr(10))%></textarea><li>每个用<b>回车</b>分隔开
      </td>
    </tr>
    <%End If%>

    <tr> 
      <td height=22 class=tablebody2>&nbsp;</td>
      <td  class=tablebody2> 
        <input type="submit" name="Submit" value="提交">
      </td>
    </tr>
  </table>
</form>
<%
rs.close
End Sub

Sub Savebminfo()
	If Not ChkBoardEditor(0) Then
		Response.Redirect "showerr.asp?ErrCodes=<li>你的权限不足，不能进行该项管理设置。&action=OtherErr"
	End If
	Dim Rname, i,upmaster
	Dim Readme, BoardType, Boardmaster, Sid, Boarduser, Rules
	Readme = Request.Form("readme")
	BoardType = Request.Form("BoardType")
	Boardmaster = Request.Form("boardmaster")
	Rules = Request.Form("Rules")
	Dim Checkinfo
	Checkinfo=checkXHTML(Request.Form("readme"))
	If Checkinfo<>"" Then
		Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,"&Dvbbs.BoardID&",'论坛基本信息','" & Dvbbs.MemberName & "','对版面"& Dvbbs.BoardType&"进行基本信息管理失败 ','" & Dvbbs.userTrueIP & "',3)")
		Response.Write "<p>论坛修改失败，原因是版面说明中"& Checkinfo&"<br />如果一定要输入带脚本或危险标签的内容，请到后台操作。</p>"
		Exit Sub
	End If
	Checkinfo=checkXHTML(Request.Form("Rules"))
	If Checkinfo<>"" Then
		Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,"&Dvbbs.BoardID&",'论坛基本信息','" & Dvbbs.MemberName & "','对版面"& Dvbbs.BoardType&"进行基本信息管理失败','" & Dvbbs.userTrueIP & "',3)")
		Response.Write "<p>论坛修改失败，原因"& Checkinfo&"<br />如果一定要输入带脚本或危险标签的内容，请到后台操作。</p>"
		Exit Sub
	End If
	If Cint(Dvbbs.Board_Setting(2)) = 1 Then
		Boarduser = Request.Form("boarduser")
		Boarduser = Replace(boarduser,chr(13)&chr(10),",")
	End If
	'Sid = Request("sid")
	'If IsNumeric(Sid) = 0 Or Sid = "" Then Response.Redirect "showerr.asp?ErrCodes=<li>非法的模板编号&action=OtherErr"
	'Sid=CLng(Sid)
	If Len(Readme) > 255 Then Response.Redirect "showerr.asp?ErrCodes=<li>论坛简介多于255个字。&action=OtherErr"
	If BoardType = "" Then Response.Redirect "showerr.asp?ErrCodes=<li>请输入论坛名称。&action=OtherErr"
	'If Boardmaster = "" Then Response.Redirect "showerr.asp?ErrCodes=<li>请输入管理成员。&action=OtherErr"
	Rname = split(Boardmaster,"|")
	For i = 0 To Ubound(Rname)
		Sql = "SELECT TOP 1 Username FROM [Dv_User] WHERE Username = '" & Replace(Rname(i),"'","") & "'"
		Set Rs = Dvbbs.Execute(Sql)
		If Rs.Eof And Rs.Bof Then
			Response.Redirect "showerr.asp?ErrCodes=<li>论坛没有" & Replace(Rname(i), "'", "") & "这个用户，不能添加为版主&action=OtherErr"
			Exit For
		End If
		Set Rs = Nothing
	Next

	Dim Classname, Titlepic
	Set Rs = Dvbbs.Execute("SELECT Usertitle, GroupPic FROM [Dv_UserGroups] WHERE UserGroupID = 3")
	If Not (Rs.Eof And Rs.Bof) Then
		Classname = Rs(0)
		Titlepic = Rs(1)
	End If
	For i = 0 To Ubound(Rname)
		Sql = "SELECT Top 1 UserGroupID From [Dv_User] WHERE Username = '" & Replace(Rname(i), "'", "") & "'"
		Set Rs = Dvbbs.Execute(Sql)
		If Rs(0) > 3 Then Dvbbs.Execute("Update [Dv_user] Set UserGroupID = 3, Userclass = '" & Classname & "', Titlepic = '" & Titlepic & "' WHERE Username = '" & Replace(Rname(i), "'", "") & "'" )
		Set Rs = Nothing
	Next

	Set Rs = Dvbbs.iCreateObject("Adodb.Recordset")
	Sql = "SELECT * FROM Dv_Board WHERE Boardid = " & Dvbbs.BoardID
	Rs.Open Sql,Conn,1,3
	If Rs.Eof And Rs.Bof Then
		Response.redirect "showerr.asp?ErrCodes=<li>您没有指定相应论坛ID，不能进行管理。&action=OtherErr"
	End If
	If Trim(Boardmaster)<>Trim(Rs("Boardmaster")) Then upmaster=1
	Rs("Boardmaster") = Boardmaster
	Rs("Readme") = Readme
	Rs("Rules") = Rules
	Rs("BoardType") = BoardType
	If Cint(Dvbbs.Board_Setting(2)) = 1 Then Rs("Boarduser") = Boarduser
	'Rs("Sid") = Clng(Sid)
	Rs.Update
		
	Dvbbs.Execute("Insert Into Dv_Log (l_AnnounceID,l_BoardID,l_touser,l_username,l_content,l_ip,l_type) values (0,"&Dvbbs.BoardID&",'论坛基本信息','" & Dvbbs.MemberName & "','对版面"& Dvbbs.BoardType&"进行基本信息管理 ','" & Dvbbs.userTrueIP & "',3)")
	
	Response.Write "<p>论坛修改成功！"
	Dvbbs.LoadBoardList()
	Dvbbs.LoadBoardData Dvbbs.BoardID
End Sub
%>

