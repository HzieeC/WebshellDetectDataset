<%
dim pageno,pagemax,rscount,rsno,loopno,i,page
'---------------------检查用户名密码-------------------------------
function Checkin(s) 
s=trim(s) 
s=replace(s," ","&amp;nbsp;") 
s=replace(s,"'","&amp;#39;") 
s=replace(s,"""","&amp;quot;") 
s=replace(s,"&lt;","&amp;lt;") 
s=replace(s,"&gt;","&amp;gt;") 
Checkin=s 
end function 
'--------------------------------------------
Rem 过滤SQL非法字符
function checkStr(str)
    str=trim(str) 
	if isnull(str) then
		checkStr = ""
		exit function 
	end if
	checkStr=replace(str,"'","''")
end function

'-------------------------------------

Rem 过滤HTML代码
function HTMLcode(fString)
if not isnull(fString) then
    fString = replace(fString, ">", "&gt;")
    fString = replace(fString, "<", "&lt;")

    fString = Replace(fString, CHR(32), "&nbsp;")
    fString = Replace(fString, CHR(9), "&nbsp;")
    fString = Replace(fString, CHR(34), "&quot;")
    fString = Replace(fString, CHR(39), "&#39;")
    fString = Replace(fString, CHR(13), "")
    fString = Replace(fString, CHR(10) & CHR(10), "</P><P> ")
    fString = Replace(fString, CHR(10), "<BR> ")

    HTMLEncode = fString
end if
end function

'--------------------------------------
Rem 过滤表单字符
function HTMLcode(fString)
if not isnull(fString) then
    fString = Replace(fString, CHR(13), "")
    fString = Replace(fString, CHR(10) & CHR(10), "</P><P>")
    fString = Replace(fString, CHR(10), "<BR>")
    HTMLcode = fString
end if
end function

'**************************************************
'函数名：IsObjInstalled
'作  用：检查组件是否已经安装
'参  数：strClassString ----组件名
'返回值：True  ----已经安装
'       False ----没有安装
'**************************************************
Function IsObjInstalled(strClassString)
	On Error Resume Next
	IsObjInstalled = False
	Err = 0
	Dim xTestObj
	Set xTestObj = Server.CreateObject(strClassString)
	If 0 = Err Then IsObjInstalled = True
	Set xTestObj = Nothing
	Err = 0
End Function

'**************************************************
'函数名：shosql
'作  用：分页效果
'参  数：spage  ----当前页
'参  数：spageno  ----每页数量
'参  数：srscount  ---数据总数
'**************************************************
sub showsql(spageno)
page=request("page")
if page="" or not isnumeric(page) then
page=1
else
page=clng(page)
end if
pageno=spageno
if page<1 then
page=1
end if

rscount=rscount

pagemax=rscount\pageno
if rscount mod pageno <>0 then
pagemax=pagemax+1
end if
if page>pagemax then
page=pagemax
end if
rsno=pageno*page-pageno
if page=pagemax then
loopno=rscount-rsno
else 
loopno=pageno
end if
end sub

'**************************************************
'函数名：JoinChar
'作  用：向地址中加入 ? 或 &
'参  数：strUrl  ----网址
'返回值：加了 ? 或 & 的网址
'**************************************************
function JoinChar(strUrl)
	if strUrl="" then
		JoinChar=""
		exit function
	end if
	if InStr(strUrl,"?")<len(strUrl) then 
		if InStr(strUrl,"?")>1 then
			if InStr(strUrl,"&")<len(strUrl) then 
				JoinChar=strUrl & "&"
			else
				JoinChar=strUrl
			end if
		else
			JoinChar=strUrl & "?"
		end if
	else
		JoinChar=strUrl
	end if
end function

'**************************************************
'过程名：showpage
'作  用：显示“上一页 下一页”等信息
'参  数：sfilename  ----链接地址
'       totalnumber ----总数量
'       maxperpage  ----每页数量
'       ShowTotal   ----是否显示总数量
'       ShowAllPages ---是否用下拉列表显示所有页面以供跳转。有某些页面不能使用，否则会出现JS错误。
'       strUnit     ----计数单位
'**************************************************
sub showpage(sfilename,totalnumber,maxperpage,ShowTotal,ShowAllPages,strUnit)
	dim n, i,strTemp,strUrl
	if totalnumber mod maxperpage=0 then
    	n= totalnumber \ maxperpage
  	else
    	n= totalnumber \ maxperpage+1
  	end if
  	strTemp= "<table width='100%'><tr><td align='center' height=25>"
	if ShowTotal=true then 
		strTemp=strTemp & "共 <b>" & totalnumber & "</b> " & strUnit & "&nbsp;&nbsp;"
	end if
	strUrl=JoinChar(sfilename)
  	if page<2 then
    		strTemp=strTemp & "首页 上一页&nbsp;"
  	else
    		strTemp=strTemp & "<a href='" & strUrl  &"act="&act&"&" &"keywords="&keywords&"&"& "page=1 '>首页</a>&nbsp;"
    		strTemp=strTemp & "<a href='" & strUrl & "page=" & (Page-1) & "&" &"act="&act&"&" &"keywords="&keywords&"'>上一页</a>&nbsp;"
  	end if

  	if n-page<1 then
    		strTemp=strTemp & "下一页 尾页"
  	else
    		strTemp=strTemp & "<a href='" & strUrl & "page=" & (Page+1) & "&" &"act="&act&"&" &"keywords="&keywords&"'>下一页</a>&nbsp;"
    		strTemp=strTemp & "<a href='" & strUrl &"page=" & n & "&" &"act="&act&"&" &"keywords="&keywords&"'>尾页</a>"
  	end if
   	strTemp=strTemp & "&nbsp;页次：<strong><font color=red>" & Page & "</font>/" & n & "</strong>页 "
    strTemp=strTemp & "&nbsp;<b>" & maxperpage & "</b>" & strUnit & "条/页"
	if ShowAllPages=True then
act="act="&request("act")
keywords="keywords="&request("keywords")
		strTemp=strTemp & "&nbsp;转到：<select name='page' size='1' onchange=""javascript:window.location='" & strUrl &act&"&"&keywords&"&"& "page=" & "'+this.options[this.selectedIndex].value;"">"   
    	for i = 1 to n   
    		strTemp=strTemp & "<option value='" & i & "'"
			if cint(Page)=cint(i) then strTemp=strTemp & " selected "
			strTemp=strTemp & ">第" & i & "页</option>"   
	    next
		strTemp=strTemp & "</select>"
	end if
	strtemp=strtemp&"&nbsp;&nbsp;数据总数:<b>"&rscount&"</b>条"
	strTemp=strTemp & "</td></tr></table>"
	response.write strTemp
end sub

%>
