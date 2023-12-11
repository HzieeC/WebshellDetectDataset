<!--#include file="../../../inc/AspCms_MainClass.asp" -->
<%
CheckAdmin("AspCms_Slide.asp")
dim action : action=getForm("action", "get")
Select Case action
	Case "save"	 : Call saveService()
End Select 
	
Sub saveService
	dim templateobj,configStr,configPath
	
	
	if not isnum(getForm("slideNum","post")) then alertMsgAndGo "幻灯片个数不正确","-1"
	configPath="../../../config/AspCms_Config.asp"
	set templateobj=new TemplateClass
	configStr=loadFile(configPath)
	configStr=templateobj.regExpReplace(configStr,"Const slideTextStatus=(\d?)","Const slideTextStatus="&getForm("slideTextStatus","post")&"")
	configStr=templateobj.regExpReplace(configStr,"Const slideNum=([\d]*)","Const slideNum="&getForm("slideNum","post")&"")
	
	configStr=templateobj.regExpReplace(configStr,"Const slideWidth=""(\S*?)""","Const slideWidth="""&getForm("slideWidth","post")&"""")
	
	configStr=templateobj.regExpReplace(configStr,"Const slideHeight=""(\S*?)""","Const slideHeight="""&getForm("slideHeight","post")&"""")
	configStr=templateobj.regExpReplace(configStr,"Const slideTexts=""([\s\S]*?)""","Const slideTexts="""&setText(getForm("slideNum","post"),getForm("slideTexts","post"))&"""")
	configStr=templateobj.regExpReplace(configStr,"Const slideLinks=""([\s\S]*?)""","Const slideLinks="""&setText(getForm("slideNum","post"),getForm("slideLinks","post"))&"""")
	configStr=templateobj.regExpReplace(configStr,"Const slideImgs=""([\s\S]*?)""","Const slideImgs="""&setText(getForm("slideNum","post"),getForm("slideImgs","post"))&"""")
	'die configStr
	'die "Const slideImgs="""&setText(getForm("slideNum","post"),getForm("slideImgs","post"))&""""
	
	createTextFile configStr,configPath,""
	set templateobj=nothing
	alertMsgAndGo "修改成功",getPageName()
	
End Sub


Function setText(num,text)
	dim i 
	text=split(text,",")
	for i=0 to num-1
		if ubound(text)+1>i then
			setText=setText&text(i)&","
		else			
			setText=setText&","
		end if	
	next
End Function


%>


<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=save" method="post" >
<DIV class=formzone>
<DIV class=namezone>幻灯片管理</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
  <TR>						
    <TD align="middle" width=100 height=30>图片设置</TD>
    <TD align="left">
    <table width="98%" border="0" align="center" cellpadding="0" cellspacing="1" bgcolor="#a9c5d0" id="">
              <tr bgcolor="#DDEFF9" align="center">
                <td width="31%" height="28">图片</td>
                <td width="69%">上传</td>
              </tr>
			<%
            Dim slideImg,slideLink,slideText,i,imgstr
            slideImg = split(slideImgs,",")
            slideLink = split(slideLinks,",")
            slideText = split(slideTexts,",")
            
            for i=0 to slideNum-1			
				if isnul(trim(slideImg(i))) then
					imgstr="这里插入一张图片"
				else
					imgstr="<img src="""&trim(slideImg(i))&""" width=""160px"" height=""120px"" />"
				end if			
            %>
              <tr align="center" bgcolor="#FFFFFF">
            	<td class="pic"><%=imgstr%></td>
                <td align="left"><table width="95%" border="0" cellspacing="2" cellpadding="0">                
                  <tr>
                    <td width="18%" align="right">图片地址：</td>
                    <td width="82%"><input class="input" style="width:300px" type="text" name="slideImgs" id="ImagePath<%=i%>" value="<%=trim(slideImg(i))%>"/></td>
                  </tr>
                  <tr>
                    <td align="right" valign="top">上传：</td>
                    <td><iframe src="../../editor/upload.asp?sortType=12&stype=file&Tobj=ImagePath<%=i%>" scrolling="No" topmargin="0" width="100%" height="40" marginwidth="0" marginheight="0" frameborder="0" align="left"></iframe></td>
                  </tr>
                  <tr>
                    <td align="right">链接地址：</td>
                    <td><input type="text" class="input" style="width:60%" name="slideLinks"  value="<%=trim(slideLink(i))%>"/>空链接则填"#"</td>
                  </tr>
                  <tr>
                    <td align="right">说明文字： </td>
                    <td><input type="text" class="input" style="width:80%" name="slideTexts"  value="<%=trim(slideText(i))%>"/></td>
                  </tr>
                </table></td>
              </tr>
              <%
			  next
			  %>                        
            </table>   
    
    
    </TD>
  </TR>
  <TR>						
    <TD align=middle width=100 height=30>幻灯片个数</TD>
    <TD><INPUT class="input" style="WIDTH: 40px" maxLength="2" name="slideNum" value="<%=slideNum%>"/></TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>幻灯片高</TD>    
    <TD><input type="text" class="input" name="slideHeight" value="<%=slideHeight%>" style="width:40px" />px</TD>
</tr>
  <TR>
    <TD align=middle width=100 height=30>幻灯片宽</TD>    
    <TD>
		 <input type="text" class="input" name="slideWidth" value="<%=slideWidth%>" style="width:40px" />px</TD>
</tr>

  <TR>
    <TD align=middle width=100 height=30>文字说明</TD>
    <TD>
	 <input type="radio"  value="1" name="slideTextStatus" <% if slideTextStatus=1 then echo "checked" end if%>/>显示
            <input type="radio" value="0" name="slideTextStatus" <% if slideTextStatus=0 then echo "checked" end if%>/>隐藏</TD></TR>
  <TR>
  <TR>
    <TD align=middle width=100 height=30>幻灯片调用标签</TD>
    <TD>{aspcms:slide} 
</TD></TR>
  <TR>
  
    </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
<INPUT class="button" type="submit" value="保存" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
<INPUT onClick="location.href='<%=getPageName()%>'" type="button" value="刷新" class="button" /> 
</DIV></DIV></FORM>

</BODY></HTML>