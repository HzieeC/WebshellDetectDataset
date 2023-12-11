<%@ LANGUAGE = VBScript.Encode %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->
<!--#include file="Inc/Function.asp"-->
<%#@~^egEAAA==@#@&NrsPkYDwk^n1m:n@#@&mGUkYPtC6h+DhCL+{ Z@#@&Nrh,YGYmsn!YS/!DDnUDnCoS:WOmVhlo/@#@&Nb:~Dk~~d$V@#@&dYMsk^nHls+{J)Nhr	b8W!OEkRCdaJ@#@&@#@&kWPMn$EnkYvJwmoE#@!@*EJ,Y4n	@#@&,~P,mEM.nxDnmon'^r	YcD5E/OcrwlLnr#b@#@&n^/n@#@&imEMDUYhlL+{F@#@&xN,r0@#@&@#@&jnY,Dk'j+.-D ZMnlD+68N+mOcrb[W98cIn1WMN?Yrb@#@&/5V{Jdn^+mD~e,0DKh~b(W!Y;/~GMNnD,8X,b8G!YEdGMNnDr@#@&Ddcra+x,/$s~1WUxBFSq@#@&dnAAAA==^#~@%>
<script language=javascript>
function ConfirmDel()
{
   if(confirm("确定要删除此记录吗？"))
     return true;
   else
     return false;
}
</script>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td width="862" align="center" valign="top">
      <br>
      <%#@~^RAQAAA==@#@&P~ik6PDkRG0,lUN,Dd (W0,Otx@#@&77D/aWU/n SDrY~J目前共有,!~条记录E@#@&dnsk+@#@&,~,P7DWDlVhEDxDkR.+1W.[1WE	O@#@&ddbW~m!DM+UY2CT+@!F,Otx@#@&,PP~~,P7d1;MDn	Yalo'8@#@&,P~Pi+U[,k0@#@&P,PPirWPvm!D.+UOalL+ q#CHCah+DKCT+@*YKOmV2!Y,Ytx@#@&d,P~dikW~vYWDCVhEY,hGN,Hm6K+.Kmon#{TPDtnU@#@&d~~,P~di^!D.xDwlT+{~YKYCVhEO~'PHmanDnmLn@#@&diP~dnsk+@#@&i7P,P~~,dm;.M+UYaCT+x,YKYl^n!OP'P\lXnn.hlo~_,F@#@&7~P,di+UN~r6@#@&@#@&~P,P7n	NPrW@#@&7P,~,kW,m!DDxDKlT+xF,Y4n	PP,~P,PP@#@&~P,P,P~P7d4WAZKUYxO@#@&PP~~,P~Pid4WAalT+PkYMok^+Hls+SOKYl^2ED~HmaK+Mnmon~O.!+SYM;+BJ条记录E@#@&PP~7,dnVkn@#@&~,PiPP,P,7k6Pcm!D.n	YnmL+ F#C\C6h+MnCon@!DWOl^KEDPO4x@#@&~,P~P,~,P7,P,dDkRsG\P~`1E..xYhCoOF*M\lXnDKlLn@#@&~P,~P,P~~idNrh,4GW0hmDV@#@&,PP,P,~P,P~di4GG0:lMV'M/R(GG3slM3~P~~,P~Pi7@#@&P~~,PP~~,P~Pid4WA;W	Y+	Y@#@&P,P~P,P~~,PPidtKhwmLnPkYMsrVnHm:n~DGYmV2;D~HCah+.nmL~OME~YMESJ条记录r@#@&P,P~~,PPinVk+@#@&7~P,P,P~P7^!D.+	Onmonx8PP~~,P~P@#@&,P~,P,PP,P,7dktGh;WUOxY@#@&P,PP,~~P,P,d7/4GSwCo~/DDor^+1Ch~OWDC^w;D~tl6h+MKlT+SYMEnSDDESJ条记录r@#@&i~~P,dx[PrW@#@&7dUN,kW@#@&d+U[,kW@#@&@#@&/;(PktWSZKUYxO@#@&P~~iNks~k@#@&P,~~k{!@#@&phQBAA==^#~@%> 
      <table width="556" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr>
          <td width="550" height="25" class="back_southidc"> 
            <div align="center"><strong>栏目管理</strong></div></td>
        </tr>
        <tr> 
          <td height="66"> 
            <div align="center">
              <table width="100%" height="62" border="0" align="center" cellpadding="0" cellspacing="1" bgcolor="#000000" class="border">
                <tr bgcolor="#ECF5FF" class="title"> 
                  <td width="78" align="center"><strong>排序号</strong></td>
                  <td width="94" align="center"><strong>语种</strong></td>
                  <td width="265" height="29" align="center"><strong> 栏目名称</strong></td>
                  <td width="108" align="center"><strong> 操作</strong></td>
                </tr>
                <%#@~^FAAAAA==[KPA4k^+P	WD~DkR3rwPqgYAAA==^#~@%>
                <tr class="tdbg"> 
                  <td align="center" bgcolor="#ECF5FF"><%=#@~^EgAAAA==.k`Ez4KEY!/K.NDE#eQYAAA==^#~@%></td>
                  <td align="center" bgcolor="#ECF5FF">
				  <%#@~^jAAAAA==~b0~M/vJSmxT;lT+E#{JqE,YtUP@#@&di77P,P,P~P~./2W	d+c.rD+PE中文E@#@&7di7,P~,PV/@#@&7did7P,P~./wKU/RMrO+,J英文r@#@&d77id~+	[Pb0~@#@&dd77,P~LhwAAA==^#~@%></td>
                  <td height="28" align="center" bgcolor="#ECF5FF"><%=#@~^CwAAAA==.k`E:kDV+r#fAMAAA==^#~@%></td>
                  <td align="center" bgcolor="#ECF5FF">
				   <%#@~^HAAAAA==~b0~M/vJSmxT;lT+E#{JqE,YtUPTggAAA==^#~@%>
				  <a href=../Aboutus.asp?Title=<%=#@~^CwAAAA==.k`E:kDV+r#fAMAAA==^#~@%> target="_blank">查看</a>&nbsp; 				  
				  <%#@~^TQAAAA==n^/n@#@&iddiP,3x:kOV'.dvJKbOVJ#@#@&7did,PvIndaWU/ MkOnv2xPrDVn#@#@&id7iP,PRIAAA==^#~@%>
				   <a href=../EnAboutus.asp?Title=<%=#@~^BwAAAA==3	KrDVtQIAAA==^#~@%> target="_blank">查看</a>&nbsp;  <%#@~^BwAAAA==n	N~b0,RgIAAA==^#~@%>
                    &nbsp;<a href="AdminAboutusModify.asp?ID=<%=#@~^CAAAAA==.k`E&fr#BwIAAA==^#~@%>">修改</a>&nbsp; 
                    &nbsp;<a href="AdminAboutusDel.asp?ID=<%=#@~^CAAAAA==.k`E&fr#BwIAAA==^#~@%>" onClick="return ConfirmDel();">删除</a></td>
                </tr>
                <%#@~^SQAAAA==@#@&dr{k3F@#@&dbWPb@*xHm6KnMnlTnPDt+	~n6bY,NG@#@&7M/ :K-+	+aO@#@&VGGa@#@&dRIAAA==^#~@%>
              </table>
            </div></td>
        </tr>
      </table>
      <%#@~^EAAAAA==@#@&+U9PkE4,@#@&7wIAAA==^#~@%> </td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->
<%#@~^OAAAAA==@#@&DdcZ^W/@#@&d+DP./{1GO4kxT@#@&1lV^~/VK/ZGxUc*P~@#@&mw4AAA==^#~@%>