<%@ CODEPAGE=65001 %>
<!--#include file="../inc/conn.asp"  -->
<!--#include file="../inc/web_config.asp"  -->
<%
''时间转换函数
Function return_RFC822_Date(byVal myDate, byVal TimeZone)   
Dim myDay, myDays, myMonth, myYear   
Dim myHours, myMinutes, mySeconds   
myDate = CDate(myDate)   
myDay = EnWeekDayName(myDate)   
myDays = Right("00" & Day(myDate),2)   
myMonth = EnMonthName(myDate)   
myYear = Year(myDate)   
myHours = Right("00" & Hour(myDate),2)   
myMinutes = Right("00" & Minute(myDate),2)   
mySeconds = Right("00" & Second(myDate),2)     
return_RFC822_Date = myDay&", "& _   
myDays&" "& _   
myMonth&" "& _    
myYear&" "& _   
myHours&":"& _   
myMinutes&":"& _   
mySeconds&" "& _    
" " & TimeZone   
End Function

''星期转换函数
Function EnWeekDayName(InputDate)   
Dim Result   
Select Case WeekDay(InputDate,1)   
Case 1:Result="Sun"   
Case 2:Result="Mon"   
Case 3:Result="Tue"   
Case 4:Result="Wed"   
Case 5:Result="Thu"   
Case 6:Result="Fri"   
Case 7:Result="Sat"   
End Select   
EnWeekDayName = Result   
End Function

''月份转换函数
Function EnMonthName(InputDate)   
Dim Result   
Select Case Month(InputDate)   
Case 1:Result="Jan"   
Case 2:Result="Feb"   
Case 3:Result="Mar"   
Case 4:Result="Apr"   
Case 5:Result="May"   
Case 6:Result="Jun"   
Case 7:Result="Jul"   
Case 8:Result="Aug"   
Case 9:Result="Sep"   
Case 10:Result="Oct"   
Case 11:Result="Nov"   
Case 12:Result="Dec"   
End Select   
EnMonthName = Result   
End Function   
%>
<%
''获取当前域名,其中rss.asp为你的文件名
sql="select top 50 * from [article] where view_yes=1 order by [time] desc"
''根据自己实际修改
set rs=server.createobject("adodb.recordset")   
rs.open sql,cn,1,1   
 response.contenttype="text/xml"   
 XMLContent=XMLContent&"<?xml version=""1.0"" encoding=""utf-8"" ?>" & vbcrlf   
 XMLContent=XMLContent&"<?xml-stylesheet type=""text/xsl"" href=""/rss/rss.xslt"" version=""1.0"" ?>" & vbcrlf
 XMLContent=XMLContent&"<rss version=""2.0"">" & vbcrlf   
 XMLContent=XMLContent&"<channel>" & vbcrlf   
 XMLContent=XMLContent&"<title>"& web_name & " 的最新动态</title>" & vbcrlf   
 XMLContent=XMLContent&"<link>" & web_url & "</link>" & vbcrlf   
 XMLContent=XMLContent&"<language>zh-cn</language>" & vbcrlf   
 XMLContent=XMLContent&"<copyright>RSS Feed By www.hitux.com</copyright>" & vbcrlf    
while not rs.eof
set rsc=server.createobject("adodb.recordset")
sqlc="select [name] from [category] where id="&rs("cid")
rsc.open sqlc,cn,1,1
if not rsc.eof then
CategoryName=rsc("name")
end if 
rsc.close
set rsc=nothing 
select case rs("ArticleType")
case 1
Content_FolderName=Article_FolderName
case 2
Content_FolderName=Product_FolderName
end select  
 XMLContent=XMLContent&"<item>" & vbcrlf   
 XMLContent=XMLContent&"<title><![CDATA[" & rs("title") & "]]></title>" & vbcrlf   
 XMLContent=XMLContent&"<link>"&web_url&Article_FolderName&"/"& rs("file_path") &"</link>" & vbcrlf    
 XMLContent=XMLContent&"<description><![CDATA[" & rs("description") & "]]></description>" & vbcrlf   
 XMLContent=XMLContent&"<pubDate>" & return_RFC822_Date(rs("time"),"08:00") & "</pubDate>" & vbcrlf   
 XMLContent=XMLContent&"<author>"&web_name&"</author>" & vbcrlf
 XMLContent=XMLContent&"<category>"&CategoryName&"</category>" & vbcrlf
 XMLContent=XMLContent&"</item>" & vbcrlf   
rs.movenext   
wend   
 XMLContent=XMLContent&"</channel>" & vbcrlf   
 XMLContent=XMLContent&"</rss>" & vbcrlf   
rs.close   
set rs=nothing   

''关闭数据库
cn.close
set cn=nothing

%>
<% '读取模板
'******************************************
'功能：生成UTF-8文件
'参数：File_name 文件名
'参数：File_path 生成文件所在相对目录
'参数：content   写入内容
'******************************************
sub Generation_File(File_name,File_path,content)
mappath = "/"&File_path&"/"&File_name
Set objStream = Server.CreateObject("ADODB.Stream")
With objStream
.Open
.Charset = "utf-8"
.Position = objStream.Size
.WriteText=XMLContent
.SaveToFile server.mappath(mappath),2
.Close
End With
Set objStream = Nothing
end sub

call Generation_File("Feed.xml","rss",XMLContent) '在xml目录下生成编码为utf-8的Feex.xml文件

response.write XMLContent
%>

