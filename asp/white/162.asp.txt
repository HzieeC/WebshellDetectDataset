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
<table width="100%" border="0" align="center" cellpadding="3" cellspacing="2" bgcolor="#FFFFFF" class="admintable">
    <tr> 
      <td height="22" colspan="7" class="admintitle">采集项目管理</td>
    </tr>
  <form name="myform" method="POST" action="Admin_ItemManage.asp">
    <tr style="padding: 0px 2px;">
      <td width="5%" height="30" align="center" class=ButtonList>选择</td>
      <td width="10%" align="center" class=ButtonList>项目名称</td>
      <td width="20%" align="center" class=ButtonList>采集地址</td>
      <td width="15%" align="center" class=ButtonList>所属栏目</td>
      <td width="5%" align="center" class=ButtonList>状态</td>
      <td width="20%" align="center" class=ButtonList>上次采集</td>
      <td width="25%" align="center" class=ButtonList>操作</td>
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
    <tr bgcolor="#f1f3f5" onMouseOver="this.style.backgroundColor='#EAFCD5';this.style.color='red'" onMouseOut="this.style.backgroundColor='';this.style.color=''">
      <td align="center">
      <input name="ItemID" type="checkbox" class="noborder" onClick="unselectall(this.form)" value="<%=ItemID%>"></td>
      <td align="center"><%=ItemName%></td>
      <td align="center"><a href="<%=ListUrl%>" target="_bank"><%=WebName%></a></td>
      <td align="center"><%Call Admin_ShowChannel_Name(ClassID)%></td>
      <td align="center"> <b>
        <%If Flag=True then
                    Response.write "√"
          Else
                 Response.write "<font color=red>×</font>"
          End If
        %>
      </b> </td>
      <td align="center">
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
       %>      </td>
      <td align="center"><a href=Admin_Itemcopy.asp?Action=Copy&ItemID=<%=ItemID%>>复制</a> <a href=Admin_ItemModify.asp?ItemID=<%=ItemID%>>修改</a> <a href=Admin_ItemModify5.asp?ItemID=<%=ItemID%>>测试</a> <a href=Admin_ItemManage.asp?Action=Del&ItemID=<%=ItemID%> onClick='return confirm("确定要删除此项目吗？请您慎重选择！这将删除该项目的项目信息，历史记录及过滤信息 3 个项目类型数据。");'>删除</a> <a href=Admin_ItemHistroly.asp?Action=search&ItemID=<%=ItemID%>>历史</a></td>
    </tr>
    <%
      iItem=iItem+1
      If iItem>=MaxPerPage Then  Exit  Do
      RsItem.MoveNext
   Loop
%>
    <tr>
      <td height="30" align="center" bgcolor="#F7F7F7">
        <input name="Action" type="hidden"  value="Del">
        <input name="chkAll" type="checkbox" class="noborder" id="chkAll" onClick=CheckAll(this.form) value="checkbox" ></td>
      <td height="30" colspan=8 bgcolor="#F7F7F7">全选
        <input type="submit" value=" 删&nbsp;&nbsp;除 " name="Del" onClick='return confirm("确定要删除选中的项目吗？请您慎重选择！这将删除该项目的项目信息，历史记录及过滤信息 3 个项目类型数据。");'>
&nbsp;&nbsp;
<input type="submit" value="清空所有记录" name="Del" onClick='return confirm("您真的要确定要清空所有项目吗？这将彻底格式化采集数据库的所有信息，请您先备份再选择！！！");'>
&nbsp;&nbsp;</td>
    </tr>
	 <tr>
      <td colspan='9' align="center" bgcolor="#F7F7F7"><%Response.Write ShowPage("Admin_ItemManage.asp",ItemNum,MaxPerPage,True,True," 个项目")%></td>
    </tr>
    <%Else%>
    <tr>
      <td colspan='9' align="center" bgcolor="#F7F7F7">
      系统中暂无采集项目！</td>
    </tr>
    <%End If
RsItem.Close
Set  RsItem=Nothing
%>
  </form>
</table>
<!--#include file="../Admin_Copy.asp"-->
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