<%@language=vbscript codepage=936 %>
<%
option explicit
response.buffer=true	
%>
<!--#include file="inc/conn.asp"-->
<!--#include file="inc/function.asp"-->
<%
Dim SqlItem,RsItem,FoundErr,ErrMsg
Dim ItemID,ItemName,ClassID,strChannelDir,SpecialID,PaginationType,MaxCharPerPage,ReadLevel
Dim Stars,ReadPoint,Hits,UpDateType,UpDateTime,IncludePicYn,DefaultPicYn,OnTop,Elite,Hot,SkinID,TemplateID
Dim Script_Iframe,Script_Object,Script_Script,Script_Div,Script_Class,Script_Span,Script_Img,Script_Font,Script_A,Script_Html,Script_Table,Script_Tr,Script_Td,Script_Tbody
Dim CollecListNum,CollecNewsNum,Passed,SaveFiles,CollecOrder,LinkUrlYn,InputerType,Inputer,EditorType,Editor,ShowCommentLink
Dim tClass,tSpecial
FoundErr=False
ItemID=Request("ItemID")

If ItemID="" Then
   FoundErr=True
   ErrMsg=ErrMsg & "<br><li>参数错误，请从有效链接进入</li><a href='javascript:history.go(-1)'>&lt;&lt; 返回上一页</a>"
Else
   ItemID=Clng(ItemID)
End If

If FoundErr<>True Then
   ItemName=Trim(Request.Form("ItemName"))
   ClassID=Trim(Request.Form("ClassID"))
   ClassID=Trim(Request.Form("ClassID"))
   SpecialID=Trim(Request.Form("SpecialID"))
   PaginationType=Trim(Request.Form("PaginationType"))
   MaxCharPerPage=Trim(Request.Form("MaxCharPerPage"))
   ReadLevel=Trim(Request.Form("ReadLevel"))
   Stars=Trim(Request.Form("Stars"))
   ReadPoint=Trim(Request.Form("ReadPoint"))
   Hits=Trim(Request.Form("Hits"))
   UpdateType=Trim(Request.Form("UpdateType"))
   UpDateTime=Trim(Request.Form("UpDateTime"))
   IncludePicYn=Trim(Request.Form("IncludePicYn"))
   DefaultPicYn=Trim(Request.Form("DefaultPicYn"))
   OnTop=Trim(Request.Form("OnTop"))
   Elite=Trim(Request.Form("Elite"))
   Hot=Trim(Request.Form("Hot"))
   SkinID=Trim(Request.Form("SkinID"))
   TemplateID=Trim(Request.Form("TemplateID"))
   Script_Iframe=Trim(Request.Form("Script_Iframe"))
   Script_Object=Trim(Request.Form("Script_Object"))
   Script_Script=Trim(Request.Form("Script_Script"))
   Script_Div=Trim(Request.Form("Script_Div"))
   Script_Class=Trim(Request.Form("Script_Class"))
   Script_Span=Trim(Request.Form("Script_Span"))
   Script_Img=Trim(Request.Form("Script_Img"))
   Script_Font=Trim(Request.Form("Script_Font"))
   Script_A=Trim(Request.Form("Script_A"))
   Script_Html=Trim(Request.Form("Script_Html"))
   CollecListNum=Trim(Request.Form("CollecListNum"))
   CollecNewsNum=Trim(Request.Form("CollecNewsNum"))
   Passed=Trim(Request.Form("Passed"))
   SaveFiles=Trim(Request.Form("SaveFiles"))
   CollecOrder=Trim(Request.Form("CollecOrder"))
   LinkUrlYn=Trim(Request.Form("LinkUrlYn"))
   InputerType=Trim(Request.Form("InputerType"))
   Inputer=Trim(Request.Form("Inputer"))
   EditorType=Trim(Request.Form("EditorType"))
   Editor=Trim(Request.Form("Editor"))
   ShowCommentLink=Trim(Request.Form("ShowCommentLink"))
   Script_Table=Trim(Request.Form("Script_Table"))
   Script_Tr=Trim(Request.Form("Script_Tr"))
   Script_Td=Trim(Request.Form("Script_Td"))
   Script_Tbody=Trim(Request.Form("Script_Tbody"))

   If ItemName="" Then
      FoundErr=True
      ErrMsg=ErrMsg & "<br><li>项目名称不能为空！</li>"
   End If
   If ClassID="" or ClassID=0 Then
      FoundErr=True
      ErrMsg=ErrMsg & "<br><li>请选择项目所属栏目！</li>"
   Else
      ClassID=Clng(ClassID)
   End If

   If Hits="" Then
      Hits=0
   Else
      Hits=Clng(Hits)
   End if
   If UpdateType="" Then
      FoundErr=True
      ErrMsg=ErrMsg & "<br><li>请选择文章录入时间类型！</li>"
   Else
      UpdateType=Clng(UpdateType)
      If UpDateType=2 Then
         If IsDate(UpdateTime)=False Then
            FoundErr=True
            ErrMsg=ErrMsg & "<br><li>文章录入时间格式不正确！</li>"
         Else
            UpDateTime=CDate(UpDateTime)
         End If
      End If
   End if
   If CollecListNum="" Then
      FoundErr=True
      ErrMsg=ErrMsg & "<br><li>请填写文章列表深度！</li>" 
   Else
      CollecListNum=Clng(CollecListNum)
   End If
   If CollecNewsNum="" Then
      FoundErr=True
      ErrMsg=ErrMsg & "<br><li>请填写文章采集数量！</li>" 
   Else
      CollecNewsNum=Clng(CollecNewsNum)
   End If

   If ShowCommentLink="Yes" Then
      ShowCommentLink=True
   Else
      ShowCommentLink=False
   End If
