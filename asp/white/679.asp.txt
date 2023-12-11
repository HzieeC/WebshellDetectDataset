<%@LANGUAGE="VBSCRIPT" CODEPAGE="936"%>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<!--#include file="../Inc/Config.asp"-->
<!--#include file="Inc/Function.asp"-->
<%
dim ID,Action,sqlDel,rsDel,FoundErr,ErrMsg,PurviewChecked,ObjInstalled
ID=trim(request("ID"))
Action=Trim(Request("Action"))
FoundErr=False
ObjInstalled=IsObjInstalled("Scripting.FileSystemObject")

if ID="" or Action="" then
	FoundErr=True
	ErrMsg=ErrMsg & "<br><li>参数不足！</li>"
end if
if FoundErr=False then
	if instr(ID,",")>0 then
		dim idarr,i
		idArr=split(ID)
		for i = 0 to ubound(idArr)
			call CheckArticle(clng(idarr(i)),Action)
		next
	else
		call CheckArticle(clng(ID),Action)
	end if
end if
if FoundErr=False then
	call CloseConn()
	response.Redirect "ProductCheck.asp"
else
	call CloseConn()
	call WriteErrMsg()
end if

sub CheckArticle(ID,CheckAction)
	PurviewChecked=False
	sqlDel="select * from Product where ID=" & CLng(ID)
	Set rsDel= Server.CreateObject("ADODB.Recordset")
	rsDel.open sqlDel,conn,1,3
	if rsDel.bof and rsDel.eof then
		FoundErr=True
		ErrMsg=ErrMsg & "<br><li>找不到文章：" & rsDel("ID") & " </li>"
	end if
		if CheckAction="Check" then
			rsDel("Passed")=True
		elseif CheckAction="CancelCheck" then
			rsDel("Passed")=False
		end if
		rsDel.update
		set rsDel=nothing
end sub
%>