<%
Response.Buffer = true
On Error Resume Next
%>
<!--#include FILE="jsinc/uploadFileClass.asp"-->
<%
dim upload_code,remNum,remFileName,editImageNum,actionType,editRemNum

upload_code = trim(Request.QueryString("upload_code"))
editImageNum = trim(Request.QueryString("editImageNum"))
actionType = trim(Request.QueryString("actionType"))
picName = trim(Request.QueryString("picName"))
editRemNum = trim(Request.QueryString("editRemNum"))

if actionType= "mod" then
   picName = Right(picName,InstrRev(picName,"/")-1)
end if

if upload_code = "ok" then
  '获取文章中的图片    
    dim upload,file,formName,formPath,iCount
	formPath="../UploadFiles/"
	
    set upload=new upload_5xSoft ''建立上传对象
	for each formName in upload.file
	set file=upload.file(formName)  ''生成一个文件对象
	
	if file.FileName<>"" then
	   Dim getimageExt
	   Set getimageExt = CreateObject("Scripting.FileSystemObject")
	   GetAnExtension = getimageExt.GetExtensionName(file.FileName)
	   if GetAnExtension<>"jpg" and GetAnExtension<>"gif" and GetAnExtension<>"JPG" and GetAnExtension<>"GIF" then
%>
          <SCRIPT language=javascript>
          alert("文件格式不对,请重新上传");
          opener.document.all("id_uploadstr").style.display="none";
          window.close();
          </SCRIPT>
<%
	   else    	

	      if file.FileSize>0 then         ''如果 FileSize > 0 说明有文件数据 
	         '生成图片名字
	         if actionType= "mod" then
	            remFileName = Right(picName,len(picName)-InstrRev(picName,"/"))
	         else
	            if editRemNum<>"" then	               
	               remNum = editRemNum
	            else
	               Randomize
	               remNum = Int((999 - 1 + 1) * Rnd + 1)&day(date)&month(date)&year(date)&hour(time)&minute(time)&second(time)
	            end if
	            remFileName = remNum&"_"&(editImageNum+1)&".gif"
	         end if
	         
	         file.SaveAs Server.mappath(formPath&remFileName)   ''保存文件 
	  	   
%>
             <SCRIPT language=javascript>
             opener.document.all("imageFileName").value = "<%=remFileName%>";
             <%if actionType<> "mod" then%>
             opener.document.all("remNum").value = "<%=remNum%>";
             opener.document.all("editImageNum").value = "<%=(editImageNum+1)%>";
             <%end if%>
             opener.document.all("imageFileName").click();
             </SCRIPT>
<%
          end if
	   end if
	end if
	next
	set file=nothing 
	
    set upload=nothing  ''删除此对象
%>
<script language=javascript>window.close();</script>
<%
end if
%>
