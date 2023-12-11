<%
Function ThunderEncode(url)
	//将Unicode编码的字符串进行Base64编码
	Dim thunderPrefix, thunderPosix, thunderTitle, thunderUrl

	thunderPrefix = "AA"
	thunderPosix = "ZZ"
	thunderTitle = "thunder://"

	thunderUrl = thunderTitle & strAnsi2Unicode(Base64encode(strUnicode2Ansi(thunderPrefix & url & thunderPosix)))

	ThunderEncode = thunderUrl 
End Function 

Function strUnicodeLen(asContents)
//计算unicode字符串的Ansi编码的长度
	Dim asContents1, len1, k, i
	asContents1="a"&asContents
	len1=len(asContents1)
	k=0
	for i=1 to len1
		Dim asc1
		asc1=asc(mid(asContents1,i,1))
		if asc1<0 then asc1=65536+asc1
		if asc1>255 then
			k=k+2
		else
			k=k+1
		end if
	next
	strUnicodeLen=k-1
End Function

Function strUnicode2Ansi(asContents)
//将Unicode编码的字符串，转换成Ansi编码的字符串
	Dim len1, i
	strUnicode2Ansi=""
	len1=len(asContents)
	for i=1 to len1
		Dim varchar, varasc, varHex, varlow, varhigh
		varchar=mid(asContents,i,1)
		varasc=asc(varchar)
		if varasc<0 then varasc=varasc+65536
		if varasc>255 then
			varHex=Hex(varasc)
			varlow=left(varHex,2)
			varhigh=right(varHex,2)
			strUnicode2Ansi=strUnicode2Ansi & chrb("&H" & varlow ) & chrb("&H" & varhigh )
		else
			strUnicode2Ansi=strUnicode2Ansi & chrb(varasc)
		end if
	next
End function

Function strAnsi2Unicode(asContents)
//将Ansi编码的字符串，转换成Unicode编码的字符串
	Dim len1, i
	strAnsi2Unicode = ""
	len1=lenb(asContents)
	if len1=0 then exit function
	for i=1 to len1
		dim varchar, varasc
		varchar=midb(asContents,i,1)
		varasc=ascb(varchar)
		if varasc > 127 then 
			strAnsi2Unicode = strAnsi2Unicode & chr(ascw(midb(asContents,i+1,1) & varchar))
			i=i+1
		else
			strAnsi2Unicode = strAnsi2Unicode & chr(varasc)
		end if
	next
End function

Function Base64encode(asContents)
//将Ansi编码的字符串进行Base64编码
//asContents应当是ANSI编码的字符串（二进制的字符串也可以）
	Dim sBASE_64_CHARACTERS
	sBASE_64_CHARACTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/" 
	sBASE_64_CHARACTERS = strUnicode2Ansi(sBASE_64_CHARACTERS)
	Dim lnPosition 
	Dim lsResult 
	Dim Char1 
	Dim Char2 
	Dim Char3 
	Dim Char4 
	Dim Byte1 
	Dim Byte2 
	Dim Byte3 
	Dim SaveBits1 
	Dim SaveBits2 
	Dim lsGroupBinary 
	Dim lsGroup64 
	Dim m3,m4,len1,len2

	len1=Lenb(asContents)
	if len1<1 then 
		Base64encode=""
		exit Function
	end if

	m3=Len1 Mod 3 
	If m3 > 0 Then 
	//补足位数是为了便于计算
		asContents = asContents & String(3-m3, chrb(0)) 
		len1=len1+(3-m3)
		len2=len1-3
	else
		len2=len1
	end if

	lsResult = ""

	For lnPosition = 1 To len2 Step 3 
		lsGroup64 = "" 
		lsGroupBinary = Midb(asContents, lnPosition, 3) 
		Byte1 = Ascb(Midb(lsGroupBinary, 1, 1)): SaveBits1 = Byte1 And 3 
		Byte2 = Ascb(Midb(lsGroupBinary, 2, 1)): SaveBits2 = Byte2 And 15 
		Byte3 = Ascb(Midb(lsGroupBinary, 3, 1)) 
		Char1 = Midb(sBASE_64_CHARACTERS, ((Byte1 And 252)/4) + 1, 1)
		Char2 = Midb(sBASE_64_CHARACTERS, (((Byte2 And 240)/16) Or (SaveBits1 * 16) And &HFF) + 1, 1) 
		Char3 = Midb(sBASE_64_CHARACTERS, (((Byte3 And 192)/64) Or (SaveBits2 * 4) And &HFF) + 1, 1) 
		Char4 = Midb(sBASE_64_CHARACTERS, (Byte3 And 63) + 1, 1) 
		lsGroup64 = Char1 & Char2 & Char3 & Char4
		lsResult = lsResult & lsGroup64
	Next 
	
	//处理最后剩余的几个字符
	if M3 > 0 then
		lsGroup64 = "" 
		lsGroupBinary = Midb(asContents, len2+1, 3) 
		Byte1 = Ascb(Midb(lsGroupBinary, 1, 1)): SaveBits1 = Byte1 And 3 
		Byte2 = Ascb(Midb(lsGroupBinary, 2, 1)): SaveBits2 = Byte2 And 15 
		Byte3 = Ascb(Midb(lsGroupBinary, 3, 1)) 
		Char1 = Midb(sBASE_64_CHARACTERS, ((Byte1 And 252)/4) + 1, 1) 
		Char2 = Midb(sBASE_64_CHARACTERS, (((Byte2 And 240)/16) Or (SaveBits1 * 16) And &HFF) + 1, 1)
		Char3 = Midb(sBASE_64_CHARACTERS, (((Byte3 And 192)/64) Or (SaveBits2 * 4) And &HFF) + 1, 1) 
		if M3=1 then
			lsGroup64 = Char1 & Char2 & ChrB(61) & ChrB(61) //用=号补足位数
		else
			lsGroup64 = Char1 & Char2 & Char3 & ChrB(61) //用=号补足位数
		end if
		lsResult = lsResult & lsGroup64 
	end if

	Base64encode = lsResult 
