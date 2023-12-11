<%@ CODEPAGE=65001 %>
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
<!-- #include file="../../plugin/p_config.asp" -->
<%

Call System_Initialize()

'检查非法链接
Call CheckReference("")

'检查权限
If BlogUser.Level>1 Then Call ShowError(6) 

If CheckPluginState("Totoro")=False Then Call ShowError(48)

	Dim i,j
	Dim s,t
	Dim aryArticle()
	s=Request.Form("edtBatch")
	t=Split(s,",")

	ReDim Preserve aryArticle(UBound(t))
	For j=0 To UBound(t)-1
		aryArticle(j)=0
	Next

	Dim objComment
	Dim objArticle

	For i=0 To UBound(t)-1
		Set objComment=New TComment
		If objComment.LoadInfobyID(t(i)) Then
			objComment.log_ID=-1-objComment.log_ID
			If objComment.log_ID>0 Then
				Dim objTestArticle
				Set objTestArticle=New TArticle
				If objTestArticle.LoadInfobyID(objComment.log_ID) Then

					For j=0 To UBound(t)-1
						If aryArticle(j)=0 Then
							aryArticle(j)=objComment.log_ID
						End If
						If aryArticle(j)=objComment.log_ID Then Exit For
					Next

					If Not((objComment.AuthorID=BlogUser.ID) Or (objTestArticle.AuthorID=BlogUser.ID) Or (CheckRights("Root")=True)) Then Response.End
				Else
					Call ShowError(9)
				End If
				Set objTestArticle=Nothing
			Else
				If Not((objComment.log_ID=0) And (CheckRights("GuestBookMng")=True)) Then Response.End
			End If

			objComment.Content=TransferHTML(objComment.Content,"[anti-html-format]")
			objComment.HomePage=TransferHTML(objComment.HomePage,"[anti-html-format]")

			objComment.Post
		End If
		Set objComment=Nothing
	Next


	For j=0 To UBound(t)-1
		If aryArticle(j)>0 Then
			Call BuildArticle(aryArticle(j),False,False)
		End If
	Next

	BlogReBuild_Comments
	BlogReBuild_GuestComments

	Response.Redirect "setting1.asp"

%>
<%
Call System_Terminate()

If Err.Number<>0 then
  Call ShowError(0)
End If
%>