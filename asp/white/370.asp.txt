<%
Dim ShowContentPage,cn
Dim ShowPageSzie,TitleImg
ShowContentPage="showcontent.asp"
ShowProductPage="cp1.asp"
ShowPageSize=25
TitleImg1="images/arrow01.gif"
TitleImg2="images/arrow01.gif"

Sub dbcn()
	If IsObject(Cn) = True Then Exit Sub
	Set Cn = Server.CreateObject("ADODB.Connection")
	On Error Resume Next
	Cn.Open "Provider=Microsoft.Jet.OLEDB.4.0; Data Source=" & Server.MapPath(".") & "\cms2007\db\#data.mdb"
	If Err.Number > 0 Then
		Response.End
	End If
End Sub
'======================
Call dbcn()
'======================
Function GDB(sql)
	Dim s,rs
	'response.Write sql & "<br>"
	Set rs=cn.execute(sql)
	If Not (rs.bof Or rs.eof) Then
		s=rs(0)
	Else
		s=0
	End If
	rs.close:Set rs=Nothing
	If IsNull(s) Then s=0
	GDB=s
End Function

'======================

Sub ShowContent(id)
	Dim s,rs,sql
	sql="select * from newsdata where d_id=" & id
	Set rs=cn.execute(sql)
	If Not (rs.bof Or rs.eof) Then
		s=s & "<div style='text-align: center; font-weight: bold; color: #404040; margin: 5px 10px 10px 10px;'>" & rs("d_title") & "</div>"
		s=s & "<div style='text-align: center; margin: 10px 10px 10px 10px;'>" & rs("d_time") & "</div>"
		s=s & "<div style='margin: 10px 10px 10px 10px;'>" & rs("d_content") & "</div>"
	Else
		s="无此ID内容!"
	End If
	rs.close:Set rs=Nothing
	s=s & "<div style='text-align: center; margin-top: 30px; font-weight: bold;'><a href='#' onclick='window.close();'>[关闭]</a></div>"
	Response.write s
End Sub


'======================
Sub ShowTopList(cid,n,tw)
	Dim s,rs,sql,css1
	style1="style='border-bottom: 1px dotted #CDCDCD;'"
	sql="select top "&n&" d_id,d_title,d_intro,d_picture,d_time from newsdata where d_class=" & cid & " order by d_time desc"
	Set rs=cn.execute(sql)
	If Not (rs.bof Or rs.eof) Then
		s=s & "<table style='width:100%;' cellpadding=3 cellspacing=2 border=0>"
		For i=1 To CInt(n)
			s=s & "<tr><td "&style1&" width='8'><img src='"&titleimg1&"' width='12' height='10' alt='' /></td>"
			s=s & "<td "&style1&">"&FormatDateTime(rs("d_time"),1)&" | <a href='"&ShowContentPage&"?id="&rs("d_id")&"' target='_blank'>"&Left(rs("d_title"),tw)&"</a></td>"
			s=s & "</tr>"
			's=s & "<tr><td background='images/line2.gif' colspan=2><img  height='1'  src='images/line2.gif'  width='0' /></td></tr>"
			rs.MoveNext
			If rs.eof Then Exit For
		Next
		s=s & "</table>"
	Else
		s="<b>暂未加入内容!</b>"
	End If
	rs.close:Set rs=Nothing
	Response.Write s
End Sub
'================================================


Sub ShowImgNews(cid)
	Dim s,rs,sql
	sql="select top 1 d_id,d_title,d_picture,d_class,d_intro from newsdata where d_class=" & cid & " order by d_time desc"
	Set rs=cn.execute(sql)
	If Not (rs.bof Or rs.eof) Then
		s=s & "<div>"
		s=s & "<a href='"&showcontentpage&"?id="&rs("d_id")&"' target='_blank'><img src='"&rs("d_picture")&"' style='width: 240px; height: 190px; border: 0px solid #FFFFFF;' alt='"&rs("d_title")&"'></a>"
		s=s & "<div style='text-align: center;'>"&rs("d_title")&"</div>"
		s=s & "</div>"
	Else
		s="<div><b>暂无图片内容!</b></div>"
	End If
	rs.close:Set rs=Nothing
	Response.Write s
End Sub
'===========================================


Sub ShowTopContent(cid)
	Dim s,rs,sql
	sql="select top 1 * from newsdata where d_class=" & cid &  " order by d_time desc"
	Set rs=cn.execute(sql)
	If Not (rs.bof Or rs.eof) Then
		's=s & "<div style='text-align: center; font-weight: bold;'>" & rs("d_title") & "</div>"
		's=s & "<div style='text-align: center;'>" & rs("d_time") & "</div>"
		s=s & "<div>" & rs("d_content") & "</div>"
	Else
		s="无此ID内容!"
	End If
	rs.close:Set rs=Nothing
	Response.write s
