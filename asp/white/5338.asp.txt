<!--#include file="AspCms_CollectFun.asp" -->
<%CheckLogin()%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml"><HEAD><TITLE>数据采集</TITLE>
<META http-equiv=Content-Type content="text/html; charset=gb2312"><LINK 
href="style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR>
    
<SCRIPT>
function SelAll(theForm){
		for ( i = 0 ; i < theForm.elements.length ; i ++ )
		{
			if ( theForm.elements[i].type == "checkbox" && theForm.elements[i].name != "SELALL" )
			{
				theForm.elements[i].checked = ! theForm.elements[i].checked ;
			}
		}
}
</SCRIPT>
</HEAD>
<BODY>
<%Select case action
	case "","Scollect"
%>
<FORM id= name= action="" method=post >
<DIV class=searchzone>
<TABLE height=30 cellSpacing=0 cellPadding=0 width="100%" border=0>
  <TBODY>
  <TR>
    <TD height=30>&nbsp;</TD>
    <TD align=right colSpan=2><span class="piliang">
      <INPUT class=button type=button value=采集发布管理 name=cc2 onClick="window.location.href='AspCms_CollectContentlist.asp'">
     <INPUT class=button type="button" value=采集规则管理 name=cc2 onClick="window.location.href='AspCms_Collect.asp'">
    </span></TD>
  </TR></TBODY></TABLE></DIV>
<DIV class=formzone>
<DIV class=namezone><FONT color=#ff0000>1.采集规则编写</FONT>  →  2.链接过滤  →  3.文章采集储存  →  4.文章归类发布</DIV>
<DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<DIV id=proplist></DIV>
<TABLE cellSpacing=0 cellPadding=2 width="100%" align=center border=0>
  <TBODY>
   <TR>
    <TD align=left width=150 height=30>&nbsp;</TD>
    <TD width="252" align="center">页码前地址</TD>
    <TD width="52" align="center">起始页码</TD>
    <TD width="421" height=30>页码后地址(页码后无地址可留空)</TD>
  </TR>
  <TR>
    <TD align=left width=150 height=30>采集列表页地址</TD>
    <TD align="left"><input class=input style="FONT-SIZE: 12px; WIDTH: 250px" 
     name=urlb value="<%=Server.HtmlEncode(formvalue(0))%>"></TD>
    <TD><INPUT class=input style="FONT-SIZE: 12px; WIDTH: 50px" 
      maxLength=200 name=pagenum value="<%=Server.HtmlEncode(formvalue(1))%>"></TD>
    <TD height=30><INPUT class=input style="FONT-SIZE: 12px; WIDTH: 50px" 
      maxLength=200 name=urla value="<%=Server.HtmlEncode(formvalue(2))%>"> <FONT color=#ff0000>*</FONT></TD></TR>
  <TR>
    <TD align=left width=150 height=30>采集页数</TD>
    <TD height=30 colspan="3"><INPUT class=input style="FONT-SIZE: 12px; WIDTH: 50px" 
      maxLength=200 name=pagelinkcount value="<%=Server.HtmlEncode(formvalue(3))%>"></TD>
  </TR>
  <TR>
    <TD align=left width=150 height=30>列表区域首尾标识</TD>
    <TD height=30 align="left" valign="middle"colspan="3"><INPUT class=input style="FONT-SIZE: 12px; WIDTH: 200px" 
     name=listpageareab value="<%=Server.HtmlEncode(formvalue(4))%>">
      ----
        <INPUT class=input style="FONT-SIZE: 12px; WIDTH: 200px" 
     name=listpageareaa value="<%=Server.HtmlEncode(formvalue(5))%>"></TD>
  </TR>
  <TR>
    <TD align=left width=150 height=30>标题区域首尾标识</TD>
    <TD height=30 colspan="3"><INPUT class=input style="FONT-SIZE: 12px; WIDTH: 200px" 
     name=listtitleb value="<%=Server.HtmlEncode(formvalue(6))%>">
----
  <INPUT class=input style="FONT-SIZE: 12px; WIDTH:200px" 
     name=listtitlea value="<%=Server.HtmlEncode(formvalue(7))%>"></TD>
  </TR>
  <TR>
    <TD align=left height=30>标题区链接位置</TD>
    <TD height=30 colspan="3"><INPUT class=input style="FONT-SIZE: 12px; WIDTH: 50px" 
      maxLength=200 name=pagelinknum value="<%=Server.HtmlEncode(formvalue(8))%>">
      <FONT color=#ff0000>*</FONT>如果一个标题区域有多个链接,请设置有效文章地址的链接所在所有链接的第几个</TD>
  </TR>
  <TR>
    <TD align=left height=30>绝对地址前缀</TD>
    <TD height=30 colspan="3"><input class=input style="FONT-SIZE: 12px; WIDTH: 250px" 
     name=Purl value="<%=Server.HtmlEncode(formvalue(9))%>">
      <FONT color=#ff0000>*</FONT>如果采集链接非完整链接那么请填写绝对地址前缀</TD>
  </TR>
  <TR>
    <TD align=left height=30>文章标题首尾标识</TD>
    <TD height=30 colspan="3"><INPUT class=input style="FONT-SIZE: 12px; WIDTH: 200px" 
     name=articletitb value="<%=Server.HtmlEncode(formvalue(10))%>">
