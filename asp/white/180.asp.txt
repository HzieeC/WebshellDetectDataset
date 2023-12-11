<!--#include file="inc/conn.asp"-->
<!--#include file="inc/function.asp"-->
<html>
<head>
<title>采集系统</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<link rel="stylesheet" type="text/css" href="../images/Admin_css.css">
</head>
<body leftmargin="2" topmargin="0" marginwidth="0" marginheight="0">
<%
Dim Action,ItemID,Arr_Item,Arr_i,ItemName,Arr_Filter
Action=Trim(Request("Action"))
ItemID=Trim(Request("ItemID"))
FoundErr=False

If Action<>"Copy" Then
   FoundErr=True
   ErrMsg=ErrMsg & "<br><li>参数错误，请从有效链接进入</li>"
End if   
If ItemID=""  Then
   FoundErr=True
   ErrMsg=ErrMsg & "<br><li>参数错误，请从有效链接进入</li>"
Else
   ItemID=Clng(ItemID)
End If

If FoundErr<>True Then
   Set Rs=ConnItem.execute("Select * from Item Where ItemID=" & ItemID)
   If Not Rs.Eof And Not Rs.Bof Then
      Arr_Item=Rs.GetRows()
   Else
      Arr_Item=""
   End if
   Set Rs=Nothing
   
   If IsArray(Arr_Item)=True Then
      set rs=server.createobject("adodb.recordset")
      sql="select top 1 * from Item" 
      rs.open sql,connItem,1,3
      rs.AddNew
         rs(1)=Arr_Item(1,0)&"复件"
         ItemName=Arr_Item(1,0)
         For Arr_i=2 To Ubound(Arr_Item,1)
            rs(Arr_i)=Arr_Item(Arr_i,0)
         Next
         ItemID=rs("ItemID")
      rs.Update
      rs.close
      set rs=Nothing

      If IsArray(Arr_Filter)=True Then
         set rs=server.createobject("adodb.recordset")
         sql="select top 1 * from Filters" 
         rs.open sql,connItem,1,3
         rs.AddNew
            rs(1)=ItemID
            For Arr_i=2 To Ubound(Arr_Filter,1)
               rs(Arr_i)=Arr_Filter(Arr_i,0)
            Next
         rs.Update
         rs.close
         set rs=Nothing
      End if      
   else
      FoundErr=True
      ErrMsg=ErrMsg & "参数错误，没有找到该项目"
   end if
   
End if

Call Main  
'关闭数据库链接

Sub Main%>
<table width="100%" border="0" align="center" cellpadding="3" cellspacing="2" class="admintable">
  <tr>
    <td height="30" class="b1_1" style="text-align:center;padding:50px;"><%=ItemName%> 项目复制完成，新的项目保存为：<%=ItemName%>复件<br><br><a href="Admin_ItemStart.asp">返回</a></td>
  </tr>
</table>
<%End Sub%>
<!--#include file="../Admin_Copy.asp"--> 
</body>         
</html>