<!--#include file="inc/conn.asp"-->
<!--#include file="inc/function.asp"-->
<%
Dim SqlItem,RsItem,Action,FoundErr,ErrMsg
Dim ItemID,ItemName,ClassID,SpecialID
Dim  PaginationType,MaxCharPerPage,ReadLevel,Stars,ReadPoint,Hits,UpdateType,UpdateTime,IncludePicYn,DefaultPicYn
Dim  OnTop,Elite,Hot,SkinID,TemplateID
Dim  Script_Iframe,Script_Object,Script_Script,Script_Div,Script_Class,Script_Span,Script_Img,Script_Font,Script_A,Script_Html,Script_Table,Script_Tr,Script_Td,Script_Tbody
Dim  CollecListNum,CollecNewsNum,Passed,SaveFiles,CollecOrder,LinkUrlYn,InputerType,Inputer,EditorType,Editor,ShowCommentLink
FoundErr=False
ItemID=Trim(Request("ItemID"))
Action=Trim(Request("Action"))

If ItemID="" Then
   FoundErr=True
   ErrMsg=ErrMsg & "<br><li>参数错误，项目ID不能为空！</li>"
Else
   ItemID=Clng(ItemID)
End If

If  FoundErr<>True  Then
      Call  GetTest()
End  If
If FoundErr<>True Then
   Call Main()
End If

'关闭数据库链接
Call CloseConn()
Call CloseConnItem()
%>
<%Sub Main
%>
<html>
<head>
<title>采集系统</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<link rel="stylesheet" type="text/css" href="../images/Admin_css.css">
<%Call SetChannel()%>
</head>
<body>
<table width="100%" border="0" align="center" cellpadding="3" cellspacing="2" class="admintable">
  <tr>
    <td height="30" class="b1_1">项目编辑 >> <a href="Admin_ItemModify.asp?ItemID=<%=ItemID%>">基本设置</a> >> <a href="Admin_ItemModify2.asp?ItemID=<%=ItemID%>">列表设置</a> >> <a href="Admin_ItemModify3.asp?ItemID=<%=ItemID%>">链接设置</a> >> <a href="Admin_ItemModify4.asp?ItemID=<%=ItemID%>">正文设置</a> >> <a href="Admin_ItemModify5.asp?ItemID=<%=ItemID%>">采样测试</a> >> <a href="Admin_ItemAttribute.asp?ItemID=<%=ItemID%>"><font color=red>属性设置</font></a> >> 完成</td>
  </tr>
</table>
<table width="100%" border="0" align="center" cellpadding="3" cellspacing="2" bgcolor="#FFFFFF" class="admintable">
<form method="post" action="Admin_ItemSuccess.asp" name="myform">
    <tr> 
      <td height="30" colspan="2" class="admintitle">编辑项目--属性设置</td>
    </tr>
    <tr> 
      <td width="20%" height="30" align="right" class="b1_1"><b>项目名称：</b></td>
      <td class="b1_1"><input name='ItemName' type='text' id='ItemName' value='<%=ItemName%>' size='27' maxlength='30'></td>
    </tr>
    <tr> 
      <td width="20%" height="30" align="right" class="b1_1"><strong> 所属栏目：</strong></td>
      <td class="b1_1"><select ID="ClassID" name="ClassID"><%call Admin_ShowChannel_Option(ClassID)%></select>      </td>
    </tr>   
    <tr>
      <td width="20%" height="30" align="right" class="b1_1"><b>点击数初始值：</b></td>
      <td class="b1_1"><input name="Hits" type="text" id="Hits" value="<%=Hits%>" size="8" maxlength="4"></td>
    </tr>
    <tr>
      <td width="20%" height="30" align="right" class="b1_1"><b>文章录入时间：</b></td>
      <td class="b1_1"><input name="UpdateType" type="radio" class="noborder" value="0" <%If UpdateType= 0 Then Response.write "checked"%>>当前时间
       &nbsp;<input name="UpdateType" type="radio" class="noborder" value="1" <%If UpdateType=1 Then Response.write "checked"%>>标签中的时间
       &nbsp;<input name="UpdateType" type="radio" class="noborder" value="2" <%If UpdateType= 2 Then Response.write "checked"%>>自定义：
       <input name="UpdateTime" type="text" value="<%=UpdateTime%>">
      　</td>
    </tr>
    <tr>
      <td width="20%" height="14" align="right" class="b1_1"><b>文章性质：</b></td>
      <td class="b1_1"><input name="OnTop" type="checkbox" class="noborder" value="yes" <%If OnTop=-1 Then Response.write "checked"%>>
        固顶文章        
        <input name="Hot" type="checkbox" class="noborder" value="yes" <%If Hot=-1 Then Response.write "checked"%>>
        推荐文章</td>
    </tr>
    <tr>
      <td height="30" class="b1_1"><b>分页设置：</b></td>
      <td class="b1_1"><select name="PaginationType">
        <option value="0" <%If PaginationType=0 Then response.write "selected"%>>不分页</option>
        <option value="1" <%If PaginationType=1 Then response.write "selected"%>>自动分页</option>
        <option value="2" <%If PaginationType=2 Then response.write "selected"%>>原文章分页</option>
      </select>
      自动分页字数:<input name="MaxCharPerPage" type="text" value="<%=MaxCharPerPage%>" size="8" maxlength="8">
