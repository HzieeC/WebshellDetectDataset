<!--#include file="inc/conn.asp"-->
<!--#include file="inc/function.asp"-->
<%
Dim SqlItem,RsItem,Action,ItemID,FilterName,FilterID,FilterObject,FilterType,FilterContent,FisString,FioString,FilterRep,Flag,PublicTf
Dim FoundErr,ErrMsg
Action=Trim(Request("Action"))
FilterID=Trim(Request("FilterID"))

If FilterID="" Then
   FoundErr=True
   ErrMsg=ErrMsg & "<br><li>参数错误，请从有效链接进入</li>" 
Else
   FilterID=Clng(FilterID)
End If
If Action="SaveEdit" and FoundErr<>True Then
   Call SaveEdit
Else
   Call Test()
End If
If FoundErr<>True Then
   Call Main()
Else
   Call WriteErrMsg(ErrMsg)
End If
'关闭数据库链接
Call CloseConn()
Call CloseConnItem()
%>
<%
Sub Main
%>
<html>
<head>
<title>采集系统</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<link rel="stylesheet" type="text/css" href="../images/Admin_css.css">
<SCRIPT language=javascript>
function showset(thisform)
{
		if(thisform.FilterType.selectedIndex==1)
		{
        	FilterType1.style.display = "none";
			FilterType2.style.display = "";
		}
		else
		{
        	FilterType1.style.display = "";
			FilterType2.style.display = "none";
	    }

}
</script>
</head>
<body>
<form method="post" action="Admin_ItemFilterModify.asp" name="form1">
<table width="100%" border="0" align="center" cellpadding="3" cellspacing="2" bgcolor="#FFFFFF" class="admintable" >                                
    <tr>                                 
      <td colspan="2" class="admintitle">编辑过滤</td>                                
    </tr>                                
    <tr>                                 
      <td width="150" class="b1_1"><strong> 过滤名称：</strong></td>                                
      <td class="b1_1"><input name="FilterName" type="text" id="FilterName" value="<%=FilterName%>" size="25" maxlength="30">                                 
      &nbsp;</td>                                
    </tr>                                
    <tr>                                 
      <td width="150" class="b1_1"><strong> 所属项目：</strong></td>                                
      <td class="b1_1"><%Call Admin_ShowItem_Option(ItemID)%>      </td>                                
    </tr>                                
    <tr>                                 
      <td width="150" class="b1_1"><strong>过滤对象：</strong></td>                               
      <td class="b1_1">                               
         <select name="FilterObject" id="FilterObject">                              
            <option value="1" <%if FilterObject=1 Then Response.Write "selected"%>>标题过滤</option>                              
            <option value="2" <%if FilterObject=2 Then Response.Write "selected"%>>正文过滤</option>                              
         </select>      </td>                               
    </tr>                               
    <tr>                                
      <td width="150" class="b1_1"><strong> 过滤类型：</strong></td>                               
      <td class="b1_1">                               
         <select name="FilterType" id="FilterType" onchange=showset(this.form)>                              
            <option value="1" <%if FilterType=1 Then Response.Write "selected"%> >简单替换</option>                              
            <option value="2" <%if FilterType=2 Then Response.Write "selected"%>>高级过滤</option>                              
         </select>      </td>                                
    </tr>     
    <tr>                                
      <td width="150" class="b1_1"><strong> 使用状态：</strong></td>                               
      <td class="b1_1">                               
         <select name="Flag" id="Flag">                              
            <option value="yes" <%If Flag=True Then Response.Write "selected"%>>启用</option>                              
            <option value="no" <%If Flag=False Then Response.Write "selected"%>>禁用</option>                              
         </select>      </td>                                
    </tr>                                     
    <tr>                                
      <td width="150" class="b1_1"><strong> 使用范围：</strong></td>                               
      <td class="b1_1">                               
         <select name="PublicTf" id="PublicTf">                              
            <option value="yes" <%If PublicTf=True Then Response.Write "selected"%>>公有</option>                            
            <option value="no" <%If PublicTf=False Then Response.Write "selected"%>>私有</option>                            
         </select>      </td>                                
    </tr>
    <tr id="FilterType1" style="display:<%if FilterType<>1 Then Response.Write "none"%>">
      <td class="b1_1"><strong>内容：</strong></td>
      <td class="b1_1"><textarea name="FilterContent" cols="49" rows="5"><%=FilterContent%></textarea></td>
    </tr>
    <tr id="FilterType2" style="display:<%if FilterType<>2 Then Response.Write "none"%>">
      <td colspan="2" class="b1_1"><table width="100%" border="0" align="center" cellpadding="0" cellspacing="0">
  <tr>
    <td class="b1_1"><strong>开始标记：</strong></td>
    <td class="b1_1"><textarea name="FisString" cols="49" rows="5"><%=FisString%></textarea></td>
  </tr>
  <tr>
    <td class="b1_1"><strong>结束标记：</strong></td>
    <td class="b1_1"><textarea name="FioString" cols="49" rows="5"><%=FioString%></textarea></td>
  </tr>
