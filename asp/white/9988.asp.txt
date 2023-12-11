<%@language=VBScript.Encode codepage=936 %>
<%#@~^TwAAAA==@#@&DnkwKx/R(;06+.'DD;n@#@&ZKU/DPn!.-khd+-+sxyP@#@&;GxkY~/4+mV/4lUxs&fxy@#@&yhcAAA==^#~@%>
<!--#include file="you.asp"-->
<!--#include file="conn.asp"-->
<!--#include file="zcm.asp"-->
<!--#include file="../inc/config.asp"-->
<!--#include file="admin.asp"-->
<!--#include file="inc/function.asp"-->
<%#@~^5gAAAA==@#@&NrsPzmYbW	SnmDnxDq9Sb~sK;x92DMS3DMHko@#@&[rsPj3bUZKEUOBSlzG!Y/W!UD@#@&zmDkW	'D.ks`]+$EndD`Jz^YbWxrbb@#@&nmDnxO(G'ODbh`M+5;/YcEhl.+	O&fE*#@#@&k6PhCDxOqG'EE,YtU@#@&dnm.nxDqG'T@#@&n^/n@#@&7nmDnUDqfx/dxL`hCM+UDqG#@#@&+	[Pb0@#@&g0MAAA==^#~@%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" border="0" align="center" cellpadding="2" cellspacing="1" class="table_southidc">
  <tr class="topbg"> 
    <td height="22" colspan="2" align="center"><strong>菜 单 栏 目 管 理</strong></td>
  </tr>
  <tr class="td_southidc"> 
    <td width="70" height="30"><strong>管理导航：</strong></td>
    <td height="30"><a href="Admin_Class_Menu.asp">菜单栏目管理首页</a> | <a href="Admin_Class_Menu.asp?Action=Add">添加菜单栏目</a>&nbsp;|&nbsp;<a href="Admin_Class_Menu.asp?Action=Order">一级菜单排序</a>&nbsp;|&nbsp;<a href="Admin_Class_Menu.asp?Action=OrderN">N级菜单排序</a>&nbsp;|&nbsp;<a href="Admin_Class_Menu.asp?Action=Reset">复位所有菜单栏目</a>&nbsp;|&nbsp;<a href="Admin_Class_Menu.asp?Action=Unite">菜单栏目合并</a></td>
  </tr>
</table>
<%#@~^TwUAAA==@#@&kW,b1YkKx{Eb9NEPDtnU@#@&d1CV^Pb9[/Vm/k`b@#@&n^/nk6~b1YrG	'JjC7+)N9E,Y4x@#@&d1l^sPUl-+zN[c*@#@&s/k0,)^YbW	'EHG[b0zJ,Otx@#@&imlss,HGNbWH`b@#@&V/k6~b1YrW	'Ejm\+tGNb0Xr~Otx@#@&7mCs^Pjl7nHKNrWH`#@#@&Vd+bW,b^DkKx'rHK-+rPOtx@#@&iml^sPtW\/slk/v#@#@&nsk+r0,)mDkGU{J?C-HG\E,Y4x@#@&d1l^sPUl-+tW-nv#@#@&nVk+k6~)mDkKxxJ9n^J~Y4nx@#@&7^mVV~9VnY/^ldk`*@#@&Vknk6P)mDkGU{JZ^nlMJPD4nx@#@&imCVs~;VnlM/Vm/dc*@#@&nsk+r0,)1YrKx{JjarM[+MJ~Y4+U~@#@&d1CV^Pja6.NDv#~@#@&n^/nk6~b1YrG	'J9GSx6D9nMJ~DtxP@#@&i^l^V~fKhU6MN+Mc#,@#@&sd+b0,b^YrG	'ErM[+MJ~O4+x@#@&imCV^~}D[Dv#@#@&+^d+b0~b1YrG	'J`2rMN+MHEPDtx~@#@&71lsV,iw}D[nM1`b~@#@&nVknb0~zmDkW	'r9WSx6D9+.HrPY4nx,@#@&i^CV^PGWAx6.9+.1vbP@#@&nsk+kW~zmOkKU{J6MND1rPD4+	@#@&d1lss,rD9nDg`#@#@&nVk+b0~b^ObWU'r]+k+OE,YtnU@#@&7mms^P]/Y`*@#@&nVk+r0,b^ObWx{E?m\+"nd+DJ,Y4+U@#@&d^l^sPUl-n"+/nOv#@#@&sk+r6PzmYbW	xJ`xrYJ~O4+x@#@&d1lV^~ixbY`b@#@&n^/nk6~b1YrG	'JjC7+ixbOJ~Dtx@#@&d1CV^Pjl7+iUbY+vb@#@&+Vkn@#@&immVsPhCbxc#@#@&+	N~r6@#@&^4`b@#@&r6PoKE	N2MD{PD!+~Y4+U@#@&dmmsV,DbOn2MDt/L`b@#@&+UN,r0@#@&^C^VP/sK/nZKU	`b@#@&@#@&@#@&/!8Pslrxv#@#@&iNks~lMD?4GASbx`q!b@#@&dWWM~k{!~OKPE8G!x[`m.M?4Khdkx#@#@&dil.DUtGAdkxck*'smsd+@#@&ixn6O@#@&d[ks~/$V/sm//S.kZslkdBkSbfwY4@#@&7/$V/Vm/dxr/+^nmDPe,o.WsPt+UE/sm/dPK.ND~8HPIGGDq9~}.9+.&fr@#@&i/OPM//Vm/dxk+D7nDcZDCO+}4N+^YcEmNGN( DmG.9/+OE*@#@&dMd;VCk/cWwx,d;^Zslk/S^KxxBq~8@#@&0IsBAA==^#~@%>
<br> 
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="table_southidc">
  <tr class="title"> 
    <td width="178" height="22" align="center"><strong>菜单名称</strong></td>
    <td width="188" align="center"><strong>菜单链接地址</strong></td>
    <td width="291" height="22" align="center"><strong>操作选项</strong></td>
  </tr>
  <%#@~^IgAAAA==~@#@&[KPStk^+,UWDP./;VCdkR+KWP@#@&TgkAAA==^#~@%>
  <tr class="td_southidc" onMouseOut="this.style.backgroundColor=''" onMouseOver="this.style.backgroundColor='#BFDFFF'"> 
    <td> <%#@~^mwUAAA==~@#@&7bfwY4'MdZ^ld/vJ9naYtrb@#@&dk6~./;Vm/d`EH6OqGE#@*!~O4+x@#@&idCDMj4WAdk	+`bf2Y4#xKMEn@#@&d+^d+@#@&diC.DUtKhJkUnvk9+aOt*'oC^/+@#@&i+UN,r6@#@&ik6PkG+aOt@*!~Y4+U@#@&dP,70KDPbxqPDW,k9+2O4P@#@&i7db0~r{kfn2Dt~Y4n	P@#@&diddb0,./;VC/k`EH6Y&9J*@*!,O4+	P@#@&7d77iDn/aGxk+ AMkYn~r@!r:T~kD^{B&:lT+JOD+mVbxnqcok6vPSkND4xB8GEP4+rL4YxB8B,\Csboxxvm4-:b[9VnE@*rP@#@&di7dVd+,@#@&7iddi.+kwW	dnRSDbYnPE@!b:LPk.m{B(hmo+&OM+n{^r	++cob0B,hb[Y4'vFFB~4ko4O'EFvE~-l^kTxxBC87:rN9s+E@*E~@#@&d77i+UN,r6P@#@&did+^/~@#@&d7dikW~mDDU4WSSk	nck*':D;+~O4+U@#@&7did7./wGUk+ hMrD+~r@!b:o,/M^'EqhlT+&OM++|sk	+&cLr0EPSk[Y4xEF{B,4+bo4O{BFv,\CVbL	'vm47:k9N^nB@*J~@#@&d77i+Vkn@#@&ddi77D/aWU/n SDrY~J@!khL,/D^xEqhlTnJY.+|Vk	+W ob0vPSk[O4'B8{B,t+bL4Y{B8vvP-C^kLx{vl(\hr9NVnv@*J~@#@&7id7x9Pk6@#@&7di+UN,kW~@#@&d,~d	+6D~@#@&iP,+UN~r6P@#@&i~Pb0~.kZVCdk`EZ4r^NE*@*ZPY4+	~@#@&d~PiDndaWxknRSDkDn~J@!kso~/.^{B(:mL+JY.n{0Gs9+.ccLb0v,hbNY4'Eq*EP4+bo4O{BFlvP7lVbLU'El(\hk[[^+v@*r~@#@&d~~V/n~@#@&7P,7M+daW	/+chMrYPE@!b:L~kDm{vqslo&OD+|0GV[nM& obWB,hr[Dt'vqlB~trTtO{B8*B,\mskTxxBm4-hbNN^nB@*JP@#@&7P,+	N~kW~@#@&7P,r0,Dd/^l/dcrfnwD4r#xZPDt+	P@#@&d,P7D/2G	/+cADbY+,E@!4@*J,@#@&d~~x[PbWP@#@&7~,D+d2Kxd+cAMkOPr@!l,tMn0{B)NskUm;Vlkd{t+x! C/agzmOkGU{HGNbWXLZsCk/q9xrP'PMd;VCk/vJZ^lkdqGJbPLPEv,YkDs+{BJ,'~DkZ^ld/cE"+CNtnJ*P'~rB@*E~LP./;sm/dvJ;Vlk/gC:JbPLPE@!Jl@*r@#@&iPPbW~DkZ^ld/cE;trV9E#@*!~O4+x~@#@&d~Pi./2Kxk+RSDbO+,J（EPLP.d;Vlkd`rZtbs[J*PLPE）J~@#@&d~PUN,kW@#@&dP~JIkBAA==^#~@%> </td>
    <td width="188" align="center"> <%#@~^kQAAAA==@#@&dGHRTlx	kvb@#@&dr0,Dd/^l/kcJdkx0i.Vr#@!@*EJ~O4+U@#@&7dM+d2Kx/n SDrY~M//^lk/`rSbU3`DsJ*@#@&7V/@#@&idDd2W	/RADrOPE没链接地址J@#@&dx[~b0d@#@&iMCQAAA==^#~@%> </td>
    <td align="center"><a href="Admin_Class_Menu.asp?Action=Add&ParentID=<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>">添加子菜单</a> 
      | <a href="Admin_Class_Menu.asp?Action=Modify&ClassID=<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>">修改设置</a> 
      | <a href="Admin_Class_Menu.asp?Action=Move&ClassID=<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>">移动菜单</a> 
      | <a href="Admin_Class_Menu.asp?Action=Clear&ClassID=<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>" onClick="return ConfirmDel3();">清空</a> 
      | <a href="Admin_Class_Menu.asp?Action=Del&ClassID=<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>" onClick="<%#@~^GwAAAA==r6P.kZ^l/k`r/tbV[J*@*T~Dt+	gAgAAA==^#~@%>return ConfirmDel1();<%#@~^BAAAAA==n^/nqQEAAA==^#~@%>return ConfirmDel2();<%#@~^BgAAAA==n	N~b0JgIAAA==^#~@%>">删除</a></td>
  </tr>
  <%#@~^JAAAAA==~@#@&7M/;Vlk/chW7+U+XY~@#@&VWK2P@#@&5wgAAA==^#~@%>
</table> 
<script language="JavaScript" type="text/JavaScript">
function ConfirmDel1()
{
   alert("此栏目下还有子栏目，必须先删除下属子栏目后才能删除此栏目！");
   return false;
}

function ConfirmDel2()
{
   if(confirm("删除栏目将同时删除此栏目中的所有菜单，并且不能恢复！确定要删除此栏目吗？"))
     return true;
   else
     return false;
	 
}
function ConfirmDel3()
{
   if(confirm("清空栏目将把栏目（包括子栏目）的所有菜单放入回收站中！确定要清空此栏目吗？"))
     return true;
   else
     return false;
	 
}
</script>
<br><br>
<%#@~^JQAAAA==@#@&+U9PkE4@#@&@#@&/!4~b9N/sm//vb@#@&twcAAA==^#~@%>
<form name="form1" method="post" action="Admin_Class_Menu.asp" onSubmit="return check()">
  <table width="100%" border="0" align="center" cellpadding="2" cellspacing="1" class="table_southidc">
    <tr class="title"> 
      <td height="22" colspan="2" align="center"><strong>添 加 菜 单 栏 目</strong></td>
    </tr>
    <tr class="td_southidc"> 
      <td width="350"><strong>所属菜单：</strong><br>
        不能指定为外部菜单 </td>
      <td> <select name="ParentID">
          <%#@~^KQAAAA==^mVs,b9:k	{U4WSZslk/m6aYkKU`Z~nm.nxDqG~T#cw4AAA==^#~@%>
        </select></td>
    </tr>
    <tr class="td_southidc"> 
      <td width="350"><strong>菜单名称：</strong></td>
      <td><input name="ClassName" type="text" size="37" maxlength="20"></td>
    </tr>
    <tr class="td_southidc"> 
      <td width="350"><strong>菜单说明：<br>
        </strong> 鼠标移至菜单名称上时将显示设定的说明文字（不支持HTML）</td>
      <td><textarea name="Readme" cols="30" rows="4" id="Readme"></textarea></td>
    </tr>
    <tr class="td_southidc"> 
      <td><strong>是否在顶部导航栏显示：</strong><br>
        此选项只对一级菜单有效。</td>
      <td><input name="ShowOnTop" type="radio" value="Yes" checked>
        是&nbsp;&nbsp;&nbsp;&nbsp; <input type="radio" name="ShowOnTop" value="No">
        否</td>
    </tr>
    <tr class="td_southidc"> 
      <td width="350"><strong>菜单链接地址：</strong><br>
        菜单链接到的URL地址。</td>
      <td><input name="LinkUrl" type="text" id="LinkUrl" size="37" maxlength="255"></td>
    </tr>
    <tr class="td_southidc"> 
      <td height="40" colspan="2" align="center"><input name="Action" type="hidden" id="Action" value="SaveAdd"> 
        <input name="Add" type="submit" value=" 添 加 " style="cursor:hand;"> &nbsp; 
        <input name="Cancel" type="button" id="Cancel" value=" 取 消 " onClick="window.location.href='Admin_Class_Menu.asp'" style="cursor:hand;"> 
       </td>
    </tr>
  </table>
