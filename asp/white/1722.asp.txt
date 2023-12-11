<%
'chk session
If Session("log_name")="" Then 
	Session.abandon
%>
<script language=JavaScript>
	top.location = "login.asp";
</script>
<%
End If 
%>