</table></td>
    </tr>
    
    <tr>
      <td class="b1_1"><strong>替换：</strong></td>
      <td class="b1_1"><textarea name="FilterRep" cols="49" rows="5" id="FilterRep"><%=FilterRep%></textarea></td>
    </tr>
    <tr>
      <td class="b1_1">&nbsp;</td>
      <td class="b1_1"><input name="Action" type="hidden" id="Action" value="SaveEdit">
        <input name="FilterID" type="hidden" id="FilterID" value="<%=FilterID%>">
        <input name="Cancel" type="button" id="Cancel" value="返&nbsp;&nbsp;回" onClick="window.location.href='Admin_ItemFilters.asp'">
&nbsp;
<input  type="submit" name="Submit" value="确&nbsp;&nbsp;定"></td>
    </tr>                                     
</table>                                
</form>
<!--#include file="../Admin_Copy.asp"-->           
</body>         
</html>
<%End Sub%>

<%
Sub SaveEdit
FilterID=Trim(Request("FilterID"))
FilterName=Trim(Request.Form("FilterName"))
ItemID=Trim(Request.Form("ItemID"))
FilterObject=Trim(Request.Form("FilterObject"))
FilterType=Trim(Request.Form("FilterType"))
FilterContent=Request.Form("FilterContent")
FisString=Request.Form("FisString")
FioString=Request.Form("FioString")
FilterRep=Request.Form("FilterRep")
Flag=Trim(Request.Form("Flag"))
PublicTf=Trim(Request.Form("PublicTf"))

If FilterID="" Then                                
   FoundErr=True                                
   ErrMsg=ErrMsg & "<br><li>参数错误！请从有效链接进入</li>"
Else
   FilterID=Clng(FilterID)                                 
End If                                

If FilterName="" Then                                
   FoundErr=True                                
   ErrMsg=ErrMsg & "<br><li>过滤名称不能为空</li>"                                 
End If                                
If ItemID="" Then                                
   FoundErr=True                                
   ErrMsg=ErrMsg & "<br><li>请选择过滤所属项目</li>"                                 
Else                                
   ItemID=Clng(ItemID)  
   If ItemID=0 Then  
      FoundErr=True                                
      ErrMsg=ErrMsg & "<br><li>请选择过滤所属项目</li>"   
   End If                                
End If       
If FilterObject="" Then                                
   FoundErr=True                                
   ErrMsg=ErrMsg & "<br><li>请选择过滤对象</li>"                                 
Else                                
   FilterObject=Clng(FilterObject)   
End If                                
                            
If FilterType="" Then                                
   FoundErr=True                                
   ErrMsg=ErrMsg & "<br><li>请选择过滤类型</li>"                                 
Else                                
   FilterType=Clng(FilterType)                                
   If FilterType=1 Then     
      If FilterContent="" Then                              
         FoundErr=True                                
         ErrMsg=ErrMsg & "<br><li>过滤的内容不能为空</li>"   
      End If   
   ElseIf FilterType=2 Then   
      If FisString="" or FioString="" Then   
         FoundErr=True                                
         ErrMsg=ErrMsg & "<br><li>开始/结束标记不能为空</li>"   
      End If   
   Else   
      FoundErr=True                                
      ErrMsg=ErrMsg & "<br><li>参数错误，请从有效链接进入</li>"   
   End If   
End If                     
If Flag="yes" Then
   Flag=True
Else
   Flag=False
End If
If PublicTf="yes" Then
   PublicTf=True
Else
   PublicTf=False
End If 

If FoundErr<>True Then                                
   SqlItem ="select top 1 *  from Filters Where FilterID=" & FilterID                                
   Set RsItem=server.CreateObject("adodb.recordset")                                
   RsItem.open SqlItem,ConnItem,2,3                                                              
   RsItem("FilterName")=FilterName                                
   RsItem("ItemID")=ItemID   
   RsItem("FilterObject")=FilterObject                                
   RsItem("FilterType")=FilterType                                
   If FilterType=1 Then                                
      RsItem("FilterContent")=FilterContent                                
   ElseIf FilterType=2 Then                                
      RsItem("FisString")=FisString                                
      RsItem("FioString")=FioString                                
   End If                                                
   RsItem("FilterRep")=FilterRep        
   RsItem("Flag")=Flag   
   RsItem("PublicTf")=PublicTf                        
   RsItem.Update                                
   RsItem.Close                                
   Set RsItem=Nothing                                
   Response.Redirect "Admin_ItemFilters.asp"                                
Else                                
   Call WriteErrMsg(ErrMsg)                                
End If                  
End Sub

Sub Test
Set RsItem=server.createobject("adodb.recordset")         
SqlItem="select * from Filters Where FilterID=" & FilterID         
RsItem.open SqlItem,ConnItem,1,1      
If Not RsItem.Eof Then
   ItemID=RsItem("ItemID")
   FilterName=RsItem("FilterName")
   FilterObject=RsItem("FilterObject")
   FilterType=RsItem("FilterType")
   FilterContent=RsItem("FilterContent")
   FisString=RsItem("FisString")
   FioString=RsItem("FioString")
   FilterRep=RsItem("FilterRep")
   Flag=RsItem("Flag")
   PublicTf=RsItem("PublicTf")
Else
   FoundErr=True
   ErrMsg=ErrMsg & "<br></li>参数错误，找不到该项目</li>"
End If
RsItem.Close
Set RsItem=Nothing
End Sub
%>