</form>
<script language="JavaScript" type="text/JavaScript">
function check()
{
  if (document.form1.ClassName.value=="")
  {
    alert("栏目名称不能为空！");
	document.form1.ClassName.focus();
	return false;
  }
}
</script>
<%#@~^BgIAAA==@#@&+U9PkE4@#@&@#@&/!4~HKNrWH`#@#@&d9k:,/slk/&fS/5sBDdZ^C/k~r@#@&dZsCk/(f{OMkhvD;E/DcJ;VC/kq9E*#@#@&7k6PZ^Cd/&f{JEPO4x@#@&i7sKEU[ADDxPMEn@#@&7i2.MHko'ADM\/TP'Pr@!8.@*@!Vb@*参数不足！@!JVk@*E@#@&id6rY~d!4@#@&inVk+@#@&idZsCk/(f{/dxLvZ^l/kqGb@#@&dnx9PrW@#@&dk5V{J/snmDPCPoDGh,Hnx!/Vm/d~St+.n,Zslkd&fxrPLPZ^lkdqG@#@&dk+O~M/Z^C/k'/.-+MR;DnlOn}4%+1OPvJ)[KN4 .mGD9dYE*@#@&dDkZ^C/kRGwx~d$V~1Gx	~FBf@#@&ik6P.//sm/dR(G0,lU[,D//sm/dRG6PO4+	@#@&idwGE	N3DM'P.!+@#@&7dADDtdL'ADMHdo~',J@!4M@*@!^k@*找不到指定的栏目！@!JVk@*E@#@&7+^d@#@&xokAAA==^#~@%>
<form name="form1" method="post" action="Admin_Class_Menu.asp" onSubmit="return check()">
  <table width="100%" border="0" align="center" cellpadding="2" cellspacing="1" class="table_southidc">
    <tr class="title"> 
      <td height="22" colspan="2" align="center"><strong>修 改 菜 单 栏 目</strong></td>
    </tr>
    <tr class="td_southidc"> 
      <td width="350"><strong>所属菜单：</strong><br>
        如果你想改变所属菜单，请<a href='Admin_Class_Menu.asp?Action=Move&ClassID=<%=#@~^BwAAAA==/^ldkqGgwIAAA==^#~@%>'>点此移动菜单</a></td>
      <td> <%#@~^2gIAAA==@#@&dr6PM/Z^lkd`rnCDxO(GJ#@!x!,YtU@#@&iP,d.+d2Kxd+cADbYn~r无（作为一级栏目）J@#@&7Vd+@#@&,P~,d9k:,DkKlM+UY;VCdk~/$snmD+	O/Vm/k@#@&d7d$VKlMnxDZsCk/'EjVnmD~CPoMWsPHx!/Vm/dPStn.PZ^C/kqf,rUPvJ,[~Dd/^ld/vEnmDnUDnlO4r#~[,E*PGMNDP(X,9+aY4J@#@&77k+Y,./hlDUOZ^lk/x/n.7+.R;.+mYn6(L+^OvJCNK[(R.mKDNk+DE#@#@&7dM/KCM+xD/Vm//cG2+	Pk;snC.xOZ^C/k~^G	x~qS8@#@&di[KPA4k^+P	WD~DknCDxO/^l/k +K0@#@&77d6WMPr'q~DW~DkKlM+UO;VlddvJ9+aO4Jb@#@&iddiDdwKxd+ch.rD+Pr'x(/wp'U4kwp[U4d2pJ@#@&i7d	+aO@#@&d77b0~DkKmDn	Y;Vlk/vEfwOtr#@*T,YtU@#@&ddi7.+kwKxd+ AMkO+,E└J@#@&77i+x[~b0@#@&i7iDnkwKx/RS.kD+~JLx8daiJ,'PM/nm.nxDZ^ld/cE;VC/kHls+Eb,[PE@!(D@*J@#@&id7M/hlDxD/Vm/dRsW-n	+6D@#@&idVKG2@#@&diDdnC.xOZ^C/kR^sK/+@#@&idd+D~M/KmDxY;Vmd/{xGY4kUL@#@&dUN,k0@#@&7LtMAAA==^#~@%> </select></td>
    </tr>
    <tr class="td_southidc"> 
      <td width="350"><strong>菜单名称：</strong></td>
      <td><input name="ClassName" type="text" value="<%=#@~^FAAAAA==.kZsm/k`J;Vmd/glh+r#5wYAAA==^#~@%>" size="37" maxlength="20"> 
        <input name="ClassID" type="hidden" id="ClassID" value="<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>"></td>
    </tr>
    <tr class="td_southidc"> 
      <td width="350"><strong>菜单说明：<br>
        </strong> 鼠标移至菜单名称上时将显示设定的说明文字（不支持HTML）</td>
      <td><textarea name="Readme" cols="30" rows="4" id="Readme"><%=#@~^EQAAAA==.kZsm/k`J"+m[HJbngUAAA==^#~@%></textarea></td>
    </tr>
    <tr class="td_southidc"> 
      <td><strong>是否在顶部导航栏显示：</strong><br>
        只选项只对一级菜单有效。</td>
      <td><input name="ShowOnTop" type="radio" value="Yes" <%#@~^OwAAAA==r6P.kZ^l/k`rjtKh6x:W2E*'KM;+,YtU~D/aWU/n SDrY~J,m4n13+[EzxQAAA==^#~@%>>
        是&nbsp;&nbsp;&nbsp;&nbsp; <input type="radio" name="ShowOnTop" value="No" <%#@~^PAAAAA==r6P.kZ^l/k`rjtKh6x:W2E*'sms/PY4nUPM+kwGxdnch.kDnPrP^4m3n[rGhUAAA==^#~@%>>
        否 </td>
    </tr>
    <tr class="td_southidc"> 
      <td width="350"> <strong>菜单链接地址：</strong><br>
        菜单链接到的URL地址。</td>
      <td><input name="LinkUrl" type="text" id="LinkUrl" value="<%=#@~^EgAAAA==.kZsm/k`Jdk	VjMVE#MQYAAA==^#~@%>" size="37" maxlength="255"></td>
    </tr>
    <tr class="td_southidc"> 
      <td height="40" colspan="2" align="center"><input name="Action" type="hidden" id="Action" value="SaveModify"> 
        <input name="Submit" type="submit" value=" 保存修改结果 " style="cursor:hand;"> 
        &nbsp; <input name="Cancel" type="button" id="Cancel" value=" 取 消 " onClick="window.location.href='Admin_Class_Menu.asp'" style="cursor:hand;"> 
        </td>
    </tr>
  </table>
</form>
<script language="JavaScript" type="text/JavaScript">
function check()
{
  if (document.form1.ClassName.value=="")
  {
    alert("栏目名称不能为空！");
	document.form1.ClassName.focus();
	return false;
  }
}
</script>
<%#@~^QwIAAA==@#@&dn	N,k0@#@&i./;VC/kR^sK/+@#@&dk+Y,.dZ^lk/xxGO4kUo@#@&+	N~d!4@#@&@#@&/;4,\K\n;Vm//v#@#@&d9khP;VCdkqfBd;^~Dk/slk/Bk@#@&7/^ld/&9'DDrhvD+5;/O`r/^ldkqGJ#*@#@&7k6P/Vm/d(G'Jr~Y4+x@#@&7dwW!x[2..{K.E@#@&id3.MH/LxAD.HkL,[~r@!(D@*@!Vb@*参数不足！@!JVr@*r@#@&7i+6bOPkE4@#@&7+^/@#@&d7/^ld/&9';SULvZVCdkq9#@#@&i+U9Pb0@#@&d@#@&dk;s'r/nsmY,MPwDWs~\+	E;VC/d~StnD~Z^ldd&f'E~LP/Vmdkq9@#@&i/+DPMdZ^ld/{/n.7+Dc/DlY68LmDPcJ)[KN8RMnmKD[dYJb@#@&d./;sm/dcWa+x,/$s~1WUxBFSf@#@&dbWPM/Z^Cd/c4K0~lU[,DdZ^C/kRnG6PY4n	@#@&dioKEU92MD':D!n@#@&d72MD\dT'2M.HkoPL~E@!(D@*@!sk@*找不到指定的栏目！@!JVr@*r@#@&i+sd@#@&K5oAAA==^#~@%>
<form name="form1" method="post" action="Admin_Class_Menu.asp">
  <table width="100%" border="0" align="center" cellpadding="2" cellspacing="1" class="table_southidc">
    <tr class="title"> 
      <td height="22" colspan="2" align="center"><strong>移 动 菜 单 栏 目</strong></td>
    </tr>
    <tr class="td_southidc"> 
      <td width="200"><strong>菜单名称：</strong></td>
      <td><%=#@~^FAAAAA==.kZsm/k`J;Vmd/glh+r#5wYAAA==^#~@%> <input name="ClassID" type="hidden" id="ClassID" value="<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>"></td>
    </tr>
    <tr class="td_southidc">
      <td width="200"><strong>当前所属菜单：</strong></td>
      <td>
        <%#@~^mQIAAA==@#@&dr6PM/Z^lkd`rnCDxO(GJ#@!x!,YtU@#@&iP,d.+d2Kxd+cADbYn~r无（作为一级栏目）J@#@&7Vd+@#@&,P~,d9k:,DkKlM+UYB/5shlDUY@#@&did5VhlM+UYxEU+s+1OPCPo.K:P\n	E/VmdkPA4+M+P;Vmd/&f~k	PcE,[PMdZ^l/kcEnmDxOnCO4JbPL~J*PG.9+D~8HP9+aO4J@#@&di/+DPMdnmDnxD'dnM\+M ZM+lDn64N+1YcJC[KN8RMnmKD[dYJb@#@&d7DkKmDn	YcWwx,d;^nCDxOS1Wx	SFBF@#@&77NKPStrVn~	WOPMdnmDnUDR+GW@#@&7diWKD~b'8PYKPMdnmDnxD`E9wY4E#@#@&di77D/aWU/n SDrY~JLx8dai[U8kwI[	8kwIr@#@&ddixaY@#@&7dikW~M/nm.+	Y`r9nwDtr#@*!~O4+U@#@&7did.nkwWUdRADbOPE└r@#@&ddi+	[Pb0@#@&id7./wKU/RhMrO+,JLx8/2IrP'PMdnmDnUD`J/sm/d1mhJb,[,J@!(D@*E@#@&d7dM/KCM+xD :K\+	naY@#@&idsWG2@#@&7dMdnmDnUDRmsGk+@#@&i7k+O,DknlM+	O'	WOtbxL@#@&d+	[Pb0@#@&7sLkAAA==^#~@%>
      </td>
    </tr>
    <tr class="td_southidc"> 
      <td width="200"><strong>移动到：</strong><br>
        不能指定为当前菜单的下属子菜单<br>
        不能指定为外部菜单</td>
      <td><select name="ParentID" size="2" style="height:300px;width:500px;"><%#@~^MgAAAA==^mVs,b9:k	{U4WSZslk/m6aYkKU`Z~Dk/slk/vJKl.n	Y(frb#hxEAAA==^#~@%></select></td>
    </tr>
    <tr class="td_southidc"> 
      <td height="40" colspan="2" align="center"><input name="Action" type="hidden" id="Action" value="SaveMove"> 
        <input name="Submit" type="submit" value=" 保存移动结果 " style="cursor:hand;">
        &nbsp; 
        <input name="Cancel" type="button" id="Cancel" value=" 取 消 " onClick="window.location.href='Admin_Class_Menu.asp'" style="cursor:hand;"></td></tr>
  </table>
</form>
<%#@~^QgEAAA==@#@&dn	N,k0@#@&i./;VC/kR^sK/+@#@&dk+Y,.dZ^lk/xxGO4kUo@#@&+	N~d!4@#@&@#@&/;4,6MNnM`*P@#@&d9r:,/5V;VCdk~Dk/Vm//BrSk;W!xO~%~@#@&7/$sZ^ldd{J/nsmOPC~wDGsPt+x!Z^C/kPAtDn~hlDUY&f'Z~GD9+MP8X~]KWOqGEP@#@&7dYP.d;VC/kxk+.7+MRZM+mO+}4%+1YcEmNW98RM+mK.[/Yr#~@#@&7M//Vmd/cW2n	P/5s;VC/kS1WU	~8~F,@#@&7k;W;xD'.d;VlkdRM+mK.[mKE	Y~@#@&bGEAAA==^#~@%>
<br>
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="table_southidc"> 
  <tr class="title">  
    <td height="22" colspan="4" align="center"><strong>一 级 栏 目 排 序</strong></td> 
  </tr> 
  <%#@~^KgAAAA==~@#@&%{F,@#@&9W,AtbVnP	WO~M/Z^C/kR+KW~@#@&XQoAAA==^#~@%> 
    <tr class="td_southidc" onMouseOut="this.style.backgroundColor=''" onMouseOver="this.style.backgroundColor='#BFDFFF'">  
      <td width="200">&nbsp;<%=#@~^FAAAAA==.kZsm/k`J;Vmd/glh+r#5wYAAA==^#~@%></td> 
<%#@~^ZwUAAA==~@#@&7b0,L@*8PD4+	P@#@&,P77M+/aGxk+RS.rYPr@!WW.h,l^YbGx{B)[skxm/^ld/|\x;clkwgzmDrW	'iw}D[nMBPsnY4WN{v2WkYE@*@!Y[~Sk[Y4xB8*Tv@*JP@#@&id.+k2KxdRSDkD+,E@!k+s+1Y~Um:+{\W7+1!h~/by'q@*@!GaYrW	~\mV;n{!@*向上移动@!&KwOkKU@*J~@#@&id0KD,r'8POW,LRq,@#@&i7dM+/aGU/RSDrYn~r@!GwDrW	P-C^E+xELk'J@*ELk'r@!JWwDkKU@*rP@#@&idUnXYP@#@&diD+k2Gxk+ch.kOn,J@!zknVmO@*rP@#@&7iDn/aG	/nchMkYPr@!k	w;Y,Yz2'tb[NxP	Ch+{Z^ld/(9,\CV!n'r[.d;VlddvJ/Vmdkq9r#LJ@*r@#@&7dM+dwKxdnchDbO+,J@!bU2EDPDX2+x4bN[+	~xm:nx1IWGO&f~\ms!+xr[M/Z^lkd`rIGWDq9E*[J@*'x(/wp@!rxaEDPOX2n{/;4srY,xCh'?;8skOP7C^En{修改@*rP@#@&di.+kwGxk+ AMkY~J@!zY9@*@!z6WM:@*J~@#@&dnVknP@#@&77M+/2G	/nRS.bYn,J@!YN,hb[Y4'vFl!v@*Lx4k2i@!zY9@*EP@#@&i+UN~r6P@#@&ir0,k/G!xY@*%,Y4+	~@#@&~,diD+kwKU/RADbYn~r@!0K.:,lmDrGx{BzNhkUm;VC/kmHx; m/w_)1YrW	xGWA	rMN+MB,h+DtGN{B2GkYB@*@!Y9Phb[Ot{B8*TB@*E,@#@&di.+kwGUk+RA.bYnPr@!k+smDPxm:xHK\n1!:~dby+{q@*@!WwDrGx,\mV;+xT@*向下移动@!&WaOkKx@*E,@#@&776W.Pbx8POKPbZW!xDRL,@#@&did.nkwW	d+chDbOnPr@!KwOkGU,\CV!n'r[r'r@*J'rLJ@!zK2DkG	@*rP@#@&diU+XY~@#@&d7./wKU/RhMrO+,J@!zd+sn1Y@*J,@#@&id.nkwWUdRADbOPE@!k	wEDPDzw'4k9NnU,xlsn';Vlkd(f,\mV;+xELDdZ^C/k`E/^l/d(GJb[r@*r@#@&idM+/aW	d+ch.kD+~E@!kxa;Y,YXanxtbN9+UPUCs+xm"GWDq9~7lV;n{J'Dk/^ldk`rIWKY&9J*[E@*Lx8dai@!bUw!YPDz2+{/!4hkO~	lh+{jE(:rO,\ls;'修改@*J,@#@&d7M+kwW	/ hMkO+,J@!&DN@*@!&0KD:@*E~@#@&dVd+~@#@&d7DdwKxdnchDrOPE@!D[,hr9Y4'B8*Zv@*Lx8/ai@!&DN@*r~@#@&d+	[~k6P@#@&FpABAA==^#~@%> 
      <td>&nbsp;</td>
	</tr> 
  <%#@~^LwAAAA==~@#@&7N'N_F,@#@&7DkZslk/ hK\+	n6DP@#@&sGWaP@#@&lAoAAA==^#~@%> 
