<%@language=vbscript codepage=936 %>
<%
option explicit
response.buffer=true
%>
<!--#include file="inc/conn.asp"-->
<!--#include file="inc/function.asp"-->
<%
Dim SqlItem,RsItem,Rs,Sql
Dim Action,FoundErr,ErrMsg
Dim ItemID,ItemName,WebName,ClassID,SpecialID,ListStr,ListPaingType,ListPaingStr2,ListPaingID1,ListPaingID2,ListPaingStr3,Flag
Dim ListUrl,ItemCollecDate
Dim CurrentPage,AllPage,iItem,ItemNum
Const MaxPerPage=10
Action=Request("Action")
If Action="Del" Then
   Call Del()
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
            if (e.Name != "chkAll"&&e.disabled!=true)
                e.checked = thisform.chkAll.checked;
        }
    }
</script>
</head>
<body>
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="border">
  <tr class='topbg'> 
    <td height="22" colspan="2" align="center" ><strong>采 集 系 统 项 目 管 理</strong></td>
  </tr>
</table>
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="border">
  <tr> 
    <td width="65" height="30"><strong>管理导航：</strong></td>
    <td height="30"><a href=Admin_ItemManage.asp>管理首页</a> | <a href="Admin_ItemAddNew.asp">添加新项目</a></td>
  </tr>
</table>
<br>
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="border" >
    <tr> 
      <td height="22" colspan="2" class="title"> <div align="center"><strong>项  目  管  理</strong></div></td>
    </tr>
</table>
<table class="border" border="0" cellspacing="1" width="100%" cellpadding="0">
  <form name="myform" method="POST" action="Admin_ItemManage.asp">
    <tr style="padding: 0px 2px;">
      <td width="38" height="22" align="center" class=ButtonList>选择</td>
      <td width="141" align="center" class=ButtonList>项目名称</td>
      <td width="130" align="center" class=ButtonList>采集地址</td>
      <td width="120" height="22" align="center" class=ButtonList>所属栏目</td>
      <td width="120" height="22" align="center" class=ButtonList>所属栏目</td>
      <td width="43" align="center" class=ButtonList>状态</td>
      <td width="157" height="22" align="center" class=ButtonList>上次采集</td>
      <td width="148" height="22" align="center" class=ButtonList>操作</td>
    </tr>
    <%            
If Request("page")<>"" then
    CurrentPage=Cint(Request("Page"))
Else
    CurrentPage=1
End if                 
Set RsItem=server.createobject("adodb.recordset")         
SqlItem="select ItemID,ItemName,WebName,ListStr,ListPaingType,ListPaingStr2,ListPaingID1,ListPaingID2,ListPaingStr3,ClassID,SpecialID,Flag from Item order by ItemID DESC"         
RsItem.open SqlItem,ConnItem,1,1

if Not RsItem.Eof then
   RsItem.PageSize=MaxPerPage
   Allpage=RsItem.PageCount
   If Currentpage>Allpage Then Currentpage=1
   ItemNum=RsItem.RecordCount
   RsItem.MoveFirst
   RsItem.AbsolutePage=CurrentPage
   iItem=0
   Do While Not RsItem.Eof
      ItemID=RsItem("ItemID")
      ItemName=RsItem("ItemName")
      WebName=RsItem("WebName")
      ClassID=RsItem("ClassID")      
      ClassID=RsItem("ClassID")
      SpecialID=RsItem("SpecialID")
      ListStr=RsItem("ListStr")
      ListPaingType=RsItem("ListPaingType")
      ListPaingStr2=RsItem("ListPaingStr2")
      ListPaingID1=RsItem("ListPaingID1")
      ListPaingID2=RsItem("ListPaingID2")
      ListPaingStr3=RsItem("ListPaingStr3")
      Flag=RsItem("Flag")
      If  ListPaingType=0  Or ListPaingType=1  Then
            ListUrl=ListStr
      ElseIf  ListPaingType=2  Then
            ListUrl=Replace(ListPaingStr2,"{$ID}",CStr(ListPaingID1))
      ElseIf  ListPaingType=3  Then
            If  Instr(ListPaingStr3,"|")>0  Then
            ListUrl=Left(ListPaingStr3,Instr(ListPaingStr3,"|")-1)
         Else
               ListUrl=ListPaingStr3
         End  If
      End  If

