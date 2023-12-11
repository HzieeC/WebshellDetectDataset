<%@language=VBScript.Encode codepage=936 %>
<!--#include file="you.asp"-->
<!--#include file="conn.asp"-->
<!--#include file="zcm.asp"-->
<!--#include file="admin.asp"-->
<!--#include file="../inc/config.asp"-->
<!--#include file="../inc/function.asp"-->
<!-- #include file="Inc/Head.asp" -->

<%#@~^zAMAAA==@#@&ZG	/DPHm6hnDhlL+{ T@#@&Nks~/DDsbsn1m:@#@&Nrh,YGYmsn!YS/!DDnUDnCoS:WOmVhlo/@#@&Nb:~jaVGC9fkMSKME+hCOtB0kWSY4nwWsN.~DtnobV+SA4k^t6r^+SDtb/0bVSsbVnZKEUOBKWDs+Uky@#@&/DDwks+HCs+xJz[:bxmiaVWC[wks+tC	lLRm/wr@#@&@#@&b0~D;;nkY`r2lT+J*@!@*JrPDtnx@#@&,P~P1;DM+UOhlonx1kUYv.;;/D`JalTnJ*#@#@&Vdn@#@&d1;DM+xDKCo'8@#@&+U[,kW@#@&@#@&b0~.botOcUl-+`2wks/hlY4~8b@!@*J&J,Y4n	@#@&iiw^Wl99rD{JcR&J~',?C\iwwksnknlO4,[~JJE@#@&n^/@#@&ijasWmN9kM'E czJ,'PUl\i2sbV/KlO4@#@&nx9~k6@#@&PME+KCDtx?.7+.cHmwnmY4cjaVGl9fr.*@#@&&WP	WY,(dr(L&xdYCs^+[`rjmMk2Obxo obVn?HdD+h}4N+mDJ*~K4+U@#@&d]nkwW	d+cDbOnPr@!(@*@!0GUDP^W^GD{Dn[@*你的服务器不支持Psj6v?^Db2DkUTRwkV?HdY:64N+^O*"P不能使用本功能@!&0KxY@*@!&4@*J@#@&3Vdn@#@&7/OP6/Gx;D+COr8L^D`EUmMkwDk	LRwks+UXdO:r(%+1YJ*@#@&db0,Dn;;nkYcJz^YbWUE*'J9n^J~Y4n	@#@&idStk1t6rV'd+M\n.c:la2lDt`"n5E/D`Esrs1C:E#*P@#@&id?nO,Y4kkWbVn,',0/KRVnYwks+vh4r1t0bs+*P@#@&77Y4kk0rVn G+s+DnP:D;n@#@&d7./2W	dR.NbD+1Y,Eb9:rx|j2sKlNwrVHl	CL+clkwE@#@&7x[PbW@#@&@#@&QiMBAA==^#~@%>
<script language="JavaScript">
function ConfirmDel()
{
if (confirm("你真的要删除此文件吗!"))
	return true;
else
	return false;
}
</script>
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td width="862" align="center" valign="top"> <br>
      <p align="center"><strong>上 
        传 文 件 管 理</strong></p>
      <%#@~^UAYAAA==@#@&P~b0,0/KRwGV9+.2XkdOk`KM;+hlY4bOtx@#@&7srsZGE	O'Z@#@&7:WYsnUk"+{T@#@&7U+DPY4+wGV9+.'6/G V+YwGV9+DvP.EnmY4#@#@&isGD,3l1t~O4+srsP(x,O4+oKV9+Dcsbs+k@#@&disrsZW!UY{sk^n/W!xD_q@#@&7iKGY^n?bynx:WYsnUk"+3O4+obVR?by@#@&ixn6D@#@&~,PPDGYmVn!OxsbVZGEUO@#@&7k6~m!D.n	YwCL@!qPD4x@#@&P,Pdim!.DxOwmonx8@#@&,~Pi+x9~r0@#@&,P~drW,`^EM.+	Y2CT+OqbCHC6hnMnCT+@*YWDl^2EDPOtx@#@&idk6~`DWYmsKEDPsW[P\CXnnDhCo#xT,YtnU@#@&7P,7im;MDxYalTn',YGYmVK;DP-,\lXn+MKCo@#@&d~P7n^/n@#@&7P,P~~,dm;.M+UYaCT+x,YKYl^n!OP'P\lXnn.hlo~_,F@#@&77+	N,kW@#@&@#@&P~P,nx9PrW@#@&drW,m;DMn	YKmo'F,Y4nx@#@&7dktGAalo+PkYDwrs+gls+SYGOmV2EDSHm6KnMnlLn@#@&7dk4Kh/KxD+xDP,~P,d@#@&idd4KhwmL+yP/D.ok^+glh+SOKYCVa;YBHCah+DKCT+@#@&i7M+daW	/+chMrYPE@!(D@*@!9k\,CVbox{v^+	YDv@*本页共显示~@!(@*EPL~sbVn/KExO~LPE@!J8@*P个文件，占用~@!4@*JPLP:GY^+jk.+wqZ c,'Pr@!z(@*~|@!z9k-@*E@#@&P~PinVk+@#@&,PP7~,P7k6~vm;MDxYhlTnO8#MHm6KnMnlTn@!DWYmsKEDPDtnx@#@&id7/4GhalLnyP/O.wks+gCs+SDWDlVaEDSHm6K+MnCL@#@&i7dktWS/GxD+	Y~P~~,d@#@&i7dktGAalon+,/ODwr^+Hm:~YKYmsw!YSHm6KnMnlTn@#@&ddi.n/aW	/nRA.bYnPr@!4M@*@![b\PCsboU'E^xODE@*本页共显示P@!4@*EPLPok^+/G!xY,'Pr@!z(@*~个文件，占用P@!4@*J~[~PKYs+Ury-qTycP'~r@!&4@*~n@!&9k7@*J@#@&,~P,P~Pi+sd@#@&,~P,PP,~7m!DM+UYKCT+xF@#@&didd4KhwCL ~/D.wks1m:+BYKOl^w;YBHCah+DhCo@#@&i77/4WSZGxOn	Y~P,~Pi@#@&7id/4GSwCo+,/OMsbV+glsn~DWOl^w;OBHlXK+MnlTn@#@&idiDn/2G	/nRS.kD+~E@!4D@*@!9k-PmsboU{B1+xD+Mv@*本页共显示,@!8@*rP'~wkV/W!xY,'~J@!z(@*~个文件，占用P@!8@*J~[,PWDVnjby+wqZ *PL~r@!&(@*,|@!JNb-@*r@#@&P,P~7xN,r0@#@&dU[Pb0@#@&~Pnsk+@#@&i.+kwGUk+RA.bYnPr找不到文件夹！可能是配置有误！E@#@&~,+	NPb0@#@&+	N~k6@#@&@#@&/E(~/4Wh;GUYxD`b@#@&~,P7NbhP1@#@&7wkVn/KEUY{T@#@&7:WDV+Uk.n'Z@#@&uLABAA==^#~@%> 
      <table width="620" border="0" align="center" cellpadding="2" cellspacing="1" bgcolor="#000000" class="border">
        <tr bgcolor="A4B6D7" class="title"> 
          <td width="158" height="25" align="center">文件名</td>
          <td width="84" height="20" align="center">文件大小</td>
          <td width="134" height="20" align="center">文件类型</td>
          <td width="119" height="20" align="center">最后修改时间</td>
          <td width="43" height="20" align="center">操作</td>
        </tr>
        <%#@~^mAAAAA==@#@&@#@&wWMP2mm4~Y4+ok^+~(	PY4nsKVN. sbV/@#@&7^{mQF@#@&db0~obV+/G!xO@*{\m6KDhloPD4+	@#@&di+arDP0K.@#@&d+^dnk6P1@*\laKDKlTnevZ;.M+xOKmonO8b,Y4x@#@&MyoAAA==^#~@%>
        <tr class="tdbg"> 
          <td height="22" bgcolor="F2F8FF"><a href="<%=#@~^GgAAAA==c`wsKl9fkMPL~Y4+ok^+ Hm:+*qwgAAA==^#~@%>" target="_blank"><strong>&nbsp;<%=#@~^DAAAAA==O4+obVR1m:cAQAAA==^#~@%></strong></a></td>
          <td width="84" align="right" bgcolor="F2F8FF"><%=#@~^DAAAAA==O4+obVR/byqgQAAA==^#~@%>字节</td>
          <td width="134" align="center" bgcolor="F2F8FF"><%=#@~^DAAAAA==O4+obVRYHwsQQAAA==^#~@%></td>
          <td width="119" align="center" bgcolor="F2F8FF"><%=#@~^GAAAAA==O4+obVRfmYJlkY\W9kWrNIgkAAA==^#~@%></td>
          <td width="43" align="center" bgcolor="F2F8FF"><a href="Admin_UploadFileManage.asp?Action=Del&FileName=<%=#@~^FgAAAA==iaVGmNGkDLY4nsbVnRglhnGggAAA==^#~@%>" onClick="return ConfirmDel()">删除</a></td>
        </tr>
        <%#@~^WAAAAA==@#@&d7wk^+ZKE	O'wks+;W;UD_F@#@&diKWDsn?by'PWOs?ryQY4+or^+Rjr.+@#@&in	N~b0@#@&16D@#@&SBgAAA==^#~@%>
      </table>
      <%#@~^DwAAAA==@#@&+U9PkE4@#@&zwIAAA==^#~@%> </td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->
