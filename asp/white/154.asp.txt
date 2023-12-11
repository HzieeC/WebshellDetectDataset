<!--#include file="inc/conn.asp"-->
<!--#include file="inc/function.asp"-->
<%
Dim SqlItem,RsItem
Dim Action,FoundErr,ErrMsg
Dim FilterID,ItemID,FilterName,FilterObject,FilterType,Flag,PublicTf,FlagName
Dim CurrentPage,AllPage,iItem,ItemNum
Const MaxPerPage=20
Action=Request("Action")
If Action="SetFlag" Then
   Call SetFlag()
End If
If FoundErr=True Then
   Call WriteErrMsg(ErrMsg)
Else
   Call Main()
End If
'关闭数据库链接
Call CloseConn()
Call CloseConnItem()
%>
<%Sub Main%>
<html>
<head>
<title>采集系统</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<link rel="stylesheet" type="text/css" href="../images/Admin_css.css">
<style type="text/css">
.ButtonList {
	BORDER-RIGHT: #000000 2px solid; BORDER-TOP: #ffffff 2px solid; BORDER-LEFT: #ffffff 2px solid; CURSOR: default; BORDER-BOTTOM: #999999 2px solid; BACKGROUND-COLOR: #e6e6e6
}
</style>
<SCRIPT language=javascript>
    function unselectall(thisform){
        if(thisform.chkAll.checked){
            thisform.chkAll.checked = thisform.chkAll.checked&0;
        }   
    }
    function CheckAll(thisform){
        for (var i=0;i<thisform.elements.length;i++){
            var e = thisform.elements[i];
            if (e.Name !="chkAll"&&e.disabled!=true)
                e.checked = thisform.chkAll.checked;
        }
    }
</script>
</head>
<body>
<table width="100%" border="0" cellpadding="3" cellspacing="2" bgcolor="#FFFFFF" class="admintable">
<form name="myform" method="POST" action="Admin_ItemFilters.asp">

        <TR>
          <TD class=ButtonList width="5%">
            选择</TD>
          <TD class=ButtonList width="15%">
            过滤名称</TD>
          <TD class=ButtonList width="15%">
            所属项目</TD>
          <TD class=ButtonList width="15%">
            过滤对象</TD>
          <TD class=ButtonList width="15%">
            过滤类型</TD>
          <TD class=ButtonList width="10%">
            状态</TD>
          <TD class=ButtonList>
            操作</TD>
        </TR>
<%
If Request("page")<>"" then
    CurrentPage=Cint(Request("Page"))
Else
    CurrentPage=1
End if      
set RsItem=server.createobject("adodb.recordset")         
SqlItem="select * from Filters order by FilterID DESC"         
RsItem.open SqlItem,ConnItem,1,1
If (Not RsItem.Eof) and (Not RsItem.Bof) Then

 RsItem.PageSize=MaxPerPage
 Allpage=RsItem.PageCount
 If Currentpage>Allpage Then Currentpage=1
 ItemNum=RsItem.RecordCount
 RsItem.MoveFirst
 RsItem.AbsolutePage=CurrentPage
 iItem=0
   Do while Not RsItem.Eof
      FilterID=RsItem("FilterID")
      ItemID=RsItem("ItemID")
      FilterName=RsItem("FilterName")
      FilterObject=RsItem("FilterObject")
      FilterType=RsItem("FilterType")
      Flag=RsItem("Flag")
      PublicTf=RsItem("PublicTf")
%>
        <TR>
          <TD align=center class="b1_1"><input name="FilterID" type="checkbox" class="noborder" onClick="unselectall(this.form)" value="<%=FilterID%>"></TD>
          <TD align="center" class="b1_1"><%=FilterName%></TD>
          <TD align="center" class="b1_1"><%Call Admin_ShowItem_Name(ItemID)%></TD>
          <TD  align="center" class="b1_1">
          <%
          If FilterObject=1 Then
             Response.Write "标题过滤" 
          ElseIf FilterObject=2 Then 
             Response.Write "正文过滤" 
          Else
             Response.Write "<font color=red>没有选择！</font>" 
          End If
          %></TD>
          <TD align="center" class="b1_1">
          <%
          If FilterType=1 Then
             Response.Write "简单替换"
          ElseIf FilterType=2 Then
             Response.Write "高级过滤"
          Else
             Response.Write "<font color=red>没有选择！</font>"
          End If
          %></TD>
          <TD align="center" class="b1_1">
          <%If Flag=True Then
               Response.Write "启用"
            Else
               Response.Write "<font color=red>禁用</font>"
            End If
          %>&nbsp;
          <%If PublicTf=False Then
               Response.Write "私有"
            Else
               Response.Write "<font color=red>公有</font>"
            End If
          %></TD>
          <TD align="center" class="b1_1">
          <%Response.Write "<a href='Admin_ItemFilters.asp?Action=SetFlag&FlagName=Passed&FilterID=" & FilterID & "'>"
            If Flag=True Then
               Response.Write "禁用"
            Else
               Response.Write "启用"
            End If
            Response.Write "</a>"
           %>&nbsp;
          <%Response.Write "<a href='Admin_ItemFilters.asp?Action=SetFlag&FlagName=Public&FilterID=" & FilterID & "'>"
            If PublicTf=False Then
               Response.Write "公有"
            Else
               Response.Write "私有"
            End If
            Response.Write "</a>"
           %>&nbsp;
             <a href="Admin_ItemFilterModify.asp?FilterID=<%=RsItem("FilterID")%>">修改</a>
             &nbsp;<a href="Admin_ItemFilters.asp?Action=SetFlag&FlagName=Del&FilterID=<%=RsItem("FilterID")%>" onclick='return confirm("确定要删除此记录吗？");'>删除</a></td>
        </TR>
