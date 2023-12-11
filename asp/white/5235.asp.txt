<!--#include file="AspCms_StyleFun.asp" -->
<%CheckAdmin("AspCms_Style.asp")%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<SCRIPT>
function SelAll(theForm){
		for ( i = 0 ; i < theForm.elements.length ; i ++ )
		{
			if ( theForm.elements[i].type == "checkbox" && theForm.elements[i].name != "SELALL" )
			{
				theForm.elements[i].checked = ! theForm.elements[i].checked ;
			}
		}
}
</SCRIPT>
<style>

/*表格样式*/
.maintable {
	line-height: 2em;
}
.maintable td{
	line-height: 2.4em;
}
#stylemain .pic{ height:200px;}
#stylemain {}
.pic img{
margin:5px;
	padding:3px;
	border: 1px dashed #66CCCC;
}

#stylemain .desc{ text-align:left; padding:2px 10px;}

</style>
<BODY>
<form action="?" method="post" name="form">
<DIV class=searchzone>

<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>
    模板文件夹：<input type="text"  class="input" name="htmlFilePath" class="my_input" style="width:100px" value="<%=setting.htmlFilePath%>"/> 防止模板被盗
        <input type="submit"  class="button"  value="修改" onClick="form.action='?action=edithtmlfilepath'"/></TD>
    <TD align=right colSpan=2><INPUT onClick="location.href='<%=getPageName()%>'" type="button" value="刷新" class="button" /> 
</TD></TR></TBODY></TABLE></DIV>
<DIV class=listzone>

	  
      
<table width="98%" border="0" align="center" cellpadding="0" id="stylemain">  
    <TR class=list>	
    <TD width="237" height=28 align="center" class=biaoti><span class="searchzone">缩略图</span></TD>
    <TD width=621 align="center" class=biaoti><span class="searchzone">简介</span></TD>
    </TR>  
              
			<%
			Dim folderArry,folderAttr,i,tempStr
			folderArry=getFolderList("../../Templates")
			if instr(folderArry(0),",")>0 then
				for i=0 to ubound(folderArry)
					folderAttr=split(folderArry(i),",")
					
					'echo folderAttr(0)&","&folderAttr(1)&","&folderAttr(2)&","&folderAttr(3)&","&folderAttr(4)&"<br>"
					
					Dim xmlObj : Set xmlObj=new XmlClass
					if isExistFile(folderAttr(4)&"/template.xml") then
						xmlObj.load folderAttr(4)&"/template.xml","xmlfile"
						'echo xmlObj.isExistNode("description")
						'echo xmlObj.getNodeValue("ScreenShot",0)&"<br>"
						'echo xmlObj.getNodeValue("description",0)&"<br>"
						
						if folderAttr(0)=setting.defaultTemplate then
							tempStr="<input type=""button"" style=""margin:10px 0px;"" value=""正在使用"" class=""button"" />"
						else
							tempStr="<input type=""submit"" style=""margin:10px 0px;"" value=""使用此模板"" class=""button"" onclick=""form.action='?action=select&style="&folderAttr(0)&"'""/>"
						end if
						echo"<tr>"&vbcrlf& _
							"<td bgcolor=""#FFFFFF"" align=""center"" class=""pic innerbiaoti""><img width=""200px"" src="""&folderAttr(4)&"/"&xmlObj.getNodeValue("ScreenShot",0)&"""/><br/>"&tempStr&"</td>"&vbcrlf& _
							"<td bgcolor=""#FFFFFF"" class=""desc innerbiaoti"">简介："&xmlObj.getNodeValue("description",0)&"</td>               "&vbcrlf& _
						"</tr>"&vbcrlf
					end if
				next
			end if
			%>
            </table>           
            

</DIV>
<DIV class=piliang>
</DIV>
</FORM>
</BODY></HTML>



