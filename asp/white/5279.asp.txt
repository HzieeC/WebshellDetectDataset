<!--#include file="AspCms_SortFun.asp" -->
<%getSort%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=edit" method="post" >
<DIV class=formzone>
<DIV class=namezone>修改分类</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
  <TR>						
    <TD align=middle width=100 height=30>分类名称</TD>
    <TD><INPUT class="input" style="WIDTH: 200px" maxLength="200" name="SortName" value="<%=SortName%>"/> <FONT color=#ff0000>*</FONT> </TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>顶级分类</TD>    
    <TD><%LoadSelect "ParentID","Aspcms_Sort","SortName","SortID",ParentID, 0,"order by SortOrder","顶级栏目"%></TD>
</tr>
  <TR>
    <TD align=middle width=100 height=30>分类类型</TD>    
    <TD><%=makeSortTypeSelect("sortType", sortType, "onChange=""setInput(sortType.value)""")%>
<Script type="text/javascript">
function setInput(t)
{
//'单篇1,文章2,产品3,下载4,招聘5,相册6,链接7
//alert("AA");
 document.getElementById("hid1").style.display="";
 document.getElementById("hid2").style.display="";
 document.getElementById("hid3").style.display="";
 document.getElementById("hid4").style.display="";
 document.getElementById("hid5").style.display="";
 document.getElementById("hid6").style.display="";
 document.getElementById("hid7").style.display="";
 document.getElementById("hid8").style.display="";
 document.getElementById("hid9").style.display="";
switch(t)
   {
   case "1":
     document.getElementById("SortTemplate").value="about.html";
     document.getElementById("ContentTemplate").value="";
     document.getElementById("SortFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/about/";
     document.getElementById("ContentFolder").value="";
     document.getElementById("SortFileName").value="about-{sortid}";
     document.getElementById("ContentFileName").value="";
     document.getElementById("hid2").style.display="none";
     document.getElementById("hid4").style.display="none";
     document.getElementById("hid6").style.display="none";
	 document.getElementById("hid9").style.display="none";    
     break
   case "2":
     document.getElementById("SortTemplate").value="newslist.html";
     document.getElementById("ContentTemplate").value="news.html";
     document.getElementById("SortFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/newslist/";
     document.getElementById("ContentFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/news/";
     document.getElementById("SortFileName").value="list-{sortid}-{page}";
     document.getElementById("ContentFileName").value="{y}-{m}-{d}/{id}";    
	 document.getElementById("hid8").style.display="none";  
	 document.getElementById("hid9").style.display="none";     
     break
   case "3":
     document.getElementById("SortTemplate").value="productlist.html";
     document.getElementById("ContentTemplate").value="product.html";
     document.getElementById("SortFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/productlist/";
     document.getElementById("ContentFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/product/";
     document.getElementById("SortFileName").value="list-{sortid}-{page}";
     document.getElementById("ContentFileName").value="{y}-{m}-{d}/{id}";   
	 document.getElementById("hid8").style.display="none";     
	 document.getElementById("hid9").style.display="none";     
     break
   case "4":  
     document.getElementById("SortTemplate").value="downlist.html";
     document.getElementById("ContentTemplate").value="down.html";
     document.getElementById("SortFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/downlist/";
     document.getElementById("ContentFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/down/";
     document.getElementById("SortFileName").value="list-{sortid}-{page}";
     document.getElementById("ContentFileName").value="{y}-{m}-{d}/{id}"; 
	 document.getElementById("hid8").style.display="none";  
	 document.getElementById("hid9").style.display="none";      
     break
   case "5":
     document.getElementById("SortTemplate").value="joblist.html";
     document.getElementById("ContentTemplate").value="job.html";
     document.getElementById("SortFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/jobtlist/";
     document.getElementById("ContentFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/job/";
     document.getElementById("SortFileName").value="list-{sortid}-{page}";
     document.getElementById("ContentFileName").value="{y}-{m}-{d}/{id}";  
	 document.getElementById("hid8").style.display="none";
	 document.getElementById("hid9").style.display="none";          
     break
   case "6":
     document.getElementById("SortTemplate").value="albumlist.html";
     document.getElementById("ContentTemplate").value="album.html";
     document.getElementById("SortFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/albumtlist/";
     document.getElementById("ContentFolder").value="{sitepath}<%=setting.languagePath&htmlDir%>/album/";
     document.getElementById("SortFileName").value="list-{sortid}-{page}";
     document.getElementById("ContentFileName").value="{y}-{m}-{d}/{id}";
	 document.getElementById("hid8").style.display="none";
	 document.getElementById("hid9").style.display="none";            
     break
   default:
     document.getElementById("hid1").style.display="none";
     document.getElementById("hid2").style.display="none";
     document.getElementById("hid3").style.display="none";
     document.getElementById("hid4").style.display="none";
     document.getElementById("hid5").style.display="none";
     document.getElementById("hid6").style.display="none";
     document.getElementById("hid7").style.display="none";
     document.getElementById("hid8").style.display="none";
   }
}

</script>
    
    </TD>
</tr>
  <TR id="hid9">
    <TD align=middle width=100 height=30>链接</TD>
    <TD><input class="input" maxlength="200" style="WIDTH: 300px" name="SortURL" value="<%=SortURL%>"/> 注：留言链接"/gbook/"</TD></TR>
  <TR>

  <TR>
    <TD align=middle width=100 height=30>排序</TD>
    <TD><input class="input" maxlength="6" style="WIDTH: 60px" name="SortOrder" value="<%=SortOrder%>"/> </TD></TR>
  <TR>
  <TR>
    <TD align=middle height=30>状态</TD>
    <TD><INPUT class="checkbox" type="checkbox" <% if SortStatus=1 then echo"checked=""checked"""%> name="SortStatus" value="1"/> 
    </TD>
  </TR>
  <TR id="hid1">
    <TD align=middle height=30>列表模板</TD>
    <TD> <input class="input" maxlength="200" style="WIDTH: 200px" name="SortTemplate" id="SortTemplate"  value="<%=SortTemplate%>"/>  
    </TD>
  </TR>
  <TR id="hid2">
    <TD align=middle height=30>内容页模板</TD>
    <TD> <input class="input" maxlength="200" style="WIDTH: 200px" name="ContentTemplate" id="ContentTemplate"  value="<%=ContentTemplate%>"/>  
    </TD>
  </TR>
  <TR id="hid3">
    <TD align=middle height=30>列表生成目录</TD>
    <TD> <input class="input" maxlength="200" style="WIDTH: 200px" name="SortFolder" id="SortFolder" value="<%=SortFolder%>"/>  
    </TD>
  </TR>
  <TR id="hid4">
    <TD align=middle height=30>内容生成目录</TD>
    <TD> <input class="input" maxlength="200" style="WIDTH: 200px" name="ContentFolder" id="ContentFolder" value="<%=ContentFolder%>"/>  
    </TD>
  </TR>
  <TR id="hid5">
    <TD align=middle height=30>列表文件命名</TD>
    <TD> <input class="input" maxlength="200" style="WIDTH: 200px" name="SortFileName" id="SortFileName" value="<%=SortFileName%>"/>
    <%=FileExt%>
    </TD>
  </TR>
  <TR  id="hid6">
    <TD align=middle height=30>内容页文件命名</TD>
    <TD> <input class="input" maxlength="200" style="WIDTH: 200px" name="ContentFileName" id="ContentFileName" value="<%=ContentFileName%>"/>
      <%=FileExt%>    </TD>
  </TR>
  <TR id="hid7">
    <TD align=middle height=30> 浏览权限 </TD>
    <TD>
    <%=userGroupSelect("GroupID", GroupID, 0)%>
        <input type="radio" name="Exclusive" value=">=" <% if Exclusive=">=" then echo "checked=""checked""" end if%> />
        隶属
        <input type="radio" name="Exclusive" value="=" <% if Exclusive="=" then echo "checked=""checked""" end if%> /> 
        专属（隶属：权限值≥可查看，专属：权限值＝可查看）    </TD>
  </TR>
  <TR id="hid8">
    <TD align="middle" height="30">
       	内容<BR><BR>
        <a href="#" onClick="SetEditorPage('Content', '{aspcms:page}')"> 插入分页<BR>
       {aspcms:page}</a></TD>
    <TD>
<%Dim oFCKeditor:Set oFCKeditor = New FCKeditor:oFCKeditor.BasePath="../../editor/":oFCKeditor.ToolbarSet="AdminMode":oFCKeditor.Width="615":oFCKeditor.Height="300":oFCKeditor.Value=decodeHtml(Content):oFCKeditor.Create "Content"
%>  <script type="text/javascript">

function SetEditorPage(EditorName, ContentStr) {
     var oEditor = FCKeditorAPI.GetInstance(EditorName) ;
	 oEditor.Focus();
	 //setTimeout(function() { oEditor.Focus(); }, 100);
     oEditor.InsertHtml(ContentStr);	
}
</script>
    </TD>
  </TR>
    <tr>
    <TD align=middle height=30 valign="top">上传图片</td>
      <td colspan="3"><iframe name="image" frameborder="0" width='100%' height="40" scrolling="no" src="../../editor/upload.asp?sortType=10&stype=sort&Tobj=content" allowTransparency="true"></iframe></td>
    </tr>
  <TR>
  <TR>
    <TD align=middle height=30>页面标题</TD>
    <TD> <input class="input" maxlength="200" style="WIDTH: 450px"  name="PageTitle" value="<%=PageTitle%>"/>  
    </TD>
  </TR>
  <TR>
    <TD align=middle height=30>页面关键词</TD>
    <TD> <input class="input" maxlength="200" style="WIDTH: 450px"  name="PageKeywords" value="<%=PageKeywords%>"/>  
    </TD>
  </TR>
  <TR>
    <TD align=middle height=30>页面描述</TD>
    <TD>  <TEXTAREA class="textarea" style="WIDTH: 450px" name="PageDesc" rows="3"><%=PageDesc%></TEXTAREA>
    </TD>
  </TR>

    </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
<INPUT type="hidden" name="SortID" value="<%=SortID%>" />
<INPUT class="button" type="submit" value="修改" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 

<INPUT onClick="location.href='<%=getPageName()%>?id=<%=SortID%>'" type="button" value="刷新" class="button" /> 
</DIV></DIV></FORM>

</BODY></HTML>