----
  <INPUT class=input style="FONT-SIZE: 12px; WIDTH: 200px" 
     name=articletita value="<%=Server.HtmlEncode(formvalue(11))%>"></TD>
  </TR>
  <TR>
    <TD align=left height=30>文章内容首尾标识</TD>
    <TD height=30 colspan="3"><INPUT class=input style="FONT-SIZE: 12px; WIDTH: 200px" 
     name=articleconb value="<%=Server.HtmlEncode(formvalue(12))%>">
----
  <INPUT class=input style="FONT-SIZE: 12px; WIDTH: 200px" 
     name=articlecona value="<%=Server.HtmlEncode(formvalue(13))%>"></TD>
  </TR>
  <TR>
    <TD align=left height=30>文章内容位置</TD>
    <TD height=30 colspan="3"><INPUT class=input style="FONT-SIZE: 12px; WIDTH: 50px" 
      maxLength=200 name=contentnum value="<%=Server.HtmlEncode(formvalue(14))%>"></TD>
  </TR><TR>
    <TD align=left height=30>规则名称</TD>
    <TD height=30 colspan="3"><INPUT class=input style="FONT-SIZE: 12px; WIDTH:100px" 
      maxLength=200 name=rulesname value="<%=rulesname%>">
      <span class="adminsubmit">
      <input type="hidden" name="DocollectID" id="DocollectID" value="<%=CollectID%>">
      </span></TD>
  </TR>
  </TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
  <input class=button type=submit value=保存 name=cc2 onClick="form.action='?action=save';">
  <input class=button type=submit value=下一步 name=cc3 onClick="form.action='?action=getlinklist';">
</DIV></DIV></FORM>
<%	case "getlinklist","delarrlink"%>

<DIV class=formzone>
<DIV class=namezone>1.采集规则编写</FONT>  → <FONT color=#ff0000>2.链接过滤</FONT>  →  3.文章采集储存  →  4.文章归类发布</DIV>
<FORM id= name= action="" method=post><DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<DIV id=proplist></DIV>
<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD width=60 align="center" class=biaoti>选择</TD>
    <TD class=biaoti width=50>编号</TD>
    <TD width="767" 
    height=28 class=biaoti>文章链接 <FONT color=#ff0000>*</FONT>请选择符合条件的链接      </TD>

    </TR>
    </TBODY></TABLE>
<DIV class=listzone>
<div style="scrollbar-face-color: #9ebfe8; scrollbar-highlight-color: #ffffff; overflow: scroll; width:100%; scrollbar-shadow-color: #ffffff; scrollbar-3dlight-color: #9ebfe8; scrollbar-arrow-color: #ffffff; scrollbar-track-color: #ffffff; scrollbar-darkshadow-color: #9ebfe8; height: 260px; p: left&gt;&lt;FONT style=" face="Arial">
<TABLE cellSpacing=0 cellPadding=3 width="95%" align=center border=0>
  <TBODY>
	<%linkList%>
    </TBODY></TABLE>
</DIV></div>
<DIV class=piliang>
<INPUT onClick="SelAll(this.form)" type="checkbox" value="1" name="SELALL" checked> 全选&nbsp;</DIV>

</DIV>
<DIV class=adminsubmit><INPUT class=button type=submit value=下一步 name=cc onClick="form.action='?action=getcontentlist'"> 
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/>
</DIV>
</FORM></DIV>
<% case "getcontentlist","gettodo"%>
<DIV class=formzone>
<DIV class=namezone>1.采集规则编写</FONT>  → 2.链接过滤  →  <FONT color=#ff0000>3.文章采集储存</FONT>  →  4.文章归类发布</DIV>
<FORM name= action="" method=post><DIV class=tablezone>
<DIV class=noticediv id=notice></DIV>
<DIV id=proplist></DIV>
<script   language=javascript> 
        document.write( " <div   id=loadingDiv> <br> &nbsp;信息采集中， "+ 
                                      "请等待 <span   id=loading> </span> </div> "); 
        var   s1   =   setInterval( "loading.innerText+= '· ' ",   300); 
        var   s2   =   setInterval( "loading.innerText   =   ' ' ",   8000); 

        function   window.onload() 
        { 
            clearInterval(s1); 
            clearInterval(s2); 
            loadingDiv.removeNode(true); 
            bodyHidden.style.display   =""; 
        } 
    </script>
<DIV class=listzone>

<TABLE cellSpacing=0 cellPadding=3 width="100%" align=center border=0>
  <TBODY>
  <TR class=list>
    <TD width=60 align="center" class=biaoti>选择</TD>
    <TD class=biaoti width=50>编号</TD>
    <TD width="767" 
    height=28 class=biaoti>文章标题(内容截取前500字节以供参考)</TD>

    </TR>
    </TBODY> <TBODY>
	<%getcontenttl%>
    </TBODY></TABLE>


</div>
<DIV class=piliang>
<INPUT onClick="SelAll(this.form)" type="checkbox" value="1" name="SELALL"  checked> 
全选&nbsp;<FONT color=#ff0000>*</FONT>选择加入临时数据表的采集结果</DIV>

</DIV>
<DIV class=adminsubmit><INPUT class=button type=submit value=下一步 name=cc onClick="form.action='?action=savecontent';"> 
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/>
</DIV>
</FORM></DIV>

<%End Select%>
</BODY></HTML>
