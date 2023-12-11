<%@ CODEPAGE=65001 %>
<%
'///////////////////////////////////////////////////////////////////////////////
'//              Z-Blog 彩虹网志个人版
'// 作    者:    朱煊(zx.asd)
'// 版权所有:    RainbowSoft Studio
'// 技术支持:    rainbowsoft@163.com
'// 程序名称:    
'// 程序版本:    
'// 单元名称:    edit_tag.asp
'// 开始时间:    2005.04.07
'// 最后修改:    
'// 备    注:    
'///////////////////////////////////////////////////////////////////////////////
%>
<% Option Explicit %>
<% On Error Resume Next %>
<% Response.Charset="UTF-8" %>
<% Response.Buffer=True %>
<!-- #include file="../c_option.asp" -->
<!-- #include file="../function/c_function.asp" -->
<!-- #include file="../function/c_system_lib.asp" -->
<!-- #include file="../function/c_system_base.asp" -->
<!-- #include file="../function/c_system_plugin.asp" -->
<!-- #include file="../plugin/p_config.asp" -->
<%

Call System_Initialize()

'plugin node
For Each sAction_Plugin_Edit_Tag_Begin in Action_Plugin_Edit_Tag_Begin
	If Not IsEmpty(sAction_Plugin_Edit_Tag_Begin) Then Call Execute(sAction_Plugin_Edit_Tag_Begin)
Next


'检查非法链接
Call CheckReference("")

'检查权限
If Not CheckRights("TagEdt") Then Call ShowError(6)

Dim EditTag
Set EditTag=New TTag

If Not IsEmpty(Request.QueryString("id")) Then

	If EditTag.LoadInfoByID(Request.QueryString("id"))=False Then Call ShowError(35)

End If


BlogTitle=ZC_BLOG_TITLE & ZC_MSG044 & ZC_MSG066

%><!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="<%=ZC_BLOG_LANGUAGE%>" lang="<%=ZC_BLOG_LANGUAGE%>">
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta http-equiv="Content-Language" content="<%=ZC_BLOG_LANGUAGE%>" />
	<link rel="stylesheet" rev="stylesheet" href="../CSS/admin.css" type="text/css" media="screen" />
	<script language="JavaScript" src="../script/common.js" type="text/javascript"></script>
	<link rel="stylesheet" href="../CSS/jquery.bettertip.css" type="text/css" media="screen">
	<script language="JavaScript" src="../script/jquery.bettertip.pack.js" type="text/javascript"></script>
	<title><%=BlogTitle%></title>
</head>
<body>

			<div id="divMain">
<div class="Header"><%=ZC_MSG241%></div>
<%
	Response.Write "<div class=""SubMenu"">" & Response_Plugin_TagEdt_SubMenu & "</div>"
%>
<div id="divMain2">
<% Call GetBlogHint() %>
<form id="edit" name="edit" method="post">
<%
	Response.Write "<input id=""edtID"" name=""edtID""  type=""hidden"" value="""& EditTag.ID &""" />"
	Response.Write "<p>"& ZC_MSG001 &":</p><p><input id=""edtName"" size=""40"" name=""edtName""  type=""text"" value="""& TransferHTML(EditTag.Name,"[html-format]") &""" />(*)</p><p></p>"
	'Response.Write "<p>"& ZC_MSG079 &":</p><p><input id=""edtOrder"" size=""40"" name=""edtOrder""  type=""text"" value="""& EditTag.Order &""" /></p><p></p>"
	Response.Write "<p>"& ZC_MSG016 &":</p><p><input id=""edtIntro"" size=""80"" name=""edtIntro""  type=""text"" value="""& TransferHTML(EditTag.Intro,"[html-format]") &""" /></p><p></p>"
	Response.Write "<p><input type=""submit"" class=""button"" value="""& ZC_MSG087 &""" id=""btnPost"" onclick='return checkTagInfo();' /></p><p></p>"
%>
</form>
</div>

			</div>

</body>
<script>

	var str17="<%=ZC_MSG118%>";

	function checkTagInfo(){
		document.getElementById("edit").action="../cmd.asp?act=TagPst";

		if(!document.getElementById("edtName").value){
			alert(str17);
			return false
		}

	}
</script>
</html>
<% 
Call System_Terminate()

If Err.Number<>0 then
	Call ShowError(0)
End If
%>