</table> 
<%#@~^KwEAAA==~@#@&7M/;Vlk/c^VK/nP@#@&7dYPMdZ^l/kxUWDtbxLP@#@&x[Pk;4,@#@&@#@&/E8~}D[+MHv#~@#@&iNksPk5V;VC/k~.d;Vlkd~b~k;G;xD~DDd~i2tW-+g;:BfGA	HW-ngEhP@#@&i/5^Z^l/k'rd+^+^Y,e~oMW:,\+	EZ^Cd/,WMNnD~8HP]WKOqG~6.9+D(9rP@#@&idY~M/;Vlk/{d+M\nDcZ.nmY+}8LmYvECNKN(R.+^GMNd+DE#,@#@&7M/ZsCk/ Wan	Pd$V;Vlk/B^W	xSFBF~@#@&fFsAAA==^#~@%>
<br>
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="table_southidc"> 
  <tr class="title">  
    <td height="22" colspan="4" align="center"><strong>N 级 栏 目 排 序</strong></td> 
  </tr> 
  <%#@~^IgAAAA==~@#@&[KPStk^+,UWDP./;VCdkR+KWP@#@&TgkAAA==^#~@%> 
    <tr class="td_southidc" onMouseOut="this.style.backgroundColor=''" onMouseOver="this.style.backgroundColor='#BFDFFF'">  
      <td width="300"> 
	  <%#@~^NgIAAA==~@#@&76WMPk{F,OW,DdZ^lddvJf2Y4J#,@#@&d,PiDn/2G	/nRS.kD+~ELx4d2p[U4k2p[U(/aiJ,@#@&7x6OP@#@&7r6PDk/Vm//vE/tbV9Jb@*T~Dtnx,@#@&id.nkwWUdRADbOPE@!ksoPkD1xB&:CozO.+{6GV9+DW Lk6B,hrNO4{Bq*E~tkL4D'BqXEP-l^rTxxEl(\:bN9s+E@*EP@#@&7n^/+,@#@&iPPi.n/aW	/nRA.bYnPr@!kso~dMm'v(slL+JOM+n|0KVND2 ob0vPSk[O4'B8XB,t+bL4Y{B8*vP-C^kLx{vl(\hr9NVnv@*J~@#@&7x[,k6P@#@&dbWPM//Vm/dcrnlMnxDqfrbx!,Y4+UP@#@&id.+k2W	/n SDkOn,J@!4@*E,@#@&i+	NPb0,@#@&iDn/aWUdRhMrYPDk/slk/vJ/VCdk1C:E#,@#@&7b0P.d;VC/kcrZ4bV9J#@*!,Otx~@#@&d7./wKU/RhMrO+,JvJ~[~.kZslkd`rZ4r^NJb~LPE#r~@#@&7x9Pk6P@#@&dMp4AAA==^#~@%></td> 
<%#@~^pgcAAA==~@#@&7b0,D/;Vmd/vJKlM+UO&fJ*@*!,YtU~P,B如果不是一级栏目，则算出相同深度的栏目数目，得到该栏目在相同深度的栏目中所处位置（之上或者之下的栏目数）,@#@&d7v所能提升最大幅度应为wW.PbxF,YG~该版之上的版面数,@#@&77k+OPD.k'^Kx	R+X+1;Y`E/Vn^DPmK;xD`Z^Cd/&f*PoDGh,Hnx!/Vm/d~St+.n,nCDUDq9{JLD/;Vmd/vJKlM+UO&fJ*'J,lx9~6D9+Mq9@!E'M//Vmd/vJ6.9+D(9r#'Jrb,@#@&id`wHK\HEs'ODk`Tb,@#@&i7k6PkkU;V^``w\W-ngEh#,Otx~iaHW-ngEh'Z~@#@&7ik6PjaHK-+gEh@*ZPO4xP@#@&P,ddi.n/aW	/nRA.bYnPr@!0KDh~mmYrG	'vb9hbxm;Vm//|HUEcldwQb^ObWx{iw}DN.HB,:Y4W[xEwG/Dv@*@!Y[~SkNO4{Bq*Zv@*J~@#@&iddM+k2W	/nRSDrOPJ@!d+^+mD~Uls+{HG\nH!:~/b"+{F@*@!KwYrG	P-l^;'T@*向上移动@!JWwDkKU@*rP@#@&id7WKDPbxF,YW,i2HK\1;:~@#@&d7di.+kwGUk+RA.bYnPr@!KwObW	P\mV!n'r[r[r@*E'b[J@!&WaYkKU@*J,@#@&d7dUnXY~@#@&7diDndaWxdnch.kDn,J@!J/V+1Y@*EP@#@&7diDndaWxknRSDkDn~J@!k	w;Y~OHwn'4rN9+U~	l:nx;VC/k(GP-mV!+'r[MdZ^ld/vJ/sm//&9J*[J@*'U4kwp@!rx2;DPOXan'kE8hbYPUCs+x?!8skO,\mVE'修改@*EP@#@&7diDndaWxknRSDkDn~J@!zDN@*@!&WKDh@*r~@#@&d7n^/+~@#@&d7dMnkwG	/RhMkDnPr@!ON,hr[Dt'Eq*ZB@*LU8/ai@!zON@*E,@#@&dinx9PrW,@#@&77DDdR1sK/n,@#@&ddE所能降低最大幅度应为sK.Pb'qPDW~该版之下的版面数~@#@&did+DPYMdxmKx	Rn6n^!Yn`rd+^+^O,mW;UD`/Vmdkq9*PwDWsPtnx!Zslk/~A4+D~nmD+	O(f{JLDdZsCk/cJhCDxO(GJ#'E,lUN,GMNnMqG@*JLDk/Vm/d`rW.[DqGE#LJJ*~@#@&idGWAx\G7+HEsxYM/cT*P@#@&7ikWPbd	Es^`GWh	HK-+gEh#,Y4n	PfKAxtW\H;:{!,@#@&d7r6P9WSUHK\nH!:@*T~Dtnx,@#@&P~idiD+kwKU/RADbYn~r@!0K.:,lmDrGx{BzNhkUm;VC/kmHx; m/w_)1YrW	xGWA	rMN+M1E~:Y4W9'v2K/YE@*@!DNPSr[Y4'EFX!v@*rP@#@&i7dM+d2Kx/n SDrY~r@!dVmY,xmh+{HG\1;h,/k.n'8@*@!K2OkKx,\CV;n{!@*向下移动@!JGwDkGU@*JP@#@&id70K.,kx8PDWPGWSUHK\n1!:~@#@&ddi7D/wKUd+chMkO+~E@!W2YbGx,\Cs!+'E'b[E@*r'b[E@!zKwYbW	@*J,@#@&didUnXYP@#@&didDd2W	/RADrOPE@!Jd+^+^O@*JP@#@&id7DdaWUk+chDbY~J@!kUw!Y~OHw+{4k9N+	~Uls+{Zsldd&f~\msE'E'M/ZsCk/cJ;sm/d&fr#[r@*LU4kwI@!bx2;DPYH2+{/E(hrY,xm:n'j;(:rY,-l^Enx修改@*JP@#@&id7DdaWUk+chDbY~J@!zON@*@!&WKD:@*EP@#@&dins/P@#@&7d7./2W	d+ch.rD+PE@!DN~hb[DtxEFl!B@*[	8/ai@!zDN@*E,@#@&i7+	NPbW~@#@&diY./ ^^Wd+,@#@&i+sdP@#@&7iDn/aG	/nchMkYPr@!Y9P^W^/2C	' @*'x(/wp@!&Y9@*rP@#@&7n	N~k6~@#@&OxUCAA==^#~@%> 
      <td>&nbsp;</td>
	</tr> 
  <%#@~^SAAAAA==~@#@&7`wtW\1!h'ZP@#@&ifGA	HW7n1!:'Z~@#@&iDkZslddc:G\U+XY~@#@&VWG2,@#@&LBIAAA==^#~@%> 
</table> 
<%#@~^UQAAAA==~@#@&7M/;Vlk/c^VK/nP@#@&7dYPMdZ^l/kxUWDtbxLP@#@&x[Pk;4,@#@&@#@&/E8~"+d+Dc*P@#@&NRQAAA==^#~@%>
<br>
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="table_southidc"> 
  <tr class="title">  
    <td height="22" colspan="3" align="center"><strong>复 位 所 有 菜 单 栏 目</strong></td> 
  </tr> 
    <tr class="td_southidc">  
    <td align="center">  
      <form name="form1" method="post" action="Admin_Class_Menu.asp?Action=SaveReset"> 
        <table width="80%" border="0" cellspacing="0" cellpadding="0"> 
          <tr>  
            <td height="150"><font color="#FF0000"><strong>注意：</strong></font><br>
              &nbsp;&nbsp;&nbsp;&nbsp;如果选择复位所有菜单，则所有菜单都将作为一级菜单，这时您需要重新对各个菜单进行归属的基本设置。不要轻易使用该功能，仅在做出了错误的设置而无法复原菜单之间的关系和排序的时候使用。 
            </td> 
          </tr> 
        </table> 
        <input type="submit" name="Submit" value="复位所有菜单">
         &nbsp; <input name="Cancel" type="button" id="Cancel" value=" 取 消 " onClick="window.location.href='Admin_Class_Menu.asp'" style="cursor:hand;">
      </form></td>
    </tr>
</table>
<%#@~^IgAAAA==@#@&+U9PkE4@#@&@#@&/!4~j	kOnv#@#@&vQYAAA==^#~@%>
<br>
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="table_southidc">
  <tr class="title"> 
    <td height="22" colspan="3" align="center"><strong>菜 单 栏 目 合 并</strong></td>
  </tr>
  <tr class="td_southidc"> 
    <td height="100"><form name="myform" method="post" action="Admin_Class_Menu.asp" onSubmit="return ConfirmUnite();">
        &nbsp;&nbsp;将菜单
<select name="ClassID" id="ClassID">
        <%#@~^IgAAAA==^mVs,b9:k	{U4WSZslk/m6aYkKU`8~!BTbrQsAAA==^#~@%>
        </select>
        合并到
        <select name="TargetClassID" id="TargetClassID">
        <%#@~^IgAAAA==^mVs,b9:k	{U4WSZslk/m6aYkKU`8~!BTbrQsAAA==^#~@%>
        </select>
        <br> <br>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
        <input name="Action" type="hidden" id="Action" value="SaveUnite">
        <input type="submit" name="Submit" value=" 合并菜单 " style="cursor:hand;">
        &nbsp;&nbsp; 
        <input name="Cancel" type="button" id="Cancel" value=" 取 消 " onClick="window.location.href='Admin_Class_Menu.asp'" style="cursor:hand;">
      </form>
	</td>
  </tr>
  <tr class="td_southidc"> 
    <td height="60"><strong>注意事项：</strong><br>
      &nbsp;&nbsp;&nbsp;&nbsp;所有操作不可逆，请慎重操作！！！<br>
      &nbsp;&nbsp;&nbsp;&nbsp;不能在同一个菜单内进行操作，不能将一个菜单合并到其下属菜单中。目标菜单中不能含有子菜单。<br>
      &nbsp;&nbsp;&nbsp;&nbsp;合并后您所指定的菜单（或者包括其下属菜单）将被删除，所有菜单将转移到目标菜单中。</td>
  </tr>
</table> 
<script language="JavaScript" type="text/JavaScript">
function ConfirmUnite()
{
  if (document.myform.ClassID.value==document.myform.TargetClassID.value)
  {
    alert("请不要在相同栏目内进行操作！");
	document.myform.TargetClassID.focus();
	return false;
  }
  if (document.myform.TargetClassID.value=="")
  {
    alert("目标栏目不能指定为含有子栏目的栏目！");
	document.myform.TargetClassID.focus();
	return false;
  }
}
</script>
<%#@~^EQAAAA==~@#@&n	N,/E(P@#@&DwMAAA==^#~@%> 

