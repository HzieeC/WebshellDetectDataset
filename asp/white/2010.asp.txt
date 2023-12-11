<!--#include file="conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!-- #include file="inc/dv_clsother.asp" -->
<%
Dvbbs.LoadTemplates("help_permission")
Dim orders
If Request("Action")="Myinfo" Then
	Dvbbs.stats=template.Strings(4)
Else
	Dvbbs.stats=template.Strings(0)
End If
Dvbbs.nav()
If Dvbbs.BoardID=0 then
	Dvbbs.Head_var 2,0,"",""
Else
	Dvbbs.Head_var 1,Application(Dvbbs.CacheName&"_boardlist").documentElement.selectSingleNode("board[@boardid='"&Dvbbs.BoardID&"']/@depth").text,"",""
End If

If Not (Request("Action")="Myinfo" And Dvbbs.UserID=0) Then
	If Cint(Dvbbs.GroupSetting(39))=0 And Not Dvbbs.master Then Dvbbs.AddErrCode(55)
End If
Dvbbs.ShowErr

If Not IsNumeric(request("orders")) or request("orders")="" Then
	orders=1
Else
	orders=request("orders")
End If

permission()
Dvbbs.activeonline()
Dvbbs.footer()
Dvbbs.PageEnd()
Sub permission()
	Response.Write Replace(Replace(Replace(template.html(0),"{$boardid}",Dvbbs.BoardID),"{$alertcolor}",Dvbbs.mainsetting(1)),"{$action}",Request("Action"))
	Response.Write "<Script Language=JavaScript>"
	dim trs,ars,rs
	If Request("Action")="Myinfo" Then
		Dim myper_1,myper_2,myper_3
		Dim UserTitle,MyGroupSetting
		myper_1=false
		myper_2=false
		myper_3=false
		Set Rs=Dvbbs.Execute("Select Uc_userid,uc_Setting From Dv_UserAccess Where Uc_boardid="&Dvbbs.Boardid&" And Uc_userid="&Dvbbs.Userid)
		If Not(Rs.Eof And Rs.Bof) Then
			myper_1=true
		MyGroupSetting = Rs(1)
			UserTitle = template.Strings(1)
		End If
		If not myper_1 Then
			Set Rs=Dvbbs.Execute("Select Pid,PSetting From Dv_BoardPermission Where Boardid="&Dvbbs.boardid&" and GroupID="&Dvbbs.UserGroupID)
		If Not(Rs.Eof And Rs.Bof) Then
				myper_2=true
				MyGroupSetting = Rs(1)
				UserTitle = template.Strings(2)
			End If
		End If
		If not(myper_1 Or myper_2) Then
			Set Rs=Dvbbs.Execute("Select UserGroupID,GroupSetting,Usertitle From Dv_UserGroups Where UserGroupID="&Dvbbs.UserGroupID)
			If Not(Rs.Eof And Rs.Bof) Then
				myper_3=true
				MyGroupSetting = Rs(1)
				UserTitle = Rs(2) & template.Strings(3)
			End If
		End If
		Set Rs=Nothing
		
		Response.Write "groupname[0]='"
		Response.Write UserTitle
		Response.Write "';"
		Response.Write "GroupSetting[0]='"
		Response.Write MyGroupSetting
	    Response.Write "';"
	Else
		'Set trs=dvbbs.execute("select * from dv_usergroups Where IsSetting='1' order by usergroupid")
		Set trs=dvbbs.execute("select * from dv_usergroups order by usergroupid")
		Dim i
		i=0
		Do While Not trs.EOF
		Response.Write "groupname["
		Response.Write i
	    Response.Write "]='"
		Response.Write Trim(trs("usertitle"))   
        Response.Write "';"    
		Set ars=dvbbs.Execute("select * from dv_BoardPermission where BoardID="&Dvbbs.boardid&" and GroupID="&trs("UserGroupID"))
        If  Not ars.EOF  Then
	    	Response.Write "GroupSetting["
			Response.Write i
       		Response.Write "]='"
        	Response.Write ars("PSetting")
	    	Response.Write "';"
		Else
       		Response.Write "GroupSetting["
        	Response.Write i
	    	Response.Write "]='"
			Response.Write trs("GroupSetting")
        	Response.Write "';"
	    End If
		i=i+1
        trs.MoveNext  
		Loop
		set trs=Nothing 
		set ars=Nothing
	End If
	Response.Write "showtoptable("&orders&")"
	Response.Write "</script>"
End Sub  
%>