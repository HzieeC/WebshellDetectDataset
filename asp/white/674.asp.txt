<%@LANGUAGE="VBSCRIPT" CODEPAGE="936"%>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<!--#include file="Inc/Function.asp"-->
<!--#include file="../Inc/Config.asp"-->
<%
dim ID,Action,sqlDel,rsDel,FoundErr,ErrMsg,ObjInstalled
ID=trim(request("ID"))
Action=Trim(Request("Action"))
FoundErr=False
ObjInstalled=IsObjInstalled("Scripting.FileSystemObject")

if ID="" or Action<>"Del" then
	FoundErr=True
	ErrMsg=ErrMsg & "<br><li>²ÎÊý²»×ã£¡</li>"
end if
if FoundErr=False then
	if instr(ID,",")>0 then
		dim idarr,i
		idArr=split(ID)
		for i = 0 to ubound(idArr)
			call DelDown(clng(idarr(i)))
		next
	else
		call DelDown(clng(ID))
	end if
end if
if FoundErr=False then
	call CloseConn()
	response.Redirect "Down_Manage.asp"
else
	call CloseConn()
	call WriteErrMsg()
end if

sub DelDown(ID)
	PurviewChecked=False
	sqlDel="select * from Download where ID=" & CLng(ID)
	Set rsDel= Server.CreateObject("ADODB.Recordset")
	rsDel.open sqlDel,conn,1,3
	if FoundErr=False then
		if DelUpFiles="Yes" and ObjInstalled=True then
			dim fso,strUploadFiles,arrUploadFiles
			strUploadFiles=rsDel("PhotoUrl") & ""
			if strUploadFiles<>"" then
				Set fso = CreateObject("Scripting.FileSystemObject")
				if instr(strUploadFiles,"|")>1 then
					arrUploadFiles=split(strUploadFiles,"|")
					for i=0 to ubound(arrUploadFiles)
						if fso.FileExists(server.MapPath("../" & arrUploadfiles(i))) then
							fso.DeleteFile(server.MapPath("../" & arrUploadfiles(i)))
						end if
					next
				else
					if fso.FileExists(server.MapPath("../" & strUploadfiles)) then
						fso.DeleteFile(server.MapPath("../" & strUploadfiles))
					end if
				end if
				Set fso = nothing
			end if
		end if
		rsDel.delete
		rsDel.update
		set rsDel=nothing		
	end if
end sub
%>