<%    iItem=iItem+1
      If iItem>=MaxPerPage Then Exit Do
      RsItem.MoveNext
   Loop 
%>
    <tr> 
      <td height="30" colspan=7 class="b1_1">  
        <input name="Action" type="hidden"  value="SetFlag">
        &nbsp;<input name="chkAll" type="checkbox" class="noborder" id="chkAll" onclick=CheckAll(this.form) value="checkbox" >
        全选      
        <input type="submit" value="删&nbsp;&nbsp;除" name="FlagName" onClick='return confirm("确定要执行删除操作吗？该操作不可恢复！");'>
        &nbsp;&nbsp;
        <input type="submit" value="启&nbsp;&nbsp;用" name="FlagName" onClick='return confirm("确定要启用所选择的项目吗？");'>
        &nbsp;&nbsp;
        <input type="submit" value="禁&nbsp;&nbsp;用" name="FlagName" onClick='return confirm("确定要禁用所选择的项目吗？");'>
        &nbsp;&nbsp;
        <input type="submit" value="公&nbsp;&nbsp;有" name="FlagName" onClick='return confirm("确定要将所选择的项目设为公有吗？");'>
        &nbsp;&nbsp;
        <input type="submit" value="私&nbsp;&nbsp;有" name="FlagName" onClick='return confirm("确定要将所选择的项目设为私有吗？");'></td>
    </tr>
    <tr>
      <td height="14" colspan=9 align="center" class="b1_1"><%
Response.Write ShowPage("Admin_ItemFilters.asp",ItemNum,MaxPerPage,True,True," 个记录")
%></td>
    </tr>
<%Else%>
<tr>
        <td colspan='9' align="center"><br>系统中暂无过滤记录！</td>
</tr> 
<%End If
 RsItem.Close
Set RsItem=Nothing
%>
</form>  
</table>
<!--#include file="../Admin_Copy.asp"-->
</body>
</html>
<%end sub%>
<%
Sub  SetFlag
   FilterID=Trim(Request("FilterID"))
   FlagName=Trim(Request("FlagName"))
   If FilterID<>"" Then
      FilterID=Replace(FilterID," ","")
   Else
      FoundErr=True
      ErrMsg=ErrMsg & "<br><li>请选择要执行操作的记录！</li>"
   End if
   If FoundErr<>True Then
      Select Case FlagName
      Case "Del","删&nbsp;&nbsp;除"
         SqlItem="Delete from [Filters] Where FilterID In(" & FilterID & ")"
      Case "Public"
         SqlItem="Update [Filters] set PublicTf=Not PublicTf Where FilterID In(" & FilterID & ")"
      Case "公&nbsp;&nbsp;有"
         SqlItem="Update [Filters] set PublicTf=True Where FilterID In(" & FilterID & ")"
      Case "私&nbsp;&nbsp;有"
         SqlItem="Update [Filters] set PublicTf=False Where FilterID In(" & FilterID & ")"
      Case "Passed"
         SqlItem="Update [Filters] set Flag=Not Flag Where FilterID In(" & FilterID & ")"
      Case "启&nbsp;&nbsp;用"
         SqlItem="Update [Filters] set Flag=True Where FilterID In(" & FilterID & ")"
      Case "禁&nbsp;&nbsp;用"
         SqlItem="Update [Filters] set Flag=False Where FilterID In(" & FilterID & ")"
      End Select
      ConnItem.Execute(SqlItem)
   End If
End  Sub
%>