End Sub

'===========================================

Sub ShowTabList(cid)
	Dim pg,s,rs,sql
	ws="&cid=" &cid
	sql="select * from newsdata where d_class=" & cid & " order by d_time desc"
	Set rs=server.CreateObject("adodb.recordset")
	rs.open sql,cn,1,1
	rs.Pagesize=ShowPageSize
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
		style1="style='border-bottom: 1px dotted #E1E1E1;'"
		s=s & "<table width='100%' cellpadding='2' cellspacing='2' border=0>"
		s=s & "<tr><td "&style1&" ></td><td "&style1&" ><strong>新闻标题</strong></td> <td "&style1&" ><strong>发布日期</strong></td> </tr>"
		For i=1 To CInt(ShowPageSize)
			s=s &  "<tr valign='top'>"
			s=s & "<td "&style1&" width='8'><img src='"&titleimg2&"' width='11' height='15' alt='' /></td>"
			s=s & "<td "&style1&"><A HREF='"&ShowContentPage&"?cid="&cid&"&id="&rs("d_id")&"' target='_blank'>"  & rs("d_title") & "</A></td>"
			s=s & "<td "&style1&">"&FormatDateTime(rs("d_time"),1)&"</td>"
			s=s & "</tr>"
			rs.Movenext
			If rs.eof Then Exit For
		Next
		s=s & "</table>"
		pglist0="页次: "& pg & "/" & pc& "&nbsp;&nbsp;&nbsp;&nbsp;"
		pglist1=""
		pglist2="<A HREF='?pg=1"&ws&"'>首页</A>, <A HREF='?pg="&pg-1&ws&"'>上一页</A>, <A HREF='?pg="&pg+1&ws&"'>下一页</A>, <A HREF='?pg="&pgs&ws&"'>尾页</A>"
		s=s & "<div style='text-align: center; margin-top: 20px;'>"&pglist0 & pglist2&"</div>"
	Else
		s="<div style='color: red; text-align: center;margin-top: 20px;'>暂未添加内容! </div>"
	End If
	Response.Write s
End Sub
'=====================================
Sub ShowGB()
	Dim s,sql,rs
	s=s & "<form name='frm1' method='post' action='?act=save'  onSubmit='return Validator.Validate(this,3)'>"
	s=s & "<table>"
	s=s & "<tr><td>姓名: </td> <td><input type='text' name='xm' dataType='Require' msg='请填写'></td> </tr>"
	s=s & "<tr><td>电话: </td> <td><input type='text' name='dh' dataType='Require' msg='请填写'>（不公开，请放心填写）</td> </tr>"
	s=s & "<tr><td>邮箱: </td> <td><input type='text' name='email' dataType='Email' msg='请正确填写e-mail'>（不公开，请放心填写）</td> </tr>"
	s=s & "<tr><td>内容: </td> <td><textarea id='lr' rows='4' cols='50' name='bz'  dataType='Require' msg='请填写'></textarea>（请留下详细联系方式和您的意见或者需求）</td> </tr>"
	s=s & "<tr><td><input type='submit' name='submit1' value='提交'> </td> <td></td> </tr>"
	s=s & "</table>"
	s=s & "</from><script language='JavaScript' src='images/val.js'></script>"

	s=s & "<div style='background: #E4E4E4; margin: 10px 3px 3px 3px;'><b>回复</b></b></div>"

	sql="select xm,hf,djsj,hfsj from GB where stat=1 order by hfsj desc"
	Set rs=server.CreateObject("adodb.recordset")
	rs.open sql,cn,1,1
	rs.Pagesize=ShowPageSize
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
		style1="style='border-bottom: 1px dotted #E1E1E1;'"
		s=s & "<table width='100%' cellpadding='2' cellspacing='3' border=0>"

		For i=1 To CInt(ShowPageSize)
			s=s & "<tr><td>["&rs("djsj")&"]&nbsp;&nbsp;<span style='font-weight: bold; color: #FF0000;'>" & rs("xm")&"</b></td></tr>"
			s=s & "<tr><td><span style='color: gray;'><i>单独给管理员的留言</i></span></td></tr>"
			s=s & "<tr><td style='border-bottom: 1px solid #959595'>&nbsp;&nbsp;&nbsp;&nbsp;回复:"&rs("hf")&"</td></tr>"
			rs.Movenext
			If rs.eof Then Exit For
		Next
		s=s & "</table>"
		pglist0="页次: "& pg & "/" & pc& "&nbsp;&nbsp;&nbsp;&nbsp;"
		pglist1=""
		pglist2="<A HREF='?pg=1"&ws&"'>首页</A>, <A HREF='?pg="&pg-1&ws&"'>上一页</A>, <A HREF='?pg="&pg+1&ws&"'>下一页</A>, <A HREF='?pg="&pgs&ws&"'>尾页</A>"
		s=s & "<div style='text-align: center; margin-top: 20px;'>"&pglist0 & pglist2&"</div>"
	Else
		s=s & "<div style='color: red; text-align: center;margin-top: 20px;'>暂无回复内容! </div>"
	End If
	response.Write s