End If
If FoundErr<>True Then
   SqlItem="Select * from Item Where ItemID=" & ItemID
   Set RsItem=server.CreateObject("adodb.recordset")
   RsItem.Open SqlItem,ConnItem,2,3
   If RsItem.Eof And RsItem.Bof Then
      FoundErr=True
      ErrMsg=ErrMsg & "<br><li>参数错误，没有找该项目</li>"
   Else
      RsItem("ItemName")=ItemName
      RsItem("ClassID")=ClassID
      RsItem("Hits")=Hits
      RsItem("UpdateType")=UpdateType
      If UpdateType=2 Then
         RsItem("UpDateTime")=UpDateTime
      End If
      If IncludePicYn="yes" Then
         RsItem("IncludePicYn")=-1
      Else
         RsItem("IncludePicYn")=0
      End If
      If DefaultPicYn="yes" Then
         RsItem("DefaultPicYn")=-1
      Else
         RsItem("DefaultPicYn")=0
      End If
      If OnTop="yes" Then
         RsItem("OnTop")=-1
      Else
         RsItem("OnTop")=0
      End If
      If Hot="yes" Then
         RsItem("Hot")=-1
      Else
         RsItem("Hot")=0
      End If

      If Script_Iframe="yes" Then
         RsItem("Script_Iframe")=-1
      Else
         RsItem("Script_Iframe")=0
      End If
      If Script_Object="yes" Then
         RsItem("Script_Object")=-1
      Else
         RsItem("Script_Object")=0
      End If
      If Script_Script="yes" Then
         RsItem("Script_Script")=-1
      Else
         RsItem("Script_Script")=0
      End If
      If Script_Div="yes" Then
         RsItem("Script_Div")=-1
      Else
         RsItem("Script_Div")=0
      End If
      If Script_Class="yes" Then
         RsItem("Script_Class")=-1
      Else
         RsItem("Script_Class")=0
      End If
      If Script_Span="yes" Then
         RsItem("Script_Span")=-1
      Else
         RsItem("Script_Span")=0
      End If
      If Script_Img="yes" Then
         RsItem("Script_Img")=-1
      Else
         RsItem("Script_Img")=0
      End If

      If Script_Font="yes" Then
         RsItem("Script_Font")=-1
      Else
         RsItem("Script_Font")=0
      End If
      If Script_A="yes" Then
         RsItem("Script_A")=-1
      Else
         RsItem("Script_A")=0
      End If

      If Script_Html="yes" Then
         RsItem("Script_Html")=-1
      Else
         RsItem("Script_Html")=0
      End If
      RsItem("CollecListNum")=CollecListNum
      RsItem("CollecNewsNum")=CollecNewsNum

      If Passed="yes" Then
         RsItem("Passed")=-1
      Else
         RsItem("Passed")=0
      End If
      If SaveFiles="yes" Then
         RsItem("SaveFiles")=-1
      Else
         RsItem("SaveFiles")=0
      End If
      If CollecOrder="yes" Then
         RsItem("CollecOrder")=-1
      Else
         RsItem("CollecOrder")=0
      End If
      If LinkUrlYn="yes" Then
         RsItem("LinkUrlYn")=-1
      Else
         RsItem("LinkUrlYn")=0
      End If
      RsItem("Flag")=True
      If Script_Table="yes" Then
         RsItem("Script_Table")=-1
      Else
         RsItem("Script_Table")=0
      End If
      If Script_Tr="yes" Then
         RsItem("Script_Tr")=-1
      Else
         RsItem("Script_Tr")=0
      End If
      If Script_Td="yes" Then
         RsItem("Script_Td")=-1
      Else
         RsItem("Script_Td")=0
      End If
	  If Script_Tbody="yes" Then
         RsItem("Script_Tbody")=-1
      Else
         RsItem("Script_Tbody")=0
      End If
	  
	  If MaxCharPerPage<>"" then
	  	RsItem("MaxCharPerPage")=MaxCharPerPage
	  else
	  	RsItem("MaxCharPerPage")=0
	  end if
	  	RsItem("PaginationType")=PaginationType

   End If
   ErrMsg="<br><li>" & ItemName & " 项目设置完成！</li>"
   RsItem.UpDate
   RsItem.Close
   Set RsItem=Nothing 
End If

If FoundErr=True Then
   Call WriteErrMsg(ErrMsg)
Else
   Call WriteSucced(ErrMsg)
End If
'关闭数据库链接
Call CloseConn()
Call CloseConnItem()
%>
