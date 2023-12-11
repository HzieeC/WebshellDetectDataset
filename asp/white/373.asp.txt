<!--#include file="../inc/access.asp"  -->
<!-- #include file="inc/functions.asp" -->
<%
act=Request("act")
pg=Request("pg")
If act="save" Then
	Set rs=server.CreateObject("adodb.recordset")
	sql="select * from gb where id=" & request("hfid")
	rs.open sql,cn,3,3
	rs("hf")=request("hf")
	rs("hfsj")=Date()
	rs("stat")=Request("stat")
	rs.update
	rs.close:Set rs=Nothing
	Response.Redirect "GBMgr.asp?pg=" & request("pg")
End If

Call header()
Call Body()
Call Footer()


Sub Body()
	Dim s,sql,rs
	id=request("id")
	sql="select * from GB where id=" & id
	Set rs=cn.execute(sql)

	s="<form name='frm1' method='post' action='?act=save'  onSubmit='return Validator.Validate(this,3)'>"
	s=s & "<input type='hidden' name='hfid' value='"&rs("id")&"'>"
	s=s & "<input type='hidden' name='pg' value='"&pg&"'>"
	s=s & "<table cellpadding='2' cellspacing='1' border='0' class='tableBorder' align=center>"
	s=s & "<tr><th class='tableHeaderText' height=25>留言反馈管理-回复</th><tr>"
	s=s & "<tr><td>"

	s=s & "<table width='100%' cellpadding='0' cellspacing='1' border=0>"
	s=s & "<tr><td class='forumRowHighLight'>姓名</td>"
	s=s & "<td class='"&css1&"'>"&rs("xm")&"</td> </tr>"

	s=s & "<tr><td class='forumRowHighLight'>电话</td>"
	s=s & "<td class='"&css1&"'>"&rs("dh")&"</td> </tr>"

	s=s & "<tr><td class='forumRowHighLight'>邮箱</td>"
	s=s & "<td class='"&css1&"'>"&rs("email")&"</td> </tr>"

	s=s & "<tr><td class='forumRowHighLight'>内容</td>"
	s=s & "<td class='"&css1&"'>"&rs("lr")&"</td> </tr>"

	s=s & "<tr><td class='forumRowHighLight'>登记日期</td>"
	s=s & "<td class='"&css1&"'>"&rs("djsj")&"</td> </tr>"

	s=s & "<tr><td class='forumRowHighLight'>是否显示</td>"
	s=s & "<td>"&sstr("stat","0|1","隐藏|显示",rs("stat"),"")&"</td> </tr>"
	s=s & "<tr><td class='forumRowHighLight'>回复</td>"
	s=s & "<td class='"&css1&"'><textarea id='hf' name='hf' rows='4' cols='60'>"&rs("hf")&"</textarea></td> </tr>"
	s=s & "<tr><td class='forumRowHighLight'><input type='submit'  value='提交'/></td><td></td> </tr>"
	s=s & "</table>"

	s=s & "</td></tr>"
	s=s & "</table>"
	s=s & "</form><script language='JavaScript' src='Images/val.js'></script>"
	Response.Write s
	rs.close:Set rs=Nothing
End Sub

Call DbconnEnd()
%>