End Sub

'=====================================
Sub SaveOrder()
	Dim rs,sql,s,act,bag4,abag4
	act=request("act")
	If act="save" Then
		bag4=Session("bag")
		abag4=Split(bag4,"|")
		For i=0 To UBound(abag4)
			s=s & " 订购产品: " & gdb("select d_title from productsdata where d_id="&abag4(i))
			s=s & " 订购数量: " & request("f" & abag4(i)) & "<br />"
		Next
		Set abag4=Nothing

		Set rs=server.CreateObject("adodb.recordset")
		sql="select * from ProOrder where id=0"
		rs.open sql,cn,3,3
		rs.addnew
		rs("dgcp")=s
		rs("lxr")=request("lxr")
		rs("lxdh")=request("lxdh")
		rs("email")=request("email")
		rs("lxdz")=request("lxdz")
		rs("bz")=request("bz")
		rs("dgsj")=Date()
		rs.update
		rs.close:Set rs=Nothing
		s="<script language=""javascript"">alert(""感谢您的订购,我们将尽快处理!"");</script>"
		Session("bag")=""
		response.Write s
	End If
End Sub
'=====================================
Sub ShowOrder()
	Dim s,rs,sql,newpid,i,j,act,bag,bag0,bag2
	act=request("act")
	Bag=Session("Bag")
	'response.Write "bag=" & bag
	pid=Request("pid")
	s=s & "<form name='frm1' method='post' action='?act=save'  onSubmit='return Validator.Validate(this,3)'>"
	s=s & "<div style='background: #E4E4E4; margin: 3px 3px 3px 3px;'><b>您已订购的产品</b></div>"

	If bag="" And pid="" Then
		s=s & "<div style='color: red;margin-left: 20px;'>您还没有订购任何产品, <a href='cp1.asp'><b>点此</b></a>选择产品订购"
		s=s & "<input type='text' name='bag' dataType='Require' msg='' value='' style='width:0px;height:0px;'></div>"
	Else
		If act="del" Then
			bag0=session("bag")
			abag0=Split(bag0,"|")
			bag2=""
			del_id=request("del_id")
			For i=0 To UBound(abag0)
				If Not CLng(i)=CLng(del_id) Then
					If bag2="" Then
						bag2=abag0(i)
					Else
						bag2=bag2 & "|" & abag0(i)
					End If
				End If
			Next
			Set abag0=Nothing
			Session("bag")=bag2
			Response.Redirect "xs1.asp"
			response.End
		End If

		s=s & "<div style='color: red;margin-left: 20px;'><a href='cp1.asp'><b>点此</b></a>继续订购</div>"
		If pid<>"" Then
			bag=session("bag")
			If bag="" Then
				bag=pid
			Else
				bag=bag & "|" & pid
			End If
			session("bag")=bag
		End If

		bag=Session("bag")
		s=s & "<table style='margin-left: 20px;width: 50%;'>"
		s=s & "<tr style='background: #FFD2D2'><td>产品名称</td> <td>数量</td> <td>操作</td> </tr>"
		aBag=Split(bag,"|")
		For i=0 To UBound(aBag)
			s=s & "<tr><td>"&gdb("select d_title from productsdata where d_id="&aBag(i))&"</td>"
			s=s & "<td><input type='text' name='f"&abag(i)&"' value='1' size='3'></td>"
			s=s & "<td><a href='?act=del&del_id="&i&"'>[取消]</a></td><tr>"
		Next
		s=s & "</table>"
		Set abag=Nothing
	End If


	s=s & "<div style='background: #E4E4E4; margin: 3px 3px 3px 3px;'><b>您的联系信息</b></div>"
	s=s & "<table style='margin-left: 20px;'>"

	s=s & "<tr> <td width='70'>联系人:</td> <td><input type='text' name='lxr' dataType='Require' msg='请填写'></td> </tr>"
	s=s & "<tr> <td>联系电话:</td> <td><input type='text' name='lxdh' dataType='Require' msg='请填写'></td> </tr>"
	s=s & "<tr> <td>E-Mail:</td> <td><input type='text' name='email' dataType='Email' msg='请正确填写email' size='40'></td> </tr>"
	s=s & "<tr> <td>联系地址:</td> <td><input type='text' name='lxdz' size='70' dataType='Require' msg='请填写'></td> </tr>"
	s=s & "<tr> <td>订购备注:</td> <td><textarea id='bz' rows='4' cols='70' name='bz'  dataType='Require' msg='请填写'></textarea></td> </tr>"
	s=s & "<tr> <td colspan=2><input type='submit' name='submit1' value='提交'></td> </tr>"
	s=s & "</table>"
	s=s & "</form><script language='JavaScript' src='images/val.js'></script>"
	Response.Write s
