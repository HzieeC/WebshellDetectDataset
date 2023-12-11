<%
Option Explicit
Response.buffer=true
NumCode
Function NumCode()
	Response.Expires = -1
	Response.AddHeader "Pragma","no-cache"
	Response.AddHeader "cache-ctrol","no-cache"
	On Error Resume Next
	Dim zNum,i,j
	Dim Ados,Ados1
	Randomize timer
	zNum = cint(8999*Rnd+1000)
	Session("CheckCode") = zNum
	Dim zimg(4),NStr
	NStr=cstr(zNum)
	For i=0 To 3
		zimg(i)=cint(mid(NStr,i+1,1))
	Next
	Dim Pos
	Set Ados=Server.CreateObject("Adodb.Stream")
	Ados.Mode=3
	Ados.Type=1
	Ados.Open
	Set Ados1=Server.CreateObject("Adodb.Stream")
	Ados1.Mode=3
	Ados1.Type=1
	Ados1.Open
	Ados.LoadFromFile(Server.mappath("../image/body.Fix"))
	Ados1.write Ados.read(1280)
	For i=0 To 3
		Ados.Position=(9-zimg(i))*320
		Ados1.Position=i*320
		Ados1.write ados.read(320)
	Next	
	Ados.LoadFromFile(Server.mappath("../image/head.fix"))
	Pos=lenb(Ados.read())
	Ados.Position=Pos
	For i=0 To 9 Step 1
		For j=0 To 3
			Ados1.Position=i*32+j*320
			Ados.Position=Pos+30*j+i*120
			Ados.write ados1.read(30)
		Next
	Next
	Response.ContentType = "image/BMP"
	Ados.Position=0
	Response.BinaryWrite Ados.read()
	Ados.Close:set Ados=nothing
	Ados1.Close:set Ados1=nothing
	If Err Then Session("CheckCode") = 9999
End Function
%>