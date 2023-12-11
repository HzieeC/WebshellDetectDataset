<!--#include file="AspCms_StyleFun.asp" -->
<%CheckAdmin("AspCms_Template.asp")%>
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
<BODY>
	  <form action="?action=delallfile" method="post" name="form">
<DIV class=searchzone>

<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>
    <INPUT onClick="location.href='?acttype=html'" type="button" value="模板管理" class="button" >
    <INPUT onClick="location.href='?acttype=css'" type="button" value="Css管理" class="button" >
    目录：
	<%
	if acttype="css" then 
		echo sitePath&"/Templates/"&setting.defaultTemplate&"/"&"css"
	else
		echo sitePath&"/Templates/"&setting.defaultTemplate&"/"&setting.htmlFilePath	
	end if
	
	
	
	%>
    
    </TD>
    <TD align=right colSpan=2>
    <INPUT onClick="location.href='AspCms_TemplateAdd.asp?acttype=html'" type="button" value="添加模板" class="button" > 
    <INPUT onClick="location.href='AspCms_TemplateAdd.asp?acttype=css'" type="button" value="添加Css" class="button" > 
    <INPUT onClick="location.href='<%=getPageName()%>'" type="button" value="刷新" class="button" /> 
</TD></TR></TBODY></TABLE></DIV>
<DIV class=listzone>
<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD width=101 align="center" class=biaoti><span class="searchzone">选择</span></TD>
    <TD width=40 align="center" class=biaoti>编号</TD>
    <TD width="136" height=28 align="center" class=biaoti><span class="searchzone">文件名称</span></TD>
    <TD width="143" height=28 align="center" class=biaoti><span class="searchzone">大小</span></TD>
    <TD width="149" height=28 align="center" class=biaoti><span class="searchzone">类型</span></TD>
    <TD width=143 align="center" class=biaoti><span class="searchzone">修改日期</span></TD>
    <TD width=128 align="center" class=biaoti><span class="searchzone">操作</span></TD>
    </TR>

<%

fileList acttype
Sub fileList(acttype)
	dim path,fileListArray,i,fileAttr,folderListArray,folderAttr,parentPath
	if acttype="css" then 
		path=sitePath&"/Templates/"&setting.defaultTemplate&"/"&"css"
	else
		path=sitePath&"/Templates/"&setting.defaultTemplate&"/"&setting.htmlFilePath	
	end if
	if not isExistFolder(path) then createFolder path,"folderdir"
	
	if not isnul(getForm("dirpath","get")) then
		path=getForm("dirpath","get")
		parentPath=Mid(getForm("dirpath","get"),1,InstrRev(getForm("dirpath","get"),"/")-1)
	end if
	

	
	if not isExistFolder(path) then	
		echo "<tr bgcolor=""#FFFFFF"" align=""center""><td colspan=""7"" algin=""center"">没有这个目录,<a href=""javascript:history.go(-1)"" class=""txt_C1"">返回</a></td></tr>"
		response.end
	end if
	fileListArray= getFileList(path)
	if instr(fileListArray(0),",")>0 then	
		for  i = 0 to ubound(fileListArray)
			fileAttr=split(fileListArray(i),",")
			echo"<tr bgcolor=""#FFFFFF"" align=""center"" onMouseOver=""this.bgColor='#CDE6FF'"" onMouseOut=""this.bgColor='#FFFFFF'"">"&vbcrlf& _
			"<td><input type=""checkbox"" name=""filename"" value="""&fileAttr(0)&""" class=""checkbox"" /></td>"&vbcrlf& _
			"<td>"&i+1&"</td>"&vbcrlf& _
			"<td><a target=""_blank"" href="""&fileAttr(4)&""">"&fileAttr(0)&"</a></td>"&vbcrlf& _
			"<td>"&fileAttr(2)&"</td>"&vbcrlf& _
			"<td>"&fileAttr(1)&"</td>"&vbcrlf& _
			"<td>"&fileAttr(3)&"</td>"&vbcrlf& _
			"<td><a href=""AspCms_TemplateEdit.asp?acttype="&acttype&"&filename="&fileAttr(0)&""" class=""txt_C1"" >编辑</a> | <a href=""?acttype="&acttype&"&action=del&filename="&fileAttr(0)&""" class=""txt_C1"" onClick=""return confirm('确定要删除吗')"">删除</a></td>"&vbcrlf& _
			
			"</tr>"&vbcrlf
		next		
	end if	
	if not instr(fileListArray(0),",")>0 then
		echo "<tr bgcolor=""#FFFFFF"" align=""center""><td colspan=""7"" algin=""center"">空文件夹,<a href=""javascript:history.go(-1)"" class=""txt_C1"">返回</a></td></tr>"
		
	end if
End Sub
%>
    </TBODY>
   </TABLE>
</DIV>
<DIV class=piliang>
<INPUT onClick="SelAll(this.form)" type="checkbox" value="1" name="SELALL"> 全选&nbsp;
<INPUT class="button" type="submit" value="删除" onClick="if(confirm('确定要删除吗')){form.action='?acttype=<%=acttype%>&action=del';}else{return false};"/> 

</DIV>
</FORM>
</BODY></HTML>