%>
    <tr onMouseOut="this.style.backgroundColor=''" onMouseOver="this.style.backgroundColor='#BFDFFF'" style="padding: 0px 2px;">
      <td width="38" align="center">
        <input type="checkbox" value="<%=ItemID%>" name="ItemID" onClick="unselectall(this.form)">
      </td>
      <td width="141" align="center"><%=ItemName%></td>
      <td width="130" align="center"><a href="<%=ListUrl%>" target="_bank"><%=WebName%></a></td>
      <td width="120" height="22" align="center"><%Call Admin_ShowChannel_Name(ClassID)%></td>
      <td width="120" align="center"><%Call Admin_ShowClass_Name(ClassID,ClassID)%></td>
      <td width="43" align="center"> <b>
        <%If Flag=True then
                    Response.write "√"
          Else
                 Response.write "<font color=red>×</font>"
          End If
        %>
      </b> </td>
      <td width="157" align="center">
        <%
       Set Rs=connItem.execute("select Top 1 CollecDate From Histroly Where ItemID=" & ItemID & " Order by HistrolyID desc")
       If Not Rs.Eof Then
          ItemCollecDate=rs("CollecDate")
       Else
          ItemCollecDate=""
       End if
       Set Rs=Nothing
       if ItemCollecDate<>"" then
          Response.Write ItemCollecDate
       Else
          Response.Write "尚无记录"
       End If
       %>
      </td>
      <td width="148" align="center"><a href=Admin_Itemcopy.asp?Action=Copy&ItemID=<%=ItemID%>>复制</a> <a href=Admin_ItemAttribute.asp?ItemID=<%=ItemID%>>属性</a> <a href=Admin_ItemModify5.asp?ItemID=<%=ItemID%>>测试</a> <a href=Admin_ItemManage.asp?Action=Del&ItemID=<%=ItemID%> onClick='return confirm("确定要删除此项目吗？请您慎重选择！这将删除该项目的项目信息，历史记录及过滤信息 3 个项目类型数据。");'>删除</a></td>
    </tr>
    <%
      iItem=iItem+1
      If iItem>=MaxPerPage Then  Exit  Do
      RsItem.MoveNext
   Loop
%>
    <tr>
      <td colspan=9 height="30">
        <input name="Action" type="hidden"  value="Del">
&nbsp;
        <input name="chkAll" type="checkbox" id="chkAll" onClick=CheckAll(this.form) value="checkbox" >
        全选 </td>
    </tr>
    <tr>
      <td colspan=9 height="30" align=center> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
          <input type="submit" value=" 删&nbsp;&nbsp;除 " name="Del" onClick='return confirm("确定要删除选中的项目吗？请您慎重选择！这将删除该项目的项目信息，历史记录及过滤信息 3 个项目类型数据。");'>
  &nbsp;&nbsp;
          <input type="submit" value="清空所有记录" name="Del" onClick='return confirm("您真的要确定要清空所有项目吗？这将彻底格式化采集数据库的所有信息，请您先备份再选择！！！");'>
  &nbsp;&nbsp; </td>
    </tr>
    <%Else%>
    <tr>
      <td colspan='9' align="center"><br>
        系统中暂无采集项目！</td>
    </tr>
    <%End If
RsItem.Close
Set  RsItem=Nothing
%>
  </form>
</table>
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="border" >
    <tr> 
      <td height="22" colspan="2">
<%
Response.Write ShowPage("Admin_ItemManage.asp",ItemNum,MaxPerPage,True,True," 个项目")
%>

      </td>
    </tr>
</table>
</body>
</html>
<%end sub%>
<%Sub Del
ItemID=Trim(Request("ItemID"))
If Request("Del")="清空所有记录" Then
   ConnItem.Execute("Delete From Item")
   ConnItem.Execute("Delete From Filters")
   ConnItem.Execute("Delete From Histroly")
Else
   If ItemID="" Then
      FoundErr=True
      ErrMsg=ErrMsg & "<br><li>请选择要删除的项目！</li>"
   Else
      ItemID=Replace(ItemID," ","")   
      ConnItem.Execute("Delete From [Item] Where ItemID In(" & ItemID & ")")
      ConnItem.Execute("Delete From [Filters] Where ItemID In(" & ItemID & ")")
      ConnItem.Execute("Delete From [Histroly] Where ItemID In(" & ItemID & ")")
   End If
End  If
End Sub
%>