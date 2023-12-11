<!--#include file="AspCms_SettingFun.asp" -->
<%CheckAdmin("AspCms_ComplanySetting.asp")%>
<%getComplanySetting%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3c.org/TR/1999/REC-html401-19991224/loose.dtd">
<HTML xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
<LINK href="../images/style.css" type=text/css rel=stylesheet>
<META content="MSHTML 6.00.3790.4807" name=GENERATOR></HEAD>
<BODY>
<DIV class=formzone>
<FORM name="" action="?action=savec" method="post">
<DIV class=tablezone>

<TABLE cellSpacing=0 cellPadding=8 width="100%" align=center border=0>
<TBODY>
    <TR>
    <TD width="225" class=innerbiaoti><STRONG>网站信息</STRONG></TD>
        <TD class=innerbiaoti width=661 height=28><STRONG>设置</STRONG></TD>
        </TR>
        <TR class=list>
        <TD><STRONG>网站标题 : </STRONG></TD>
        <TD width=661><INPUT class="input" size="30" style="width:300px;"  value="<%=SiteTitle%>" name="SiteTitle"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>网页附加标题 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=AdditionTitle%>" name="AdditionTitle"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>LOGO图片URL : </STRONG></TD>
        <TD align="left"><INPUT class="input" size="30" style="width:300px;"  value="<%=SiteLogoUrl%>" name="SiteLogoUrl" id="SiteLogoUrl"/>
        </TD>
    </TR>
    <TR class=list>   
        <TD valign="top"><STRONG>上传 : </STRONG></TD>
        <TD align="left"><iframe src="../editor/upload.asp?sortType=12&stype=file&Tobj=SiteLogoUrl" scrolling="no" topmargin="0" height="40" width="100%" marginwidth="0" marginheight="0" frameborder="0" align="left"></iframe>
        </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>站点网址 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=SiteUrl%>" name="SiteUrl"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>企业名称 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=CompanyName%>" name="CompanyName"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>企业地址 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=CompanyAddress%>" name="CompanyAddress"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>邮政编码 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=CompanyPostCode%>" name="CompanyPostCode"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>联系人 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=CompanyContact%>" name="CompanyContact"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>电话号码 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=CompanyPhone%>" name="CompanyPhone"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>手机号码 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=CompanyMobile%>" name="CompanyMobile"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>公司传真 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=CompanyFax%>" name="CompanyFax"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>电子邮箱 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=CompanyEmail%>" name="CompanyEmail"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>备案号 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:300px;"  value="<%=CompanyICP%>" name="CompanyICP"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>统计代码 : </STRONG></TD>
        <TD>        
        <textarea class="textarea" name="StatisticalCode" cols="60" style="width:80%" rows="3"><%=decode(StatisticalCode)%></textarea>
		</TD>
    </TR>
    <TR class=list>
        <TD><STRONG>版权信息 : </STRONG></TD>
        <TD>
        <textarea class="textarea" name="CopyRight" cols="60" style="width:80%" rows="3"><%=decode(CopyRight)%></textarea>
         </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>站点关键词 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:80%;"  value="<%=SiteKeywords%>" name="SiteKeywords"/> </TD>
    </TR>
    <TR class=list>
        <TD><STRONG>站点描述 : </STRONG></TD>
        <TD><INPUT class="input" size="30" style="width:80%;"  value="<%=SiteDesc%>" name="SiteDesc"/> </TD>
    </TR>
</TBODY></TABLE>
</DIV>
<DIV class=adminsubmit>
<INPUT type="hidden" value="<%=LanguageID%>" name="LanguageID" />
<INPUT class="button" type="submit" value="保存" />
<INPUT class="button" type="button" value="返回" onClick="history.go(-1)"/> 
 </DIV></FORM>
</DIV>
</BODY>
</HTML>
