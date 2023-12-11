<!--#include file="AspCms_linksFun.asp" -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=add" method="post" >
<DIV class=formzone>
<DIV class=namezone>添加友情链接</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
<TBODY>
    <TR>						
        <TD align=middle width=100 height=30>网站名称</TD>
        <TD><INPUT class="input" style="WIDTH: 150px" maxLength="200" name="LinkText"/> <FONT color=#ff0000>*</FONT> </TD>
    </TR>
    <TR>
        <TD align=middle width=100 height=30>链接地址</TD>
        <TD><INPUT class="input" style="WIDTH: 150px" maxLength="200" name="LinkUrl"/> <FONT color=#ff0000>*</FONT></TD>
    </tr>
    <TR>
        <TD align=middle width=100 height=30>立刻发布</TD>
        <TD><input type="checkbox"  checked="checked" value="1" name="LinkStatus"/> </TD></TR>
    <TR>
    <TR>
        <TD align=middle width=100 height=30>排序</TD>
        <TD><input class="input" size="11"  value="9" name="LinkOrder"/> </TD></TR>
    <TR>
    <TR>
        <TD align=middle height=30>链接类型</TD>
        <TD><select name="LinkType" id="LinkType" onChange="selectLinkType(this.options[this.selectedIndex].value)">
        <option value="0" <% if LinkType="0" then echo "selected"%>>文字链接</option>
        <option value="1" <% if LinkType="1" then echo "selected"%>>图片链接</option>
        </select> 
        </TD>
    </TR>
    <TR>
        <TD align=middle height=30>链接图片</TD>
        <TD>
        <input type="text" class="input" name="ImageURL" id="ImageURL" style="width:300px;" />
        图片上传尺寸请控制在88*31
        </TD>
    </TR>
    <TR>
        <TD align=middle height=30 valign="top">上传</TD>
        <TD>       
        <iframe src="../../editor/upload.asp?sortType=11&stype=file&Tobj=ImageURL" scrolling="no" topmargin="0" width="100%" height="40" marginwidth="0" marginheight="0" frameborder="0" align="center"></iframe>
        </TD>
    </TR>   
    <TR>
        <TD align=middle height=30>备注说明</TD>
        <TD><textarea class="textarea" type="text"  rows="3" style="width:500px" name="LinkDesc"></textarea></TD>
    </TR>
</TBODY>
</TABLE>
        
</DIV>
<DIV class=adminsubmit>
<INPUT class="button" type="submit" value="添加" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
</DIV></DIV></FORM>

</BODY></HTML>