<span class="note">如果不分页请保持为0</span></td>
    </tr>
    <tr>
      <td width="20%" height="30" align="right" class="b1_1"><b>标签过滤：</b></td>
      <td class="b1_1">
        <input name="Script_Iframe" type="checkbox" class="noborder" value="yes" <%If Script_Iframe=-1 Then response.write "checked"%>>
        Iframe
        <input name="Script_Object" type="checkbox" class="noborder" value="yes" <%If Script_Object=-1 Then response.write "checked"%>>
        Object
        <input name="Script_Script" type="checkbox" class="noborder" value="yes" <%If Script_Script=-1 Then response.write "checked"%>>
        Script
        <input name="Script_Div" type="checkbox" class="noborder"  value="yes" <%If Script_Div=-1 Then response.write "checked"%>>
        Div
        <input name="Script_Class" type="checkbox" class="noborder"  value="yes" <%If Script_Class=-1 Then response.write "checked"%>>
        Class
        <input name="Script_Table" type="checkbox" class="noborder"  value="yes" <%If Script_Table=-1 Then response.write "checked"%>>
        Table
        <input name="Script_Tr" type="checkbox" class="noborder"  value="yes" <%If Script_Tr=-1 Then response.write "checked"%>>
        Tr
        <br>
        <input name="Script_Span" type="checkbox" class="noborder"  value="yes" <%If Script_Span=-1 Then response.write "checked"%>>
        Span&nbsp;&nbsp;
        <input name="Script_Img" type="checkbox" class="noborder" value="yes" <%If Script_Img=-1 Then response.write "checked"%>>
        Img&nbsp;&nbsp;&nbsp;
        <input name="Script_Font" type="checkbox" class="noborder"  value="yes" <%If Script_Font=-1 Then response.write "checked"%>>
        Font&nbsp;&nbsp;
        <input name="Script_A" type="checkbox" class="noborder" value="yes" <%If Script_A=-1 Then response.write "checked"%>>
        A&nbsp;&nbsp;
        <input name="Script_Html" type="checkbox" class="noborder" onclick='return confirm("确定要选择该标记吗？这将删除正文中的所有Html标记，结果将导致该文章的可阅读性降低！");' value="yes" <%If Script_Html=-1 Then response.write "checked"%>>
        Html&nbsp;
        <input name="Script_Td" type="checkbox" class="noborder"  value="yes" <%If Script_Td=-1 Then response.write "checked"%>>
        Td&nbsp;&nbsp;&nbsp;
		<input name="Script_Tbody" type="checkbox" class="noborder"  value="yes" <%If Script_Tbody=-1 Then response.write "checked"%>>
        Tbody</td>
    </tr>
    <tr>
      <td width="20%" height="30" align="right" class="b1_1"> <b>列表深度：</b></td>
      <td class="b1_1">
        <input name="CollecListNum" type="text" id="CollecListNum" value="<%=CollecListNum%>" size="8" maxlength="8"> <span class="note">0为所有的列表</span></td>
    </tr>
    <tr>
      <td width="20%" height="30" align="right" class="b1_1"> <b>文章数量：</b></td>
      <td class="b1_1">
        <input name="CollecNewsNum" type="text" id="CollecNewsNum" value="<%=CollecNewsNum%>" size="8" maxlength="8"> <span class="note">0为所有的文章(每一列表的文章限制数量)</span></td>
    </tr>
    <tr>
      <td width="20%" height="30" align="right" class="b1_1"> <b>采集选项：</b></td>
      <td class="b1_1">
        <input name="Passed" type="checkbox" class="noborder" value="yes" <%if Passed=-1 then response.write "checked"%>>
        立即发布(既文章是否通过审核)
        <input name="SaveFiles" type="checkbox" class="noborder" value="yes" <%if SaveFiles=-1 then response.write "checked"%> <%If IsObjInstalled("Scripting.FileSystemObject")=False Then Response.Write "disabled"%>>
        保存图片
        <input name="CollecOrder" type="checkbox" class="noborder" value="yes" <%if CollecOrder=-1 then response.write "checked"%>>
        倒序采集</td>
    </tr>
    <tr>
    <td height="30" colspan="2" align="right" class="b1_1">
       <input type="hidden" value="<%=ItemID%>" name="ItemID">        
      <input type="submit" value=" 完&nbsp;&nbsp;成 " name="submit"></td>
    </tr>              
