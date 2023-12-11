<%
Option Explicit
Response.Buffer = True
Response.Expires = -1
Response.AddHeader "Pragma","no-cache"
Response.AddHeader "cache-ctrol","no-cache"
Dim RndNum,ImgFileContent
Randomize Timer
RndNum = Cint(7999*Rnd+1000)
Session("cnbruce.com_ValidateCode") = Cstr(RndNum)
ImgFileContent=NumCode(RndNum)
Response.ContentType = "image/BMP"
Response.BinaryWrite ImgFileContent

Function NumCode(NumS)
    Dim NumI,NumJ
    Dim AdoM,AdoN
    Dim Arr_Img(4),NStr
        NStr=Cstr(NumS)
        For NumI=0 To 3
            Arr_Img(NumI)=Cint(Mid(NStr,NumI+1,1))
        Next
    Dim Position
    Set AdoM=Server.CreateObject("Adodb.Stream")
        AdoM.Mode=3
        AdoM.Type=1
        AdoM.Open
        Set AdoN=Server.CreateObject("Adodb.Stream")
        AdoN.Mode=3
        AdoN.Type=1
        AdoN.Open
        AdoM.LoadFromFile(Server.Mappath("validatebody.fix"))
        AdoN.Write AdoM.Read(1280)
        For NumI=0 To 3
            AdoM.Position=(9-Arr_Img(NumI))*320
            AdoN.Position=NumI*320
            AdoN.Write AdoM.Read(320)
        Next    
        AdoM.LoadFromFile(Server.Mappath("validatehead.fix"))
        Position=Lenb(AdoM.Read())
        AdoM.Position=Position
        For NumI=0 To 9 Step 1
            For NumJ=0 To 3
                AdoN.Position=NumI*32+NumJ*320
                AdoM.Position=Position+30*NumJ+NumI*120
                AdoM.Write AdoN.Read(30)
            Next
        Next
        AdoM.Position = 0
        NumCode = AdoM.Read()
        AdoM.Close:Set AdoM=Nothing
        AdoN.Close:Set AdoN=Nothing
End Function
%>