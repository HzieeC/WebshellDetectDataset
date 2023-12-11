<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<%
act=Request("act")
If act="del" Then
	cn.execute("delete from gb where id=" & request("del_id"))
End If

Call header()
Call Body()
Call Footer()


Sub Body()
	Dim s,sql,rs

	's="<form name='frm1' method='post' action='?act=save'  onSubmit='return Validator.Validate(this,3)'>"
	s=s & "<table cellpadding='0' cellspacing='1' border='0' class='tableBorder' align=center>"
	s=s & "<tr><th class='tableHeaderText' height=25>留言反馈管理</th><tr>"
	s=s & "<tr><td>"
	sql="select * from GB order by djsj desc"
	Set rs=server.CreateObject("adodb.recordset")
	rs.open sql,cn,1,1
	rs.Pagesize=list_pagesize
	If Not rs.eof Then
		pc=rs.pagecount
		rc=rs.recordcount
		pg=request("pg")
		If pg="" Then
			pg=1
		Else
			If CInt(pg)>CInt(pgs) Then pg=1
			If CInt(pg)<=0 Then pg=1
		End If
		rs.Absolutepage=pg

		s=s & "<table width='100%' cellpadding='0' cellspacing='1' border=0>"
		s=s & "<tr style='font-weight: bold;'>"
		s=s & "<td class='forumRowHighLight'>姓名</td>"
		s=s & "<td class='forumRowHighLight'>电话</td>"
		s=s & "<td class='forumRowHighLight'>邮箱</td>"
		s=s & "<td class='forumRowHighLight'>内容</td>"
		s=s & "<td class='forumRowHighLight'>登记日期</td>"
		s=s & "<td class='forumRowHighLight'>回复</td>"
		s=s & "<td class='forumRowHighLight'>回复日期</td>"
		s=s & "<td class='forumRowHighLight'>是否显示</td>"
		s=s & "<td class='forumRowHighLight' width='40'>操作</td>"
		s=s & "</tr>"
		For i=1 To CInt(list_pagesize)
			If i Mod 2=0 Then
				css1="forumRowHighLight"
			Else
				css1="forumRow"
			End If
			s=s & "<tr>"
			s=s & "<td class='"&css1&"'>"&rs("xm")&"</td>"
			s=s & "<td class='"&css1&"'>"&rs("dh")&"</td>"
			s=s & "<td class='"&css1&"'>"&rs("email")&"</td>"
			s=s & "<td class='"&css1&"'>"&rs("lr")&"</td>"
			s=s & "<td class='"&css1&"'>"&rs("djsj")&"</td>"
			s=s & "<td class='"&css1&"'>"&rs("hf")&"</td>"
			s=s & "<td class='"&css1&"'>"&rs("hfsj")&"</td>"
			s=s & "<td class='"&css1&"'>"&gstr("0|1","隐藏|显示",rs("stat"))&"</td>"
			s=s & "<td class='"&css1&"'><a href='?act=del&del_id="&rs("id")&"' onclick='return confirm(""确认删除?"");'>[删除]</a><br /><a href='GBhf.asp?id="&rs("id")&"&pg="&pg&"'>[管理]</a></td>"
			s=s & "</tr>"
			rs.Movenext
			If rs.eof Then Exit For
		Next
		s=s & "</table>"

		For i=1 To Clng(pc)
				If CLng(pg)=i Then
					pagelist=pagelist & " <b style='color:blue;'>"& i &"</b> "
				Else
					pagelist=pagelist & " <a href=""?pg="& i & ws &  """>"& i &"</a> "
				End If
		Next
		s=s & "<div style='text-align: center; margin-top: 20px;'>分页: "&pagelist&"</div>"
	Else
		s="<div style='color: red; text-align: center;margin-top: 20px;'>暂无回复内容! </div>"
	End If

	s=s & "</td></tr>"
	s=s & "</table>"
	's=s & "</form><script language='JavaScript' src='Images/val.js'></script>"
	Response.Write s
End Sub

Call DbconnEnd()
%>