</form>         
</table>              
<!--#include file="../Admin_Copy.asp"-->   
</body>         
</html>
<%End Sub%>
<%
Sub GetTest()
   SqlItem="Select * from Item Where ItemID=" & ItemID
   Set RsItem=server.CreateObject("adodb.recordset")
   RsItem.Open SqlItem,ConnItem,1,1
   If  RsItem.Eof  And  RsItem.Bof  Then
      FoundErr=True
      ErrMsg=ErrMsg & "<br><li>参数错误，找不到该项目</li>"
   Else
      ItemName=RsItem("ItemName")      
      ClassID=RsItem("ClassID")
      SpecialID=RsItem("SpecialID")
      PaginationType=RsItem("PaginationType")
      MaxCharPerPage=RsItem("MaxCharPerPage")
      ReadLevel=RsItem("ReadLevel")
      Stars=RsItem("Stars")
      ReadPoint=RsItem("ReadPoint")
      Hits=RsItem("Hits")
      UpdateType=RsItem("UpdateType")
      UpdateTime=RsItem("UpdateTime")
      IncludePicYn=RsItem("IncludePicYn")
      DefaultPicYn=RsItem("DefaultPicYn")
      OnTop=RsItem("OnTop")
      Elite=RsItem("Elite")
      Hot=RsItem("Hot")
      SkinID=RsItem("SkinID")
      TemplateID=RsItem("TemplateID")
      Script_Iframe=RsItem("Script_Iframe")
      Script_Object=RsItem("Script_Object")
      Script_Script=RsItem("Script_Script")
      Script_Div=RsItem("Script_Div")
      Script_Class=RsItem("Script_Class")
      Script_Span=RsItem("Script_Span")
      Script_Img=RsItem("Script_Img")
      Script_Font=RsItem("Script_Font")
      Script_A=RsItem("Script_A")
      Script_Html=RsItem("Script_Html")
      Passed=RsItem("Passed")
      SaveFiles=RsItem("SaveFiles")
      CollecOrder=RsItem("CollecOrder")
      LinkUrlYn=RsItem("LinkUrlYn")
      CollecListNum=RsItem("CollecListNum")
      CollecNewsNum=RsItem("CollecNewsNum")
      InputerType=RsItem("InputerType")
      Inputer=RsItem("Inputer")
      EditorType=RsItem("EditorType")
      Editor=RsItem("Editor")
      ShowCommentLink=RsItem("ShowCommentLink")
      Script_Table=RsItem("Script_Table")
      Script_Tr=RsItem("Script_Tr")
      Script_Td=RsItem("Script_Td")
	  Script_Tbody=RsItem("Script_Tbody")
   End  If
   RsItem.Close
   Set RsItem=Nothing
End  Sub
%>
<%
Sub SetChannel
Dim Arr_Channel,i_Channel,i_Class,i_Special,tmpDepth,i,ArrShowLine(20)
Dim Rs,Sql,C_ClassName,C_SpecialName,C_ClassID
Set Rs=server.createobject("adodb.recordset")
Sql = "select ID from "&tbname&"_Class order by num ASC"
Rs.Open Sql,Conn,1,1
If Not Rs.Eof Then
   Arr_Channel=Rs.GetRows()
End If
Rs.Close
Set Rs=Nothing

If IsArray(Arr_Channel)= True then
   i_Class=0
   i_Special=0
   For i=0 To Ubound(ArrShowLine)
      ArrShowLine(i)=False
   Next
End if
End sub
%>