End Function 

Function Base64decode(asContents) 
//将Base64编码字符串转换成Ansi编码的字符串
//asContents应当也是ANSI编码的字符串（二进制的字符串也可以）
Dim sBASE_64_CHARACTERS 
sBASE_64_CHARACTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/" 
sBASE_64_CHARACTERS = strUnicode2Ansi(sBASE_64_CHARACTERS)
	Dim lsResult 
	Dim lnPosition 
	Dim lsGroup64, lsGroupBinary 
	Dim Char1, Char2, Char3, Char4 
	Dim Byte1, Byte2, Byte3 
	Dim M4,len1,len2

	len1= Lenb(asContents) 
	M4 = len1 Mod 4

	if len1 < 1 or M4 > 0 then
	//字符串长度应当是4的倍数
		Base64decode = "" 
		exit Function 
	end if

	//判断最后一位是不是 = 号
	//判断倒数第二位是不是 = 号
	//这里m4表示最后剩余的需要单独处理的字符个数
	if midb(asContents, len1, 1) = chrb(61) then m4=3 
	if midb(asContents, len1-1, 1) = chrb(61) then m4=2

	if m4 = 0 then
		len2=len1
	else
		len2=len1-4
	end if

	For lnPosition = 1 To Len2 Step 4 
		lsGroupBinary = "" 
		lsGroup64 = Midb(asContents, lnPosition, 4) 
		Char1 = InStrb(sBASE_64_CHARACTERS, Midb(lsGroup64, 1, 1)) - 1 
		Char2 = InStrb(sBASE_64_CHARACTERS, Midb(lsGroup64, 2, 1)) - 1 
		Char3 = InStrb(sBASE_64_CHARACTERS, Midb(lsGroup64, 3, 1)) - 1 
		Char4 = InStrb(sBASE_64_CHARACTERS, Midb(lsGroup64, 4, 1)) - 1 
		Byte1 = Chrb(((Char2 And 48)*16) Or (Char1 * 4) And &HFF) 
		Byte2 = lsGroupBinary & Chrb(((Char3 And 60)*4) Or (Char2 * 16) And &HFF) 
		Byte3 = Chrb((((Char3 And 3) * 64) And &HFF) Or (Char4 And 63)) 
		lsGroupBinary = Byte1 & Byte2 & Byte3 
		lsResult = lsResult & lsGroupBinary 
	Next


	//处理最后剩余的几个字符
	if M4 > 0 then 
		lsGroupBinary = "" 
		lsGroup64 = Midb(asContents, len2+1, m4) & chrB(65) //chr(65)=A，转换成值为0
		if M4=2 then
		//补足4位，是为了便于计算 
			lsGroup64 = lsGroup64 & chrB(65) 
		end if
		Char1 = InStrb(sBASE_64_CHARACTERS, Midb(lsGroup64, 1, 1)) - 1 
		Char2 = InStrb(sBASE_64_CHARACTERS, Midb(lsGroup64, 2, 1)) - 1 
		Char3 = InStrb(sBASE_64_CHARACTERS, Midb(lsGroup64, 3, 1)) - 1 
		Char4 = InStrb(sBASE_64_CHARACTERS, Midb(lsGroup64, 4, 1)) - 1 
		Byte1 = Chrb(((Char2 And 48)*16) Or (Char1 * 4) And &HFF) 
		Byte2 = lsGroupBinary & Chrb(((Char3 And 60)*4) Or (Char2 * 16) And &HFF) 
		Byte3 = Chrb((((Char3 And 3) * 64) And &HFF) Or (Char4 And 63)) 

		if M4=2 then
			lsGroupBinary = Byte1
		elseif M4=3 then
			lsGroupBinary = Byte1 & Byte2
		end if

		lsResult = lsResult & lsGroupBinary 
	end if

	Base64decode = lsResult 
End Function


%>