<%
actionType=trim(Request.QueryString("actionType"))
picName = trim(Request.QueryString("picName"))
editImageNum  = trim(Request.QueryString("editImageNum"))
editRemNum = trim(Request.QueryString("editRemNum"))
uploadPath = "../UploadFiles/"
%>
<HTML>
<HEAD>
<TITLE>添加/编辑图片</TITLE>
<LINK href="../css.css" type=text/css rel=stylesheet>

<script language=javascript>
function IsVote(loadpos)
{
    document.all("id_uploadstr").style.display="block"
	var newWindow = null
	
    if (! newWindow || newWindow.closed)
	{
	   newWindow=window.open(loadpos,"surveywin","scrollbars,dependent,width=10,height=10,top=50000,left=50000");
	}
}

function saveImage()
{    
    var returnStr;
    var  sData = dialogArguments;
    returnStr = "<img border=1 src=";
    
    if (document.all("imageFileName").value!="")
    {
        //更新editbox的新闻    
        if (document.all("editImageNum").value==1)
        {          
           sData.document.all("editFirstImageName").value = document.all("imageFileName").value;
        }
        
        if(document.all("actionType").value!="mod")
        {
           sData.document.all("editRemNum").value = document.all("remNum").value;
        }
        
        sData.document.all("editImageNum").value = document.all("editImageNum").value;
        
        //返回值
        returnStr += "<%=uploadPath%>"+document.all("imageFileName").value;
        
        if (document.all("widthvalue").value!="")
        {
            returnStr += " width="+document.all("widthvalue").value;
        }
        
        if (document.all("heightvalue").value!="")
        {
            returnStr += " height="+document.all("heightvalue").value;
        }
        
        returnStr += " align="+document.all("duiqiStr").value;
        
        returnStr += ">";
        
        window.returnValue = returnStr;        
        
        window.close();
    }
    else
       window.close();       
	
}

function checkDisplay()
{
   temp_allcolorradio = document.all("filetype");
   if (temp_allcolorradio[0].checked)
   {
		document.all("urlType").style.display = "none";
		document.all("uploadType").style.display = "block";
		document.all("tomodify").style.display = "none";
   }
   else if(temp_allcolorradio[1].checked)
   {
	    document.all("urlType").style.display = "block";
		document.all("uploadType").style.display = "none";
		document.all("tomodify").style.display = "none";
   }
   else
   {
        document.all("urlType").style.display = "none";
		document.all("uploadType").style.display = "none";
		document.all("tomodify").style.display = "block";
   }
}

function toInsertImage()
{
    var returnStr;
    returnStr = "<img border=1 src=";
    
    if ((document.all("imageUrl").value != null)&&(document.all("imageUrl").value!=="http://"))
    {	  
	    returnStr += document.all("imageUrl").value;
        
        if (document.all("widthvalue").value!="")
        {
            returnStr += " width="+document.all("widthvalue").value;
        }
        
        if (document.all("heightvalue").value!="")
        {
            returnStr += " height="+document.all("heightvalue").value;
        }
        
        returnStr += " align="+document.all("duiqiStr").value;
        
        returnStr += ">";
        
        window.returnValue = returnStr;
        window.close();
    }
}

function modifyImage()
{
    var returnStr;
    returnStr = "<img border=1 src=";
    
    if ((document.all("picName").value != null))
    {	  
	    returnStr += document.all("picName").value;
        
        if (document.all("widthvalue").value!="")
        {
            returnStr += " width="+document.all("widthvalue").value;
        }
        
        if (document.all("heightvalue").value!="")
        {
            returnStr += " height="+document.all("heightvalue").value;
        }
        
        returnStr += " align="+document.all("duiqiStr").value;
        
        returnStr += ">";
        
        window.returnValue = returnStr;
        window.close();
    }
}
</script>
</HEAD>