End Sub



'========================
Sub SaveGB()
	Dim sql,rs,act,s
	act=Request("act")
	If act="save" Then
		Set rs=server.CreateObject("adodb.recordset")
		sql="select * from GB where id=0"
		rs.open sql,cn,3,3
		rs.Addnew
		rs("xm")=request("xm")
		rs("dh")=request("dh")
		rs("email")=request("email")
		rs("lr")=request("lr")
		rs("djsj")=Date()
		rs.update
		rs.close:Set rs=Nothing
		s="<script language=""javascript"">alert(""感谢您的留言反馈,我们将尽快处理!"");</script>"
		response.Write s
	End If
End Sub

'=========================
Sub ShowProductList(cid)
	Dim s,rs,sql,i,dw,sw
	sql="select * from ProductsData where d_class=" & cid
	Set rs=server.CreateObject("adodb.recordset")
	rs.open sql,cn,1,1

	If Not rs.eof Then
		rs.Pagesize=ShowPageSize
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

		For i=1 To CInt(ShowPageSize)
			If (i+3) Mod 4=0 Then
				s=s & "<div style='width:100%;margin-bottom: 10px;'>"
			End If
			s=s & "<span style='width:25%;text-align:center;padding:4px 4px 4px 4px;'>"
			s=s & "<a href='Showproducts.asp?id="&rs("d_id")&"' target='_blank' >"
			s=s & "<img src='"&rs("d_picture")&"'  style='width: 150px' style='border: 1px solid white;'/><br />"
			s=s & "<strong>" & rs("d_title") & "</strong>"
			s=s & "</a>"
			s=s & "<br />" & rs("d_intro") & ""
			s=s & "<br /><a href='xs1.asp?pid="&rs("d_id")&"'><img src='images/buy.gif' border=0/></a>"
			s=s & "</span>"
			If (i Mod 4=0) Then
				s=s & "</div>"
			End If
			rs.Movenext
			If rs.eof Then Exit For
		Next
		s=s & "</table>"
		pglist0="页次: "& pg & "/" & pc& "&nbsp;&nbsp;&nbsp;&nbsp;"
		pglist1=""
		pglist2="<A HREF='?pg=1"&ws&"'>首页</A>, <A HREF='?pg="&pg-1&ws&"'>上一页</A>, <A HREF='?pg="&pg+1&ws&"'>下一页</A>, <A HREF='?pg="&pgs&ws&"'>尾页</A>"
		s=s & "<div style='text-align: center; margin-top: 20px;'>"&pglist0 & pglist2&"</div>"
	Else
		s="<div style='color: red; text-align: center;margin-top: 20px;'>暂未添加内容! </div>"
	End If
	rs.close:Set rs=Nothing
	Response.Write s
End Sub


'======================
Sub ShowProduct(id)
	Dim s,rs,sql
	sql="select * from productsdata where d_id=" & id
	Set rs=cn.execute(sql)
	If Not rs.eof Then
		's=s & "<div>"
		's=s & "<span><img src="" /></span>"
		's=s & "<span> </span>"
		's=s & "</div>"
		s=s & "<div style='font-weight: bold; color: #FF3300'>" & rs("d_title") & "</div>"
		s=s & "<div><a href='xs1.asp?pid="&rs("d_id")&"'><img src='images/buy.gif' border=0 /></a></div>"
		s=s & "<div>" & rs("d_intro") & "</div>"
		s=s & "<div>" & rs("d_content") & "</div>"
		s=s & "<p><a href='#' onclick='window.close();'>[关闭窗口]</a></p>"
	Else
		s="无此产品详细内容!"
	End If
	rs.close:Set rs=Nothing
	Response.Write s
End Sub

'================
Sub ShowPC(cid,showpage)
	Dim s,rs,sql
	s="<ul style='list-style:none;line-height: 2;text-align: left;'> "
	sql="select * from productsClass where ParentID=" & cid
	Set rs=cn.execute(sql)
	If Not rs.eof Then
		Do While Not rs.eof
			s=s & "<li><a href='"&showpage&"?cid="&rs("id")&"'>" & rs("name") & "</a></li>"
			rs.MoveNext
		Loop
	Else
		s=s & "<li style='color: red;'>暂无分类！</li>"
	End If
	rs.close:Set rs=Nothing
	s=s & "</ul>"
	Response.Write s
End Sub

'===========================
%>