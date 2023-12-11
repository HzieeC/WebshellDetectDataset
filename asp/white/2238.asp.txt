<%

	Private Const ASP_BITS_TO_A_BYTE = 8
	Private Const ASP_BYTES_TO_A_WORD = 4
	Private Const ASP_BITS_TO_A_WORD = 32
	
	Private ASP_m_lOnBits(30)
	Private ASP_m_l2Power(30)
	
	Private Function ASP_LShift(lValue, iShiftBits)
		If iShiftBits = 0 Then
			ASP_LShift = lValue
			Exit Function
		ElseIf iShiftBits = 31 Then
			If lValue And 1 Then
				ASP_LShift = &H80000000
			Else
				ASP_LShift = 0
			End If
			Exit Function
		ElseIf iShiftBits < 0 Or iShiftBits > 31 Then
			Err.Raise 6
		End If
	
		If (lValue And ASP_m_l2Power(31 - iShiftBits)) Then
			ASP_LShift = ((lValue And ASP_m_lOnBits(31 - (iShiftBits + 1))) * ASP_m_l2Power(iShiftBits)) Or &H80000000
		Else
			ASP_LShift = ((lValue And ASP_m_lOnBits(31 - iShiftBits)) * ASP_m_l2Power(iShiftBits))
		End If
	End Function
	
	Private Function ASP_Str2binold(varstr) 
		ASP_Str2binold="" 
		 For i=1 To Len(varstr) 
			 varchar=mid(varstr,i,1) 
			 varasc = Asc(varchar) 
			 If varasc<0 Then 
				varasc = varasc + 65535 
			 End If 
			 If varasc>255 Then 
				varlow = Left(Hex(Asc(varchar)),2) 
				varhigh = right(Hex(Asc(varchar)),2) 
				ASP_Str2binold = ASP_Str2binold & chrB("&H" & varlow) & chrB("&H" & varhigh) 
			 Else 
				ASP_Str2binold = ASP_Str2binold & chrB(AscB(varchar)) 
			 End If 
		 Next 
	End Function 
	Private Function ASP_Str2bin(varstr) 
		ASP_Str2bin="" 
		 For i=1 To Len(varstr) 
			 varchar=mid(varstr,i,1) 
			 code = Server.UrlEncode(varchar)
			 if len(code) = 1 then
			    ASP_Str2bin = ASP_Str2bin & chrB(AscB(code))
			 else
				codearr = split(code,"%")
				for j=1 to ubound(codearr)
				   ASP_Str2bin = ASP_Str2bin & chrB("&H" & codearr(j))
				next

			 End If 
		 Next 
	End Function 
	Private Function ASP_RShift(lValue, iShiftBits)
		If iShiftBits = 0 Then
			ASP_RShift = lValue
			Exit Function
		ElseIf iShiftBits = 31 Then
			If lValue And &H80000000 Then
				ASP_RShift = 1
			Else
				ASP_RShift = 0
			End If
			Exit Function
		ElseIf iShiftBits < 0 Or iShiftBits > 31 Then
			Err.Raise 6
		End If
	
		ASP_RShift = (lValue And &H7FFFFFFE) \ ASP_m_l2Power(iShiftBits)
	
		If (lValue And &H80000000) Then
			ASP_RShift = (ASP_RShift Or (&H40000000 \ ASP_m_l2Power(iShiftBits - 1)))
		End If
	End Function
	
	Private Function ASP_RotateLeft(lValue, iShiftBits)
		ASP_RotateLeft = ASP_LShift(lValue, iShiftBits) Or ASP_RShift(lValue, (32 - iShiftBits))
	End Function
	
	Private Function ASP_AddUnsigned(lX, lY)
		Dim lX4
		Dim lY4
		Dim lX8
		Dim lY8
		Dim lResult
	
		lX8 = lX And &H80000000
		lY8 = lY And &H80000000
		lX4 = lX And &H40000000
		lY4 = lY And &H40000000
		
		lResult = (lX And &H3FFFFFFF) + (lY And &H3FFFFFFF)
	
		If lX4 And lY4 Then
			lResult = lResult Xor &H80000000 Xor lX8 Xor lY8
		ElseIf lX4 Or lY4 Then
			If lResult And &H40000000 Then
				lResult = lResult Xor &HC0000000 Xor lX8 Xor lY8
			Else
				lResult = lResult Xor &H40000000 Xor lX8 Xor lY8
			End If
		Else
			lResult = lResult Xor lX8 Xor lY8
		End If
	
		ASP_AddUnsigned = lResult
	End Function
	
	Private Function ASP_md5_F(x, y, z)
		ASP_md5_F = (x And y) Or ((Not x) And z)
	End Function
	
	Private Function ASP_md5_G(x, y, z)
		ASP_md5_G = (x And z) Or (y And (Not z))
	End Function
	
	Private Function ASP_md5_H(x, y, z)
		ASP_md5_H = (x Xor y Xor z)
	End Function
	
	Private Function ASP_md5_I(x, y, z)
		ASP_md5_I = (y Xor (x Or (Not z)))
	End Function
	
	Private Sub ASP_md5_FF(a, b, c, d, x, s, ac)
		a = ASP_AddUnsigned(a, ASP_AddUnsigned(ASP_AddUnsigned(ASP_md5_F(b, c, d), x), ac))
		a = ASP_RotateLeft(a, s)
		a = ASP_AddUnsigned(a, b)
	End Sub
	
	Private Sub ASP_md5_GG(a, b, c, d, x, s, ac)
		a = ASP_AddUnsigned(a, ASP_AddUnsigned(ASP_AddUnsigned(ASP_md5_G(b, c, d), x), ac))
		a = ASP_RotateLeft(a, s)
		a = ASP_AddUnsigned(a, b)
	End Sub
	
	Private Sub ASP_md5_HH(a, b, c, d, x, s, ac)
		a = ASP_AddUnsigned(a, ASP_AddUnsigned(ASP_AddUnsigned(ASP_md5_H(b, c, d), x), ac))
		a = ASP_RotateLeft(a, s)
		a = ASP_AddUnsigned(a, b)
	End Sub
	
	Private Sub ASP_md5_II(a, b, c, d, x, s, ac)
		a = ASP_AddUnsigned(a, ASP_AddUnsigned(ASP_AddUnsigned(ASP_md5_I(b, c, d), x), ac))
		a = ASP_RotateLeft(a, s)
		a = ASP_AddUnsigned(a, b)
	End Sub
	
	Private Function ASP_ConvertToWordArray(sMessage)
		Dim lMessageLength
		Dim lNumberOfWords
		Dim lWordArray()
		Dim lBytePosition
		Dim lByteCount
		Dim lWordCount
		
		Const MODULUS_BITS = 512
		Const CONGRUENT_BITS = 448
		
		lMessageLength = LenB(sMessage)
		
		lNumberOfWords = (((lMessageLength + ((MODULUS_BITS - CONGRUENT_BITS) \ ASP_BITS_TO_A_BYTE)) \ (MODULUS_BITS \ ASP_BITS_TO_A_BYTE)) + 1) * (MODULUS_BITS \ ASP_BITS_TO_A_WORD)
		ReDim lWordArray(lNumberOfWords - 1)
		
		lBytePosition = 0
		lByteCount = 0
		Do Until lByteCount >= lMessageLength
			lWordCount = lByteCount \ ASP_BYTES_TO_A_WORD
			lBytePosition = (lByteCount Mod ASP_BYTES_TO_A_WORD) * ASP_BITS_TO_A_BYTE
			lWordArray(lWordCount) = lWordArray(lWordCount) Or ASP_LShift(AscB(MidB(sMessage, lByteCount + 1, 1)), lBytePosition)
			lByteCount = lByteCount + 1
		Loop
	
		lWordCount = lByteCount \ ASP_BYTES_TO_A_WORD
		lBytePosition = (lByteCount Mod ASP_BYTES_TO_A_WORD) * ASP_BITS_TO_A_BYTE
		
		lWordArray(lWordCount) = lWordArray(lWordCount) Or ASP_LShift(&H80, lBytePosition)
		
		lWordArray(lNumberOfWords - 2) = ASP_LShift(lMessageLength, 3)
		lWordArray(lNumberOfWords - 1) = ASP_RShift(lMessageLength, 29)
	
		ASP_ConvertToWordArray = lWordArray
	End Function
	
	Private Function ASP_WordToHex(lValue)
		Dim lByte
		Dim lCount
		
		For lCount = 0 To 3
			lByte = ASP_RShift(lValue, lCount * ASP_BITS_TO_A_BYTE) And ASP_m_lOnBits(ASP_BITS_TO_A_BYTE - 1)
			ASP_WordToHex = ASP_WordToHex & Right("0" & Hex(lByte), 2)
		Next
	End Function
	
	Public Function ASP_MD5(sMessage)
		ASP_m_lOnBits(0) = CLng(1)
		ASP_m_lOnBits(1) = CLng(3)
		ASP_m_lOnBits(2) = CLng(7)
		ASP_m_lOnBits(3) = CLng(15)
		ASP_m_lOnBits(4) = CLng(31)
		ASP_m_lOnBits(5) = CLng(63)
		ASP_m_lOnBits(6) = CLng(127)
		ASP_m_lOnBits(7) = CLng(255)
		ASP_m_lOnBits(8) = CLng(511)
		ASP_m_lOnBits(9) = CLng(1023)
		ASP_m_lOnBits(10) = CLng(2047)
		ASP_m_lOnBits(11) = CLng(4095)
		ASP_m_lOnBits(12) = CLng(8191)
		ASP_m_lOnBits(13) = CLng(16383)
		ASP_m_lOnBits(14) = CLng(32767)
		ASP_m_lOnBits(15) = CLng(65535)
		ASP_m_lOnBits(16) = CLng(131071)
		ASP_m_lOnBits(17) = CLng(262143)
		ASP_m_lOnBits(18) = CLng(524287)
		ASP_m_lOnBits(19) = CLng(1048575)
		ASP_m_lOnBits(20) = CLng(2097151)
		ASP_m_lOnBits(21) = CLng(4194303)
		ASP_m_lOnBits(22) = CLng(8388607)
		ASP_m_lOnBits(23) = CLng(16777215)
		ASP_m_lOnBits(24) = CLng(33554431)
		ASP_m_lOnBits(25) = CLng(67108863)
		ASP_m_lOnBits(26) = CLng(134217727)
		ASP_m_lOnBits(27) = CLng(268435455)
		ASP_m_lOnBits(28) = CLng(536870911)
		ASP_m_lOnBits(29) = CLng(1073741823)
		ASP_m_lOnBits(30) = CLng(2147483647)
		
		ASP_m_l2Power(0) = CLng(1)
		ASP_m_l2Power(1) = CLng(2)
		ASP_m_l2Power(2) = CLng(4)
		ASP_m_l2Power(3) = CLng(8)
		ASP_m_l2Power(4) = CLng(16)
		ASP_m_l2Power(5) = CLng(32)
		ASP_m_l2Power(6) = CLng(64)
		ASP_m_l2Power(7) = CLng(128)
		ASP_m_l2Power(8) = CLng(256)
		ASP_m_l2Power(9) = CLng(512)
		ASP_m_l2Power(10) = CLng(1024)
		ASP_m_l2Power(11) = CLng(2048)
		ASP_m_l2Power(12) = CLng(4096)
		ASP_m_l2Power(13) = CLng(8192)
		ASP_m_l2Power(14) = CLng(16384)
		ASP_m_l2Power(15) = CLng(32768)
		ASP_m_l2Power(16) = CLng(65536)
		ASP_m_l2Power(17) = CLng(131072)
		ASP_m_l2Power(18) = CLng(262144)
		ASP_m_l2Power(19) = CLng(524288)
		ASP_m_l2Power(20) = CLng(1048576)
		ASP_m_l2Power(21) = CLng(2097152)
		ASP_m_l2Power(22) = CLng(4194304)
		ASP_m_l2Power(23) = CLng(8388608)
		ASP_m_l2Power(24) = CLng(16777216)
		ASP_m_l2Power(25) = CLng(33554432)
		ASP_m_l2Power(26) = CLng(67108864)
		ASP_m_l2Power(27) = CLng(134217728)
		ASP_m_l2Power(28) = CLng(268435456)
		ASP_m_l2Power(29) = CLng(536870912)
		ASP_m_l2Power(30) = CLng(1073741824)
		
		
		Dim x
		Dim k
		Dim AA
		Dim BB
		Dim CC
		Dim DD
		Dim a
		Dim b
		Dim c
		Dim d
		
		Const S11 = 7
		Const S12 = 12
		Const S13 = 17
		Const S14 = 22
		Const S21 = 5
		Const S22 = 9
		Const S23 = 14
		Const S24 = 20
		Const S31 = 4
		Const S32 = 11
		Const S33 = 16
		Const S34 = 23
		Const S41 = 6
		Const S42 = 10
		Const S43 = 15
		Const S44 = 21
		
		x = ASP_ConvertToWordArray(ASP_Str2bin(sMessage))
		
		a = &H67452301
		b = &HEFCDAB89
		c = &H98BADCFE
		d = &H10325476
		
		For k = 0 To UBound(x) Step 16
			AA = a
			BB = b
			CC = c
			DD = d
			
			ASP_md5_FF a, b, c, d, x(k + 0), S11, &HD76AA478
			ASP_md5_FF d, a, b, c, x(k + 1), S12, &HE8C7B756
			ASP_md5_FF c, d, a, b, x(k + 2), S13, &H242070DB
			ASP_md5_FF b, c, d, a, x(k + 3), S14, &HC1BDCEEE
			ASP_md5_FF a, b, c, d, x(k + 4), S11, &HF57C0FAF
			ASP_md5_FF d, a, b, c, x(k + 5), S12, &H4787C62A
			ASP_md5_FF c, d, a, b, x(k + 6), S13, &HA8304613
			ASP_md5_FF b, c, d, a, x(k + 7), S14, &HFD469501
			ASP_md5_FF a, b, c, d, x(k + 8), S11, &H698098D8
			ASP_md5_FF d, a, b, c, x(k + 9), S12, &H8B44F7AF
			ASP_md5_FF c, d, a, b, x(k + 10), S13, &HFFFF5BB1
			ASP_md5_FF b, c, d, a, x(k + 11), S14, &H895CD7BE
			ASP_md5_FF a, b, c, d, x(k + 12), S11, &H6B901122
			ASP_md5_FF d, a, b, c, x(k + 13), S12, &HFD987193
			ASP_md5_FF c, d, a, b, x(k + 14), S13, &HA679438E
			ASP_md5_FF b, c, d, a, x(k + 15), S14, &H49B40821
			
			ASP_md5_GG a, b, c, d, x(k + 1), S21, &HF61E2562
			ASP_md5_GG d, a, b, c, x(k + 6), S22, &HC040B340
			ASP_md5_GG c, d, a, b, x(k + 11), S23, &H265E5A51
			ASP_md5_GG b, c, d, a, x(k + 0), S24, &HE9B6C7AA
			ASP_md5_GG a, b, c, d, x(k + 5), S21, &HD62F105D
			ASP_md5_GG d, a, b, c, x(k + 10), S22, &H2441453
			ASP_md5_GG c, d, a, b, x(k + 15), S23, &HD8A1E681
			ASP_md5_GG b, c, d, a, x(k + 4), S24, &HE7D3FBC8
			ASP_md5_GG a, b, c, d, x(k + 9), S21, &H21E1CDE6
			ASP_md5_GG d, a, b, c, x(k + 14), S22, &HC33707D6
			ASP_md5_GG c, d, a, b, x(k + 3), S23, &HF4D50D87
			ASP_md5_GG b, c, d, a, x(k + 8), S24, &H455A14ED
			ASP_md5_GG a, b, c, d, x(k + 13), S21, &HA9E3E905
			ASP_md5_GG d, a, b, c, x(k + 2), S22, &HFCEFA3F8
			ASP_md5_GG c, d, a, b, x(k + 7), S23, &H676F02D9
			ASP_md5_GG b, c, d, a, x(k + 12), S24, &H8D2A4C8A
			
			ASP_md5_HH a, b, c, d, x(k + 5), S31, &HFFFA3942
			ASP_md5_HH d, a, b, c, x(k + 8), S32, &H8771F681
			ASP_md5_HH c, d, a, b, x(k + 11), S33, &H6D9D6122
			ASP_md5_HH b, c, d, a, x(k + 14), S34, &HFDE5380C
			ASP_md5_HH a, b, c, d, x(k + 1), S31, &HA4BEEA44
			ASP_md5_HH d, a, b, c, x(k + 4), S32, &H4BDECFA9
			ASP_md5_HH c, d, a, b, x(k + 7), S33, &HF6BB4B60
			ASP_md5_HH b, c, d, a, x(k + 10), S34, &HBEBFBC70
			ASP_md5_HH a, b, c, d, x(k + 13), S31, &H289B7EC6
			ASP_md5_HH d, a, b, c, x(k + 0), S32, &HEAA127FA
			ASP_md5_HH c, d, a, b, x(k + 3), S33, &HD4EF3085
			ASP_md5_HH b, c, d, a, x(k + 6), S34, &H4881D05
			ASP_md5_HH a, b, c, d, x(k + 9), S31, &HD9D4D039
			ASP_md5_HH d, a, b, c, x(k + 12), S32, &HE6DB99E5
			ASP_md5_HH c, d, a, b, x(k + 15), S33, &H1FA27CF8
			ASP_md5_HH b, c, d, a, x(k + 2), S34, &HC4AC5665
			
			ASP_md5_II a, b, c, d, x(k + 0), S41, &HF4292244
			ASP_md5_II d, a, b, c, x(k + 7), S42, &H432AFF97
			ASP_md5_II c, d, a, b, x(k + 14), S43, &HAB9423A7
			ASP_md5_II b, c, d, a, x(k + 5), S44, &HFC93A039
			ASP_md5_II a, b, c, d, x(k + 12), S41, &H655B59C3
			ASP_md5_II d, a, b, c, x(k + 3), S42, &H8F0CCC92
			ASP_md5_II c, d, a, b, x(k + 10), S43, &HFFEFF47D
			ASP_md5_II b, c, d, a, x(k + 1), S44, &H85845DD1
			ASP_md5_II a, b, c, d, x(k + 8), S41, &H6FA87E4F
			ASP_md5_II d, a, b, c, x(k + 15), S42, &HFE2CE6E0
			ASP_md5_II c, d, a, b, x(k + 6), S43, &HA3014314
			ASP_md5_II b, c, d, a, x(k + 13), S44, &H4E0811A1
			ASP_md5_II a, b, c, d, x(k + 4), S41, &HF7537E82
			ASP_md5_II d, a, b, c, x(k + 11), S42, &HBD3AF235
			ASP_md5_II c, d, a, b, x(k + 2), S43, &H2AD7D2BB
			ASP_md5_II b, c, d, a, x(k + 9), S44, &HEB86D391
			
			a = ASP_AddUnsigned(a, AA)
			b = ASP_AddUnsigned(b, BB)
			c = ASP_AddUnsigned(c, CC)
			d = ASP_AddUnsigned(d, DD)
		Next
		
		ASP_MD5 = LCase(ASP_WordToHex(a) & ASP_WordToHex(b) & ASP_WordToHex(c) & ASP_WordToHex(d))
	End Function


'使用方法是 md5 ("字符串") 下面是使用示范
'Response.Write "ASP_MD5('a')的加密结果为[" & ASP_MD5 ("a") & "]<br>"
'Response.Write "ASP_MD5('b')的加密结果为[" & ASP_MD5 ("b") & "]"

%>

