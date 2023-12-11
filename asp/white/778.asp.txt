<!--#include file="Inc/conn.asp"-->
<!--#include file="inc/ubbcode.asp"-->
<%
dim ID
dim sqlAnnounce
dim rsAnnounce
dim AnnounceNum
dim ChannelID
ID=Trim(request("ID"))
sqlAnnounce="select * from affiche where ID=" & Clng(ID)
sqlAnnounce=sqlAnnounce & " order by ID Desc"
Set rsAnnounce= Server.CreateObject("ADODB.Recordset")
rsAnnounce.open sqlAnnounce,conn,1,1
%>
<html>
<head>
<title>本站公告</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<style type="text/css">
a:active { text-decoration: none; color: #0000FF}
a:hover { text-decoration: none; color: #FF0000}
a:link { text-decoration: none; color: #0000FF}
a:visited { text-decoration: none; color: #990000}
BODY { text-decoration: none; font-size: 12px}
TABLE { text-decoration: none; font-size: 12px}
</style>
</head>
<BODY text=#000000 bgColor=#ffffff leftMargin=0 topMargin=0>
<TABLE cellSpacing=0 cellPadding=0 width=400 border=0>
  <TBODY>
    <TR background="Images/Annouce_r7_c1.gif"> 
      <TD colSpan=4 background="Images/Annouce_r7_c1.gif"> <div align="right"><img src="Images/Annouce_r1_c1.gif" width="90" height="25">&nbsp;&nbsp;</div></TD>
    </TR>
    <TR> 
      <TD colSpan=4 background="Images/Annouce_r2_c1.gif"><IMG height=43 alt="" 
      width=2 border=0 name=Annouce_r2_c1></TD>
    </TR>
    <TR> 
      <TD vAlign=top width=16 background=Images/Annouce_r4_c1.gif>&nbsp;</TD>
      <TD width=89 vAlign=top><img src="Images/Annouce_r3_c1.gif" width="87" height="112"></TD>
      <TD vAlign=top width=279 background=file:///G|/temp/%B1%BE%D5%BE%CD%A8%B8%E6.files/Annouce_r3_c2.gif 
    rowSpan=2> <TABLE cellSpacing=5 cellPadding=0 width="100%" border=0>
          <TBODY>
            <TR> 
              <TD height="68"><%
if rsAnnounce.bof and rsAnnounce.eof then 
	response.write "<p>&nbsp;&nbsp;没有通告或找不到指定的通告</p>" 
else 
	AnnounceNum=rsAnnounce.recordcount
	dim i
	do while not rsAnnounce.eof
		response.Write "<p align='center'>" & rsAnnounce("title") & "</p><p align='left'>" & ubbcode(dvHTMLEncode(rsAnnounce("Content"))) & "</p><p align='right'>" & "&nbsp;&nbsp;<br>" & FormatDateTime(rsAnnounce("Time"),1) & "</p>"
		rsAnnounce.movenext
		i=i+1
		if i<AnnounceNum then response.write "<hr>"
	loop	
end if  
%></TD>
            </TR>
          </TBODY>
        </TABLE></TD>
      <TD vAlign=top width=16 bgColor=#fedfbe><IMG height=112 alt="" 
      src="Images/Annouce_r3_c3.gif" width=16 border=0 
  name=Annouce_r3_c3></TD>
    </TR>
    <TR> 
      <TD colspan="2" background=Images/Annouce_r4_c1.gif>&nbsp;</TD>
      <TD bgColor=#fedfbe>&nbsp;</TD>
    </TR>
    <TR> 
      <TD colSpan=4><IMG height=24 alt="" src="Images/Annouce_r5_c1.gif" 
      width=400 border=0 name=Annouce_r5_c1></TD>
    </TR>
    <TR> 
      <TD colSpan=4><IMG src="Images/Annouce_r6_c1.gif" alt="" 
  name=Annouce_r6_c1 
      width=400 height=39 border=0 useMap=#Annouce_r6_c1Map></TD>
    </TR>
  </TBODY>
</TABLE>
<%
rsAnnounce.close
set rsAnnounce=nothing
call CloseConn()
%>
<map name="Annouce_r6_c1Map">
  <area shape="rect" coords="170,13,230,27" href="javascript:window.close();">
</map>
