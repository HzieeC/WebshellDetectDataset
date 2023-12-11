<!--#include file="Conn.asp"-->
<!-- #include file="inc/const.asp" -->
<!--#include file="inc/md5.asp"-->
<%
Response.Clear
dim rs

If Not Dvbbs.Forum_ChanSetting(0)=1 Then
	Response.Write "本论坛没有开启手机短信互动功能。"
	Response.End
End If

'随时可通过此地址获得论坛当前的挑战随机数
'挑战随机数
Dim MaxUserID,MaxLength
MaxLength=12
set rs=Dvbbs.Execute("select Max(userid) from [dv_user]")
MaxUserID=rs(0)

Dim num1,rndnum
Randomize
Do While Len(rndnum)<4
	num1=CStr(Chr((57-48)*rnd+48))
	rndnum=rndnum&num1
loop
MaxUserID=rndnum & MaxUserID
MaxLength=MaxLength-len(MaxUserID)
select case MaxLength
case 7
	MaxUserID="0000000" & MaxUserID
case 6
	MaxUserID="000000" & MaxUserID
case 5
	MaxUserID="00000" & MaxUserID
case 4
	MaxUserID="0000" & MaxUserID
case 3
	MaxUserID="000" & MaxUserID
case 2
	MaxUserID="00" & MaxUserID
case 1
	MaxUserID="0" & MaxUserID
case 0
	MaxUserID=MaxUserID
end select

Response.Write MaxUserID
set rs=nothing
%>