<%#@~^jQYAAA==@#@&/;(PktWSwmL+y`d0bVnUm:+BOWDlV	;h4DB:C62nMwCob@#@&d[rsPxS~b~dYMP:2@#@&ik0,YKOl^x;:(+.~sWN,hlXw+M2Co'ZPOtnU@#@&~P,~d	'~OKYlsU!:8+M~'Phm6a+DalTn@#@&P~dVdn@#@&P,~Pix',OGYmV	Eh4n.,-~:mawD2CT+_q@#@&P~dU9Pr6@#@&PPi/D.K:2',J@!Om4V~l^ko	xvmxD+.B@*@!6W.:,Uls+xvktWA2mon/E~s+O4W9'BhWkOB,l^YbWUxEJPL~/6kVUC:PLPEB@*@!DD@*@!D[@*r@#@&7kYDPnswx/D.:+haPLPJ共,@!(@*J,[~YKYCs	E:(nD,[Pr@!&4@*P个文件，占用,@!8@*E~LPPWDs+Uk"n'F!+*,[~J@!&(@*~n[	4/aiLU4kwI[	4d2pJ@#@&7/6kVUC:'xWrx/4mDc/6rVxCh#@#@&~,dr0,/!D.xDnlT+@!+PDtnx@#@&~~,PdidYMK+s2x/DD:+hw~',J首页~上一页[	8/aiE@#@&PP7n^/n@#@&~,P~idkYD:+s2'kY.K:2~LPJ@!CP4D+6xvJ,[,/Wksn	lh+,'PrwCL'Fv@*首页@!zC@*LU(/2pJ@#@&P,P,7dkY.K:2xkYD:n:aP[,E@!l,tM+W'vE,[~/6rVxChP[~EalL+{E,[~vZ!DDxDKlT+RF*P'~rB@*上一页@!&l@*[x(d2ir@#@&P~dnU9Pr0@#@&@#@&P~7b0PUR1E.DUDwCT+@!FPDtU@#@&P~P,d7dDDKhw{/YMPn:aPLPE下一页P尾页E@#@&P~ds/@#@&~,PP77kY.Kha'dDD:+:aPL~J@!l~tM+WxEJPL~/6kVUC:PLPEwCL'EPL~`;E..xYKCT+QF*~LPEE@*下一页@!zl@*[	8/aiE@#@&P~~,ddkOD:+:axdYMK:2P'~r@!CP4.+6'vE,[PdWbVnxmhP',Jalo'r~[,x~[,Jv@*尾页@!zl@*E@#@&PPinUN,k6@#@&P~~i/OD:n:a'dOMK+h2,[~JLU(/2p页次：@!kYDKxT@*@!6WUY,mGsKD'MnN@*JPL~/EMDxOnCLP'Pr@!z6WUO@*zJ~',x~[,E@!zdDDKxo@*页Pr@#@&,P~PkY.P:w{dYMK+s2~[,JLx8/2I@!4@*J,'Psla2DwCLP'Pr@!J4@*rPLPJ个文件J页J@#@&dkY.K:2xkYD:n:aP[,E'x(/ai转到：@!/nsmOP	C:'v2mo+v~kk"+{v8B~Kx1tl	oxBNl-lkm.raY)k;4skYvbv@*rP,P@#@&~~,PWWM~k,'~q,YW~U,P~@#@&~,P7i/DDK:ax/DDP+sw~',J@!K2YbWx,-CV!+{BEP'~bP'PrvJ@#@&77b0P^r	YcZ!.M+UDnmo+*'1rxD`r#,Y4n	P/D.K:w{dOD:+sw~[~E,/nV^YN~E@#@&d7dDDP+s2{/OMK:w,[,E@*第rP'PbP'~r页@!zK2YbWx@*E~P,@#@&dU+aO@#@&7/D.K:2xkYDPnsw~[,E@!zdVmY@*J@#@&dkY.K:2xkYD:n:aP[,E@!zDN@*@!&Y.@*@!zWWMh@*@!zOC(V+@*E@#@&7DdaWUk+chDbY~/DDP+sw@#@&xN,dE(@#@&2r8BAA==^#~@%>