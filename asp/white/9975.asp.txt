<%@ LANGUAGE = VBScript.Encode %>
<!--#include file="you.asp"-->
<!--#include file="conn.asp"-->
<!--#include file="zcm.asp"-->
<!--#include file="admin.asp"-->
<%#@~^YgAAAA==@#@&YrDV'KMkscI;;+kYcEDkY^nJ*#@#@&^GxD+	YxIn5!+dYvEmKxOn	YJb@#@&SCxT;mon{I;E/DcJdlUo!lLnr#@#@&/B0AAA==^#~@%>
<%#@~^LgAAAA==r6P];!+/DR5;+MXjYMkULvJ:m.3r#'rdGEDtbN^J~O4+UhhAAAA==^#~@%>
<%#@~^eQIAAA==@#@&qW,YbYV'rEP:tnx@#@&.nkwW	d+chDbOnPr?}I]5~@!(D@*J@#@&D/2G	/+ AMkO+,E请输入内容标题e@!C,tM+0{Jr%l7ldmMk2O=tkkOWMXRTGcO8#rJ@*返回重输@!&C@*J@#@&Mn/aWUdR+U[@#@&nx9~b0@#@&q6PmKxDnxD'EJ,K4n	@#@&Mn/aWxkn hMkD+~Jj6"IeP@!8D@*J@#@&M+/2G	/nRS.bYn,J请输入内容e@!l,tMn0{JELm\Cd1DkaO)4k/DG.XcoK`RFbEr@*返回重输@!zm@*J@#@&.nkwWUdRnx9@#@&+U9Pb0@#@&@#@&j+DP./,'~jD\.R;D+mOnr(LmO`E)Gr9Ac]+1W.[k+YEb@#@&d;^xr/n^+1YPCP6.WsP^E^Y;.PJ@#@&DkRWanUPk;^~^WUUBFS&@#@&DkRC[9x+A@#@&Dd`rObYsJ*'YbY^n@#@&Dd`rmGUD+xDE#{mW	OnxD@#@&Dd`EJmxLEmL+r#xJmxo;CT+@#@&MdvJ^KE	Y+MJ*x!@#@&./vJOrs+J*xfmY+vb@#@&M/cE2NCO@#@&Dk m^Wdn@#@&DndaWU/ M+[bDmY,Jz[:bxmZ!VO;M+Rmdwr@#@&U[Pb0@#@&B7AAAA==^#~@%>
<!-- #include file="Inc/Head.asp" -->
<table width="560" border="0" align="center" cellpadding="2" cellspacing="1" class="table_southidc">
  <tr> 
    <td class="back_southidc" height="25"> <div align="center"><strong>添加企业文化文章 
        <br>
        </strong></div></td>
  </tr>
  <tr> 
    <form method="post" action="Admin_CultureNewsAdd.asp?mark=southidc">
      <td><div align="center"> 
          <table width="100%" border="0" cellpadding="0" cellspacing="1">
            <tr bgcolor="#E7E7E7"> 
              <td width="9%" height="25" bgcolor="#ECF5FF"><div align="center">标&nbsp;&nbsp;题:</div></td>
              <td width="59%" bgcolor="#ECF5FF"><input type="text" name="title" class="inputtext" maxlength="60" size="45"></td>
              <td width="13%" bgcolor="#ECF5FF"><div align="center">语言:</div></td>
              <td width="19%" bgcolor="#ECF5FF"><select name="Language" id="Language">
                  <option value="1">中文</option>
                  <option value="0">英文</option>
                </select></td>
            </tr>
            <tr bgcolor="#E7E7E7"> 
              <td height="25" colspan="4" bgcolor="#ECF5FF"> <div align="center">内&nbsp;&nbsp;容</div></td>
            </tr>
            <tr bgcolor="#E7E7E7"> 
              <td height="300" colspan="4" bgcolor="#ECF5FF"> <input type="hidden" name="content" value=""> 
                <iframe ID="eWebEditor1" src="Southidceditor/ewebeditor.asp?id=content&style=southidc" frameborder="0" scrolling="no" width="550" HEIGHT="450"></iframe> 
              </td>
            </tr>
            <tr bgcolor="#ECF5FF"> 
              <td height="25" colspan="4"> <div align="center"> 
                  <input name="submit" type="submit" value="确定">
                  &nbsp; 
                  <input name="reset" type="reset" value="取消">
                </div></td>
            </tr>
          </table>
        </div></td>
    </form>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->