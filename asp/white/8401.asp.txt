<%
Option Explicit

Const CodeType = 8 '注：1,4,7,10,13,16为黑白型 2,5,8,11,14,17为彩色背景型 3,6,9,12,15,18为噪点型
Const listcode = "0123456789abcdefghijklmnopqrstuvwxyz"

Response.buffer = True
Response.Expires = -1
Response.AddHeader "Pragma", "no-cache"
Response.AddHeader "cache-ctrol", "no-cache"

Dim zNum, rNum, i, j, listnum
Dim Ados, Ados1

'得到验证码的字符串
Dim zimg(6), NStr
Randomize Timer
For i = 0 To 5
    rNum = Fix(35 * Rnd) '将35改为9即为使用纯数字密码
    zimg(i) = rNum
    listnum = listnum & Mid(listcode, rNum + 1, 1)
Next
Session("CheckCode") = listnum
'*********************

Dim Pos
Set Ados = Server.CreateObject("Adodb.Stream")
Ados.Mode = 3
Ados.Type = 1
Ados.Open
Set Ados1 = Server.CreateObject("Adodb.Stream")
Ados1.Mode = 3
Ados1.Type = 1
Ados1.Open

'得到验证码图像实体部分
Ados.LoadFromFile Server.mappath("body" & CodeType & ".Fix")
Ados1.write Ados.Read(2880)
For i = 0 To 5
    Ados.Position = (35 - zimg(i)) * 480
    Ados1.Position = i * 480
    Ados1.write Ados.Read(480)
Next

'得到图像头部信息
Ados.LoadFromFile Server.mappath("head.fix")
Pos = LenB(Ados.Read())
Ados.Position = Pos

'将头部信息与实体部分合并成横向排列
On Error Resume Next
For i = 0 To 15
    For j = 0 To 5
        Ados1.Position = i * 32 + j * 480
        Ados.Position = Pos + 30 * j + i * 270
        Ados.write Ados1.Read(30)
    Next
Next

'输出图像
Ados.Position = 0
Response.ContentType = "image/BMP"
Response.BinaryWrite Ados.Read()

Ados.Close
Set Ados = Nothing
Ados1.Close
Set Ados1 = Nothing
%>