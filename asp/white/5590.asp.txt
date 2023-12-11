<%@ CODEPAGE=65001 %>
<%
'///////////////////////////////////////////////////////////////////////////////
'// 插件应用:    Z-Blog 1.7
'// 插件制作:    
'// 备    注:    
'// 最后修改：   
'// 最后版本:    
'///////////////////////////////////////////////////////////////////////////////
%>
<% Option Explicit %>
<% On Error Resume Next %>
<% Response.Charset="UTF-8" %>
<% Response.Buffer=True %>
<!-- #include file="../../c_option.asp" -->
<!-- #include file="../../function/c_function.asp" -->
<!-- #include file="../../function/c_function_md5.asp" -->
<!-- #include file="../../function/c_system_lib.asp" -->
<!-- #include file="../../function/c_system_base.asp" -->
<!-- #include file="../../function/c_system_event.asp" -->
<!-- #include file="../../function/c_system_plugin.asp" -->
<%

Call System_Initialize()

'检查非法链接
Call CheckReference("")

'检查权限
If BlogUser.Level>1 Then Call ShowError(6) 

If CheckPluginState("Totoro")=False Then Call ShowError(48)

BlogTitle="TotoroⅡ（基于Totoro的Z-Blog的评论及引用管理审核系统增强版）"

%><!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="<%=ZC_BLOG_LANGUAGE%>" lang="<%=ZC_BLOG_LANGUAGE%>">
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta http-equiv="Content-Language" content="<%=ZC_BLOG_LANGUAGE%>" />
	<link rel="stylesheet" rev="stylesheet" href="../../CSS/admin.css" type="text/css" media="screen" />
	<script language="JavaScript" src="../../script/common.js" type="text/javascript"></script>
	<title><%=BlogTitle%></title>
</head>
<body>

			<div id="divMain">
<div class="Header"><%=BlogTitle%></div>
<div class="SubMenu"><span class="m-left"><a href="setting.asp">TotoroⅡ设置</a></span><span class="m-left"><a href="setting1.asp">审核评论<%
	Dim objRS1
	Set objRS1=objConn.Execute("SELECT COUNT([comm_ID]) FROM [blog_Comment] WHERE [log_ID]<0")
	If (Not objRS1.bof) And (Not objRS1.eof) Then
		Response.Write "("&objRS1(0)&"条未审核的评论)"
	End If
%></a></span><span class="m-left m-now"><a href="setting2.asp">审核引用<%
	Dim objRS2
	Set objRS2=objConn.Execute("SELECT COUNT([tb_ID]) FROM [blog_TrackBack] WHERE [log_ID]<0")
	If (Not objRS2.bof) And (Not objRS2.eof) Then
		Response.Write "("&objRS2(0)&"条未审核的引用)"
	End If
%></a></span></div>
<div id="divMain2">
<%
	Dim intPage
	Dim i
	Dim objRS
	Dim strSQL
	Dim strPage

	intPage=Request.QueryString("page")

	Call CheckParameter(intPage,"int",1)

	Set objRS=Server.CreateObject("ADODB.Recordset")
	objRS.CursorType = adOpenKeyset
	objRS.LockType = adLockReadOnly
	objRS.ActiveConnection=objConn
	objRS.Source=""

	strSQL="WHERE ([log_ID]<0) "
	If CheckRights("Root")=False Then strSQL=strSQL & "AND( (SELECT [log_AuthorID] FROM [blog_Article] WHERE [blog_Article].[log_ID] =[blog_TrackBack].[log_ID] ) =" & BlogUser.ID & ")"


	Call GetBlogHint()

	Response.Write "<table border=""1"" width=""100%"" cellspacing=""1"" cellpadding=""1"">"
	Response.Write "<tr><td>"& ZC_MSG048 & ZC_MSG076 &"</td><td>"& ZC_MSG014 &"</td><td>"& ZC_MSG060 &"</td><td>"& ZC_MSG055 &"</td><td></td><td width='5%'  align='center'><a href='' onclick='BatchSelectAll();return false'>"& ZC_MSG229 &"</a></td></tr>"


	objRS.Open("SELECT * FROM [blog_TrackBack] "& strSQL &" ORDER BY [tb_ID] DESC")
	objRS.PageSize=ZC_MANAGE_COUNT
	If objRS.PageCount>0 Then objRS.AbsolutePage = intPage

	If (Not objRS.bof) And (Not objRS.eof) Then

		For i=1 to objRS.PageSize

			Response.Write "<tr>"
			Response.Write "<td>" & -1-objRS("log_ID") & "</td>"
			Response.Write "<td><a title="""& objRS("tb_Title") &""" target=""_blank"" href="""&objRS("tb_Url")&""">" & Left(objRS("tb_Blog"),14) & "</a></td>"
			Response.Write "<td>" & Left(objRS("tb_Title"),12) & "</td>"
			Response.Write "<td><a href="""" onclick='javascript:$(this).parent().html(""" & TransferHTML(objRS("tb_Excerpt"),"[html-format][enter][""]") & """);return false;'>" & Left(objRS("tb_Excerpt"),18) & "</a></td>"
			Response.Write "<td align=""center""></td>"
			Response.Write "<td align=""center"" ><input type=""checkbox"" name=""edtDel"" id=""edtDel"" value="""&objRS("tb_ID")&"""/></td>"
			Response.Write "</tr>"

			objRS.MoveNext
			If objRS.eof Then Exit For

		Next

	End If

	Response.Write "</table>"

	For i=1 to objRS.PageCount
		strPage=strPage &"<a href='"&ZC_BLOG_HOST&"plugin/totoro/setting2.asp?page="& i &"'>["& Replace(ZC_MSG036,"%s",i) &"]</a> "
	Next
	Response.Write "<br/><form id=""frmBatch"" method=""post"" action=""""><p><input type=""hidden"" id=""edtBatch"" name=""edtBatch"" value=""""/><input class=""button"" type=""submit"" onclick='BatchDeleteAll(""edtBatch"");if(document.getElementById(""edtBatch"").value){this.form.action="""&ZC_BLOG_HOST&"plugin/totoro/trackbackdel.asp"&""";return window.confirm("""& ZC_MSG058 &""");}else{return false}' value=""删除所选择的引用"" id=""btnPost""/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<input class=""button"" type=""submit"" onclick='BatchDeleteAll(""edtBatch"");if(document.getElementById(""edtBatch"").value){this.form.action="""&ZC_BLOG_HOST&"plugin/totoro/trackbackpass.asp"&""";return window.confirm("""& ZC_MSG058 &""");}else{return false}' value=""通过所选择的引用"" id=""btnPost""/></p><form><br/>" & vbCrlf

	Response.Write "<hr/>" & ZC_MSG042 & ": " & strPage

	objRS.Close
	Set objRS=Nothing


%>

</div>
<script language="javascript">


	//斑马线
	var tables=document.getElementsByTagName("table");
	var b=false;
	for (var j = 0; j < tables.length; j++){

		var cells = tables[j].getElementsByTagName("tr");

		cells[0].className="color1";
		for (var i = 1; i < cells.length; i++){
			if(b){
				cells[i].className="color2";
				b=false;
			}
			else{
				cells[i].className="color3";
				b=true;
			};
		};
	}

function ChangeValue(obj){

	if (obj.value=="True")
	{
	obj.value="False";
	return true;
	}

	if (obj.value=="False")
	{
	obj.value="True";
	return true;
	}
}
</script>
</body>
</html>
<%
Call System_Terminate()

If Err.Number<>0 then
  Call ShowError(0)
End If
%>

