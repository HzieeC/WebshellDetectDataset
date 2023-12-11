<%
'去掉字符串中的html标记 保留<br>
Function NoHtml(str) 
	Set re=new RegExp 
	re.IgnoreCase =true 
	re.Global=True 
	str = replace(str,"<br>","{br}") 
	str = replace(str,"<p>","{p}") 
	str = replace(str,"</p>","{/p}") 
	str = replace(str,"<b>","{b}") 
	str = replace(str,"</b>","{/b}") 
	str = replace(str,"<strong>","{strong}") 
	str = replace(str,"</strong>","{/strong}") 	
	re.Pattern="(\<.[^\<]*\>)" 
	str=re.replace(str,"") 
	re.Pattern="(\<\/[^\<]*\>)" 
	str=re.replace(str,"")
	re.Pattern="/(^[\\s]*)/g"
	str=re.replace(str,"")
	str = replace(str,"{br}","<br>")
	str = replace(str,"{p}","<p>")
	str = replace(str,"{/p}","</p>")
	str = replace(str,"{b}","<b>")
	str = replace(str,"{/b}","</b>")
	str = replace(str,"{strong}","<strong>")
	str = replace(str,"{/strong}","</strong>")		
	NoHtml=str 
	Set re=Nothing 
End Function 

'去掉字符串头尾的连续的回车和空格
Function trimVBcrlf(str)
	trimVBcrlf=rtrimVBcrlf(ltrimVBcrlf(str))
End Function

'去掉字符串开头的连续的回车和空格
Function ltrimVBcrlf(str)
	Dim pos,isBlankChar
	pos=1
	isBlankChar=true
	While isBlankChar
		If  mid(str,pos,1)=" " then
			pos=pos+1
		ElseIf mid(str,pos,2)=VBcrlf then
			pos=pos+2
		Else
			isBlankChar=false
		End if
	Wend
	ltrimVBcrlf=right(str,len(str)-pos+1)
End Function

'去掉字符串末尾的连续的回车和空格
Function rtrimVBcrlf(str)
	Dim pos,isBlankChar
	pos=len(str)
	isBlankChar=true
	While isBlankChar and pos>=2
		If mid(str,pos,1)="" then
			pos=pos-1
		ElseIf mid(str,pos-1,2)=VBcrlf then
			pos=pos-2
		Else
			isBlankChar=false
		End if
	Wend
	rtrimVBcrlf=rtrim(left(str,pos))
End Function



  '/*   函数名称：ReplaceHtml　ClearHtml   
  '/*   函数语言：VBScript   Language     
  '/*   作　　用：清除文件HTML格式函数   
  '/*   传递参数：Content   (注：需要进行清除的内容)   
  '/*   函数说明：正则匹配(正则表达式)模式进行数据匹配替换   
    
Function   ClearHtml(Content)   
	  Content=ReplaceHtml("<hr />","",Content)
	  Content=ReplaceHtml("&#[^>]*;",   "",   Content)   
	  Content=ReplaceHtml("</?marquee[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("</?object[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("</?param[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("</?embed[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("</?table[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("&nbsp;","",Content)   
	  Content=ReplaceHtml("</?tr[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("</?th[^>]*>","",Content)   
	  Content=ReplaceHtml("</?p[^>]*>","",Content)   
	  Content=ReplaceHtml("</?a[^>]*>","",Content)   
	  Content=ReplaceHtml("</?img[^>]*>","",Content)   
	  Content=ReplaceHtml("</?tbody[^>]*>","",Content)   
	  Content=ReplaceHtml("</?li[^>]*>","",Content)   
	  Content=ReplaceHtml("</?span[^>]*>","",Content)   
	  Content=ReplaceHtml("</?div[^>]*>","",Content)   
	  Content=ReplaceHtml("</?th[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("</?td[^>]*>",   "",   Content) 
	  Content=ReplaceHtml("</?script[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("(javascript|jscript|vbscript|vbs):",   "",   Content)   
	  Content=ReplaceHtml("on(mouse|exit|error|click|key)",   "",   Content)   
	  Content=ReplaceHtml("<\\?xml[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("<\/?[a-z]+:[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("</?font[^>]*>",   "",   Content)   
	  Content=ReplaceHtml("</?b[^>]*>","",Content)   
	  Content=ReplaceHtml("</?u[^>]*>","",Content)   
	  Content=ReplaceHtml("</?i[^>]*>","",Content)   
	  Content=ReplaceHtml("</?strong[^>]*>","",Content)   
	  Content=ReplaceHtml("　　","",Content)
	  ClearHtml=Content   
End   Function   

Function   NoLabel(Content)   
	  Content=Replace(Content,"&#[^>]*;","")   
	  Content=Replace(Content,"</?marquee[^>]*>","")   
	  Content=Replace(Content,"</?object[^>]*>","")   
	  Content=Replace(Content,"</?param[^>]*>","")   
	  Content=Replace(Content,"</?embed[^>]*>","")   
	  Content=Replace(Content,"</?table[^>]*>","")   
	  Content=Replace(Content,"&nbsp;","")   
	  Content=Replace(Content,"</?tr[^>]*>","")   
	  Content=Replace(Content,"</?th[^>]*>","")   
	  Content=Replace(Content,"</?p[^>]*>","")   
	  Content=Replace(Content,"</?a[^>]*>","")   
	  Content=Replace(Content,"</?img[^>]*>","")   
	  Content=Replace(Content,"</?tbody[^>]*>","")   
	  Content=Replace(Content,"</?li[^>]*>","")   
	  Content=Replace(Content,"</?span[^>]*>","")   
	  Content=Replace(Content,"</?div[^>]*>","")   
	  Content=Replace(Content,"</?th[^>]*>",   "")   
	  Content=Replace(Content,"</?td[^>]*>",   "") 
	  Content=Replace(Content,"</?script[^>]*>",   "")   
	  Content=Replace(Content,"(javascript|jscript|vbscript|vbs):",   "")   
	  Content=Replace(Content,"on(mouse|exit|error|click|key)",   "")   
	  Content=Replace(Content,"<\\?xml[^>]*>",   "")   
	  Content=Replace(Content,"<\/?[a-z]+:[^>]*>",   "")   
	  Content=Replace(Content,"</?font[^>]*>",   "")   
	  Content=Replace(Content,"</?b[^>]*>","")   
	  Content=Replace(Content,"</?u[^>]*>","")   
	  Content=Replace(Content,"</?i[^>]*>","")   
	  Content=Replace(Content,"</?strong[^>]*>","")   
	  Content=Replace(Content,"　　","")
	  NoLabel=Content   
End   Function   
  
Function   BusinessStr(code)   
	  code=ReplaceHtml("&lt;","<",code)   
	  code=ReplaceHtml("&gt;",">",code)
	  code=ReplaceHtml("&amp;","&",code)
	  code=ReplaceHtml("<FONT face=Verdana>","",code)
	  code=ReplaceHtml("<P>","",code)
	  code=ReplaceHtml("</P>","",code)
	  BusinessStr=code   
End   Function
   
Function   ReplaceHtml(patrn,   strng,content)   
	  If  IsNull(content)   Then   
		   content=""   
	  End  If   
	  Set   regEx   =   New   RegExp '   建立正则表达式。   
	  regEx.Pattern   =   patrn '   设置模式。   
	  regEx.IgnoreCase   =   true             '   设置忽略字符大小写。   
	  regEx.Global   =   True '   设置全局可用性。   
	  ReplaceHtml=regEx.Replace(content,strng) '   执行正则匹配   
End   Function 
%>
