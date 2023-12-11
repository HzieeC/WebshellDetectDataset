<!--#include file="AspCms_qqkfFun.asp" -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE></TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312">
<LINK href="../../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<FORM name="form" action="?action=save" method="post" >
<DIV class=formzone>
<DIV class=namezone>在线客服设置</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>    
  <TR>
    <TD height="28" colspan="2" class=innerbiaoti>QQ客服设置</TD>
  </TR>
  <TR>						
    <TD align=middle width=193 height=30>显示状态</TD>
    <TD width="568">
        <input type="radio"  value="1" name="serviceStatus" <% if serviceStatus=1 then echo "checked" end if%>/>显示
        <input type="radio" value="0" name="serviceStatus" <% if serviceStatus=0 then echo "checked" end if%>/>隐藏    </TD>
  </TR>
  <TR>						
    <TD align=middle width=193 height=30>样式</TD>
    <TD>
        <input type="radio"  value="1" name="serviceStyle" <% if serviceStyle=1 then echo "checked" end if%>/>样式一
        <input type="radio" value="2" name="serviceStyle" <% if serviceStyle=2 then echo "checked" end if%>/>样式二    </TD>
  </TR>
  <TR>						
    <TD align=middle width=193 height=30>显示位置</TD>
    <TD>
    	<input type="radio" value="left" name="serviceLocation" <% if serviceLocation="left" then echo "checked" end if%>/>左边
        <input type="radio" value="right" name="serviceLocation" <% if serviceLocation="right" then echo "checked" end if%>/>右边   (只有样式一可以设置到右边)  </TD>
  </TR>
  <TR>						
    <TD align=middle width=193 height=60 valign="top">QQ</TD>
    <TD style="line-height:1.5em;"><INPUT class="input" maxLength="200"  name="serviceQQ"  style="width:400px" value="<%=serviceQQ%>"/>多个请用空格隔开<br>
    	例1：技术支持|123456 产品咨询|8887443<br>
        例2：123456 8887443 84123134</TD>
  </TR>
  <TR>						
    <TD align=middle width=193 height=60 valign="top">旺旺</TD>
    <TD style="line-height:1.5em;"><INPUT class="input" maxLength="200" name="serviceWangWang" style="width:400px" value="<%=serviceWangWang%>"/>多个请用空格隔开<br>
    	例1：销售一号|123456 销售2号|8887443<br>
        例2：123456 8887443 84123134
    </TD>
  </TR>
  <TR>						
    <TD align=middle width=193 height=30>联系方式链接</TD>
    <TD><INPUT class="input" maxLength="200" name="serviceContact"  style="width:400px" value="<%=serviceContact%>"/></TD>
  </TR>
  <TR>						
    <TD align=middle width=193 height=30>调用标签</TD>
    <TD>{aspcms:onlineservice}</TD>
  </TR>
  <TR>
    <TD height="28" colspan="2" class=innerbiaoti>其它客服设置</TD>
  </TR>
  <TR>						
    <TD align=middle width=193 height=30>显示状态</TD>
    <TD>
        <input type="radio" value="1" name="servicekfStatus" <% if servicekfStatus=1 then echo "checked" end if%>/>显示
        <input type="radio" value="0" name="servicekfStatus" <% if servicekfStatus=0 then echo "checked" end if%>/>隐藏</TD>
  </TR>
  <TR>						
    <TD align=middle width=193 height=30>其它客服系统</TD>
    <TD><textarea class="textarea" name="servicekf" cols="40" rows="6" style="width:500px"><%=decode(servicekf)%></textarea></TD>
  </TR>
  <TR>						
    <TD align=middle width=193 height=30>调用标签</TD>
    <TD>{aspcms:kf}</TD>
  </TR>
    </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>

<input type="submit" class="button" value="保 存"/>
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
<INPUT onClick="location.href='<%=getPageName()%>'" type="button" value="刷新" class="button" /> 
</DIV></DIV></FORM>

</BODY></HTML>

