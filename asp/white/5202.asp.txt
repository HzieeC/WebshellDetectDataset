<!--#include file="AspCms_MainClass.asp" -->
<%	
dim act : act=getForm("act","get")
select case act
	case "t"
		echo "document.write("&getTodayVisits&")"
	case "y"	
		echo "document.write("&getYesterdayVisits&")"
	case "m"
		echo "document.write("&getMonthVisits&")"
	case "a"
		echo "document.write("&getAllVisits&")"
end select
%>
