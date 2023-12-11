<!--#include file="AspCms_AdvFun.asp" -->
<%CheckAdmin("AspCms_AdvList.asp")%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE>添加广告</TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312"><LINK 
href="../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR>
<script language="javascript" type="text/javascript" src="../js/getdate/WdatePicker.js"></script>
<script>
function ShowBar(type)
{
	var picbar1  = document.getElementById("ipath");
	var picbar2 = document.getElementById("lpath");
	var picbar3 = document.getElementById("cc");
	var picbar4 = document.getElementById("advnr");
	var picbar5 = document.getElementById("ipathr");
	var picbar6 = document.getElementById("lpathr");
	if(type==1 )
	{
		picbar1.style.display="none";
		picbar2.style.display="";
		picbar3.style.display="none";
		picbar4.style.display="";
		picbar5.style.display="none";
		picbar6.style.display="none";
	}
	else if(type==2 || type==4)
	{
		picbar1.style.display="";
		picbar2.style.display="";
		picbar3.style.display="";
		picbar4.style.display="none";
		picbar5.style.display="none";
		picbar6.style.display="none";
	}
	else if(type==3|| type==6)
	{
		picbar1.style.display="none";
		picbar2.style.display="none";
		picbar3.style.display="none";
		picbar4.style.display="";
		picbar5.style.display="none";
		picbar6.style.display="none";
	}
	else if(type==5)
	{
		picbar1.style.display="";
		picbar2.style.display="";
		picbar3.style.display="";
		picbar4.style.display="none";
		picbar5.style.display="";
		picbar6.style.display="";
	}	
	
}
</script> 
</head>
<body>
<FORM id= name= action="" method=post >
<DIV class=formzone>
<DIV class=namezone>修改广告</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
  <TR>						
    <TD align=middle width=100 height=30>广告名称</TD>
    <TD><INPUT class="input" style="WIDTH: 200px" maxLength="200" name="advname" value="<%=AdvArray(1,0)%>"/> <FONT color=#ff0000>
      <input type="hidden" name="id" id="id" value="<%=AdvArray(0,0)%>">
      *</FONT></TD>
  </TR>
 <TR>
    <TD align=middle width=100 height=30>广告类型</TD>    
    <TD><label>
    <select name="advclassde"  disabled id="advclassde" onChange="ShowBar(this.value)">
      <option value="1" <%if AdvArray(2,0)=1 then%> selected<%end if%>>文本</option>
      <option value="2" <%if AdvArray(2,0)=2 then%> selected<%end if%>>图片</option>
      <option value="3" <%if AdvArray(2,0)=3 then%> selected<%end if%>>代码</option>
    </select>
    <input name="advclass" type="hidden" value="<%=AdvArray(2,0)%>">
    </label></TD>
 </tr>
  <TR id="ipath" <%if AdvArray(2,0)=1 or AdvArray(2,0)=3 or AdvArray(2,0)=6 then%>style="display:none"<%end if%>>
    <TD align=middle width=100 height=30>图片地址</TD>
    <TD><input class="input" style="WIDTH: 300px" name="ImagePath" id="ImagePath" value="<%=fImagePath%>"/><br><iframe src="../editor/upload.asp?sortType=14&stype=file&amp;Tobj=ImagePath" scrolling="no" topmargin="0" height="40" width="100%" marginwidth="0" marginheight="0" frameborder="0" align="left"></iframe>
</TD>
  </TR>
  <TR id="lpath" <%if AdvArray(2,0)=3 or AdvArray(2,0)=6 then%>style="display:none"<%end if%>>
    <TD align=middle width=100 height=30>链接地址</TD>
    <TD><input class="input" style="WIDTH: 300px" name="LinkPath" id="LinkPath" value="<%=fLinkPath%>"/></TD>
  </TR>
  <TR id="ipathr" <%if AdvArray(2,0)<>5 then%>style="display:none"<%end if%>>
    <TD align=middle width=100 height=30>右侧图片地址</TD>
    <TD><input class="input" style="WIDTH: 300px" name="ImagePathr" id="ImagePathr" value="<%=fImagePathr%>"/><br><iframe src="../editor/upload.asp?sortType=14&stype=file&amp;Tobj=ImagePathr" scrolling="no" topmargin="0" height="40" width="100%" marginwidth="0" marginheight="0" frameborder="0" align="left"></iframe>
</TD>
  </TR>
  <TR id="lpathr" <%if AdvArray(2,0)<>5 then%>style="display:none"<%end if%>>
    <TD align=middle width=100 height=30>右侧链接地址</TD>
    <TD><input class="input" style="WIDTH: 300px" name="LinkPathr" id="LinkPathr"  value="<%=fLinkPathr%>"/></TD>
  </TR>
  <TR id="cc" <%if AdvArray(2,0)=1 or AdvArray(2,0)=3 or AdvArray(2,0)=6 then%>style="display:none"<%end if%>>
    <TD align=middle height=30>图片尺寸</TD>
    <TD>宽
      <INPUT class="input" style="WIDTH: 30px" maxLength="200" name="imgw" value="<%=AdvArray(5,0)%>"/>
      高
      <INPUT class="input" style="WIDTH: 30px" maxLength="200" name="imgh" value="<%=AdvArray(6,0)%>"/></TD>
  </TR>  
  <TR>
    <TD align=middle width=100 height=30 >投放时间</TD>
    <TD><input maxlength="20" name="stime" class="Wdate" onClick="WdatePicker({dateFmt:'yyyy-MM-dd HH:mm:ss'})" value="<%=AdvArray(7,0)%>"/> </TD>
  </TR>
  <TR>
    <TD align=middle width=100 height=30>结束时间</TD>
    <TD><input maxlength="20" name="etime" class="Wdate" onClick="WdatePicker({dateFmt:'yyyy-MM-dd HH:mm:ss'})" value="<%=AdvArray(8,0)%>"/></TD>
  </TR>
  <TR>
    <TD align=middle height=30>广告状态</TD>
    <TD><input type="radio" name="AdvStatus" value="1" <%if AdvArray(9,0)=1 then%>checked<%end if%> />
启用
  <input type="radio" name="AdvStatus" value="0" <%if AdvArray(9,0)<>1 then%>checked<%end if%> />
禁用</TD>
  </TR>
  <TR id="advnr" <%if AdvArray(2,0)=2 or AdvArray(2,0)=4 or AdvArray(2,0)=5 then%>style="display:none"<%end if%>>
    <TD align=middle height=30>广告内容</TD>
    <TD>  <TEXTAREA class="textarea" style="WIDTH: 450px" name="advcontent" rows="3"><%=decodeHtml(AdvArray(10,0))%></TEXTAREA>    </TD>
  </TR>
    </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
  <INPUT class="button" type="submit" value="保存" onClick="form.action='?action=editsave';"/>
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/>
</DIV>
</DIV></FORM>
</body>
</html>
