<%
dim rsSkin,sqlSkin,tSkinID,Skin_CSS,Body_Label
sqlSkin="select * from Skin where IsDefault=True"
set rsSkin=server.CreateObject("adodb.recordset")
rsSkin.open sqlSkin,conn,1,1
if rsSkin.bof and rsSkin.eof then
	rsSkin.close
	sqlSkin="select top 1 * from Skin"
	rsSkin.open sqlSkin,conn,1,1
	call main()	
else
	call main()
end if
rsSkin.close
set rsSkin=nothing

sub main()
	Body_Label=rsSkin("Body")
	Skin_CSS=split(rsSkin("Skin_CSS"),"|||")
%>
<style type="text/css">
<%=Skin_CSS(0)%>
BODY
{
<%=Skin_CSS(1)%>
}
TD
{
<%=Skin_CSS(2)%>
}
Input
{
<%=Skin_CSS(3)%>
}
Button
{
<%=Skin_CSS(4)%>
}
Select
{
<%=Skin_CSS(5)%>
}
.border
{
<%=Skin_CSS(6)%>
}
.border2
{
<%=Skin_CSS(7)%>
}
.FootBg
{
<%=Skin_CSS(8)%>
}
.title
{
<%=Skin_CSS(9)%>
}
.tdbg
{
<%=Skin_CSS(10)%>
}
.txt_css
{
<%=Skin_CSS(11)%>
}
.title_lefttxt
{
<%=Skin_CSS(12)%>
}
.title_left
{
<%=Skin_CSS(13)%>
}
.tdbg_left
{
<%=Skin_CSS(14)%>
}
.title_left2
{
<%=Skin_CSS(15)%>
}
.tdbg_left2
{
<%=Skin_CSS(16)%>
}
.tdbg_leftall
{
<%=Skin_CSS(17)%>
}
.title_righttxt
{
<%=Skin_CSS(18)%>
}
.title_right
{
<%=Skin_CSS(19)%>
}
.tdbg_right
{
<%=Skin_CSS(20)%>
}
.title_right2
{
<%=Skin_CSS(21)%>
}
.tdbg_right2
{
<%=Skin_CSS(22)%>
}
.tdbg_rightall
{
<%=Skin_CSS(23)%>
}
</style>
<%
end sub
%>