</body> 
</html> 
<%#@~^YoQAAA==~@#@&@#@&/!4PUl7nb9Nc#@#@&7[b:P;slk/qGS/Vm/k1C:nSUtGh}UKKwS]lNhnBSrx0iMVShD\rMN.qG@#@&d9kh~k;VB./BYDk@#@&d9ksP]WGO&fSnm.+	Y9naYtSKmDnxDKmY4BnmD+	YUODBnCDxOHm:+B\lXZVmddqG~tlaIGGDq9@#@&7Nb:~KM+\(9B1n6D(G~/4k^N@#@&@#@&7Z^ld/glhn{YDbh`M+;!ndYvJ;VC/dHm:nJ*b@#@&dj4KhrUPKwxYMrs`.;!+/D`rjtKh6x:W2E*#@#@&7IlNsnxYMks`.+5;/O`r]+mNhnr##@#@&iSrx0iMVxDDb:`M+$;+kYcJdkUV`DVrb#@#@&dbW~Z^lk/Hlhn{JEPD4+	@#@&7isW;U92.D{PMEn@#@&id2MDtdo{2.Dt/L~LPJ@!8D@*@!Vb@*栏目名称不能为空！@!z^k@*J@#@&7n	N~k6@#@&ikW~UtWA6	KGw{EI+drPDt+	@#@&7dUtGh}xPGa'KM;+@#@&dsd+@#@&idjtGA}xPWaxsmVdn@#@&dnU9Pr0@#@&ikW,sKEx92M.':D;+,Y4n	@#@&i7+XkY,d;4@#@&i+UN~r6@#@&@#@&7/Y~.kP'~^KxURam;D+vJ/V^Y,HC6vZsCk/qGbPwDWs~\+	E;VC/dE*@#@&dtC6;VCdkqfx.k`T#@#@&ikW,kkxE^Vv\lXZslk/(9*PY4nx@#@&di\C6;Vm/dq9xZ@#@&dUN,kW@#@&dDd 1VG/@#@&d/^lk/qG'tC6;VC/kq9Q8@#@&id+DPDkx^W	xc+a+^;D+cJknVmO~sl6c.KWOk9b,s.K:,H+	E;slk/E#@#@&7\m6IKGY&f'Mdc!*@#@&dr0~rkx;V^cHm6]GKYq9b,Y4+	@#@&d7tlXIWKY&9'Z@#@&dx[~b0@#@&7DkRm^Gd+@#@&iIGWO(G'\lX]WKY(93F@#@&7@#@&7k6~hl.xDqf@*!,Otx@#@&idd5^'JknVmY,M~sMWsP\+U;;VC/k~h4+.n,ZVCdkq9'r~LPKmDxY&f,'PrJ@#@&id.dcWwUPk;VB^Gx	~8~q@#@&7ikWPMdR(WW~mxN~.kRnW6~Dtn	@#@&ddisK;x92.D{K.;@#@&i7dADDtdL'ADMHdo~',J@!4M@*@!^k@*所属栏目已经被删除！@!JVk@*Ei@#@&din	N~b0@#@&dik6~sKEUNAD.x:DE~Y4+x@#@&7diDkR^VGd@#@&di7/Y~.k'xGO4kUo@#@&id76bYPkE(@#@&idnVk+7@#@&ddi]WKYqGx./vJ"WGY(9r#@#@&i7dhl.n	Y1Ch'./vE;VCk/gl:J*@#@&id7nmDnUDf+aOt{D/vE9+aY4Jb@#@&7idKlMnxDnCO4'DdcrnCDUDnCDtr#@#@&di7Z4ksN{DdcrZtbsNr#@#@&77dhlM+UYKCDtxnm.+	YKCDtP'~r~EPL~hl.xDqf,P,~PE得到此栏目的父级栏目路径@#@&didK.\rM[+Mqf{.d`rrMNnD(9r#@#@&i7db0~/4kV[@*ZPOtUid@#@&didd9ks~Dkn.+7r.[DqG@#@&iddiv得到与本栏目同级的最后一个栏目的6D9+Mq9@#@&7id7/OPM/K.\r.[D(f{^KxUc+X+m!YcJk+s+1Y~\m6`}.NDqGb~sMWsP\+U;;VC/k~h4+.n,nl.n	Y(f{E,[~hlM+xDqGb@#@&d7din.n7rD9nD&f'MdKD\}D[+.(G`T#@#@&did7dYPO.k'^W	Uc+am!Y+vJknVmOP;VCdkqf,WDK:PtnUE;Vm/dPA4DnPhCDxO(G'J~',nCDUDq9,[,JPmx9~rMNnD&fxE,[Ph.+7rD9n.qG#@#@&7d77hDn\&9'DDdcZ#@#@&7id7@#@&7id7E得到同一父栏目但比本栏目级数大的子栏目的最大rMN+MqG，如果比前一个值大，则改用这个值。@#@&id7dk+O~M/nMn\}DN.(f{mKxURnam;YcJk+sn1YP\CX`6D9nMq9*PwDWsPtnx!Zslk/~A4+D~nmD+	OKlDt,Vr3n~EJ~[,KlM+UOhlY4~LPE~uvr#@#@&diddb0,cxKYcDkn.n7rD9nD&fR(GWPmx9P./K.\6D9nD&f nK0#b~Dtnx@#@&id7idb0P	WD~qk1;V^`.dhD+76D9+D&9c!*#,POtnU@#@&7di7PidrW,D/K.\6D9nMq9v!*@*nM+76D9+.qGPO4x@#@&7diddi7KD\}D[+.(G'./h.+7r.[Dq9cZ#@#@&i7id7i+	NPb0@#@&did7dx[~b0@#@&7did+	[~k6@#@&d7dnsk+@#@&i7din.n7qfxT@#@&7din	N~b0@#@&@#@&dinx9Pr0@#@&77M/R1sWk+@#@&7nVk+@#@&7drW,HC6"GWDq9@*ZPY4n	@#@&di7k+O,YM/'1W	UR6nm!Yncr/+^nmDPZ^Cd/&f,0.Wh~t+UE;slk/~A4+Dn~"WGY&9{J~LPtl6"WKOqGP'PrPCU9Pf2Y4'!rb@#@&idin.+-(G'ODkc!*@#@&7idY.dcmsWkn@#@&7i+^/+@#@&i7dhDn\&fxT@#@&dinx9Pk6@#@&dinM+-r.[D(f{T@#@&d7KmD+UOhlOt{EZJ@#@&dxN,k6@#@&@#@&7/$VxEU+V^Y,ePw.G:,Hx;ZsCk/~4nDPKCM+xO(G'EPL~hl.xDqf,[,EPz19P;VCdk1lsn'EJPL~/Vm/k1C:n~LPEBr@#@&i/nO,D/xdD-+M ;DnmYr4N+1O`rl[W94 .mWM[/YJ*@#@&dM/cW2+U~k;s~1Gx	~qS8@#@&7r6PUWDcM/ (W6Pl	N,./c+G0*PO4x@#@&7dwWE	[3DM':D;+@#@&idr0,KlM+UO&f'T~Dtnx@#@&id7ADMH/T'A.Dt/LPLPE@!(D@*@!sk@*已经存在一级栏目：JPL~/Vm/k1C:n~LPE@!Jsk@*J@#@&id+sd@#@&di7AD.t/T'2MDtdo,[~J@!4.@*@!Vk@*“EPLPnm.nxD1m:nP'~r”中已经存在子栏目“J~[,/Vm/dHm:+~',J”！@!z^r@*J@#@&di+x9PbW@#@&d7DkR^sK/+@#@&di/+D~./{xKY4kUL@#@&7dakDPd;(@#@&7n	N~k6@#@&d.kR1VWk+@#@&@#@&dd;^'EjV+1OPDWw,q~e,sMWhP\n	E/Vmd/r@#@&7M/RG2x~/$sBmG	xBF~2@#@&~P,P./cl[[	+h@#@&dM/`r/slk/&fE#x/^ld/&9@#@&P~~iD/cE;VC/kHm:nr#{ZVm/kHls+@#@&iDdcr?tKAr	KWaEb'UtKh6xPGa@#@&dMd`rIGGDqfEb{IGWD(G@#@&iDk`JhlMnxDq9J*'KCM+xD(f@#@&dbW~nmDxOq9@*ZPOtU@#@&d7.k`J9naY4J*xhl.xDf+aY4QF@#@&7+^/n@#@&ddMd`rf+aO4J*'Z@#@&dnU9Pr0@#@&dM/cEhlDnUDnCY4E*'KmDxYhlD4@#@&d./vJ6.9+D&9J*'nMn-rMND(f@#@&iDd`r/tbV[E*'!@#@&iDd`r]l[s+r#'"+m[:@#@&dM/cEdkx0iD^J#{Jrx0jMV@#@&7.k`EnMn\&fEb{nDn-&f@#@&i.k`Eg+XYqGJ*x!@#@&7DkR;29lY@#@&iD/c/sWk+@#@&~P~~k+OPMd'gWO4bxo@#@&i@#@&dE更新与本栏目同一父栏目的上一个栏目的“H6O&f”字段值@#@&db0,KD\(f@*!~O4+x@#@&dimW	U +X+1EO+cE!w[lDnPt+U;;Vldd,/nY,H6O&f{JPLP;slk/(f,[~E,ht.+,ZVmddqG'rP'PK.\(f*@#@&i+U[,k0@#@&i@#@&dbW,nCM+	YqG@*Z~Y4+U@#@&d7v更新其父类的子栏目数@#@&di^W	xRanm!Y`EE2[mYnPtnx!ZsCk/PdnDP^tbs9'^4k^N_8PS4+M+~Z^ldd&f'r'nmD+	O(f*@#@&d7@#@&7iB更新该栏目排序以及大于本需要和同在本分类下的栏目排序序号@#@&i7mKxU 6+^;D+cJ!29lOPt+x!Z^C/kPd+DP6.9+D&9'}DN.(f3F,h4+.n,DGWDrN{J~',DWGObN~[,E,lU9P}DND&9@*rP'PhDn-}DN.qG#@#@&77mKx	Rn6n^!Yn`r;w9lOn,H+U;;VC/k~k+O,rMN+MqGxJ,[~nM+-6MN+M(f,[PrQqPStDnP/sm/dqGxJ,[~/^l/d(G#@#@&in	N~b0@#@&d@#@&,~P,mCV^P/sK/+;Gx	`#@#@&7I/aWU/n "+[kMnmDPE)9:kUm;VC/kmt+U!Rm/wrP,@#@&x[PkE8@#@&@#@&k;4,?l7n\W9k6Xc#@#@&iNr:,/Vm/dHm:+S]l[:SUtGSr	KWa~drx0j.V@#@&7[b:PD./BD/@#@&7Nb:,Zsldd&fS/$s~M//sm//Sr@#@&7Nbh,?Vbx;WE	YBJlHW;Y;W;UD@#@&i/Vm//&9xYMks`.+5;/O`r/Vm/d(GJ#b@#@&dr0,/^ldkqG'JrPD4+	@#@&disG;	N2M.':DE@#@&di2MD\/LxAD.HkLPLPE@!(D@*@!sb@*参数不足！@!z^r@*J@#@&dV/@#@&7d;VC/kq9x;SxTcZ^l/k(9#@#@&i+UN~r6@#@&d;slk/HCs+'O.b:cD5!+dD`rZVm/kHls+E#*@#@&7UtWS6x:Ww{O.ks`M+5EndD`E?4Gh}xPGaJ#b@#@&d]+m[s+xDDb:`M+$;+kYcJ"+C[s+J*b@#@&dSbUVjMV{Y.khcM+5EdYvJJr	3j.sr#b@#@&7b0~;Vm//glsn'rJ~Y4+U@#@&ddwGE	N2M.xKME@#@&d73MD\/Tx2MD\dTP[~E@!4.@*@!sb@*栏目名称不能为空！@!JVb@*J@#@&inx9Pr0@#@&7@#@&dk6~sKEx93.D{KMEnPO4x@#@&i7+XkO~kE4@#@&i+UN,r6@#@&i@#@&d/$V{E/VnmDPM~wDWs~HxE;sC/kPStnDn~;VC/k(f{J~',ZVCdkq9@#@&7k+O,DkZVm/kx/D-+MR/.lY64N+mD~cJzNKN8R.n1W.NknYr#@#@&iD//sm/dRK2x~k;^~mKx	SFB&@#@&ikW~M/Z^C/kR4KW~l	N,DdZsCk/ +KWPDtnU@#@&d7oKEUNA.M'PME@#@&idA.Dt/L'AD.\koPL~J@!4D@*@!sk@*找不到指定的栏目！@!JVr@*E@#@&d7Dk/Vm/d 1VWdn@#@&7dknDP.kZ^l/k'	GY4kUo@#@&776kD~/!4@#@&7nx9Pb0@#@&7r6PjtKAr	KG2{J5ndrPOtU@#@&7i?4Wh}x:Gw{K.E@#@&7V/@#@&id?4GAr	KKwxsCsk+@#@&inx9PrWi@#@&7r6PoW!U92.M':DEPD4+	@#@&diDd/^l/k m^W/@#@&di/Y~Dd/^ld/{UWDtrUT@#@&776rY,d!4@#@&dxN,k6@#@&i@#@&P,P7.kZVmd/vJZ^Cd/gls+E#x/^ld/gC:@#@&7M/ZsCk/cJU4Kh6	KKwJ*'U4WSrUKKw@#@&iD/;slk/`r]nl9:Jb']nmNh+@#@&dM//sm//cEdkU3`.^Jb{Sbx3`D^@#@&iDdZ^lddcEw9CY@#@&i.dZ^lk/ msGk+@#@&id+DP.d;Vldd{xGY4r	o@#@&d@#@&dk+D~Dk'UWDtrUT@#@&id+DPYMdxxKY4kUo@#@&i@#@&P,~P1lss,ZVGdZGx	c*@#@&iI/wKxknR"+[kM+^O,Jb9hk	{Z^Cd/|Hx;RCdaJ~P@#@&+	N~d!4@#@&@#@&@#@&/!8,fn^+D+Z^lkd`*@#@&d9kh~k;VB./BnD-(fB16Oq9S;VC/k(f@#@&7/^l/d(G'ODbhvIn$E/YvJ;slk/(fr#b@#@&dk6~Z^l/k(9'rJ,Y4+U@#@&d7sK;x92..{KD;n@#@&7dA.MHdT'ADDt/T~[,J@!4M@*@!sb@*参数不足！@!Jsk@*J@#@&77+XkDPdE8@#@&dnVkn@#@&d7/^l/d(G'/S	LvZsm/kqf*@#@&7+	N~k6@#@&7@#@&dk5V{J/snmDP;VC/d(G~]WKOqG~9naYtSKmDnxD(G~/4k^N~hD-qG~H+XY(9,sDKhPt+x!/slk/,h4+.n,ZslkdqG'E';Vldd&f@#@&idY~M/{/+M\.R;DnlD+68N+mD~`rbNK[8RM+1W.NdnDJb@#@&7DkRG2xPd5^~^W	UBFS2@#@&dk6PMdR(WWPmx[~M/RG0,YtU@#@&idwW;x[3MDxKM;+@#@&77ADD\dT'3DM\ko~LPr@!4M@*@!sk@*栏目不存在，或者已经被删除@!&Vb@*E@#@&d+^d+@#@&dirWPM/vJ/trs9Jb@*Z~Y4+U@#@&dd7oKEUNA.M'PME@#@&idi3DMHdo{2..t/o,'Pr@!4M@*@!Vb@*该栏目含有子栏目，请删除其子栏目后再进行删除本栏目的操作@!zsk@*E@#@&7dUN,kW@#@&d+U[,kW@#@&7b0~wW!xNADMxKMEnPDtnU@#@&di./cmVKdn@#@&di/nY~.k'UWD4k	o@#@&id+arDPdE(@#@&dn	N,k0@#@&iKD\(f{DdcrnD-qGJ#@#@&716Dq9'.dvJH+XOqGJb@#@&dkW~M/cJGnaY4r#@*!PDtU@#@&d7mKxU 6+1;Y`J!2[lD+,Hnx;/^ld/,d+DP^4bVNx^4ksN q,h4DPZ^lkdqG'EPLP.dvJnm.+	YqGEb#@#@&i+UN~r6@#@&dMdR9+snD+@#@&7M/ Ea[mYn@#@&iD/cm^G/@#@&dk+O~M/'	GY4kxT@#@&d@#@&iB修改上一栏目的H+aO&f和下一栏目的KD-qG@#@&7b0PK.\(f@*T,Y4x@#@&dimKUxc+a+1EOn,JEa[lD+PtnUE;Vm/dPdnDPH+XOqG'E~LP1naDq9PL~rPA4+M+P;Vmd/&fxJ,[~KM+\&9@#@&d+	[~k6@#@&dr0~H6OqG@*!,Y4n	@#@&771WUxcnX+^!YPJ!w9CYP\+	E/sm//,d+DPnMn-qG'rP'PK.\(f,'PrPA4D+~/^ld/&9{J~LPg+6DqG@#@&i+UN,kW@#@&dmmsV,ZVKdnZKx	`b@#@&7M+dwKU/R.n9kDn^DPEb9hbxm;Vm//|HUEcldwr@#@&7i@#@&UN,/E(@#@&@#@&/!4~ZsnmD/Vmd/v#@#@&iNkh~kY.Z^Ck/(G~M/~DDkSZ^ld/&f@#@&iZVmd/&f'D.r:vI;;+dOvJ/Vmd/&fEb*@#@&7r6P/Vmdkq9{JrPY4+	@#@&idoW!x[3MD':.E@#@&i73DMHkox2..t/LPL~J@!4.@*@!Vk@*参数不足！@!JVr@*r@#@&d76bYPkE(@#@&i+s/@#@&7iZVmd/&f';JUovZ^ld/(9*@#@&dUN,kW@#@&d/O.;VC/k(G'^kYM`Z^lkdqG#@#@&i/nO,D/{^W	xRanm!Y`E/nsmOP;slk/(9BZtrs9~KlMn	YKmY4P0MWs~Hx;Z^ldd,ht.+,ZVmddqG'rP'P/sm/dqGb@#@&drW,D/ 8K0~l	[,Ddc+K0PDtU@#@&d7sKEU[ADD{PD!+@#@&772MDt/L'3.MHdo,'Pr@!8.@*@!Vr@*栏目不存在，或者已经被删除@!zsk@*E@#@&7i+XkY,/!8@#@&dnx9PrW@#@&dbWPM/`8b@*!,Y4+U@#@&7i/nY,ODk'^G	xRnam;Ycr/n^+1YP;Vmd/&f~0MWh~t+x!/Vm//,A4+M+,nCDnUDq9'r~[,DdcZ##@#@&id[W,A4ksP	WY,YMdRWW@#@&d77kYD;slk/qGxdYMZ^ld/(9,[~JBEPLPO.k`!b@#@&d7dD.kRhK\x+XY@#@&diVGWa@#@&7iYDk m^W/@#@&di/Y~Y.d{mGx	 +X+^;D+`EdVnmD~;VCk/&fP6DKhPt+UE;VCdkPh4nDPnm.nxDnmY4Psr0+~Br~[,Ddcy#P'~r~EPL~M/cZ#,[Pr~uvJ*@#@&diNG~Stk^nP	WY,O./c+K0@#@&77i/OD;slk/(9{/Y./^ld/&9,[~r~rP[,YMd`Z#@#@&id7OM/RsG\x+XO@#@&id^WGw@#@&idODk m^Wdn@#@&d7dY~YMd{xGDtbxo@#@&inx9Pr0@#@&7.kRm^G/@#@&idnY,Dk'UWO4bxL@#@&nx9Pd;(@#@&@#@&@#@&dE(~Ul-HK\+v#@#@&d9khP;VCdkqfBd;^~Dk/slk/Bk@#@&7[b:~DhCDxO(G@#@&7[b:~YMdBDd@#@&iNksPhCDxOqG~]GKYqGSfwY4S/tbV9~Kl.n	YKlD4~hl.n	Y1Ch~rnm.xO&fBknmDUYhlOtBn.n7rD9nD&f~h.n\&fB1n6O(G@#@&d;slk/(9{YDrhvDn;!nkYcrZ^l/kqGE#*@#@&db0~/^l/k(f{JJ,O4+	@#@&d7sG;	N3DMxKMEn@#@&dd3.MHdo{3MD\ko,[Pr@!(.@*@!Vr@*参数不足！@!zsr@*J@#@&7d6kD~dE(@#@&dnVdn@#@&7d;slk/(9{ZSULvZslkd&fb@#@&i+x9PbW@#@&d@#@&i/5s{J/s+1YPC~oDK:,Hnx;/^ld/,AtDn~;Vldd&fxJ,',Zsm/kqf@#@&id+DP./;VCdk'/.\DR;.nlD+}4%+^O,`Eb9GN(R.n1WD[dYE#@#@&iDd;Vm//cWanx,/5VBmGU	~FBf@#@&dk6~./;Vm/dR8G6PCx9~DkZsCk/RnG6POtU@#@&7isKEx92M.':D;+@#@&77ADDtdo{2DM\do,[,J@!4.@*@!Vr@*找不到指定的栏目！@!&Vb@*E@#@&dd.d;VC/k 1VGk+@#@&di/OPM//Vm/dx	WY4rxT@#@&i7n6bY,/;4@#@&i+UN,r0@#@&@#@&iDnC.xOqGxDDrs`M+;!+kO`rnCDxO(GJ#*@#@&ik0,.KlM+	Y(fxErPOtU@#@&d7.hlDnUDq9'Z@#@&dn^/@#@&idMKlM+UY&fx/dxov.nmD+	O(f*@#@&dnx[~b0@#@&i@#@&ikW~M/ZsCk/cJhCM+UDqGJ#@!@*MKlM+UY&f~O4+x,~PE更改了所属栏目，则要做一系列检查@#@&i7r0,Dhl.+UO&fxDk/Vm/dcrZVCdkq9J*~Dtn	@#@&ddisK;x92.D{K.;@#@&i7dADDtdL'ADMHdo~',J@!4M@*@!^k@*所属栏目不能为自己！@!JVk@*E@#@&7dU9Pr6@#@&ddE判断所指定的栏目是否为外部栏目或本栏目的下属栏目@#@&7db0~DkZsCk/`rKlM+xD(9J*'ZPOtnU@#@&7dir0,DKCM+xO(G@*TPD4x@#@&diddk+D~YM/xmKxU 6+1;Y`Jkns+1Y,DGWOr9PoDKhPt+U;;Vldd,h4+Mn,Sr	3`DV{BE~l	N~Z^ldd&f'r'DhlDUOqG#@#@&7d77b0~YMdR(WW~mxN~OM/ +KW,Y4x@#@&didi7sKEUNAD.x:DE@#@&iddi73DMHkox2..t/LPL~J@!4.@*@!Vk@*不能指定外部栏目为所属栏目@!JVr@*r@#@&d7idV/@#@&7did7k6P.d;Vlkd`rDWKOrNr#{Y./cT*POtU@#@&d77iddoG!x[2M.{K.!+@#@&didi7dAD.Hkox3MDHkLPLPJ@!8.@*@!Vb@*不能指定该栏目的下属栏目作为所属栏目@!zsr@*J@#@&i7didnU9PkW@#@&d7din	N~b0@#@&didiODkR^VK/n@#@&ddi7/YPD.d'	WDtrxL@#@&d7dUN,kW@#@&ddnsk+@#@&i7i/nDPDD/{mKUxc+a+1EOnvJ/s+1YP;sC/kqGPoDGh,Hnx!/Vm/d~St+.n,nCDUDnCDt,Vk0+,vJLDdZ^lddvJnm.+	YnmO4J*[r~EP'~M//Vmd/vJ/sm//(9r#~[,EuB~mx9PZ^lkdqG'E[MnC.xY&9#@#@&di7r0,xKY~`O.kRnW6~l	N~OM/R8G6#~Y4n	@#@&ididsKE	[2MDxKMEn@#@&ddi72MDHkLx2MDt/LP'~r@!8D@*@!Vb@*您不能指定该栏目的下属栏目作为所属栏目@!&^k@*E@#@&d7dU9Pr6@#@&ddiYMdR1VG/@#@&7id/OPDD/{UGY4k	o@#@&77x[PbWdi@#@&7xN~r6@#@&@#@&7b0~wW!xNADMxKMEnPDtnU@#@&di./;Vlkd m^Wk+@#@&77k+OPMdZ^ldd{xWO4bxL@#@&7i+abY,/E(@#@&7+	N~k6@#@&7@#@&dbWPM/Z^Cd/vJhl.+UO&fE#{TPDtnU@#@&d7KmDnxD(G'.kZ^l/k`r/Vm/dqGJb@#@&ddbKlM+xD(9'Z@#@&dnVdn@#@&7dhCDxO(G'Dd/^ld/vEhl.xDqfr#@#@&dikKlM+UO&f'MdZ^l/kcEnmDxOq9E*@#@&dUN,kW@#@&dfn2DtxDk/^ldk`rf+aY4E#@#@&7Z4ks[{D/;slk/`r/4k^Nr#@#@&7]KWOqGxDkZsCk/`E]KWOqGE*@#@&inmD+	YhCY4'./;VCdk`JhCDxYhCOtr#@#@&7n.n7q9'MdZ^lddvJn.n7q9J*@#@&dH6Dqf{Dk/Vm/d`r1naDqfrb@#@&dDk/slk/cmsWdn@#@&7/OPM//sm//xUKY4k	L@#@&7@#@&i@#@&,PE假如更改了所属栏目@#@&,Pv需要更新其原来所属栏目信息，包括深度、父级qG、栏目数、排序、继承版主等数据@#@&~,B需要更新当前所属栏目信息@#@&~PE继承版主数据需要另写函数进行更新OO取消，在前台可用;sC/kqGPrx~KmDnxDKlDt来获得@#@&,PNrh,:./B\m6]KWDqf@#@&,~/Y~:M/x^Kxxcn6mEDncJk+^+^Y~hm6cDKGYbNb~wDWh~t+UE;sm/dr#@#@&P,HmaIKWOqG'h.k`!*@#@&,P/O~:M/{xGY4r	o@#@&,~k6Prd	EVsctlaIKGDq9*PDt+	@#@&7Hm6]WKY(9{!@#@&~PxN,rW@#@&P,Nr:~VBxKlMnxDnCO4~:KCM+UYhCDt@#@&P,NksPhCDxO?$VS/^l/k/W!xY@#@&~P9ksP./K.\6D9nD&f@#@&,PkW~1VUov2mDn	YbN#@!@*MKlM+UY&f~C	NP	GY,`khC.+	Y&fx!~C	N~DhCDxO(G'!b~Dtnx,~E假如更改了所属栏目@#@&iB更新原来同一父栏目的上一个栏目的g+6DqG和下一个栏目的KD\(f@#@&7r6PnMn\&f@*Z~Otx@#@&7d^G	x +Xnm!Yn~rEw[CD+~HU!Zsm/kP/Y,H+XY(f{J~',1+XOqGP[,E~h4+M+~ZsCk/(f{EPLPK.\q9@#@&dnx9~b0@#@&db0Pg+XOqG@*TPDtnU@#@&di^W	xRanm!YPEE2[mYnPtnx!ZsCk/PdnDPKD-&fxrPLPnM+7(f,[~J,h4nM+P;slk/qGxEPLPg+aY(9@#@&7+	[Pb0@#@&i@#@&7r6Prnm.xO&f@*!Pmx9~Dhl.+	Y(9{!PD4+	PPiv如果原来不是一级分类改成一级分类@#@&idE得到上一个一级分类栏目@#@&d7d$VxJknVmO~;Vldd&fS1aDq9,0MW:,HUE;VC/kPA4D+,]WKYqGxEPLPtlaIGGDq9PL~J,lU[,f+2O4'TJ@#@&iddY,D/{/.\D ZM+COr4NnmD`Jz[GN(RM+^W.[k+OJ*@#@&id.dcWwnU,/5VB^KxUBFB&@#@&diKD\(f{DdcZ#P,~P,PB得到新的h.n\&f@#@&7d.dvFb';slk/(9,PP~~E更新上一个一级分类栏目的1n6D(G的值@#@&idM/R!w9CY@#@&diDd 1VWkn@#@&ddknOPM/{xGY4r	o@#@&i7@#@&d7\m6IGGDq9'tCXIGKY&f_8@#@&7dE更新当前栏目数据@#@&dimGU	R+Xnm!Y+vE;w9lD+~HnU!ZslkdPk+O~9+wO4{!SrM[D(G'Z~DKWDrN{J':m6.GKYk9'JBwlMnUYbN{!SnC.xOnmOt{BTvBnDn-&fxJ,',n.\&fPLPrS16OqG'T~St+MnP;Vlkd(f{JLZsldd&fb@#@&7dE如果有下属栏目，则更新其下属栏目数据。下属栏目的排序不需考虑，只需更新下属栏目深度和一级排序q9cMWWOr9#数据@#@&i7b0~1tbVN@*!,Otx@#@&id7r{!@#@&7dinlMnUYhlDtxnC.xOnmOt,[~EBJ@#@&7idd+D~M/x1W	xR6^ED+cJk+sn1YPC~sMW:,\nx!Z^ld/~A4+.+,KlM+UOhlY4~^kV+,vuJ'hlM+xDnmOt,[~Z^ldd&f[rYBr#@#@&77d9W,h4ksn,xGY,./c+GW@#@&d77ikxk3q@#@&7idi:nmDUYhlOt{Dn2^lmcDk`JhC.+	YhlOtEbBnCDUYhlO4BJJb@#@&d7di^KxUc+X+m!YcJ!w[lD+~\xE;slk/PknOP9+aY4'[naY4Or'NwO4LJ~.GKYrN{EL:CXDKWYbNLE~hl.+	YKCDt'EE[snlMnUYhlDt'Jv~StnD~Z^ldd&f'E'M/cJ;sm/d&fr##@#@&i7diDdRsW-n	+6D@#@&idd^GGw@#@&id7Dd 1VG/@#@&id7dYP.d{xGY4r	o@#@&di+x9PbW@#@&d7@#@&d7v更新其原来所属栏目的栏目数，排序相当于剪枝而不需考虑@#@&di^W	xRanm!Y`EE2[mYnPtnx!ZsCk/PdnDP^tbs9'^4k^NO8PS4+M+~Z^ldd&f'r'khlDUOqG#@#@&7d@#@&i+s/r0,kKCM+xO(G@*TPmU9P.hlM+xDqG@*!,Y4+	P~~,B如果是将一个分栏目移动到其他分栏目下@#@&7dE得到当前栏目的下属子栏目数@#@&i7KlM+	YKlO4{nCDUYhlO4,[PESr@#@&didY~M/{mW	xcn6m;Y`EdV+1OP1WE	Oce*PwDG:~\x;Z^C/kPA4D+~KmDnxDKmY4,Vb3+,BuE[hl.+	YKCDtPL~Z^l/k(9[r]EJb@#@&7iZslkdZKEUO{D/cT*@#@&dir6Prkx!VVvZ^C/kZGE	Yb~Dt+	@#@&idd;sC/kZKEUYxq@#@&7dUN,kW@#@&dd.dcmsWkn@#@&7i/YPM/{UWDtrxT@#@&7i@#@&i7B获得目标栏目的相关信息id@#@&77/Y,Y./x^KxURa+1EOnvJ/nsmOPC~wDGsPt+x!Z^C/kPAtDn~;VlkdqG'JL.KlM+	Y(fb@#@&d7k6~YM/cE;tks[r#@*!,O4+Uid@#@&didE得到与本栏目同级的最后一个栏目的6D9+.qG@#@&7id/OPM/nMn-rMND(fx^KxURa+1EOnvJ/nsmOPtCX`6MNDqG#,oDK:~Hx;/^l/k~h4+D~KlM+	Y(fxE,[~YMd`rZsCk/q9E*#@#@&i7in.\}DND&9'M/KD\6.9+D&9`Z#@#@&77dE得到与本栏目同级的最后一个栏目的Z^ld/(9@#@&7did;^'EdV+^O,Zslkd&fSg+XYqGP6.WsP\+	E/sm//,AtD+,KCDxDq9'E~LPODkcJ;VCdkqfEb,[~J,C	N~}D9+D&f{EPLPKD\6.9+D&9@#@&ddidnY,Dk'd+.-D mMnlD+G8N+mOcrl[W98cDn1WMN/Yrb@#@&d7dM/ Ga+x,d;^~mKUU~8~2@#@&d77hDn\&9'M/cT*PP~~E得到新的n.+7(G@#@&idiD/vF*xZ^ld/&f~~,PPE更新上一个栏目的H+XYqG的值@#@&didM/ E2[mYn@#@&7diDd 1VWdn@#@&7didY~M/{xWDtbUo@#@&7di@#@&7idB得到同一父栏目但比本栏目级数大的子栏目的最大}.NDqG，如果比前一个值大，则改用这个值。@#@&didk+OP.dhDn\}.ND(9{mWUUc+a+1;D+cr/V+1Y,\lX`6D9+.(G#Pw.WsPHU;Z^lk/~h4nM+~nm.+	YKCDtPsr0+~Br~LPOM/vJnmDUYhlOtr#~',J~r~[,YDkcEZ^lk/(fEb,[~JBYBr#@#@&iddrW,`UWDcM/KM+7rD9+M(fc4G0,lU[,D/h.+7rD9n.qGRWW#b~Dtnx@#@&did7r6PxGO,qd1!s^`.knM+\}D9nD&fc!*#~~Dt+	@#@&idd,77k6PM/KDn-}D[+M(fv!b@*hD+-6MNnD&9,Y4x@#@&didi7dhDn\}D[nMqf{./hD+76.ND&fc!b@#@&d7di7+	N~r6@#@&77idnx9~b0@#@&did+	N,r0@#@&7dVdn@#@&di7nM+\&9x!@#@&id7n.n7r.N.qG'O.k`J6.9+.qGE*@#@&idxN,k6@#@&id@#@&idv在获得移动过来的栏目数后更新排序在指定栏目之后的栏目排序数据@#@&dd1Gx	R+Xn^ED+vJ;w[CD+~HUE;VCdkP/nO,r.N.&fx}D9+D&f3EPLP/Vm/d/KExD~[,J_8~AtDP.WGObNxJ,'PDDdcrDWGObNE#,',J~mx9PrMN.qG@*EPLPK.\rM[+Mqf*@#@&di@#@&d7B更新当前栏目数据@#@&id^W	UR6n^!Y+cE!w[lDn,Hn	E;Vlk/,d+DP[+aY4xr[YMd`rN+aO4J*[r_q~6.9+.qGxJLn.n7rD[nMq9[rQ8~.KWDkN{JLODk`EDKWOr9J#LE~hlDUOqG'r[.nC.xOqG'JBnC.xYKCDtxBr~LPOM/vJnmDUYhlOtr#~',J~r~[,YDkcEZ^lk/(fEb,[~JESnM+-(G'J~',n.+7(GP',JB1+XY&9'ZPAtDn~;VlkdqG'JL/slk/&fb@#@&7i@#@&div如果有子栏目则更新子栏目数据，深度为原来的相对深度加上当前所属栏目的深度@#@&d7dYP.d{mGx	 6n1ED+`r/s+1Y~e,s.GsPHUE;Vlkd~h4+M+~nC.xOnmOt,VrVPBYELnCDUDnCDtLZVm/k(fLJYB,W.[DP(zP}DN.(fr#@#@&7drx8@#@&di[W,h4r^+PUGDP./cnK0@#@&didk{k3q@#@&d7dbnC.xYhCY4'YMdcJhlM+UYKCDtE#,'Pr~E~LPY.dvJ/Vmdkq9r#,[Pr~r~[,Dnw^l^nvD/vEnmD+	OKlDtr#SnC.xOnmOtBJEb@#@&d771WUxcnX+^!Y`J!w9CYP\+	E/sm//,d+DPN2Ot{NwOtRELNnwD4[r_E'DD/cE9+2Y4E*[E3FBrD9+M(f{J'nM+-6MN+M(fLJ_r'r[r~MWGYr[{J'YMd`rDGGDkNEbLJSnm.xOhlDt'EJLrnmDnxDnCO4[JE~h4+D~/Vm/kq9'E'M/cJ;slk/(9r##@#@&id7Dk sW-x6Y@#@&i7VKW2@#@&d7.kRm^G/@#@&i7d+DPM/xxGO4kUo@#@&diY.dcmVGd@#@&didY~DDk'xKY4rxT@#@&di@#@&7iB更新所指向的上级栏目的子栏目数@#@&7d1Wx	 n6m!Yn`E;aNCY~Hx;/^l/d~k+OP14bV[{m4kV9_8~h4+.+,ZsCk/qGxJLDnm.nxDqG#@#@&77@#@&7dE更新其原父类的子栏目数7di@#@&7imWUUc+a+1;D+crEaNlD+,\+	E/Vm/d~k+Y,^tbVN{^4k^N F~h4nM+~Z^C/kq9xr[kKCM+UY&9*@#@&i+^/+,P,~B如果原来是一级栏目改成其他栏目的下属栏目@#@&7dE得到移动的栏目总数@#@&7i/+D~Dk'mKUUR6m;Yncr/nV^Y,mG;	Y`Mb,s.Ws~t+U!Z^l/kPS4+M+~DKWOr9'JL.WKYk9b@#@&id;VC/d/KEUY{./v!b@#@&dd.dcmsWkn@#@&7i/YPM/{UWDtrxT@#@&7i@#@&i7B获得目标栏目的相关信息id@#@&77/Y,Y./x^KxURa+1EOnvJ/nsmOPC~wDGsPt+x!Z^C/kPAtDn~;VlkdqG'JL.KlM+	Y(fb@#@&d7k6~YM/cE;tks[r#@*!,O4+Uid@#@&didE得到与本栏目同级的最后一个栏目的6D9+.qG@#@&7id/OPM/nMn-rMND(fx^KxURa+1EOnvJ/nsmOPtCX`6MNDqG#,oDK:~Hx;/^l/k~h4+D~KlM+	Y(fxE,[~YMd`rZsCk/q9E*#@#@&i7in.\}DND&9'M/KD\6.9+D&9`Z#@#@&77dk;^'E/nsmOP;slk/(9B1+aO&f~0MGsP\x!ZVm/k~h4+.+,nC.xY&9'rP[,O./vJ;VC/d(GJbPL~J,lU[,rD[nMq9'r~LPKM+7rD9+M(f@#@&7di/nO,D/{d+M\+M ^DlD+G4%n1YcJm[W94 .mW.[k+OJ*@#@&d7iDkRWa+	~/$VSmKxUS8~&@#@&didnMn-qG'M/c!b@#@&d7dMd`8#x/^l/d(G@#@&di7M/ !w9lY@#@&7di/nY,Ddx	WY4rxT@#@&i77@#@&didv得到同一父栏目但比本栏目级数大的子栏目的最大r.[D(f，如果比前一个值大，则改用这个值。@#@&diddnDPDdKM+-rM[D(G'1Wx	Ra+1EO+vJdn^+mD~Hm6`}.[+MqG#~s.GsP\+	;Z^ldd,htn.PKlMn	YKmY4PVb3~BrP'PDDdcrnlMnxDnlD4E#,[,JSJ~',Y./vEZ^ldd&fJb~LPE~uvr#@#@&didk6PvUWD`./hDn-}DN.qGR4KW~l	N,Ddn.n7r.N.qGRnG6##~O4+U@#@&7id7b0,xWDP&d1!Vs`M/K.\rM[+MqfvTb#,PDtnx@#@&id7Pi7k6P.dhD+-6MNnD&9v!b@*nM+\}D9nD&f~Y4+U@#@&ddi7dinD-6D9+Mq9'.dhDn\}.ND(9v!#@#@&id7din	N~b0@#@&didinx9Pr0@#@&77i+x9~k6@#@&i7nVk+@#@&7d7KM+-qGx!@#@&77inDn-}D[+M(G'OM/vJrMN.qGJb@#@&d7n	NPbW@#@&d@#@&77B在获得移动过来的栏目数后更新排序在指定栏目之后的栏目排序数据@#@&id^WUUc+a+1;Y`E;aNlOn,Hnx!/^ldkPk+Y,rM[+Mq9'}D[nMqf3EPLPZ^Cd/;W!xOP'E3F~h4nDP.GKYk[xrP'PD.k`EMWKYk9J*~[,J~l	N~6MN+M(f@*JPL~KD\}D[+.(G#@#@&i7@#@&d7^Kxx nX+^EDnvJ;aNmY+,HUE;VC/kPdnDPnMn\&f'r~'PhD\(f~',JS1aY&fxT,htn.P/Vmdkq9{J,[P;Vmd/&fb@#@&d7dYPMd'1Wx	 n6m!Yn`EdVnmD~e,s.GsPHnU!Zslkd,h4DPDKWDrN{J'DKWOr9[J,GD9+D,8zP}D9+.q9E*@#@&dir'Z@#@&7iNW~A4ks+,UKY~M/c+W6@#@&7dikxk3F@#@&iddbWPM/`r2CDxDk[JbxZPOtU@#@&d77inl.n	YKlD4{Y.k`rnlM+	OnmY4J*P'~r~J,'PDD/vE/Vm/kq9Jb@#@&d7di^W	x nX+m;O`EEa[mYn,HxE;Vmd/,/nY,Nn2Dt'9nwDt_r'ODk`rNnwO4r#'J3q~}D[nMqfxELn.+76MNnMqG[J3JLr[r~.WKYr[{J[D./vJDKGOk9J*[E~KCM+UYhCY4'vELnl.n	YKlD4LJvBwmD+	Yb['r[.nmDnUDqfLEPSt+Mn~Z^lk/(fxELDd`r/Vm/d(GJ#b@#@&d7dsk+@#@&diddhlMnxDnCY4'O.k`JhCDxYhCOtr#,[~JSE,[~YMd`rZsCk/q9E*P'PrSrP',DwVmmcDk`EnmDnUDnlD4J*~JZSE~rJ*@#@&d77imGx	 +X+^;D+`E;aNCY~t+U!Z^l/kPknY,NnwDtx[wY4QJLYDkcENwDtE#'E3FSrM[+Mq9xr[n.n7r.N.&f'r_r[kLJB.WKYrN{J'OM/`r.WKYk9Eb[r~hl.+UOhlOt{vJLnC.xYKCDt'JE~StnM+,ZVm/k(f{J'Dk`E/^l/k(fr##@#@&7di+	N~kW@#@&d7dMdRsW-n	+6O@#@&d7VKGa@#@&idM/R1VKd+@#@&7dk+O~M/'	GY4kxT@#@&diYM/ msGk+@#@&i7/Y~OM/'UGDtrxT@#@&d7E更新所指向的上级栏目栏目数di@#@&id1Gx	Rn6m;O`J!2NmY+,\nx!Z^ld/~dY~m4rV9'^4bVNQq,h4+Mn,Zsm/kqf{JL.nmDnxDq9b@#@&@#@&7+	NPbW@#@&,Px[PrW@#@&7@#@&~P1lss,ZVGdZGx	c*@#@&,P"+/aW	d+cInNbDn^DPJz[:bx{;sC/k{t+UE CkwEP,@#@&x[~kE4@#@&@#@&dE(~`w6MND`*@#@&7Nb:~Z^ldd&f~k5V}DN.SDkrMNnDS\K\n1!h~1IGGDqfSO"WGY&9BkSM/BnD\&9~g+aY&f@#@&iZVmd/&f'D.r:vD;;+dOvJ/Vmd/&fEb*@#@&7^"WGY&9{K.b:vD+$EdYvJ^IKWO(GJ#*@#@&iHW7nHEs'DDr:c.;;+kO`rHG-1EhE*#@#@&ir6P/^lk/qG'rEPDtnx@#@&77wWE	[2MD':.;+@#@&id3D.\kox2M.Hko~',J@!8.@*@!sk@*参数不足！@!JVr@*J@#@&dVkn@#@&d7Z^ldd&f';JxT`Z^Cd/&f*@#@&dnU9Pr0@#@&db0~^"WWO(G'EJ,O4+U@#@&idsKE	[2MDxYMEn@#@&ddA.Dt/o{3.Dt/TP'PE@!(D@*@!^r@*错误参数！@!zsr@*J@#@&7Vd+@#@&id^"WKYqG';rxD`^IKWO(G#@#@&7+	NPbW@#@&ik6P\W-ngEh'rEPDtnU@#@&d7oKEUNA.M'OME@#@&idA.Dt/L'AD.\koPL~J@!4D@*@!sk@*错误参数！@!JVr@*E@#@&dnVkn@#@&d7\K\+H;s'/k	OvHG7+gE:*@#@&7db0~HK\nH!:'Z~Y4+x@#@&7disKEUN3.M'PD!n@#@&d77ADD\dT'3DM\ko~LPr@!4M@*@!sk@*请选择要提升的数字！@!&Vb@*E@#@&ddUN,k0@#@&7+	N,kW@#@&7b0~sK;x92..{KD;n,Y4+	@#@&d76bYPkE(@#@&i+UN,kW@#@&@#@&iv得到本栏目的nM+\&9S16Dq9@#@&7k+OPMd'1WUUc+6n^!Yn`rdVn1Y,nD\&9~g+aY&f~WMW:,\+	EZ^Cd/,h4+.+~/^ld/&9'rP'~;Vldd&fb@#@&7hDn7qG'Dk`Zb@#@&dH+XY(9{D/vq#@#@&dMd m^Wk+@#@&7dY~DkxxKY4r	o@#@&7E先修改上一栏目的1n6D(G和下一栏目的n.\&f@#@&dbWPhDn\&f@*T,YtU@#@&dd1GUxc+X+^EOn,J;w9CYP\n	EZsCk/~/O,1nXY&f'rPL~16OqGP'~rPh4nDPZ^Cd/&f{J~[~KM+-qG@#@&i+U[,k0@#@&ikWPgnXY(G@*ZPY4+	@#@&id^W	x nX+m!O+,JEa[CYPt+UE/sm/dPknY,n.n7qfxE,[~nMn7q9,[,JPSt.+,Zslk/(9{JPL~16Y&9@#@&i+	N~kW@#@&@#@&d9r:,:.dBHla]KWOqG@#@&ddY,:Dk'1Gx	Rn6m;O`JknVmY,hC6vDKWOk[b,s.Ws~Hx;/^l/dE*@#@&dtCXIGKY&f'sDkc!*_q@#@&dv先将当前栏目移至最后，包括子栏目@#@&dmKUxc+6^;Y`rE2NCOP\+	;Z^ldd,/+O~"WGY&9{J~LPtl6"WKOqGP'PrPA4D+,]WKYqGxEPLP1IGWO(G#@#@&i@#@&iB然后将位于当前栏目以上的栏目的]GKYq9依次加一，范围为要提升的数字@#@&dd;^6MNnM'r/+^+1OPCPoDK:~\xE;slk/PS4nDPhl.+UO&fx!,Cx9P]GKYq9@!rP'P1]KWO&f,[PrPK.ND~4HP]GKYqG~N/mr@#@&dk+DP./6.9+.'knD7+. ;D+COr8L^D`EmNKN4cD^WMNd+DJb@#@&dDk6D9+DcG2+	Pk;sr.[DSmKUxBFSf@#@&drW,DdrM[D (W6Pl	N,./}D[+MRnG6PY4nx@#@&dinakDPkE8P~~,P~P,v如果当前栏目已经在最上面，则无需移动@#@&dnU9PkW@#@&dr'8@#@&d[KPStk^+,UWDP./}D[nMR+KW@#@&ddD]GWDqG'./6.9+.`r]WKY(9r#P~~,P~PE得到要提升位置的]KWO&f，包括子栏目@#@&dimKUxc+a+1EOnvJEa[lD+PtnUE;Vm/dPdnDP]WKOqG']GKYq9Q8PAt.P]KWDqf{J,'PDIGWDq9b@#@&dir'b_F@#@&7db0,k@*HG-1;:,Otx@#@&idd.d}D[+Mcrn.\&fJ*';slk/(f@#@&77iD/}.NDR!2[lD+@#@&7d7^KxURa+1EOnvJE2[mYnPtn	E/^lk/Pk+D~16OqG'E~LPDk6D9+DvE/Vm/kq9Jb~LPEPS4+M+~/^l/d(G'EPL~;VCk/&f#@#@&i7d6rY,NG@#@&ddUN,k0@#@&7dM/}D[+. sW-+	n6D@#@&7^WW2@#@&d./}.9+.c:K\+	+XO@#@&dr0,Dd6MN+M +K0PD4nx@#@&id^WUUc+a+1;Y`E;aNlOn,Hnx!/^ldkPk+Y,nMn\&fx!,h4nM+P;slk/qGxEPLP;VC/d(G#@#@&inVk+@#@&idDd6MNnDvEg+aDqGJ#{Z^C/kq9@#@&d7.krD9nDcEw9CO+@#@&id^WUUc+a+1;Y`E;aNlOn,Hnx!/^ldkPk+Y,nMn\&fxJ,[~.krD9nDvJZ^Cd/&fr#~[~E,h4+MnP;VCdkqfxE,[~Z^Ck/(G#@#@&dx9~k6d@#@&iDd6MN+M m^W/@#@&dk+DP./6.9+.'	GY4kUL@#@&d@#@&iB然后再将当前栏目从最后移到相应位置，包括子栏目@#@&i^KxUc+X+m!YcJ!w[lD+~\xE;slk/PknOP"WKY(fxE,[~Y"GWDq9~LPJ~A4+.+,]KWO&f{JPLPtC6"WGY&fb@#@&dmmsV,ZVKdnZKx	`b@#@&7M+dwKU/R]n9kDn^DPEb9hbxm;Vm//|HUEcldwQb^ObWx{6D9+Dr@#@&+	N,/;4@#@&@#@&dE(~fKhU6MN+.c*@#@&d9rsP/^lk/qG~k5V}D[+M~.d}DN.~tW\H;:Bm"WGY(9BY]WKOqG~rSM/~K.\(fBH6O&f@#@&d;Vmd/&fxYMkhcM+;!n/D`J;sC/kqGJb#@#@&im]WKOqG'P.b:`.n$En/Dcrm]KWDqfr#*@#@&iHG\1;h{YDbh`M+;!ndYvJtW-+H;sJb#@#@&db0~/^l/d(G'EJ,O4+U@#@&idsKE	[2MDxKMEn@#@&ddA.Dt/o{3.Dt/TP'PE@!(D@*@!^r@*参数不足！@!zsr@*J@#@&7Vd+@#@&id/^lk/qG';JxT`/Vm/d(G#@#@&7+	NPbW@#@&ik6P^IGGDq9'rEPDtnU@#@&d7oKEUNA.M'OME@#@&idA.Dt/L'AD.\koPL~J@!4D@*@!sk@*错误参数！@!JVr@*E@#@&dnVkn@#@&d7^"WWO(G'/k	Ovm]KWDqf*@#@&7+	N~k6@#@&7b0PtG\1EsxEJ,Y4+U@#@&7isGE	[2MDxOME+@#@&id3DM\koxADMH/TPL~J@!4.@*@!Vr@*错误参数！@!zVb@*J@#@&dsd+@#@&id\W-ngEh';rxD`\G7+1;h*@#@&dir6P\K\1Es'Z~Y4+U@#@&d77wWE	[2MD':.;+@#@&id72..t/L'A.Dt/L~LPJ@!8M@*@!Vb@*请选择要提升的数字！@!zsb@*r@#@&idUN,kW@#@&dnU9Pk6@#@&ik0,oGE	NAD.'P.!+~Y4nx@#@&776kO~kE8@#@&7x[,k6@#@&@#@&iv得到本栏目的nM+-qG~HnXYqG@#@&i/+D~./{mKxURnam;YcJk+sn1YPK.\(fBH6O&f,0DK:,\+	E/Vm/d~St+MnP;Vlkd(f{J,[~ZsCk/(f*@#@&in.n7qfx.k`T#@#@&i1nXY&f'M/vq#@#@&7DkR^sK/+@#@&dk+Y,.d'	WDtrxL@#@&dv先修改上一栏目的1aY&f和下一栏目的K.\q9@#@&dr0,KM+-&f@*!PDtU@#@&d7mKxU 6+1;YPJ!2[lD+,Hnx;/^ld/,d+DPHnXYq9xrP'PgnXY(GPLPJ,h4nDP/Vm/d(G'J,'PhD+7(9@#@&dx[PrW@#@&7k6~16O(G@*!~O4+U@#@&7imG	xc+6m!O+,J;w9lOn,H+	;Z^l/k~d+DPhDn\(9{J~[,KD\(9,[PE~StnD~;VCk/&f'rPL~16OqG@#@&7xN,r0@#@&@#@&7[ksPsDd~\CXIGWD(f@#@&7dYPh.k'^W	Uc+am!Y+vJknVmOPslacMWWDrN*PsMGhPt+	E/VCdkJb@#@&7Hm6]GKYq9xsDd`Zb3F@#@&dE先将当前栏目移至最后，包括子栏目@#@&imKUxc+a+1EOnvJEa[lD+PtnUE;Vm/dPdnDP]WKOqG'E~LPHCa"WGY&9,[~rPSt+M+,]WKY(f{J~',mIKGY&f#@#@&7@#@&dE然后将位于当前栏目以下的栏目的IGWO(G依次减一，范围为要下降的数字@#@&dk5V}D[nM'Jdn^+^Y,M,s.K:,H+	E;slk/~h4+.n,nlMnxDqf{T~l	N,IGWO(G@*EPL~m"WGO&fP'~rPGD9nMP8HP"WWDqGE@#@&dd+DP.d}DN.'k+D7n.R;DlO+68N+^YvEl9W[8cD+^GMNd+DE*@#@&iDkrD9+M Wa+UPk;s6MN+MSmKxxBqS&@#@&ikWP.d}D[+M 4K0~C	NP.d}D[+M WW,Y4+x@#@&i7+XkOPkE8~,PP,~P,B如果当前栏目已经在最下面，则无需移动@#@&7nx9Pb0@#@&7r{F@#@&i[W,h4r^+PUGDP./}.9+.c+K0@#@&diOIKWOqG'.d}DN.`rIWKO(fr#,P~P~~,B得到要提升位置的]WKOqG，包括子栏目@#@&7imWUUc+a+1;D+crEaNlD+,\+	E/Vm/d~k+Y,]WKYqGx]WKY&fRF~A4+.+,]WKY(9{JP'~DIGWD(G#@#@&dik'b_8@#@&idr0,k@*\K\+g;:,YtU@#@&idiDdr.[DcJgn6Dq9E*'ZsCk/(f@#@&id7M/}DNDc;w9lO+@#@&77imW	UR6+1;O+vJ!w[lOn,Hnx!/Vm/d~k+Y~KM+-qGxrP',DkrD9+McJ;VC/kq9E*P[,EPSt+Mn~Z^lk/(fxE,[~Z^C/kq9b@#@&d776rY,[K@#@&idxN,k6@#@&id./}D[nMR:K-+	+6D@#@&d^WKw@#@&7.kr.N.RsW-n	+6O@#@&dr0,.kr.9+MR+K0,Otx@#@&id^G	xRa+1EYcEEaNmYnP\n	E/Vmd/,/nO,1+aO&fx!,A4+.P;Vlk/&9'rP'P;VCdkqf*@#@&i+Vkn@#@&idM/6D[nM`EnMn\&fEb{ZVCdkq9@#@&7iDd}D9+DcEa[lD+@#@&id^G	xRa+1EYcEEaNmYnP\n	E/Vmd/,/nO,1+aO&fxJ,',Dd}D9+DvJ;slk/(fr#~',JPS4+M+P;sC/kqG'EP'~;VC/k(f*@#@&7xN~r6d@#@&i.kr.9+MRm^Wkn@#@&dd+DP.d}DN.'	WY4rUo@#@&i@#@&dv然后再将当前栏目从最后移到相应位置，包括子栏目@#@&d^W	UR6n^!Y+cE!w[lDn,Hn	E;Vlk/,d+DP]WKY(9{JPL~Y"WWD(9PLPrPAtn.P]WKOqG'E~LPHCa"WGY&9*@#@&immVV,Z^G/ZGx	`b@#@&dDdwKx/ ]+9kM+^Y~EzNhk	mZ^ldd|H+U;cldwQ)1YrKx{rD9+ME@#@&+UN,/;8@#@&@#@&dE(Pja6.NDg`b@#@&79khPk5V}D[nM~Dd6MNnDB\K\ngEs~Z^lkdqG~r@#@&d[rsPnm.+	YqGS6D9+Mq9~KCM+UYhCY4~/4bVNSKM+-qGSg+aDqG@#@&iZ^C/kq9':DrhvD+$;+kY`r/slk/&fE#b@#@&d\W7n1!:xOMk:c.;;+kOvJ\K\1EsJ*b@#@&dr0,ZsCk/qGxJrPY4nU@#@&disGEU[AD.'D.E@#@&7i2D.\kox2M.t/L,[,J@!(D@*@!Vb@*错误参数！@!z^k@*E@#@&ds/@#@&i7/Vm/kq9'/J	ocZ^C/kq9b@#@&dnU9Pr0@#@&ikW,HK\+gEsxJrPOtx@#@&idsK;x92DMxOD!+@#@&7d3.MHdo{3DMHdL,[PE@!(D@*@!^r@*错误参数！@!&^k@*J@#@&ds/@#@&diHG-1EsxZbxYv\G\1!:b@#@&7ikWPtG\1;h{!PO4x@#@&i7isG!x92DM':.E@#@&did3.MH/Tx2MDHkL~[,J@!4.@*@!sb@*请选择要提升的数字！@!z^r@*r@#@&7i+x[~b0@#@&in	N~b0@#@&db0,oW!x[2MDxPME+,Otx@#@&77+XkDPdE8@#@&dnx9~k6@#@&@#@&dNrh,/5VB.k~G^NKDNDkSkb~ODk~O6MN+M(f@#@&dE要移动的栏目信息@#@&dk+DP./x^KxURa+1EOnvJ/nsmOPhCM+UDqG~rMN.qG~KlM+UOhlY4Sm4kV9SKD\&fS1naDq9Pw.WsP\n	EZsCk/~h4nM+~;Vm//&f{E[;VC/kq9b@#@&dhCDxY&9xDk`Z#@#@&76MNnD&9'M/cq*@#@&7KmDnxDKmY4{Dk` *PL~JBJ~[,ZsCk/qG@#@&imtbs['M/v&b@#@&7hDn\&9'M/c**@#@&7H6OqGxM/cl#@#@&dM/c^VK/n@#@&ddnDPDkxxKYtbUL@#@&db0~m4r^N@*!,Otx@#@&id/nO,Dd'1G	x 6mED+vE/VnmDP^G!xYvM#,sDKh~Hx!Zsldd,h4+MnPhl.n	YnCO4Psk0n,BYr[hlDxDKlDt'JuBEb@#@&diGV9WD9n./{Dk`T#@#@&id./c^VK/n@#@&dddnDP./{UKY4bxT@#@&i+^d+@#@&7dKV[GMN+Md'Z@#@&inUN,k6@#@&dv先修改上一栏目的H6OqG和下一栏目的KD\(9@#@&drW,n.+7(G@*T,Y4+x@#@&i7mKxUR6n^!Y+,EEaNlDn~Hx!Zsldd,/nY,H+XY(9{JP'~g+aY&9,[~rPSt+M+,/Vm/dqG'E~LPnMn\&f@#@&7nx9Pb0@#@&7r6PH+XOqG@*T~Dt+U@#@&d7mKU	RnX+1EYPr;w9lO+,HnU!ZVmd/,/+D~KD\&fxJ~',n.+7(f,[~E,htn.P/Vmdkq9{J,[Pg+XOqG@#@&dx[~b0@#@&7@#@&dB和该栏目同级且排序在其之上的栏目 RRO O更新其排序，范围为要提升的数字@#@&7/5s{Jd+^nmDP/sm//(9Br.N.&fS1tbVNBnm.+	YKlDtSKM+\&9~g+6D(9PwDK:~HnU!ZslkdPStn.PnC.xOqGxr[KmDxY&fLEPmx[P}D[nMqf@!E[}DN.(fLJ,W.Nn.,4zP}.ND(9,N+d^r@#@&dknDP.k'k+D7+M ZM+CYr8%mYvEl9WN( .+1WMNd+OE*@#@&dMdRKwnU,/;sS1WUxBqB&@#@&db'F@#@&i[W,h4k^+~UKYPMdRW0@#@&7dDrMNnD(9{Dd`8b@#@&d7^Kxx nX+^EDnvJ;aNmY+,HUE;VC/kPdnDPrM[+Mqf{E'Y}D9+.q9QKV[WM[+M/QrLJPA4DnP;sm/d&f{J[M/vT#*@#@&dikW~M/`yb@*ZPY4nU@#@&didrkxr3F@#@&i7dk+O~DD/x^KxURam;D+vJ/V^Y,Zslk/(9BrD9nD&fPw.G:,Hx;ZsCk/~h4nDPKCM+xOKmY4P^r0+~E]r[Dk`2b[r~E[M/cT*[JuvPKDN.~4HP}D[+.(GJb@#@&7dikW~	WY~cDDdRG6PC	N,YDkR(G0*POtx@#@&iddi[W,htbsnP	WDPODd WW@#@&7did7^Kxx nX+^EDnvJ;aNmY+,HUE;VC/kPdnDPrM[+Mqf{E'Y}D9+.q9QKV[WM[+M/Qrb[J~A4+.+,/^ldkqG'JLYMd`Z#b@#@&d77idkbxkb_F@#@&7didiY./ hK\nxaY@#@&77idVGGa@#@&di7x[,k6@#@&idiODkR^VK/n@#@&ddid+DPYMdxxKY4kUo@#@&idnx9~k6@#@&7ik'rQ8@#@&dir6Pr@*HK\+gEs~Y4+U@#@&d77M/`Wb';Vlkd(f@#@&id7Dd !w[lDn@#@&d771WxU 6nm!O`E!w9lYPtnx!Zslk/~dYPgn6Dqf{E~[,Dk`T#~',J~h4nDP/sm//(9{J~[,/^ldkqG#di@#@&7di+akDP[G@#@&dinx9Pk6@#@&diDkRhW-n	+aY@#@&d^WG2@#@&d.dc:G\U6O@#@&ik0,Dk +K0~Y4+U@#@&dd1Gx	R+Xn^ED+vJ;w[CD+~HUE;VCdkP/nO,n.+7(G'T,h4+DP;slk/(f{J~',ZVmd/&f#@#@&7+^/@#@&d7.k`X#{/Vm/d(G@#@&77M/ Ea[mYn@#@&idmKx	 +X+^ED+cE!wNmO+,H+	;/Vm/kPd+O~hDn\&9'rP'~M/`Tb,[~J,A4+.P;Vlk/&9'rP'P;VCdkqf*@#@&i+x9~r0i@#@&d./ ^^Wd+@#@&dk+O~M/'UGDtrxT@#@&d@#@&dE更新所要排序的栏目的序号@#@&imKUxc+a+1EOnvJEa[lD+PtnUE;Vm/dPdnDP6D9nD&fxELYr.[D(fLE,h4DPZ^lkdqG'E[;VCdkqf*@#@&iB如果有下属栏目，则更新其下属栏目排序@#@&7r0,m4ksN@*T,Y4+	@#@&idrx8@#@&77k+OPMd{mG	xc+6m!O+vJd+^+^O,ZVmd/&fPw.G:,Hx;ZsCk/~h4nDPKCM+xOKmY4P^r0+~E]r[nmDUYhlOtLJYv,WD9nD,4X,6.ND&fE#@#@&id[W,AtbVn~	WY~.kRnW6@#@&d7imKxxc+Xnm!Yn`rE2[mY+,\+	EZ^Cd/,/Y~r.[D(f{E[Dr.[Dq9Qb[EPS4Dn,Z^l/kqGxJLDd`Z#b@#@&ddir'b_F@#@&7diDkRhW-n	+aY@#@&diVGGa@#@&77M/ m^Gk+@#@&di/+DPMd'	WOtbxL@#@&d+	[Pb0@#@&7^l^V,ZsWdn;WUxvb@#@&d.nkwWUdR]+9rM+^DPrbNsk	mZ^ld/|HnU!Rlk2gzmYbGU'}D9+.1E@#@&+UN,dE(@#@&@#@&/E8~GWAx}.9+.g`*@#@&iNbhPk;srMNn.BD/}.ND~tG-+gEs~/VCdkq9~b@#@&iNrh,nl.n	Y(fB6MNnMqG~nmDUYhlOtBZ4r^N~h.+7qfBHn6DqG@#@&d/sm/dqGxKMkhcM+;;nkYcJ;sm/d&fr##@#@&i\W7+HEs'O.b:`Mn;!+/DcEHK\1;:Eb*@#@&dbWP;VCdkqfxErPOtU@#@&7isKEx92M.'DD;+@#@&77ADDtdo{2DM\do,[,J@!4.@*@!Vr@*错误参数！@!&Vb@*E@#@&ddnabY~/!8@#@&7Vk+@#@&di/Vm/dqG'/r	Y`;slk/qGb@#@&i+	N~kW@#@&dr0,\W7+H;s'JE~Dtnx@#@&idoKE	N2MD{OD!+@#@&id3.MH/Tx2MDHkL~[,J@!4.@*@!sb@*错误参数！@!z^r@*r@#@&7i+6rO,/;4@#@&i+sk+@#@&diHK-+gEh';kUOvHW7n1!:#@#@&7db0,HG\nH!:x!,Otx@#@&iddoG!x[2M.{K.!+@#@&didA.Dt/L'AD.\koPL~J@!4D@*@!sk@*请选择要下降的数字！@!JVr@*E@#@&d7dakDPd;(@#@&77x[PbW@#@&7x9Pk6@#@&@#@&iNr:,/5sBD/BGV9WD9n./Bkb~ODdSDr.N.qG@#@&7E要移动的栏目信息@#@&7dY~Dkx1WU	R6+1EDn`r/nVmO~hlDUY&f~}.[+MqG~Kl.n	YKlD4~1trs9~n.n7q9~gnXY(GPwDWsPtnx!Zslk/~A4+D~Z^l/k(9'r[;VC/d(G#@#@&iKlM+UO&f'.dv!b@#@&7}D[D&f'M/vq#@#@&7nmDnUDnlD4'M/`yb~[,JBJ~[~/^ld/&9@#@&d^4bVNx.k`f#@#@&in.\&f'M/v*#@#@&716O(G'Dkc**@#@&i.dR1VK/n@#@&7k+OPMd'	WO4bxo@#@&@#@&7B先修改上一栏目的gnXY(G和下一栏目的nM+\&f@#@&db0~nM+-(G@*!,Otx@#@&77mKx	Rn6n^!YnPr;w9lOn,H+U;;VC/k~k+O,16Y&f{EPLPH+XY(9,[Pr~h4+D~/Vm/kq9'E~LPKD-qG@#@&7xN~r6@#@&dbW,1nXY&f@*ZPD4+	@#@&dimGU	R+Xnm!Y+,E;w9lD+~HnU!ZslkdPk+O~hD+-(G'EPL~hDn7qGP[,J,AtDnP;VCdkqf{EPLP1aOqG@#@&dnx[~b0@#@&i@#@&iB和该栏目同级且排序在其之下的栏目RR OOR更新其排序，范围为要下降的数字@#@&dd;^xr/n^+1YP;Vmd/&fSrMNn.&f~14k^N~hC.+	YhlOtSKM+-qGS16O(GPs.GsP\+	;;VCk/,htD~nmDnxDq9xr[nm.+	YqG'EPmx9P6D[nMq9@*r'rMNn.&f[E~KD[+M~(X~}D9+D&fr@#@&i/nY,Ddxk+D7nDcZDCO+}4N+^YcEmNGN( DmG.9/+OE*@#@&dMdcW2x,/;^~1Gx	~q~2@#@&7b'!,~P,PPE同级栏目@#@&dbk{!~P~~,B同级栏目和子栏目@#@&i[W,h4r^+PUGDP./cnK0@#@&dimW	xcn6m;Y`E;aNlDnPt+x!/slk/,/nY~6MNnD&9'r[6.9+D(93kr[r~StnM+,ZVm/k(f{J'Dk`Tb*@#@&i7k6PDkc+#@*!,Y4+U@#@&d7dknY,Y.d{mWUUc+a+1;D+cr/V+1Y,/Vm/dqG~6.9+D&9PwDWs~\+	E;VC/d~StnD~nmDnUDnlO4,Vr3~E]ELDk`&*[rSJLDd`Z#'EuBPK.NDP(z~rMND(fEb@#@&7dir0,xGO,`Y.dc+G0,C	N~DDkR4K0*~Y4+U@#@&d77iNW,AtbV+,UGY,YM/ +GW@#@&7di7dbkxrb_F@#@&id7di^KxUc+X+m!YcJ!w[lD+~\xE;slk/PknOP}D9+.q9xr[6D9nD&fQrb[J~A4+.+,/^ldkqG'JLYMd`Z#b@#@&d77idYMdRsW\Un6D@#@&d7d7sKW2@#@&7di+U[,k0@#@&id7YMdcmsK/@#@&idid+DPODk'UGDtk	L@#@&ddU[Pb0@#@&7drr{kr_8@#@&idrxb_F@#@&idr0,r@*'\K\1EsPD4+	@#@&did.dv*#{/Vm//&9@#@&idiDdR;29lO+@#@&did^G	xRnam;YcrE29lD+Pt+	;Z^ld/,/nO,nD-qG'J,'~Dk`Z#~[~E,h4+MnP;VCdkqfxE,[~Z^Ck/(G#id@#@&di7+XkOP9W@#@&id+	[Pb0@#@&77DkRsW-+UnXY@#@&isWKw@#@&iD/ hK\nxaD@#@&ik6PDkRG0,Y4+	@#@&7imW	UR6+1;O+vJ!w[lOn,Hnx!/Vm/d~k+Y~H6OqGxZPA4+M+P;Vmd/&fxJ,[~/^l/k(f*@#@&ins/@#@&d7DdcW#xZ^C/kq9@#@&dd.dcE2NmO@#@&id1Wx	Ra+1EO+vJ;29lY~HxE;sC/kPk+OPHnXY(f{EPLP.dv!#~',J~h4nM+~;Vm//&f{EPLP/Vm/d(G#@#@&7+	NPbW7@#@&dM/ msGk+@#@&id+DP.d{xWO4bxL@#@&7@#@&7E更新所要排序的栏目的序号@#@&dmKx	 +X+^ED+cE!wNmO+,H+	;/Vm/kPd+O~}D[+M(f{J'6MN+.(G_rkLE,h4DPZ^lkdqG'E[;VCdkqf*@#@&iB如果有下属栏目，则更新其下属栏目排序@#@&7r0,m4ksN@*T,Y4+	@#@&idrx8@#@&77k+OPMd{mG	xc+6m!O+vJd+^+^O,ZVmd/&fPw.G:,Hx;ZsCk/~h4nDPKCM+xOKmY4P^r0+~E]r[nmDUYhlOtLJYv,WD9nD,4X,6.ND&fE#@#@&id[W,AtbVn~	WY~.kRnW6@#@&d7imKxxc+Xnm!Yn`rE2[mY+,\+	EZ^Cd/,/Y~r.[D(f{E[}D[nMqfQrb_r[r~StnM+,ZVm/k(f{J'Dk`Tb*@#@&i7db'k3q@#@&idiDdRhG7+U+XO@#@&d7sKWw@#@&id./c^^Wd@#@&ddk+D~Dk'UWDtrUT@#@&inx9Pk6@#@&d1l^V~ZsGk+/W	U`*@#@&7M+/2G	/nR"n9k.mDPJzNsrx|Zslk/m\xEcC/agb1OrW	'}D[+.Hr@#@&+	[PkE8@#@&@#@&d;(Pjl7n"+dYv#@#@&d9r:,kS/$VS.k~?!^m//tdL~bZKEUYSKM+-qGS16O(G@#@&7d$VxJkn^+^DP;Vlk/&9PwDG:,HnU!ZVmd/,WD9n.P(X,IGWO(G~6D9nD&fE@#@&d/nO,Dd'knM\nMR;D+mY64N+^YvJC[KN4c.+1WD9dnYr#@#@&7Dd Kwnx,d;^~^G	x~qS8@#@&db/KEUD'M/RM+1GD9mGE	Y@#@&ik'8@#@&inD-(f{!@#@&7NG~StrV~xKY~.kR+GW@#@&7dMdc:G7+	+6D@#@&7db0~DkRnG6PY4nx@#@&di7H+XY&fx!@#@&idnVkn@#@&d77g+6O(G'./vT*@#@&idxN,k6@#@&id./c:G-wD-kKE/@#@&7d1W	x +an1EO+vEEaNCOPHnU!Zslkd,/nDP"WWDqGxJ,[~k,[~EBrD9nD&f'ZSKlM+	Y(fxTBZ4k^['Z~KCM+xOKmY4'ETE~9wDt'Z~h.+7q9'rP'~hD+7(f,[PrSH+XY&fxJ~',1n6D(f,[~E,htn.P/Vmdkq9{J,[PM/vT#*@#@&din.n7qf{./v!#@#@&7db'b_q@#@&7iDdRsG\xnaD@#@&7sKW2@#@&7M/ 1VK/+@#@&id+DP./{xGO4kxT7@#@&d@#@&7jE1m/dHdL{J复位成功！请返回@!l,4D0xvzN:rU|Zslkd|Hn	Ecl/aB@*栏目管理首页@!zm@*做栏目的归属设置。E@#@&d^C^VPq.kD+?!^^+k/t/L`j;1mn/k\/T#@#@&xN~d!4@#@&@#@&kE8,?m\+`xbO+v#@#@&iNrh,ZVmd/&f~:C.oY;VC/d(G~KlMnxDnCO4~kKCM+UYhCDtSG+aYtBkhCDxOqG~/4bVNBKD\qGSH+XY&f@#@&7[b:~DkSYM/SrB?E^^/dHkL@#@&7;Vm//&f{ODb:cD;;nkY`r/Vm//&9E#*@#@&dPl.LY/Vmd/&fxOMk:c.;;+kOvJPmDT+Y;Vmd/&fE#*@#@&7b0P;slk/qGxEJ,Y4+U@#@&7isGE	[2MDxPME+@#@&id3DM\koxADMH/TPL~J@!4.@*@!Vr@*请指定要合并的栏目！@!zVb@*J@#@&dsd+@#@&id/VCdkq9';JxT`/sm//(9*@#@&dU9Pr6@#@&dk6P:CDT+OZ^ldd&f'rEPDt+	@#@&disKEUN3.M'PD!n@#@&d73MDHdL{2.DtdTP',J@!4D@*@!^r@*请指定目标栏目！@!zsk@*J@#@&i+Vkn@#@&dd:C.oY;VC/d(G'/S	L`:l.LYZsCk/(f*@#@&dn	N,k0@#@&ir0,Zslk/(9{KlML+DZVmddqGPDtnx@#@&idoW!UNAD.x:DEn@#@&d72M.t/L{2MDHko,'Pr@!8D@*@!sr@*请不要在相同栏目内进行操作@!z^r@*r@#@&inUN,k6@#@&drW,sGE	[2MDxPME+~O4+U@#@&7i+abY,/E(@#@&7+	N~k6@#@&7E判断目标栏目是否有子栏目，如果有，则报错。@#@&id+DPDkx^W	xc+a+^;D+cJknVmO~;tks[,0.Ws~t+U!Z^l/kPS4+M+~Z^ldd&f'r~[,KlMLnY;Vm/dq9b@#@&7k6~DkR8G6PlU[,DdRG6PO4+	@#@&idwGE	N3DM'P.!+@#@&7dADDtdL'ADMHdo~',J@!4M@*@!^k@*目标栏目不存在，可能已经被删除！@!JVk@*E@#@&7+^d@#@&idb0PM/vT#@*!~Y4+U@#@&ddioW!xNA..':D!+@#@&77i2.Dtdo{2..t/o~',J@!4M@*@!Vr@*目标栏目中含有子栏目，不能合并！@!JVk@*J@#@&di+UN,kW@#@&d+	[Pb0@#@&7r0,sKEUN3.M'PD!nPDtnU@#@&d7nXkOPk;(@#@&i+	NPb0@#@&@#@&dv得到当前栏目信息@#@&ddnDPDkxmKxxcna+1ED+cJdn^+^Y,/Vm/d(G~nC.xOqGShl.xDnlDtBKD\(fB1naDqfB9+aYt,W.WsPt+UE/sm/dPS4+M+~/^l/d(G'E[;sm/d&f*@#@&ikhCDxOqG'.dvF#@#@&dG+wD4xDk`l#@#@&7r6Prnm.+	Y(9{!PO4x@#@&i7hl.xDnlDt{./v!b@#@&dnsk+@#@&7dhlDUOnmY4'./c+*P'PrSJ,[~.k`!b@#@&dnx9~b0@#@&dbnlM+	OnmY4'M/cT*@#@&iKD\qGx./v&*@#@&dHnXY(f{./vcb@#@&d@#@&7E判断是否是合并到其下属栏目中@#@&dknDP.k'1Wx	Ra+1EO+vJdn^+mD~Z^l/k(9P6DK:~HnU!ZslkdPStn.PZsCk/(f{ELKCMoYZ^lkdqG[EPmx[~hlDUYhlY4~sk0+,BE[KCM+UYhCY4[EYEJ#@#@&ikWP	GDPcM/c+W6PmUN,DdR(WWb,YtU@#@&ddwG;x92MDxY.;@#@&di3DMHdL{2D.\ko~[,E@!4.@*@!^k@*不能将一个栏目合并到其下属子栏目中@!z^r@*r@#@&di+arDP/!8@#@&d+	[~k6@#@&d@#@&7v得到当前栏目的下属栏目&f@#@&id+DP.d{mWUUc+a+1;D+cr/V+1Y,/Vm/dqGPW.K:Ptnx!ZVmddPStDnPKCM+UYhCY4Psr0+PvELnCDUDnCDtLJ]EJ*@#@&ikx!@#@&7r6PxKOPvD/cnG0,l	N~Dd (WW#,Otx@#@&idNG~StrV~	WO,DkR+K0@#@&didrnmDnUDnlD4'bnlMnUYhlDt~[~EBJ~[,./v!b@#@&dd7r{kQF@#@&id7M/c:W7+	n6D@#@&diVGGa@#@&inx9Pk6@#@&db0,k@*!~O4+U@#@&7dhl.n	YnCO4'rnm.xOhlDt@#@&ds/@#@&dinC.xYhCY4'Z^Cd/&f@#@&7+U[,kW@#@&7@#@&dv先修改上一栏目的H6Y(9和下一栏目的hDn\&9@#@&7b0,nD\&9@*ZPOtx@#@&idmKUxc+6^;YPrE2NCOP\+	;Z^ldd,/+O~g+aY&9{J~LPg+6DqG~[,J~h4+.n,ZVmd/&f'r~'PhD\(f@#@&i+UN,r0@#@&7r6P1naDq9@*Z~Dtn	@#@&dd1W	UR6nm!Yn~rEw9CYPHU;Z^lk/~/nO,n.+7(f{J~',nDn-&f~[,E,h4DPZ^lkdqG'EPLPHnXYqG@#@&i+x9~r0i@#@&d@#@&7v删除被合并栏目及其下属栏目@#@&7mKUxc+an1EYncrNnVOPWMWsPHx!/Vm/dPStn.PZ^C/kqf,rUPvJLnCDnUDnCY4'J*Jb@#@&d@#@&7E更新其原来所属栏目的子栏目数，排序相当于剪枝而不需考虑@#@&dbW,fnaY4@*!,Y4nx@#@&7d1WUUc+6^ED+`r;2NmYP\+U;;VC/k~/Y~/4kV[x;trV9R8PA4+M+P;Vmd/&fxJLkKCM+xD(f*@#@&inUN,k6@#@&d@#@&i?;m1n/kHdL{J栏目合并成功！已经将被合并栏目及其下属子栏目的所有数据转入目标栏目中。@!8.@*@!8D@*同时删除了被合并的栏目及其子栏目。E@#@&71l^VPqDbO+UE^m/d\ko`U;m1+/k\do*@#@&dd+O~M/xxKOtbxL@#@&d/nO,Y./{UKY4bxT@#@&x9~/!4@#@&@#@&qdoiAA==^#~@%> 