<BODY bgcolor="#EFEBDE" oncontextmenu="return false;">
<table width=100% border=0 cellspacing=0 cellpadding=0 align=center bgcolor="#EFEBDE" height="200">
  <tr> 
    <td valign=top> 
      <table width="100%" border="0" cellspacing="0" cellpadding="0" height="21">
	    <form name=setpro>
        <tr bgcolor="#b7d3ff"> 
          <td> 
            <div align="left">&nbsp;插入图片方法选择：
            <input type=radio name=filetype value=1 checked onclick="checkDisplay();">上传图片<input onclick="checkDisplay();" type=radio name=filetype value=2>图片URl            
            <%if actionType="mod" then%>
            <input onclick="checkDisplay();" type=radio name=filetype value=3 ID="Radio1">修改属性
            <%end if%>
            </div>
          </td>
        </tr>
		</form>
        <tr bgcolor="#000000"> 
          <td height="1"><space height=1 type=block></td>
        </tr>
      </table>
      <table width=100% border=0 cellspacing=0 cellpadding=4>
        <tr valign="top" align="center"> 
          <td colspan=2> 
            <!--上传中显示-->
            <div align="center" id="id_uploadstr" style="display:none">
			  <table width="85%" border="0" cellspacing="0" cellpadding="0" ID="Table1">
                <tr> 
                  <td class=tdblack>
                  <font style="font-size:10.5pt"><i><br><br>正在上传文件......</i></font><br><br><br><br>
                  <b>注意：</b>请不要关闭此窗口,在文件上传结束后，本窗口将自动关闭。上传可能需要几分钟，请耐心等候！<br><br><br><br><br><br> 
                  </td>
                </tr>
              </table>
			</div>
			<!--上传中显示-->				
			<table width="95%" border="0" cellspacing="0" cellpadding="0" ID="Table5"> 
			 <form name=modpro>
              <tr> 
                <td height="25" class=tdblack>
                对齐方式：<select id="duiqiStr" NAME="duiqiStr">
                   <option value="left">左对齐</option>
                   <option value="middle">中间</option>
                   <option value="right">右对齐</option>
                </select>
                <br>                
                图片大小：宽
                    <input type="text" name="widthvalue" size=3 maxlength=3 class="button1">
                    像素&nbsp;&nbsp;高
                    <input type="text" name="heightvalue" size=3 maxlength=3 class="button1">
                    像素<br>
                    （不输入为默认大小）<br>
                <input type="hidden" name="actionType" value="<%=actionType%>">
                <input type="hidden" name="picName" value="<%=picName%>">     
                <!--图片返回值-->
                <input type=hidden value="" name="imageFileName" id="imageFileName" onclick="saveImage();">
                <input type=hidden value="" name="remNum" id="remNum">
                <input type=hidden value="<%=editImageNum%>" name="editImageNum" id="editImageNum">
                <!--图片返回值-->        
                </td>
              </tr> 
			  </form>
            </table>
            <div id="tomodify" style="display:none" align=left>
            <%if actionType="mod" then%>
              <input type="button" name="bSaveModify" value="保存修改" onclick="javascript:modifyImage()" class="button2">  
            <%end if%>
            </div>     
            <!--中文版显示-->       
            <div align="center" id="urlType" style="display:none">
			  <table width="95%" border="0" cellspacing="0" cellpadding="0" ID="Table4">
                <tr> 
                  <td class=tdblack>
                  图片路径：<br> 
                    <input type="text" name="imageUrl" style="width:300" value="http://" ID="Text3" class="button1"> 
                  <br>
                    <input type="button" name="insertImage" value="插入图片" onclick="javascript:toInsertImage();" ID="Button1" class="button2"> 
                  </td>
                </tr>
              </table>
			</div>
			
            <div align="center" id=uploadType style="display:block">
              <table width="95%" border="0" cellspacing="0" cellpadding="0" ID="Table2">
                <tr> 
                  <td class=tdblack><b>注意：</b>请勿手动关闭此窗口,文件格式只支持<b>jpg</b>和<b>gif</b> 
                  </td>
                </tr>
              </table>              
              <form name="upload_code" method="post" action="uploadPic.inc.asp?upload_code=ok&editImageNum=<%=editImageNum%>&actionType=<%=actionType%>&picName=<%=picName%>&editRemNum=<%=editRemNum%>" target=surveywin enctype="multipart/form-data">
                <table width="95%" border="0" cellspacing="0" cellpadding="0" ID="Table3">                               
                  <tr> 
                    <td height="25" class=tdblack>文件路径: 
                      <input type="file" name="codefilename" style="width:200" size="30" ID="File1" class="button1">
                    </td>
                  </tr>
                  <tr> 
                    <td height="28"> 
                      <input type="submit" name="Submit1" onclick='IsVote();' value="上传文件" ID="Submit1" class="button2">
                      <input type="reset" name="Submit2" value="重新选择" ID="Reset1" class="button2">
                    </td>
                  </tr>
                </table>
              </form>
            </div>
            <!--中文版显示-->
          </td>
        </tr>
      </table>
    </td>    
  </tr>
 
</table>
</BODY>
</HTML>

