<!--#include file="Inc/conn.asp"-->
<%
action=CheckStr(Trim(request("action")))
id=LaoYRequest(request("id"))
typee=CheckStr(Trim(request("typee")))


if action="show" then
	  set rs=server.createobject("adodb.recordset")
	  sql="Select * from "&tbname&"_XinQing Where ArticleID="&id&""
      rs.open sql,conn,1,1
	  If Not(rs.Eof And rs.Bof) Then
      mood1=rs("mood1")
      mood2=rs("mood2")
      mood3=rs("mood3")
      mood4=rs("mood4")
      mood5=rs("mood5")
      mood6=rs("mood6")
      mood7=rs("mood7")
      mood8=rs("mood8")
	  Rs.Close
	  Set Rs = Nothing
      Response.Write(""&mood1&","&mood2&","&mood3&","&mood4&","&mood5&","&mood6&","&mood7&","&mood8&"")
      else
	  conn.execute("Insert Into "&tbname&"_XinQing(ArticleID)values('"&id&"')")
      Response.Write("0,0,0,0,0,0,0,0")
	  end if
else
if action="mood" then
Select case typee
case "mood1"
conn.execute("Update "&tbname&"_XinQing set mood1=mood1+1 where ArticleID="&id&"")
case "mood2"
conn.execute("Update "&tbname&"_XinQing set mood2=mood2+1 where ArticleID="&id&"")
case "mood3"
conn.execute("Update "&tbname&"_XinQing set mood3=mood3+1 where ArticleID="&id&"")
case "mood4"
conn.execute("Update "&tbname&"_XinQing set mood4=mood4+1 where ArticleID="&id&"")
case "mood5"
conn.execute("Update "&tbname&"_XinQing set mood5=mood5+1 where ArticleID="&id&"")
case "mood6"
conn.execute("Update "&tbname&"_XinQing set mood6=mood6+1 where ArticleID="&id&"")
case "mood7"
conn.execute("Update "&tbname&"_XinQing set mood7=mood7+1 where ArticleID="&id&"")
case else
conn.execute("Update "&tbname&"_XinQing set mood8=mood8+1 where ArticleID="&id&"")
End Select
	  set rs=server.createobject("adodb.recordset")
	  sql="Select * from "&tbname&"_XinQing Where ArticleID="&id&""
      rs.open sql,conn,1,1
      mood1=rs("mood1")
      mood2=rs("mood2")
      mood3=rs("mood3")
      mood4=rs("mood4")
      mood5=rs("mood5")
      mood6=rs("mood6")
      mood7=rs("mood7")
      mood8=rs("mood8")
	  Rs.Close
	  Set Rs = Nothing
      Response.Write(""&mood1&","&mood2&","&mood3&","&mood4&","&mood5&","&mood6&","&mood7&","&mood8&"")
else
      Response.Write("0,0,0,0,0,0,0,0")
end if
end if
 %>