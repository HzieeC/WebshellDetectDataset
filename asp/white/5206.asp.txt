<!--#include file="AspCms_MainClass.asp" -->
<%	
if conn.Exec("select count(*) from {prefix}Visits where year(AddTime)="&Year(date)&" and month(AddTime)="&month(date)&" and day(AddTime)="&day(date),"r1")(0)>0 then
	conn.Exec"update {prefix}Visits set Visits=Visits+1 where year(AddTime)="&Year(date)&" and month(AddTime)="&month(date)&" and day(AddTime)="&day(date),"exe"
else		
		conn.Exec"insert into {prefix}Visits(Visits,AddTime) values(1,'"&date&"')","exe"
end if 
%>
