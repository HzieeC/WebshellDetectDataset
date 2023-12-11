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
    <td height="30"><a href="Admin_Class_MenuEn.asp">菜单栏目管理首页</a> | <a href="Admin_Class_MenuEn.asp?Action=Add">添加菜单栏目</a>&nbsp;|&nbsp;<a href="Admin_Class_MenuEn.asp?Action=Order">一级菜单排序</a>&nbsp;|&nbsp;<a href="Admin_Class_MenuEn.asp?Action=OrderN">N级菜单排序</a>&nbsp;|&nbsp;<a href="Admin_Class_MenuEn.asp?Action=Reset">复位所有菜单栏目</a>&nbsp;|&nbsp;<a href="Admin_Class_MenuEn.asp?Action=Unite">菜单栏目合并</a></td>
  </tr>
</table>
<%#@~^SAUAAA==@#@&kW,b1YkKx{Eb9NEPDtnU@#@&d1CV^Pb9[/Vm/k`b@#@&n^/nk6~b1YrG	'JjC7+)N9E,Y4x@#@&d1l^sPUl-+zN[c*@#@&s/k0,)^YbW	'EHG[b0zJ,Otx@#@&imlss,HGNbWH`b@#@&V/k6~b1YrW	'Ejm\+tGNb0Xr~Otx@#@&7mCs^Pjl7nHKNrWH`#@#@&Vd+bW,b^DkKx'rHK-+rPOtx@#@&iml^sPtW\/slk/v#@#@&nsk+r0,)mDkGU{J?C-HG\E,Y4x@#@&d1l^sPUl-+tW-nv#@#@&nVk+k6~)mDkKxxJ9n^J~Y4nx@#@&7^mVV~9VnY/^ldk`*@#@&Vknk6P)mDkGU{JZ^nlMJPD4nx@#@&imCVs~;VnlM/Vm/dc*@#@&nsk+r0,)1YrKx{JjarM[+MJ~Y4+U~@#@&d1CV^Pja6.NDv#~@#@&n^/nk6~b1YrG	'J9GSx6D9nMJ~DtxP@#@&i^l^V~fKhU6MN+Mc#,@#@&sd+b0,b^YrG	'ErM[+MJ~O4+x@#@&imCV^~}D[Dv#@#@&+^d+b0~b1YrG	'J`2rMN+MHEPDtx~@#@&71lsV,iw}D[nM1`b~@#@&nVknb0~zmDkW	'r9WSx6D9+.HrPY4nx,@#@&i^CV^PGWAx6.9+.1vbP@#@&nsk+kW~zmOkKU{J6MND1rPD4+	@#@&d1lss,rD9nDg`#@#@&nVk+b0~b^ObWU'r]+k+OE,YtnU@#@&7mms^P]/Y`*@#@&nVk+r0,b^ObWx{E?m\+"nd+DJ,Y4+U@#@&d^l^sPUl-n"+/nOv#@#@&sk+r6PzmYbW	xJ`xrYJ~O4+x@#@&d1lV^~ixbY`b@#@&n^/nk6~b1YrG	'JjC7+ixbOJ~Dtx@#@&d1CV^Pjl7+iUbY+vb@#@&+Vkn@#@&immVsPhCbxc#@#@&+	N~r6@#@&rW,sGE	[AD.{KME+,Y4nx@#@&7mmVs~qDkDn2MDHkLc#@#@&x[PrW@#@&^l^sP;VGdZWUUv#@#@&@#@&@#@&d!4,:lbxvb@#@&d[ksPC.M?tKASbx+vqT#@#@&i0GD~r{!~YK~E(W;U9`l..UtGhdr	+b@#@&idlMDU4WSSrx`rb{sl^d+@#@&d	naY@#@&iNr:~d$V/Vmd/BDd/^l/dSb~rf2Dt@#@&dk;V;Vmd/{Jd+^+^O,ePw.WsP2	\nx!Z^ld/~GMNnD,8X,IGGDqfS6MNnD&9r@#@&i/YPM/;slk/x/D-nMRZMnlD+r(%nmD`rl[W[8cDnmK.Nk+OE*@#@&7.kZslkdcW2x,/;^Z^C/k~^W	xSqBF@#@&64oBAA==^#~@%>
<br> 
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="table_southidc">
  <tr class="title"> 
    <td width="178" height="22" align="center"><strong>菜单名称</strong></td>
    <td width="188" align="center"><strong>菜单链接地址</strong></td>
    <td width="291" height="22" align="center"><strong>操作选项</strong></td>
  </tr>
  <%#@~^IgAAAA==~@#@&[KPStk^+,UWDP./;VCdkR+KWP@#@&TgkAAA==^#~@%>
  <tr class="td_southidc" onMouseOut="this.style.backgroundColor=''" onMouseOver="this.style.backgroundColor='#BFDFFF'"> 
    <td> <%#@~^nQUAAA==~@#@&7bfwY4'MdZ^ld/vJ9naYtrb@#@&dk6~./;Vm/d`EH6OqGE#@*!~O4+x@#@&idCDMj4WAdk	+`bf2Y4#xKMEn@#@&d+^d+@#@&diC.DUtKhJkUnvk9+aOt*'oC^/+@#@&i+UN,r6@#@&ik6PkG+aOt@*!~Y4+U@#@&dP,70KDPbxqPDW,k9+2O4P@#@&i7db0~r{kfn2Dt~Y4n	P@#@&diddb0,./;VC/k`EH6Y&9J*@*!,O4+	P@#@&7d77iDn/aGxk+ AMkYn~r@!r:T~kD^{B&:lT+JOD+mVbxnqcok6vPSkND4xB8GEP4+rL4YxB8B,\Csboxxvm4-:b[9VnE@*rP@#@&di7dVd+,@#@&7iddi.+kwW	dnRSDbYnPE@!b:LPk.m{B(hmo+&OM+n{^r	++cob0B,hb[Y4'vFFB~4ko4O'EFvE~-l^kTxxBC87:rN9s+E@*E~@#@&d77i+UN,r6P@#@&did+^/~@#@&d7dikW~mDDU4WSSk	nck*':D;+~O4+U@#@&7did7./wGUk+ hMrD+~r@!b:o,/M^'EqhlT+&OM++|sk	+&cLr0EPSk[Y4xEF{B,4+bo4O{BFv,\CVbL	'vm47:k9N^nB@*J~@#@&d77i+Vkn@#@&ddi77D/aWU/n SDrY~J@!khL,/D^xEqhlTnJY.+|Vk	+W ob0vPSk[O4'B8{B,t+bL4Y{B8vvP-C^kLx{vl(\hr9NVnv@*J~@#@&7id7x9Pk6@#@&7di+UN,kW~@#@&d,~d	+6D~@#@&iP,+UN~r6P@#@&i~Pb0~.kZVCdk`EZ4r^NE*@*ZPY4+	~@#@&d~PiDndaWxknRSDkDn~J@!kso~/.^{B(:mL+JY.n{0Gs9+.ccLb0v,hbNY4'Eq*EP4+bo4O{BFlvP7lVbLU'El(\hk[[^+v@*r~@#@&d~~V/n~@#@&7P,7M+daW	/+chMrYPE@!b:L~kDm{vqslo&OD+|0GV[nM& obWB,hr[Dt'vqlB~trTtO{B8*B,\mskTxxBm4-hbNN^nB@*JP@#@&7P,+	N~kW~@#@&7P,r0,Dd/^l/dcrfnwD4r#xZPDt+	P@#@&d,P7D/2G	/+cADbY+,E@!4@*J,@#@&d~~x[PbWP@#@&7~,D+d2Kxd+cAMkOPr@!l,tMn0{B)NskUm;Vlkd{t+x!3URm/ag)mOrKxxHK[k6X'/^l/d(G'EPL~M//^lk/`rZ^C/kq9J*P'~rBPDrY^+'EE~[,DkZslddvJ]+m[HJb~LPJv@*rP'PMd;VCk/vJZ^lkd1m:nJ*P'~r@!zm@*J@#@&d,~r0,DkZslddvJ/tbsNr#@*T,YtnU,@#@&d,~iDnkwKx/RS.kD+~J（rP'~M/Z^C/k`J;4rV9J*P'PE）E,@#@&d,~+	N~r6@#@&7~,14kBAA==^#~@%> </td>
    <td width="188" align="center"> <%#@~^ggAAAA==@#@&dr6PM/Z^lkd`rSrx0j.sr#@!@*EJ,YtU@#@&idM+dwGUk+ hMrYP.d;VlddvJJk	V`Dsr#@#@&dVkn@#@&d7D/2G	/+cADbY+,E没链接地址E@#@&dx[PrWi@#@&dnCAAAA==^#~@%> </td>
    <td align="center"><a href="Admin_Class_MenuEn.asp?Action=Add&ParentID=<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>">添加子菜单</a> 
      | <a href="Admin_Class_MenuEn.asp?Action=Modify&ClassID=<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>">修改设置</a> 
      | <a href="Admin_Class_MenuEn.asp?Action=Move&ClassID=<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>">移动菜单</a> 
      | <a href="Admin_Class_MenuEn.asp?Action=Clear&ClassID=<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>" onClick="return ConfirmDel3();">清空</a> 
      | <a href="Admin_Class_MenuEn.asp?Action=Del&ClassID=<%=#@~^EgAAAA==.kZsm/k`J;Vmd/&fE#8wUAAA==^#~@%>" onClick="<%#@~^GwAAAA==r6P.kZ^l/k`r/tbV[J*@*T~Dt+	gAgAAA==^#~@%>return ConfirmDel1();<%#@~^BAAAAA==n^/nqQEAAA==^#~@%>return ConfirmDel2();<%#@~^BgAAAA==n	N~b0JgIAAA==^#~@%>">删除</a></td>
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
<form name="form1" method="post" action="Admin_Class_MenuEn.asp" onSubmit="return check()">
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
      <td><input name="ClassName" type="text" size="37" maxlength="30"></td>
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
        <input name="Cancel" type="button" id="Cancel" value=" 取 消 " onClick="window.location.href='Admin_Class_MenuEn.asp'" style="cursor:hand;"> 
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
<%#@~^CAIAAA==@#@&+U9PkE4@#@&@#@&/!4~HKNrWH`#@#@&d9k:,/slk/&fS/5sBDdZ^C/k~r@#@&dZsCk/(f{OMkhvD;E/DcJ;VC/kq9E*#@#@&7k6PZ^Cd/&f{JEPO4x@#@&i7sKEU[ADDxPMEn@#@&7i2.MHko'ADM\/TP'Pr@!8.@*@!Vb@*参数不足！@!JVk@*E@#@&id6rY~d!4@#@&inVk+@#@&idZsCk/(f{/dxLvZ^l/kqGb@#@&dnx9PrW@#@&dk5V{J/snmDPCPoDGh,2UHUE;VCdkPh4nM+~Z^Ck/(G'rP[,Z^C/kq9@#@&ddnDPDk/Vm//{dnD7+MR/DnCD+64NnmDPcEzNW[8cDnmK.9/nDJ*@#@&iDk/Vm/dRKwnU,/;^SmKxxBqS&@#@&ikWP.d;VC/k 4K0~C	NP.d;VC/k WW,Y4+x@#@&i7sKEUNAD.x:DE@#@&id2M.\/T'AD.HdL,[~J@!8D@*@!sr@*找不到指定的栏目！@!zsr@*J@#@&in^/n@#@&eYoAAA==^#~@%>
<form name="form1" method="post" action="Admin_Class_MenuEn.asp" onSubmit="return check()">
  <table width="100%" border="0" align="center" cellpadding="2" cellspacing="1" class="table_southidc">
    <tr class="title"> 
      <td height="22" colspan="2" align="center"><strong>修 改 菜 单 栏 目</strong></td>
    </tr>
    <tr class="td_southidc"> 
      <td width="350"><strong>所属菜单：</strong><br>
        如果你想改变所属菜单，请<a href='Admin_Class_MenuEn.asp?Action=Move&ClassID=<%=#@~^BwAAAA==/^ldkqGgwIAAA==^#~@%>'>点此移动菜单</a></td>
      <td> <%#@~^3AIAAA==@#@&dr6PM/Z^lkd`rnCDxO(GJ#@!x!,YtU@#@&iP,d.+d2Kxd+cADbYn~r无（作为一级栏目）J@#@&7Vd+@#@&,P~,d9k:,DkKlM+UY;VCdk~/$snmD+	O/Vm/k@#@&d7d$VKlMnxDZsCk/'EjVnmD~CPoMWsP2	HUE;VC/kPA4D+,/Vm//&9~k	PvJ~[~.kZslkd`rnC.xYKCDtE#,',Jb,WMN+MP(zPG+2Y4J@#@&id/OPM/nm.nxDZ^ld/xdD-+M ZM+COr4%n1YcJm[KN8cDmWMNknYr#@#@&id.dhlDUY;Vlkd Wa+	Pd;sKmDnxD/Vm/dS1WxUS8~q@#@&7iNG,h4kVP	GY,DdnmDnUDZVmd/c+W6@#@&did6W.Prx8POW,./hl.n	YZsCk/cJGnaY4r#@#@&didi.+kwGxk+ AMkY~JLx4k2I[	4kwI[U8kwIJ@#@&didUnXY@#@&7idr0,.knCM+	YZ^lkd`rfnwDtEb@*!PD4+	@#@&i77dM+kwGxdnch.kDnPr└J@#@&iddnU9Pr0@#@&id7M+kwW	/ hMkO+,J'U(/wpEPLPDkKCDxDZslddvJ/Vmd/glhnr#P'~r@!8D@*E@#@&7idM/nmDUY;VC/kRhG7+xaY@#@&disGWa@#@&d7DdKmDnxD/Vm/d 1VWdn@#@&7dknDP.knmD+	Y;slk/xxKY4r	o@#@&7+	NPbW@#@&i4dMAAA==^#~@%> </select></td>
    </tr>
    <tr class="td_southidc"> 
      <td width="350"><strong>菜单名称：</strong></td>
      <td><input name="ClassName" type="text" value="<%=#@~^FAAAAA==.kZsm/k`J;Vmd/glh+r#5wYAAA==^#~@%>" size="37" maxlength="30"> 
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
        &nbsp; <input name="Cancel" type="button" id="Cancel" value=" 取 消 " onClick="window.location.href='Admin_Class_MenuEn.asp'" style="cursor:hand;"> 
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
<%#@~^RQIAAA==@#@&dn	N,k0@#@&i./;VC/kR^sK/+@#@&dk+Y,.dZ^lk/xxGO4kUo@#@&+	N~d!4@#@&@#@&/;4,\K\n;Vm//v#@#@&d9khP;VCdkqfBd;^~Dk/slk/Bk@#@&7/^ld/&9'DDrhvD+5;/O`r/^ldkqGJ#*@#@&7k6P/Vm/d(G'Jr~Y4+x@#@&7dwW!x[2..{K.E@#@&id3.MH/LxAD.HkL,[~r@!(D@*@!Vb@*参数不足！@!JVr@*r@#@&7i+6bOPkE4@#@&7+^/@#@&d7/^ld/&9';SULvZVCdkq9#@#@&i+U9Pb0@#@&d@#@&dk;s'r/nsmY,MPwDWs~3xt+	E/VCdkPAt.+,ZsCk/q9xrP'P;sm/d&f@#@&dk+D~DkZslk/xdD\.R;D+mOnr(LmOPcEzNGN( DmG.9/+OE*@#@&dMd;VCk/cWwx,d;^~^W	xSqB&@#@&7k6PDk/slk/c4G0~C	N~Dk/Vm/d W0~O4+U@#@&7isG!x92DM':.E@#@&di2..t/o{3DMH/T~'Pr@!(D@*@!sr@*找不到指定的栏目！@!&Vb@*J@#@&7n^/+@#@&3poAAA==^#~@%>
<form name="form1" method="post" action="Admin_Class_MenuEn.asp">
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
        <%#@~^mwIAAA==@#@&dr6PM/Z^lkd`rnCDxO(GJ#@!x!,YtU@#@&iP,d.+d2Kxd+cADbYn~r无（作为一级栏目）J@#@&7Vd+@#@&,P~,d9k:,DkKlM+UYB/5shlDUY@#@&did5VhlM+UYxEU+s+1OPCPo.K:P3Ut+UE;sm/d,h4+DP;slk/(f,kU~vJPL~DkZVmdd`rnmDnxOKmY4J*~[,Jb~KDNn.,4zPGnaY4r@#@&ddk+D~DknCDxOxk+D7nDcZDCO+}4N+^YcEmNGN( DmG.9/+OE*@#@&di.knCM+	YRKwUPk;snmDnUD~mKUxBF~8@#@&diNKPAtrsPUWD~DknC.xY nK0@#@&i7i0GMPb'F,YK~DknCDxOcrf+aOtr#@#@&77diD/2WUdRADbO+,J'U(/wI'	4dwp'	4dair@#@&idiU+XY@#@&id7r6PDkKlM+xDcEfwDtE#@*T,Y4+	@#@&id77M+/2G	/nRS.bYn,J└r@#@&idinx9Pr0@#@&77iD+k2W	/+cA.kD+,J'x8daiEPL~DknC.xYcE;VC/kHm:nr#,[Pr@!(.@*r@#@&did.dhlDUYc:W7nU+XY@#@&7dsGKw@#@&i7DknC.xY ^^Wd+@#@&iddY,D/hlMnxD'UWDtrUT@#@&inx9Pk6@#@&dY7oAAA==^#~@%>
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
        <input name="Cancel" type="button" id="Cancel" value=" 取 消 " onClick="window.location.href='Admin_Class_MenuEn.asp'" style="cursor:hand;"></td></tr>
  </table>
</form>
<%#@~^RAEAAA==@#@&dn	N,k0@#@&i./;VC/kR^sK/+@#@&dk+Y,.dZ^lk/xxGO4kUo@#@&+	N~d!4@#@&@#@&/;4,6MNnM`*P@#@&d9r:,/5V;VCdk~Dk/Vm//BrSk;W!xO~%~@#@&7/$sZ^ldd{J/nsmOPC~wDGsPAxHx!/Vm/dPStn.Pnm.+	YqGxTPKD9+.P8z,IGWD(frP@#@&i/+O~M//Vmdk'dD7+DcZMnlD+64N+^OvJl9GN(RD^GD9/YE#~@#@&d./;slk/ Ga+x~d$V/Vmdk~^Kx	~FBF,@#@&ik/W!xOxM/Z^C/kRD^GD9mKEUY~@#@&H2IAAA==^#~@%>
<br>
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="table_southidc"> 
  <tr class="title">  
    <td height="22" colspan="4" align="center"><strong>一 级 栏 目 排 序</strong></td> 
  </tr> 
  <%#@~^KgAAAA==~@#@&%{F,@#@&9W,AtbVnP	WO~M/Z^C/kR+KW~@#@&XQoAAA==^#~@%> 
    <tr class="td_southidc" onMouseOut="this.style.backgroundColor=''" onMouseOver="this.style.backgroundColor='#BFDFFF'">  
      <td width="200">&nbsp;<%=#@~^FAAAAA==.kZsm/k`J;Vmd/glh+r#5wYAAA==^#~@%></td> 
<%#@~^awUAAA==~@#@&7b0,L@*8PD4+	P@#@&,P77M+/aGxk+RS.rYPr@!WW.h,l^YbGx{B)[skxm/^ld/|\x;Axcl/agz^YbWU'`w6.9+DE~:YtK[xBaWkYv@*@!O9PAk9Ot{BqXZB@*E~@#@&7dMnkwG	/RhMkDnPr@!d+^+^O,xlsn'tW\H;:,/byn'q@*@!W2YbGx,\Cs!+'T@*向上移动@!zGwDrKx@*rP@#@&di0K.Pb'qPDW~% FP@#@&didDd2W	/RADrOPE@!K2YbWU~7lV;n{J'kLE@*J'b[r@!zKwDrW	@*EP@#@&77	+6D~@#@&ddMndwKxk+ h.rD+~J@!&/Vn^D@*J~@#@&d7DdaWUk+chDbY~J@!kUw!Y~OHw+{4k9N+	~Uls+{Zsldd&f~\msE'E'M/ZsCk/cJ;sm/d&fr#[r@*r@#@&id.+kwGUk+RS.kD+Pr@!rxaEDPOX2n{trN9nx,xCh'm]GKY(f,-mV;'r[DkZ^C/k`EIKWO(GJ#LE@*Lx4k2I@!bxaEOPOza+x/!8:bY~Um:+xj!4hkD~7ls!+{修改@*J,@#@&7dM+dwKxdnchDbO+,J@!JO[@*@!z6W.:@*E,@#@&ds/P@#@&idDndaWU/ SDrD+,J@!DN,Ak9Y4'EFXTE@*[	8/ai@!JO[@*rP@#@&7+U[,kWP@#@&db0~r;WEUO@*L~Y4n	P@#@&P,ddM+k2W	/nRSDrOPJ@!WWM:Pm^OkKx{B)Nhr	{/Vmd/|HnU!2x Ckw_b1ObWU{fKhx}D9nDEPh+DtG[{BwKdYE@*@!D[~hbNDtxBqXZB@*J,@#@&id.nkwWUdRADbOPE@!/V+1Y,Uls+xHK\nH!:Pkry'F@*@!GwDkKx~\Cs!+x!@*向下移动@!zKwOrKx@*E~@#@&7d6GMPr{F,YW,k;GE	YRL,@#@&7idDdwKx/ ADbYPE@!G2DkGx,-l^Enxr[k'E@*J'kLE@!zGaYbWx@*J,@#@&idU+XY~@#@&ddMn/aWxkn hMkD+~J@!&k+s+1O@*rP@#@&idDndaWU/ SDrD+,J@!bxa;Y,Yzw'4r9N+	~xm:+{/slk/&f~\Cs!+xJL./;VCdk`J/sm/dqGE*[E@*J@#@&diDdwKxd+ch.rD+Pr@!k	wED~OXa+{trN[n	PUlsn'1IGGDqf~-mV;+{ELDd;Vm//vJ"GWDq9J*[E@*Lx4k2i@!kxa;OPDXa+x/;8skOP	C:'j;(:kO~7lsEx修改@*J~@#@&idD/aGxk+ hMkOn,J@!JON@*@!z6G.:@*J,@#@&dnsk+~@#@&7dM+d2Kx/n SDrY~r@!O9PSkNDt{vFl!v@*Lx8dai@!JON@*JP@#@&7+	N,kWP@#@&fJEBAA==^#~@%> 
      <td>&nbsp;</td>
	</tr> 
  <%#@~^LwAAAA==~@#@&7N'N_F,@#@&7DkZslk/ hK\+	n6DP@#@&sGWaP@#@&lAoAAA==^#~@%> 
</table> 
<%#@~^LQEAAA==~@#@&7M/;Vlk/c^VK/nP@#@&7dYPMdZ^l/kxUWDtbxLP@#@&x[Pk;4,@#@&@#@&/E8~}D[+MHv#~@#@&iNksPk5V;VC/k~.d;Vlkd~b~k;G;xD~DDd~i2tW-+g;:BfGA	HW-ngEhP@#@&i/5^Z^l/k'rd+^+^Y,e~oMW:,3xt+x!/slk/,W.Nn.,4zP"GWDq9S}DNn.&fEP@#@&i/nDPM/Z^lkd'k+.\D /M+lDnr(L+1OcJmNKN8R.n1W.NknYr#~@#@&dDd/^ld/cGa+U,/$VZ^lkd~1WUxBFSq,@#@&L1wAAA==^#~@%>
<br>
<table width="100%" border="0" align="center" cellpadding="0" cellspacing="1" class="table_southidc"> 
  <tr class="title">  
    <td height="22" colspan="4" align="center"><strong>N 级 栏 目 排 序</strong></td> 
  </tr> 
  <%#@~^IgAAAA==~@#@&[KPStk^+,UWDP./;VCdkR+KWP@#@&TgkAAA==^#~@%> 
    <tr class="td_southidc" onMouseOut="this.style.backgroundColor=''" onMouseOver="this.style.backgroundColor='#BFDFFF'">  
      <td width="300"> 
	  <%#@~^NgIAAA==~@#@&76WMPk{F,OW,DdZ^lddvJf2Y4J#,@#@&d,PiDn/2G	/nRS.kD+~ELx4d2p[U4k2p[U(/aiJ,@#@&7x6OP@#@&7r6PDk/Vm//vE/tbV9Jb@*T~Dtnx,@#@&id.nkwWUdRADbOPE@!ksoPkD1xB&:CozO.+{6GV9+DW Lk6B,hrNO4{Bq*E~tkL4D'BqXEP-l^rTxxEl(\:bN9s+E@*EP@#@&7n^/+,@#@&iPPi.n/aW	/nRA.bYnPr@!kso~dMm'v(slL+JOM+n|0KVND2 ob0vPSk[O4'B8XB,t+bL4Y{B8*vP-C^kLx{vl(\hr9NVnv@*J~@#@&7x[,k6P@#@&dbWPM//Vm/dcrnlMnxDqfrbx!,Y4+UP@#@&id.+k2W	/n SDkOn,J@!4@*E,@#@&i+	NPb0,@#@&iDn/aWUdRhMrYPDk/slk/vJ/VCdk1C:E#,@#@&7b0P.d;VC/kcrZ4bV9J#@*!,Otx~@#@&d7./wKU/RhMrO+,JvJ~[~.kZslkd`rZ4r^NJb~LPE#r~@#@&7x9Pk6P@#@&dMp4AAA==^#~@%></td> 
<%#@~^rgcAAA==~@#@&7b0,D/;Vmd/vJKlM+UO&fJ*@*!,YtU~P,B如果不是一级栏目，则算出相同深度的栏目数目，得到该栏目在相同深度的栏目中所处位置（之上或者之下的栏目数）,@#@&d7v所能提升最大幅度应为wW.PbxF,YG~该版之上的版面数,@#@&77k+OPD.k'^Kx	R+X+1;Y`E/Vn^DPmK;xD`Z^Cd/&f*PoDGh,2UHUE;VCdkPh4nM+~nm.xO&f{J[M/;slk/cJhl.n	YqGE#LJPmU[P}D9+.q9@!r[./;slk/cE}DNn.&fE#LEr#~@#@&idjaHK-+gEh'DDdcZ#P@#@&dik0,rdx!V^`iw\G7+HEsbPDtnU,jw\G7+HEsxZP@#@&dik0,ja\W7+HEs@*T~Dt+	~@#@&PPi77D/aWU/n SDrY~J@!0G.sPl^ObWU'E)9:r	{;Vlk/|\+	E3xcld2QbmDrW	'ja6.NDgB~:nO4W['E2WkYv@*@!YN~AbNOt{v8*TE@*rP@#@&di7D/2W	/n SDkDnPr@!/snmDP	lh+x\K\n1!hPkk"n{F@*@!GaYrW	~7ls!+{!@*向上移动@!zK2YbWU@*rP@#@&idd6GD,k'8~OW,jaHG\nH!:~@#@&7did.nkwWUdRADbOPE@!WaYkKx,-l^En'r[r'r@*JLr[r@!zK2OkKx@*J~@#@&7idU+XOP@#@&77iD+d2Kxd+cAMkOPr@!zk+^nmD@*EP@#@&77iD+k2W	/+cA.kD+,J@!kU2!Y~YH2+{tr[9+x~Um:n';sm/d&f,\l^ExJLDdZ^lddvJZ^C/kqfrb'J@*[	4dwI@!bx2ED~YHwnxkE4hrDPUlsn{?;(:bYP7l^;+{修改@*EP@#@&77iD+k2W	/+cA.kD+,J@!zO[@*@!&0K.:@*J~@#@&ddnsk+~@#@&7id./aWxk+cADbYnPr@!O[,hk9Ot{BFlTv@*Lx(/2i@!&DN@*J,@#@&idnU9PkW~@#@&7dD.kR^^Wk+P@#@&i7B所能降低最大幅度应为wW.Pb'q~DWP该版之下的版面数,@#@&id/O~YM/{mGxU 6nm!O+vJdn^+mO~1W;xDc;VCk/&f#,sMG:,2UHx;/^l/k~h4+D~KlM+	Y(fxELDdZ^C/k`EKmD+UO&fE#LE,lU9PKDND&9@*r[./;VCdk`JK.NDqGEb[rJ*P@#@&77GWAxtG\1;h{YDdcZ#~@#@&7ikW,kkxE^Vv9WSx\W7+H;s#PD4+	PfKAUHK\1;:xT,@#@&dir0,fGA	HW-ngEh@*Z~Dtn	P@#@&P,di7D/2W	/n SDkDnPr@!0K.hPmmDkGxxvzNhk	mZ^ldd|H+U;Ax lk2Qb^DkKx'GWSUrMNnDgB~hYtK['EwWkOv@*@!Y9PAk[O4'vFlTB@*J~@#@&dd7./2W	dRAMkD+Pr@!knVmOP	lhn{HW7n1!:Pkr"+{F@*@!GwOrKx~\msE'T@*向下移动@!zW2ObWU@*r~@#@&7id6WD,k{qPDW~fKhU\K\+g;:,@#@&i77dM+kwGxdnch.kDnPr@!G2DkWU~7lsExr[rLJ@*J[b[r@!zKwOkKx@*E,@#@&i7d	+6D~@#@&idiDn/2G	/nRS.kD+~E@!z/nsmO@*r~@#@&7idM+/aW	d+ch.kD+~E@!kxa;Y,YXanxtbN9+UPUCs+xZ^C/kq9~7lV;n{J'Dk/^ldk`rZVm/k(fr#'J@*[U8kwi@!rxaEY,Ozw'kE8:rO,xC:x?!4hrDP\Cs!+x修改@*r~@#@&7idM+/aW	d+ch.kD+~E@!zY9@*@!J0WMh@*J,@#@&d7+sdP@#@&i7dM+d2Kx/n SDrY~r@!O9PSkNDt{vFl!v@*Lx8dai@!JON@*JP@#@&7dx9Pr0~@#@&d7YMdR1VGdP@#@&7Vd+,@#@&d7M+kwW	/ hMkO+,J@!O9PmKs/alx{+@*[	4kwI@!&O9@*EP@#@&dx[~b0P@#@&BxgCAA==^#~@%> 
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
      <form name="form1" method="post" action="Admin_Class_MenuEn.asp?Action=SaveReset"> 
        <table width="80%" border="0" cellspacing="0" cellpadding="0"> 
          <tr>  
            <td height="150"><font color="#FF0000"><strong>注意：</strong></font><br>
              &nbsp;&nbsp;&nbsp;&nbsp;如果选择复位所有菜单，则所有菜单都将作为一级菜单，这时您需要重新对各个菜单进行归属的基本设置。不要轻易使用该功能，仅在做出了错误的设置而无法复原菜单之间的关系和排序的时候使用。 
            </td> 
          </tr> 
        </table> 
        <input type="submit" name="Submit" value="复位所有菜单">
         &nbsp; <input name="Cancel" type="button" id="Cancel" value=" 取 消 " onClick="window.location.href='Admin_Class_MenuEn.asp'" style="cursor:hand;">
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
    <td height="100"><form name="myform" method="post" action="Admin_Class_MenuEn.asp" onSubmit="return ConfirmUnite();">
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
        <input name="Cancel" type="button" id="Cancel" value=" 取 消 " onClick="window.location.href='Admin_Class_MenuEn.asp'" style="cursor:hand;">
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
<%#@~^ZYUAAA==~@#@&@#@&/!4PUl7nb9Nc#@#@&7[b:P;slk/qGS/Vm/k1C:nSUtGh}UKKwS]lNhnBSrx0iMVShD\rMN.qG@#@&d9kh~k;VB./BYDk@#@&d9ksP]WGO&fSnm.+	Y9naYtSKmDnxDKmY4BnmD+	YUODBnCDxOHm:+B\lXZVmddqG~tlaIGGDq9@#@&7Nb:~KM+\(9B1n6D(G~/4k^N@#@&@#@&7Z^ld/glhn{YDbh`M+;!ndYvJ;VC/dHm:nJ*b@#@&dj4KhrUPKwxYMrs`.;!+/D`rjtKh6x:W2E*#@#@&7IlNsnxYMks`.+5;/O`r]+mNhnr##@#@&iSrx0iMVxDDb:`M+$;+kYcJdkUV`DVrb#@#@&dbW~Z^lk/Hlhn{JEPD4+	@#@&7isW;U92.D{PMEn@#@&id2MDtdo{2.Dt/L~LPJ@!8D@*@!Vb@*栏目名称不能为空！@!z^k@*J@#@&7n	N~k6@#@&ikW~UtWA6	KGw{EI+drPDt+	@#@&7dUtGh}xPGa'KM;+@#@&dsd+@#@&idjtGA}xPWaxsmVdn@#@&dnU9Pr0@#@&ikW,sKEx92M.':D;+,Y4n	@#@&i7+XkY,d;4@#@&i+UN~r6@#@&@#@&7/Y~.kP'~^KxURam;D+vJ/V^Y,HC6vZsCk/qGbPwDWs~3xt+	E/VCdkJb@#@&7Hm6/sm//(9{Dd`Zb@#@&7b0,k/	E^s`tlaZ^ldd&f#,Otx@#@&77Hm6;VC/d(G'T@#@&7+	N~r6@#@&7.kR^VKd@#@&iZ^l/kqGxHm6/Vm/d(G_F@#@&dk+Y,.d'1W	x +an1EO+vE/Vn^DP:CavDGWDr9#~wDK:PAxtnx!Zslk/Eb@#@&dtC6"WWD(9'M/v!b@#@&7b0~kkUE^Vc\m6IGGDq9#,O4+U@#@&idHm6"GWDq9'Z@#@&7xN,r0@#@&dMd m^Wk+@#@&7]KWOqGxHm6]GKYq9Q8@#@&d@#@&ikW,nmD+	Y&9@*ZPOtx@#@&id/$s'r/+^n^Y,e,s.Wh~Ax\+	;Z^ldd,htn.P/Vmdkq9{J,[PhlMnxDq9PLPEE@#@&di./cWwU~/$VBmGxUS8~q@#@&7db0~.kR4GW,lUN,.kRnK0,Ytx@#@&didoW!x[3MD':.E@#@&i772MDt/L'3.MHdo,'Pr@!8.@*@!Vr@*所属栏目已经被删除！@!zsk@*Ei@#@&idxN,k6@#@&idr0,sG;	N2M.':DE~Otx@#@&7d7.kR^VKd+@#@&77i/+O~M/xxKO4kUT@#@&ddi+XrY,/;4@#@&77V/7@#@&ddi]GWDqG'./cE"WGY&9J*@#@&7idnC.xO1mh'.k`rZVm/kHls+E#@#@&77inlMnxDf+aO4'M/vJ9+2O4Jb@#@&7dinC.xYKCDtxDkcrnCM+	YnmY4E#@#@&7diZ4r^N'Md`rZtbs[J*@#@&d7dKCM+UYhCY4'KCM+xOKmY4PL~r~E,[,nlM+	OqGP~P,Pv得到此栏目的父级栏目路径@#@&ddiKD\rM[nD&f{Dd`E6MNnD&9J*@#@&7idkW~;trV9@*ZPO4+	dd@#@&i7diNr:,DdKM+\}.NDqG@#@&didiB得到与本栏目同级的最后一个栏目的6D[nMq9@#@&7diddnDPDdKM+-rM[D(G'1Wx	Ra+1EO+vJdn^+mD~Hm6`}.[+MqG#~s.GsP3xtnx!ZsCk/PA4DnPhCM+UDqG'J,[,KlM+UY&fb@#@&ddi7nM+\}.[+MqG'./K.\6D9nD&fcT*@#@&77idd+D~DDd{mKxxc+Xnm!Yn`r/nsmY,/Vm//&9~0MWsP3x\n	E/Vmd/,h4nM+PKCM+UY&9{J~LPhlDxD(f,[~J,lU[,rD9nD&f'r~'PhD\6D[nMq9#@#@&did7KM+\(9{Y./vT*@#@&idid@#@&di7dE得到同一父栏目但比本栏目级数大的子栏目的最大r.ND(9，如果比前一个值大，则改用这个值。@#@&di7dk+Y,.dnM+7r.Nn.&fxmKUxc+an1EYncr/nV^DP\m6vrD9+M(f*PoDK:~3	H+	;Z^l/k~AtDPKl.n	YKlD4P^kVn,BJ~',nCDUDnCDt,[Pr~uvJ*@#@&did7r6P`	GYvD/h.n\}D9+.q9 (WWPmUN,DdKM+\6.9+.qG WW*#,Ytx@#@&did7db0~UKYP&d1!VVv.dnM+7r.Nn.&fc!*bP,Y4n	@#@&77id~dir6P.knM+\}D9nD&fc!*@*K.\rM[+Mqf,O4+	@#@&d7d77idKD-rMNn.&f'.dhDn\}.9+.&fv!#@#@&i7did7+	N~r6@#@&i7did+	[~k6@#@&d7d7n	N~k6@#@&id7n^/+@#@&id7dh.\(G'Z@#@&idinx9Pr0@#@&@#@&id+	[Pb0@#@&77DkR1VG/n@#@&dnVkn@#@&d7r6PHCa"WGY&9@*!~Dtx@#@&di7/Y~YM/x^Kxxcn6mEDncJk+^+^Y~/^ld/&9P6DGh,2x\n	E/VmdkPA4+M+P"WKOqG'EPLP\CXIWKOqGP[,E~l	N,fnwO4{!E#@#@&didK.\q9xDDd`Zb@#@&7idDD/cm^G/@#@&di+sd@#@&i7dhD+7(9'Z@#@&d7+U[,kW@#@&7dhDn-}DNn.&fx!@#@&idKmDxYhlD4'r!E@#@&dnU9Pk6@#@&@#@&dk5s'r?VnmO~CPoDKhPAx\n	EZsCk/~4nM+~hlM+xDqGxJ,[~nmDnUDqf,'PrPbg9~Z^lk/Hlhn{BEPL~Z^lddgl:n~LPEBr@#@&ddY,D/{/.\D ZM+COr4NnmD`Jm[GN(RM+^W.[k+OJ*@#@&iDd Kw+U~k;s~1G	xS8~8@#@&ik6~xKYcDkR8G6Pl	[PM/RGW#,Y4+U@#@&7isGE	[2MDxPME+@#@&idr0,KmDn	Y&f'ZPD4+	@#@&did3.MH/Tx2MDHkL~[,J@!4.@*@!sb@*已经存在一级栏目：EPL~Z^lddgl:n~LPE@!Jsb@*E@#@&id+^/@#@&id72MD\dT'2M.HkoPL~E@!(D@*@!sk@*“E,[~nm.+	YHCs+P'~r”中已经存在子栏目“J~[,/^ldk1m:+,[,E”！@!JVr@*r@#@&7i+x9~k6@#@&i7./cm^Wd+@#@&idd+D~Dk'UGDtkUL@#@&7dabY~kE(@#@&i+	[Pb0@#@&iDd 1VWkn@#@&@#@&id5V{JU+s+^O,YGw,qPCPo.K:P3Ut+UE;sm/dr@#@&dDkRK2+	Pd;^~^G	x~8S&@#@&P,~~DkRmN[xnA@#@&7DkcJ;VCdkqfEb{Zslkd&f@#@&P,PdM/vEZ^ld/glhnr#';slk/1mhn@#@&dM/cJj4Kh6x:Gwr#xj4Wh6U:W2@#@&7M/crIKWY&frb'"WGY&f@#@&iD/vEnmD+	O(fr#{nCDnUDq9@#@&7k6PKCM+xO(G@*TPD4x@#@&diD/vJGnwDtE#{nC.xYGnwDt_8@#@&dVk+@#@&77M/cJGnwDtEb{!@#@&7x[PbW@#@&7M/vJnmDUYhlOtr#xKmD+	OnmYt@#@&7Dk`rr.Nn.&fE#{KD\6.9+D(9@#@&7DkcrZ4bV9J#{!@#@&dM/cJ"+C[s+J*xIlNsn@#@&iDk`ESrU0j.Vrb'dkUV`DV@#@&iDd`rKM+-&fr#'hD-qG@#@&dM/cEg+6D(fr#'Z@#@&dM/cE2NCO@#@&dMdR;VGd@#@&~~,Pd+D~M/xgWDtk	o@#@&d@#@&7B更新与本栏目同一父栏目的上一个栏目的“g+aO&f”字段值@#@&7k6PnMn-qG@*ZPOtnU@#@&7d1Gx	RnamEOnvJ;w9CD+~Axt+x!Z^C/kPd+DPHnXYqGxJ,[P;sC/kqGP'PE~StnD~Z^ldd&f'E~LPKD-&fb@#@&i+x9PbW@#@&d@#@&ikW~hlDUY&f@*Z~Otx@#@&7dv更新其父类的子栏目数@#@&d7mKUxc+an1EYncrE2NmOP3	HxE;Vmd/,/nY,m4r^N'14k^N_8~AtDP/VCdkq9'r'nmDnUDqfb@#@&d7@#@&7iB更新该栏目排序以及大于本需要和同在本分类下的栏目排序序号@#@&dimW	xcn6m;Y`E;aNlDnPAxHU;Z^lk/~/nO,r.N.qG'6.9+D(93F~h4nM+~MWKYk9'r~[,DGWDk[~LPJ,Cx9PrM[nD&f@*J~[~KM+-rM[+Mq9b@#@&d7^KxURam;D+vJEaNmO+,2UHx;/^l/k~/YP}.[+MqG'EP'~hDn\}.ND(9,[PEQ8PAt.P/^lk/qG'r~[,Zslk/(9*@#@&inx9Pk6@#@&d@#@&,P~P^C^V~Z^G/ZGU	`#@#@&iIn/aG	/ncINkM+1OPrb[:bxm/^l/kmHxEAU lkwrP~@#@&n	N~/!8@#@&@#@&d!4PjC7+\W9r6Xc*@#@&dNb:,/Vm/d1m:nS"+l9h+B?tKA6x:Wa~JkUV`Ds@#@&7Nb:~OM/~.d@#@&7Nbh,Zsm/kqfB/$s~M//Vm/dSb@#@&i[ksP?0rUZKE	YSSCzKEOZK;xD@#@&7;Vldd&fxYMrs`.;!+/D`r/Vm/dqGJbb@#@&dbWP;Vlkd(f{JrPOtnU@#@&7dwGE	N3.M'K.;@#@&di3MD\ko{2DMHkLPLPE@!(D@*@!^k@*参数不足！@!&Vb@*J@#@&7+^/@#@&d7/^ld/&9';SULvZVCdkq9#@#@&i+U9Pb0@#@&d;slk/Hls+xOMk:v.+$E+kOcJ;Vm/d1ChJb#@#@&dUtGA}xKG2{Y.kscM+5!+kY`r?4Gh}xPWaJbb@#@&d"nl9:+{O.ks`M+5EndD`EICNs+Eb*@#@&7JbxVjMs{Y.b:vD+$EdYvJJk	3i.^J#*@#@&ik0,/slk/glh+xErPOtU@#@&d7oKEx[3MDxKM;@#@&idADDt/Tx2MD\/TP'~r@!4M@*@!^k@*栏目名称不能为空！@!&sk@*J@#@&7+U[,kW@#@&7@#@&drW,sW;U92.D{PMEn,Y4+x@#@&i7+XkOPkE8@#@&d+	[Pb0@#@&7@#@&i/$VxJdn^+^Y,MPwDGh,2x\n	E/VmdkPA4+M+P;Vmd/&fxJ,[~/^l/k(f@#@&dknOPM/;VC/dxk+.\.R;DnCD+r8%mOPvEzNG94cD+1WM[/YE#@#@&7.kZVmd/cWwU~/$VBmGxUS8~f@#@&7k6P.d;Vlddc4G0,C	N~M/;Vlk/cnW6POtx@#@&idsK;x92DMxPD!+@#@&7d3.MHdo{3DMHdL,[PE@!(D@*@!^r@*找不到指定的栏目！@!&^k@*J@#@&di./;VC/kR^sK/+@#@&di/+D~./;Vm/d'UGDtrxT@#@&idnabYPd;(@#@&dU9Pr6@#@&dk6PU4WSrUKKwxEI+/r~Y4+x@#@&7dUtKh6xPGa'PD!n@#@&dnsk+@#@&7i?4WS6	KGa'wlVk+@#@&dx[Pb07@#@&dk6~sKEx93.D{KMEnPO4x@#@&i7DkZsCk/R^sK/n@#@&7i/nDPM/Z^lkd'	WOtbxL@#@&ddakDP/!8@#@&i+	N~kW@#@&d@#@&,~PiDd/^l/dcrZslkdglhJ*'Z^lkd1m:n@#@&d.d;Vlkd`r?tKA6x:WaJb'j4Kh6x:Gw@#@&7.kZVCdk`EIC9:nr#{I+mNsn@#@&d./;VCdk`Jdrx0jD^Eb'dk	3iDs@#@&d./;slk/ ;aNlOn@#@&7Dk/^ldkR1VWk+@#@&dk+OPM//sm//{UWDtk	L@#@&i@#@&dd+O~M/xxKOtbxL@#@&d/nO,Y./{UKY4bxT@#@&i@#@&~P,P^l^V~/^W//W	x`*@#@&d"+kwGxdncInNb.+1Y~EzN:rU|Zslkd|Hn	EAxRm/aEP,@#@&+	N~d!4@#@&@#@&@#@&/!8~fVYnZsCk/c#@#@&d9kh~k;VS.k~KD-&fSg+XYqG~;slk/(f@#@&7/^l/k(f{YDbhcI;!+dYcE;VC/k(fr#b@#@&dkW~;VC/k(G'ErPDt+	@#@&7dwW;x92..{KD!n@#@&ddA..Hko{2.D\dTP'Pr@!4M@*@!sb@*参数不足！@!&sb@*E@#@&7i+abY,/E(@#@&7+^/n@#@&d7/^l/k(f{ZS	LcZ^lk/(fb@#@&dnx9~k6@#@&7@#@&dd5^'E/smO,Z^l/kqGSIKWOqG~9naYtBKlM+xD(9~;tbV[~K.\(fBH+XY(9,sDGh,2UHU!Zsm/kPh4+MnP;VC/kq9xr[Z^C/kqf@#@&7/Y,Dd'dnM\nDc/DlOn}4Ln^DPcJz[KN8cDmWMNknYr#@#@&iDd Kw+	~/$V~1GUxBFB&@#@&7r6P./c8W6PCU9PDd WWPD4x@#@&disW!x93DM'PD!+@#@&id2M.Hko'A..Hko,[~J@!8M@*@!Vb@*栏目不存在，或者已经被删除@!JVr@*r@#@&7n^/n@#@&7ikW,Dk`J;tbsNr#@*!,Y4n	@#@&i7dwWE	[3DM':D;+@#@&id72M.Hkox3MDHdL,[~J@!8M@*@!^k@*该栏目含有子栏目，请删除其子栏目后再进行删除本栏目的操作@!z^k@*E@#@&d7+	N~r6@#@&inx9Pk6@#@&db0,sGEU[AD.':.EPO4x@#@&7iDdR1sK/n@#@&id/Y,./{xGY4kUL@#@&din6bYPk;8@#@&dx[PrW@#@&7nMn\&fx.k`JK.\(frb@#@&7g+XYqG'Md`r1n6Dq9E*@#@&ir0,D/vE9+aY4Jb@*T~Dtnx@#@&dimGU	R+an1EO+vE!w[mYP2	HUE;VC/kPdnDPm4rV9'm4rsN F,h4+.n,ZslkdqG'E~LPDdcrnCDUDq9r#*@#@&i+	[Pb0@#@&iDd 9+VO+@#@&dMd EaNmYn@#@&7M/ m^G/@#@&7k+Y~.k'UWD4bxL@#@&i@#@&iB修改上一栏目的gn6Dq9和下一栏目的nM+-(G@#@&ir0,nD-(f@*!,Y4+U@#@&d7mKUxc+an1EYn~rE2NmOP3	HxE;Vmd/,/nY,1naDqf{EPLP1aOqGPLPEPA4DnP;slk/(9{JP'~hDn\&9@#@&7x9Pk6@#@&7k6PH+XY(9@*!PD4+	@#@&i7^W	xc+a+^;D+~J!2NmYn~AxHnU!Zslkd,/nDPhD+7qGxJ,[~nM+-(GP[,EPSt+Mn~Z^lk/(fxE,[~1aY&f@#@&i+x[~b0@#@&i^mVs,Z^W/ZKUxv#@#@&iDndaWxknRM+Nb.nmDPrb[:rU|Zslkd{t+U;AxRCdaJ@#@&i7@#@&n	N,/E(@#@&@#@&kE8P;VnCMZVmd/v#@#@&7[ksPkY.ZsCk/(fB./BY.dBZVCdkq9@#@&7;VCk/&f'DDbh`"+5E/OcrZVmd/&fJ*b@#@&ik6P/VCdkq9'rEPDtnU@#@&d7oKEUNA.M'PME@#@&idA.Dt/L'AD.\koPL~J@!4D@*@!sk@*参数不足！@!JVr@*E@#@&d7+XrY,/;8@#@&dnsk+@#@&i7;VCk/&f';S	L`;VC/kq9b@#@&dUN,k0@#@&7/DD;VC/d(G'^/D.`;VCdkqfb@#@&dd+D~M/x1W	xR6^ED+cJk+sn1YP;slk/qGS/tbV9~Kl.n	YKlD4P6DGh,2x\n	E/VmdkPA4+M+P;Vmd/&fxJ,[~/^l/k(f*@#@&irWPM/c4G0~C	N~Dk +K0~O4+x@#@&idoW!U92.M':DE@#@&7dAD.Hkox3MDHkLPLPJ@!8.@*@!Vb@*栏目不存在，或者已经被删除@!zsr@*J@#@&i7+XkO~kE4@#@&i+UN,r6@#@&ik6PDk`8b@*ZPOtx@#@&id/OPDD/{^Gx	R6nm;O`E/s+1Y~/^l/d(GPWDKh,2Ut+	EZ^lkdPStnDPKCM+xD(f{JPL~./v!*#@#@&779W~h4rVPUGDPY.dc+G0@#@&id7kYMZVm/k(f{/OD;VCdkqf,'Pr~J,'~YM/v!b@#@&7idODk :K\nU6Y@#@&idsWK2@#@&7iYM/R1VKd+@#@&7dk+O~DD/{^W	xRanm!Y`E/nsmOP;slk/(9,0DGh,2UHU!Zsm/kPh4+MnPhl.+	YKCDtP^r3PBr~'PM/v bP'~r~EPL~Dk`Tb,[PESuBE#@#@&id[KPStk^+,UWDPODkRnG6@#@&i7dkYD;sC/kqG'dY./^ld/&9PLPESrP[~OM/c!*@#@&d7iYM/RsW7nx6O@#@&d7sKWw@#@&diYDk ^VK/@#@&d7dY~YMd'	WO4bxo@#@&i+UN,r6@#@&iDkRm^Wkn@#@&dd+DP.d{xWD4k	o@#@&nUN,/!4@#@&@#@&@#@&dE(~?m\n\K\+cb@#@&7Nbh,Zsm/kqfB/$s~M//Vm/dSb@#@&i[ksPDhC.+	Y&f@#@&7[b:~YMd~M/@#@&iNkh~hl.+	O&fS"WKYqG~GnwDtSZ4ks[BnlMnxDnlD4SnmDxO1Ch~rnm.+	Y(9BknC.xOnmO4~KM+7rD9+M(fBn.+7q9Sg+6D(f@#@&d;sC/kqG'ODrhvDn;!n/D`E/^l/d(GJb#@#@&ikW,Z^l/kqGxJrPOtx@#@&idsK;x92DMxPD!+@#@&7d3.MHdo{3DMHdL,[PE@!(D@*@!^r@*参数不足！@!&^k@*J@#@&din6bY~/!4@#@&i+Vkn@#@&dd;sC/kqG'/SULvZslkdqG#@#@&i+x[~b0@#@&i@#@&dd$V{J/V^Y,e~sMWh~AxHUE;Vlkd~h4+M+~ZsCk/(f{EPLP/sm//(9@#@&7/O,Dd;Vm//{/.\D ZM+COr4NnmDP`r)[W94cDnmG.9/nYrb@#@&d.d;VlddcW2+	~k;sBmKxxBFBf@#@&dr0,Dd/^l/k 4K0PmU[PM/;VC/d WWPD4+	@#@&7isW;U92.D{PMEn@#@&id2MDtdo{2.Dt/L~LPJ@!8D@*@!Vb@*找不到指定的栏目！@!z^k@*J@#@&77M//Vmd/cmsGk+@#@&7i/nY,.kZsm/k'xKY4rxT@#@&di+arDP/!8@#@&d+	[~k6@#@&@#@&d.KmDnxD(f{Y.rs`Dn5!+dYvEhl.xDqfr#*@#@&ikWPMnC.xY&9'rJPD4nx@#@&id.nC.xOqGx!@#@&7n^/+@#@&id.nm.xO&f{ZS	ov.nmDnxDq9b@#@&dUN,k0@#@&7@#@&db0~Dd/^ld/vEnmDnUDqfEb@!@*.nm.xO&f,Ytx,~PE更改了所属栏目，则要做一系列检查@#@&dikW~MnlMnxDqf{.dZ^lk/cJ/sm/dqGE#,Y4n	@#@&77isGE	[AD.{KME+@#@&i7dAD.Hkox3MDHkLPLPJ@!8.@*@!Vb@*所属栏目不能为自己！@!zsr@*J@#@&i7+	N~r6@#@&77E判断所指定的栏目是否为外部栏目或本栏目的下属栏目@#@&dir6P.kZ^l/k`rKlM+UY&fEb{!PD4+	@#@&i77k6PMnCDnUDq9@*Z~Y4+U@#@&dd77k+OPD.k'^Kx	R+X+1;Y`E/Vn^DPDKGYbNPw.G:,2	Hnx;/^ld/,AtDn~dkxViMVxBE~mx[,Z^l/kqGxJLDKlM+UO&f#@#@&diddbW~YM/c4G0~C	N~YMdRWW~Dt+U@#@&d7di7wW;	NADD{KM;+@#@&7did73MDHkL'ADDtdLPLPr@!8D@*@!^k@*不能指定外部栏目为所属栏目@!Jsk@*J@#@&idd7n^/n@#@&7id7ik6PDkZ^C/k`EDKWOr9J#{ODk`!*~Otx@#@&7d77idoW!UNAD.x:DEn@#@&d7di7i2.MHko'ADM\/TP'Pr@!8.@*@!Vb@*不能指定该栏目的下属栏目作为所属栏目@!JVk@*E@#@&idid7+U[,kW@#@&7didnU9PkW@#@&d7diOM/ 1VK/+@#@&i7di/nY,Y.d{xWD4k	o@#@&77dx9Pr0@#@&idnVkn@#@&d77k+Y~OM/xmKU	RnX+1EY`rd+^+^Y,ZsCk/qG~sMW:,3UHx!Zsldd,h4+MnPhl.n	YnCO4Psk0n,BELDkZVm/kcJhl.+	YKCDtJ*'JBJPL~./;Vm/d`E/^ld/&9J*P'~r]B~C	N~Z^Ck/(G'r[DhlMnxDq9#@#@&77ik0,UWDP`D.dRW6PCx[~DDdR(G0*PO4x@#@&7id7sK;	N3MD{KD!+@#@&did72MD\dT'2M.HkoPL~E@!(D@*@!sk@*您不能指定该栏目的下属栏目作为所属栏目@!JVr@*r@#@&id7n	NPrW@#@&7diOM/ 1VK/+@#@&i7dk+OPDDdx	WY4rxT@#@&i7nx9Pb07d@#@&i+UN,r0@#@&@#@&ik0~oKEUNA.M'PMEPY4+	@#@&id./;VCdkRm^G/@#@&i7d+DPM//VCdk'UWD4k	o@#@&id+arDPdE(@#@&dn	N,k0@#@&i@#@&ikWPM//sm//vEnmD+	O(fr#{!~Y4n	@#@&diKlM+UO&f'.d;VC/kcrZsm/kqfr#@#@&dikKlM+UO&f'Z@#@&i+Vkn@#@&idhl.+UO&fxDk/Vm/dcrnl.n	Y(frb@#@&7ikhlDxD(f{DdZ^lddvJnm.+	YqGEb@#@&dx[PrW@#@&7f2Y4'.d;VlddvJ9+aO4Jb@#@&iZtbV9xDkZslk/cE;tk^[J*@#@&i]GWDqG'.//sm/d`r]WKY(9r#@#@&7hl.+	OhlO4'M/Z^lkd`rnCDxOKmYtrb@#@&dnMn-qG'M//VCdk`EnMn\&fEb@#@&dHnXY(f{.kZsm/k`Jg+XOqGJb@#@&d.d;VlkdR1VWkn@#@&i/Y~Dd/^ld/{UWDtrUT@#@&7@#@&d@#@&,~E假如更改了所属栏目@#@&,PE需要更新其原来所属栏目信息，包括深度、父级qf、栏目数、排序、继承版主等数据@#@&,~B需要更新当前所属栏目信息@#@&~PE继承版主数据需要另写函数进行更新OR取消，在前台可用/^l/k(f,kx,KCDxDnCY4来获得@#@&P~NbhPsDdStl6]GKY(f@#@&,PdY,:Dk'1Gx	Rn6m;O`JknVmY,hC6vDKWOk[b,s.Ws~2	HnU!ZVCdkJb@#@&~,HCXIKWY&f{hDk`T#@#@&~~k+Y,hDk'xKO4k	o@#@&~PrW,kdx!sVvHCa"WWO(G#~Y4n	@#@&iHm6IKWD(f{!@#@&,PnU9Pk6@#@&,PNbh~3Bxhl.+UOhlOtBhnmDnUDnlO4@#@&~P9rsPKmDxYU;^SZ^ld/;W;UD@#@&,~Nb:PMdKD\}D[+.(G@#@&P,r0,msUT`wC.xOk9b@!@*.hlM+xDqG~l	N~xKY~cbnlMnxDqf{T~l	N,DKl.n	Y(f{T#,Y4n	PPv假如更改了所属栏目@#@&dv更新原来同一父栏目的上一个栏目的1aDq9和下一个栏目的hD\qG@#@&7k6PKD\(9@*!PD4+	@#@&i7^W	xc+a+^;D+~J!2NmYn~AxHnU!Zslkd,/nDPg+6DqGxJ,[~16O(GP[,EPSt+Mn~Z^lk/(fxE,[~nMn\&f@#@&i+x[~b0@#@&ir6PH6Dqf@*!,Otx@#@&id^G	xRa+1EY~EEaNmYnP3Ut+UE;slk/~dYPK.\(f{E,[~hD\qGPL~J,h4+M+~/^l/k(f{JPL~H+XY&f@#@&7n	N~k6@#@&i@#@&7b0PrKmDnxD(G@*T,l	NPMnm.+	Y(f{!~O4+x,~dE如果原来不是一级分类改成一级分类@#@&i7v得到上一个一级分类栏目@#@&di/5VxEk+s+1OP;VCdkqfSH6OqG~6DGsPAxHx!/Vm/dPStn.PIKGY&f'r~'PtlXIGWO(GP'Pr~l	N~9wY4xZJ@#@&i7k+O,Dk'/D7nDcZ.+mYn6(L+1O`rbNK[8RM+1W.NdnDJb@#@&7dM/ Ga+x~d$VSmKU	~qB&@#@&dinMn\&fxDk`Tb,PP,~PE得到新的nD-(f@#@&id./cq*'/Vmd/&f~~,PPv更新上一个一级分类栏目的H6OqG的值@#@&d7M/cEw9lDn@#@&d7DkR^sK/+@#@&di/+D~./{xKY4kUL@#@&7d@#@&diHCa"WWO(G'\lX]KWO&f3F@#@&div更新当前栏目数据@#@&d7mKxU 6+1;Y`J!2[lD+,2UHnU!ZslkdPk+O~9+wO4{!SrM[D(G'Z~DKWDrN{J':m6.GKYk9'JBwlMnUYbN{!SnC.xOnmOt{BTvBnDn-&fxJ,',n.\&fPLPrS16OqG'T~St+MnP;Vlkd(f{JLZsldd&fb@#@&7dE如果有下属栏目，则更新其下属栏目数据。下属栏目的排序不需考虑，只需更新下属栏目深度和一级排序q9cMWWOr9#数据@#@&i7b0~1tbVN@*!,Otx@#@&id7r{!@#@&7dinlMnUYhlDtxnC.xOnmOt,[~EBJ@#@&7idd+D~M/x1W	xR6^ED+cJk+sn1YPC~sMW:,3UHx!Zsldd,h4+MnPhl.n	YnCO4Psk0n,BYr[hlDxDKlDt~[,ZsCk/qG'JuBJ*@#@&did9W~h4r^+~xKOPM/ nK0@#@&7id7k{r3F@#@&diddsnm.+	YKlDtx.wVm^+vD/vEKlM+	YKlO4r#Snm.+	YKCDt~EE*@#@&di7imG	xc+6m!O+vJ;w9lOn,2xtnx!ZVmddPk+DP[+2O4'[+aOt J'[wY4'r~.WKObNxr[sl6MWKOk9[E~hl.n	YnmOt{BJLhKlM+	YKlO4LJvPS4+M+~/^l/d(G'E[MdvJ/^lk/qGJ*b@#@&d7diDd sW\U+XY@#@&77d^WKw@#@&77iDdR1sWk+@#@&idddnDP./{UKY4bxT@#@&idUN,kW@#@&d7@#@&ddE更新其原来所属栏目的栏目数，排序相当于剪枝而不需考虑@#@&idmKUUR6m;YncrE2NmO+,2U\xE/sm/dPknDP^4k^N'1tbsN F~h4+.n,ZVmd/&f'r'rnmDxOq9b@#@&7d@#@&dVdnb0PrKmDnxD(G@*T,l	NPMnm.+	Y(f@*!~O4+x,~P,B如果是将一个分栏目移动到其他分栏目下@#@&77B得到当前栏目的下属子栏目数@#@&idKl.n	YKlD4'hl.n	YnCO4P'PrSr@#@&idk+Y,DkxmKxUR6n^!Y+vE/V+1O~mKE	Yceb~wDG:,3xt+U;;Vldd,h4+Mn,nCM+	YnmY4~Vb3nPE]E'hlDUYhlY4~'P;Vm/dq9'r]vJ*@#@&id/sm///G!xO'Mdv!b@#@&idk6Pbdx!Vs`;VCdkZW!UY*PY4nU@#@&did/VCdkZGE	O'8@#@&7i+x[~b0@#@&i7M/ 1VK/+@#@&i7/Y~Dk'UGDtk	L@#@&dd@#@&7dE获得目标栏目的相关信息di@#@&d7dY~YMd'1WUUc+6n^!Yn`rdVn1Y,ePwDKhPAx\+	E/sm//,AtD+,/slk/&fxJ'.hl.+	OqG#@#@&idkW~DDd`r/4ks9J*@*!,Y4nxid@#@&id7v得到与本栏目同级的最后一个栏目的}DN.qG@#@&i77/Y,Ddn.n7r.N.qG'^G	xRnam;Ycr/n^+1YPtlXcrMNnD&fb~wDWs~2	H+	;/Vm/kPAtn.PKlMnxDq9xrP[~OM/cJ;sm/d&fr##@#@&i7dhDn\}D[nMqf{./hD+76.ND&fc!b@#@&d7dE得到与本栏目同级的最后一个栏目的/Vm/d(G@#@&77i/5V{Ek+smDPZ^lkdqG~H+XY(9,0DKhPAxHU;Z^lk/~h4nM+~nm.+	Y(9{JP'~DDd`r/^ldkqGJ#,[,EPmx[P}D[nMqf{EPLPnMn-rMND(f@#@&id7/OPM/xdD\n.cm.+mOW8N+1Y`rl9GN(R.+1W.[k+Yrb@#@&ddi.dRKwx~/5sBmGx	SFB&@#@&iddK.\(f{.k`T*P,PPE得到新的nMn\&f@#@&id7.k`F*xZ^l/k(9P,P,Pv更新上一个栏目的1naDq9的值@#@&7diDd !wNCO@#@&di7M/ 1VK/+@#@&i7dk+OPM/xUKYtbUo@#@&di7@#@&idiB得到同一父栏目但比本栏目级数大的子栏目的最大6D[nMq9，如果比前一个值大，则改用这个值。@#@&7di/nO,D/K.\6D9nMq9{mKxxc+Xnm!Yn`r/nsmY,\lX`rM[nD&f*PoDGh,2UHUE;VCdkPh4nM+~nm.xOhlDtP^k0nPEJ~[,Y.dvJnm.+	YnmO4J*PLPE~E~LPODkcJ;VCdkqfEb,[~JBYEJb@#@&iddb0,cxKYcDkn.n7rD9nD&fR(GWPmx9P./K.\6D9nD&f nK0#b~Dtnx@#@&id7ik6PxKY,(/gEsVvDdKM+\}.NDqGcT#*P,Y4+U@#@&d7d,7db0~.knDn-}D[+M(G`T*@*hD+7rM[+Mq9PDtnU@#@&di7didnMn-rMND(fx.kn.+76D9+.(G`!b@#@&d7di7x[,k6@#@&idi7+	N~k6@#@&7id+	[Pb0@#@&77+^/@#@&d77hDn\&9'Z@#@&7idn.n7r.N.&fxDDk`J}D9nD&fE#@#@&77xN,r0@#@&di@#@&diB在获得移动过来的栏目数后更新排序在指定栏目之后的栏目排序数据@#@&7d^G	x +Xnm!YncrEw[CD+~2	\x;;Vm//,/OP}D[+Mq9x}DN.qG_J,'~Z^lk//W;UDP'PrQF,h4nM+P.GKYrN{E,[~DDk`JMWKOk9JbPLPE~mxN,6D9+D&9@*J,[,n.+-6MNnD&9#@#@&77@#@&d7v更新当前栏目数据@#@&7d1G	x 6mED+vEEaNCYP3Ut+x!/Vm//,dnY,NwOtxELY./vENwO4r#[EQ8~6D9nMq9{JLnD\}.ND(fLJQqBDWKOk9'JLO./vJMWGYr[r#'JBKlM+UO&f'E'MnCDUDq9LJBnlM+	OnmY4'EJ~',YDkcJhlDUOnmY4JbP'~r~EPL~YM/cE;Vldd&fE#,',JvBnM+\&f{EPLPKD\(9,[PrS16Y&9x!,h4+.+~/^ld/&9'r[/sm//(9*@#@&di@#@&d7E如果有子栏目则更新子栏目数据，深度为原来的相对深度加上当前所属栏目的深度@#@&ddk+D~Dk'^W	x nX+m!O+vJ/snmDPCPoDGh,2UHUE;VCdkPh4nM+~nm.xOhlDtP^k0nPE]E[hl.n	YnmOtLZVmddqG[r]vPG.9+.P(zP}D[nMqfEb@#@&7dbx8@#@&id9WPStbs+,xGY,Dd W0@#@&didk{rQF@#@&id7kKCM+UYhCY4'O.k`JKCM+UYhCDtE*PLPJBJ,'PDDd`rZsCk/qGE#,[PrSEPLPM+2VC^`./vEnmDnUDnlO4r#Snm.xOhlDt~rJ*@#@&id7mKxU 6+1;Y`J!2[lD+,2UHnU!ZslkdPk+O~9+wO4{NnwD4 J'9+aYtLJ3E[DDd`rNn2DtJ*'J3F~}.[+MqG'E[K.\6D9nD&f'E3J[r'r~.WKObNxr[DD/vJMGWDk[J*[EShlDUYhlY4xvJLkhl.+UOhlOtLEB,h4nM+P/sm/dqGxr[.k`rZVm/k(fr#b@#@&d77M/RsG\x+XO@#@&id^WGw@#@&id./c^VK/n@#@&dddnDP./{UKY4bxT@#@&idD./cmsWk+@#@&id/OPDD/{UGY4k	o@#@&77@#@&7dE更新所指向的上级栏目的子栏目数@#@&id^G	xRnam;YcrE29lD+PAxtnx!Zslk/~dYP14k^N'14rV9_8PAtn.P/Vmd/&fxELDnC.xOqGb@#@&7i@#@&ddE更新其原父类的子栏目数di7@#@&d7mKxU 6+1;Y`J!2[lD+,2UHnU!ZslkdPk+O~1tks[{m4k^[ F~StD+,Z^C/kq9'r[rKmD+	OqG#@#@&7nVk+,P~Pv如果原来是一级栏目改成其他栏目的下属栏目@#@&d7B得到移动的栏目总数@#@&di/nO,D/x^KxURam;D+vJ/V^Y,mGE	YcM*PsMG:,2xtnUE;Vm/dPA4DnPMGWDk[xr[DGGDk[#@#@&id/^lk/ZKE	O'M/c!*@#@&7iD/c^VK/+@#@&7dk+DP./xUKY4k	L@#@&d7@#@&ddv获得目标栏目的相关信息7i@#@&didY~DDk'mKx	 +X+^ED+cEk+V^Y,ePw.G:,2	Hnx;/^ld/,AtDn~;Vldd&fxJL.hl.xDqf*@#@&7db0~YM/cE;tk^[J*@*!,O4+	di@#@&d77E得到与本栏目同级的最后一个栏目的r.N.qG@#@&7id/nO,DdnMn7r.9+Mqf{mKUxc+a+1EOnvJ/s+1YPtCa`}D9+.q9b,s.Ws~2	HnU!ZVCdkPAt.PKmDxY&f{EPLPODk`E/^l/k(fr##@#@&7dinM+-r.[D(f{./hDn-}DNn.&fc!*@#@&d7i/$V'r/s+1Y~Z^ldd&f~gn6Dqf,W.WsPAx\+U;;VC/k~h4+.n,nl.n	Y(f{E,[~DDk`J;Vmd/&fE#,[~E,lx9~rMN+M(9'rPLPKDn-}D[+M(f@#@&77i/+O~M/x/.7+.cmM+lD+K8LmO`rl[G94RMnmKDNknOJ*@#@&d7d.dcW2+	~/$VS^KxxSqB&@#@&i7in.\&f'M/vT#@#@&7diDdc8#';slk/qG@#@&didM/ E2[mYn@#@&7di/nO,D/xUKY4k	L@#@&7id@#@&didE得到同一父栏目但比本栏目级数大的子栏目的最大6D9+.qG，如果比前一个值大，则改用这个值。@#@&7id/OPM/nMn-rMND(fx^KxURa+1EOnvJ/nsmOPtCX`6MNDqG#,oDK:~2	HnU!ZVmd/,ht.nPhlM+UYKCDt~VbV+,BE~LPY.dvJKlMn	YKmY4J#,[,E~rP'PDDdcrZVmd/&fJ*~'Pr~uBE#@#@&id7k6~`	WOcM/n.n7r.N.&f (W6Pl	N,./hDn\}D[nMqfcnW6##,O4+	@#@&d7d7r6PUWD~qk1;s^`DdKM+-rM[D(G`Z##,PD4+	@#@&did~7ik0,./hD+76.ND&fc!b@*hDn\}.ND(9,YtnU@#@&7di7idKM+7rD9+M(f{DdnM+-6MN+M(fv!#@#@&7didi+UN~r6@#@&di7dx[~b0@#@&7idnx9~b0@#@&di+Vk+@#@&didKD\(9{!@#@&7dinD-6D9+Mq9'O.k`ErM[+Mq9E*@#@&77x[PbW@#@&7@#@&idB在获得移动过来的栏目数后更新排序在指定栏目之后的栏目排序数据@#@&i7mKxUR6n^!Y+vEEaNlDn~2	Hx;ZsCk/~/OP}D[nMqfx6MNnD&93J~LP;Vlk/;GE	Y~[r_q~St+MnPMWWDr['rPLPODdcrDGWDrNr#~',JPCU9P6D9nMq9@*J,[PhD-rMNnD&fb@#@&dd@#@&dimW	U +X+1EO+cE!w[lDnPAx\n	EZsCk/~/O,n.\&f'rPL~nM+-qGP'~r~1aY&f'Z~AtDP/VCdkq9'r~[,ZsCk/q9b@#@&7dknDP.k'1Wx	Ra+1EO+vJdn^+mD~e,sDKh~2	Hx;ZsCk/~h4nDP.GKYk[xr[.WKObN'rPKDND,8X,r.ND(9r#@#@&7db'!@#@&7d9W,h4ksn,xGY,./c+GW@#@&d77b'r_8@#@&d7ik6PDk`r2lM+UYbNEb{!PD4+	@#@&i77dhlM+UYKCDtxYMd`rnC.xYKCDtE#,',JSrPLPYM/vEZ^ld/&fEb@#@&di7d1Wx	 n6m!Yn`E;aNCY~2	HnU!ZVCdkPd+D~9+2Dt{N+aY4QJLY./vJ[naYtrb[r_FB6.ND&fxJ'KM+-rM[+Mq9'r_J'rLJSDKGDk[{JLYDk`r.WKYrNr#'EBnlMnxDnlD4xBr[hl.+UOhlOtLEBBwC.xYr[{J'DhCM+UDqG[J,h4nDP/Vm/d(G'JL./vJZ^Cd/&fr#b@#@&7idnVkn@#@&d77inl.n	YKlD4{Y.k`rnlM+	OnmY4J*P'~r~J,'PDD/vE/Vm/kq9Jb~LPE~r~[,Dn2^lmncM/cJhCM+UDnmYtr#BE!BJSJr#@#@&iddi^W	xRanm!Y`EE2[mYnPAUHx;/^l/d~k+OP9naY4{NwY4_r'YM/cJ9+2O4J#LE_8~rM[nD&f{J'n.n7r.N.qG[EQr[k'EBDGWDr9'ELYM/`rDKGYbNE#LJSKmD+	OnmYt{vE[hlM+UYKCDt'JE~h4+.n,ZVCdkq9'r'M/crZ^l/kqGE#*@#@&didnU9Pk6@#@&iddMd :K\xn6O@#@&d7VKGw@#@&77M/R^sK/n@#@&7i/nDPM/'	WD4k	o@#@&idO.kRm^G/@#@&i7d+DPDDd'UGDtrxT@#@&idv更新所指向的上级栏目栏目数7i@#@&771WUxcnX+^!Y`J!w9CYP3xt+U;;VlkdPk+Y,^4k^N{m4ks[3F~h4nDP/sm//(9{J'DhCM+UDqG#@#@&@#@&7+	N~k6@#@&~,+x9~k6@#@&i@#@&P,mmVsP/sK/nZKUxv#@#@&,PIndaWU/ "+[bDmY,Jz[:bxmZ^ldd|H+	;2	Rlk2EP,@#@&+UN~d!4@#@&@#@&/!4~iarD[nM`b@#@&79kh,Z^l/kqGS/$V6D9+.SM/rM[+M~HK-n1!:Bm]WGO&fSY"GWDq9Sb~DdShDn\&9B1nXY&f@#@&d;slk/(f{Y.rs`D5E/YvE/Vm/kq9Jbb@#@&7m"GWDq9x:DkhcM+5EdD`E1IKWY&frb#@#@&7HK\nH!:'D.ks`D5;+kYvJ\W-ngEhJ*b@#@&drW,ZVCdkq9'rE,Y4x@#@&disK;x92.D{K.;@#@&i72MDHkLx2MDt/LP'~r@!8D@*@!Vb@*参数不足！@!&^k@*E@#@&dnVkn@#@&7iZ^l/kqGxZdxL`;VCdkqf*@#@&i+x9~r0@#@&ikWP^]KWOqGxJrPO4x@#@&7isGE	[AD.{YME+@#@&i72MD\/T'3.MH/T~[,J@!(.@*@!^k@*错误参数！@!&Vr@*r@#@&ds/@#@&7imIGGDq9';r	Yc1IKWY&f*@#@&i+UN,kW@#@&dk6~HK\+g;h'rJ,Y4+U@#@&d7sK;x92..{YD;n@#@&7dA.MHdT'ADDt/T~[,J@!4M@*@!sb@*错误参数！@!Jsk@*J@#@&7nVk+@#@&7d\G7+HEsxZbxOctW\nH!:b@#@&7ikW,HK\+gEsx!,Y4+	@#@&7idsK;x92DMxPD!+@#@&7d73MD\/Tx2MD\dTP[~E@!4.@*@!sb@*请选择要提升的数字！@!JVb@*J@#@&i7+	N~k6@#@&7xN,r0@#@&dbW~sKE	N3D.x:D;+,Otx@#@&id+arDPdE(@#@&dn	N,k0@#@&@#@&dE得到本栏目的n.+7q9Sg+6D(f@#@&dknOPM/{mGxU 6nm!O+vJdn^+mO~hDn\&9B1nXY&fP6DKhPAx\+	E/sm//,AtD+,/slk/&fxJ~',ZslkdqG#@#@&inDn-&fxDkcZ#@#@&dg+6DqGxDk`q#@#@&7.kRm^G/@#@&idnY,Dk'UWO4bxL@#@&7B先修改上一栏目的g+aO&f和下一栏目的n.n7q9@#@&7b0~hD\qG@*Z~Y4+U@#@&d7^Kxxcn6mEDn~J!w9lO+~3	Hnx!/Vm/d~k+Y~H6OqGxrP',16Y&f,'PrPAtDn~;VlkdqG'J,'~nM+7q9@#@&7x[PbW@#@&drW,1+aO&f@*!,O4+U@#@&idmKx	 +X+^ED+~E!wNmO+,2xtnUE;Vm/dPdnDPKD-qG'E~LPn.n7q9PL~rPA4+M+P;Vmd/&fxJ,[~H6Y&9@#@&d+	[~k6@#@&@#@&d[rsPhDkSHm6]GKYq9@#@&dd+D~sDd{mKxxc+Xnm!Yn`r/nsmY,hlX`DKGOk9#,s.Wh~Ax\+	;Z^lddr#@#@&7tlaIKGDq9{:M/`Z#3q@#@&dv先将当前栏目移至最后，包括子栏目@#@&d^G	xRa+1EYcEEaNmYnP3Ut+UE;slk/~dYP]GKY(f{E,[~tlXIWKY&9PLPEPStn.PIKGY&f'r~'P1IKWOq9b@#@&7@#@&7B然后将位于当前栏目以上的栏目的"WGO&f依次加一，范围为要提升的数字@#@&7k;srM[Dxr/V+1Y,MPwDG:,2U\xE;slk/PS4nDPhl.+UO&fx!,Cx9P]GKYq9@!rP'P1]KWO&f,[PrPK.ND~4HP]GKYqG~N/mr@#@&dk+DP./6.9+.'knD7+. ;D+COr8L^D`EmNKN4cD^WMNd+DJb@#@&dDk6D9+DcG2+	Pk;sr.[DSmKUxBFSf@#@&drW,DdrM[D (W6Pl	N,./}D[+MRnG6PY4nx@#@&dinakDPkE8P~~,P~P,v如果当前栏目已经在最上面，则无需移动@#@&dnU9PkW@#@&dr'8@#@&d[KPStk^+,UWDP./}D[nMR+KW@#@&ddD]GWDqG'./6.9+.`r]WKY(9r#P~~,P~PE得到要提升位置的]KWO&f，包括子栏目@#@&dimKUxc+a+1EOnvJEa[lD+PAU\+	E;VC/d~k+OP"GWDq9x"WWO(G_qPS4Dn,IKWY&f{EPLPOIKWO(G#@#@&7db'k3q@#@&idb0~k@*\K\n1!hPDtnU@#@&d77M/6D9nM`EhD\qGJ*xZ^ld/&f@#@&iddMdrMN+M ;w9lD+@#@&77imGx	 +X+^;D+`E;aNCY~Ax\x!ZVm/k~/Y~16O(G'J,'PM/rM[nDvJ;VC/d(GJbPL~J,h4nM+P/sm/dqGxrP',Z^l/kqGb@#@&d7d6rO,NW@#@&di+x9~r0@#@&id./6.9+.RsG\xnaD@#@&7sKW2@#@&7M/6MNDRsW7nx6O@#@&drW,D/}.NDRGWPDtx@#@&771WUxcn6m;O`J;29lO+,3	Hn	E;Vlk/,d+DPKD\(9{!PS4+M+P;sC/kqG'EP'~;VC/k(f*@#@&7V/n@#@&d7Dk6MNnM`r1+XY&9J*'/Vm/d(G@#@&i7DkrD9n.R!w9lO+@#@&id^W	UR6n^!Y+cE!w[lDn,2Ut+	EZ^lkdPk+OPhDn-&f'r~[,D/}.[+M`rZsldd&fE#,'PrPA4D+~/^ld/&9{J~LP;Vlk/&9#@#@&7+	N~r6d@#@&7DkrD9n.R1VK/n@#@&7k+OPMdrMNn.{xWO4bxL@#@&7@#@&7E然后再将当前栏目从最后移到相应位置，包括子栏目@#@&dmKx	 +X+^ED+cE!wNmO+,2xtnUE;Vm/dPdnDP]WKOqG'E~LPY]GKY(f,',J~StD+,IKGY&fxJ,[~\m6IKGY&f#@#@&7mmV^P/VGdZGx	c#@#@&7./wGUk+ I[bDn1Y,Jb9:bU{;VC/k{\n	E2	 lkwgz^OkKx{r.Nn.r@#@&+	[PkE8@#@&WX LmxUkvb@#@&d!4,fWSx}.NDc#@#@&7[b:P;slk/qGSd;^rMNnDS.kr.N.~tW-ngE:S^"WGY&9BY]KWDqfBkB./Bn.+7q9Sg+6D(f@#@&d;sC/kqG'ODrhvDn;!n/D`E/^l/d(GJb#@#@&im]KWDqf{KMr:vDn;!+dOvJm"GWDqfrbb@#@&dtW-+H;s'ODbh`M+5;/YcEtW-+g;sJb*@#@&d@#@&dbWP;VC/kq9xrJPD4+	@#@&i7oW!x92.DxPMEn@#@&7dAD.\ko'3.MHdo,',J@!(D@*@!Vb@*参数不足！@!&Vb@*E@#@&dnsk+@#@&7d;Vlkd(f{ZdxL`/sm/dqGb@#@&dnU9PkW@#@&dr0,^"WGDqG'JrPD4+	@#@&disG;	N2M.'DDE@#@&di2MD\/LxAD.HkLPLPE@!(D@*@!sb@*错误参数！@!z^r@*J@#@&dV/@#@&7d1IGWDq9x;kxDcm"WWD(9#@#@&i+UN~r6@#@&dbWPtW-ngE:xErPOtU@#@&7isKEx92M.'DD;+@#@&77ADDtdo{2DM\do,[,J@!4.@*@!Vr@*错误参数！@!&Vb@*E@#@&d+sd@#@&di\K\ngEs'ZbxDcHK\n1!:b@#@&ddbWPtW\H;:{!,Y4+U@#@&d7dwGE	N3.M'K.;@#@&di7AD.t/T'2MDtdo,[~J@!4.@*@!Vk@*请选择要提升的数字！@!z^k@*r@#@&di+	N~kW@#@&dnx9~k6@#@&7b0PoG!x[2M.{K.!+,Ytx@#@&di+akDPd;(@#@&inx9Pk6@#@&@#@&dE得到本栏目的n.+-(G~H+XOqG@#@&7k+Y~.k'^W	Uc+am!Y+vJknVmOPhDn-&f~gn6Dqf,W.WsPAx\+U;;VC/k~h4+.n,ZVCdkq9'r~LP/^lk/qG#@#@&dhDn\&fx.k`!*@#@&i1+XO(f{Dk`q#@#@&iDdR1sWk+@#@&i/+O~M/xxKO4kUT@#@&dB先修改上一栏目的g+XOqG和下一栏目的n.+7q9@#@&dk6~nM+\&9@*!,Y4+U@#@&7imGx	 +X+^;D+PE;aNCY~Ax\x!ZVm/k~/Y~16O(G'J,'Pg+6D(9PLPrPAtn.P/Vmd/&fxE,[PK.\(f@#@&i+U9Pb0@#@&dbWPg+aY&f@*T,YtU@#@&dd1GUxc+X+^EOn,J;w9CYP3Ut+x;/^ld/,dY~hD\qG'r~[,n.+7q9~LPJ,AtD+,/slk/&fxJ~',1n6D(f@#@&7n	NPrW@#@&@#@&i[b:~sDk~Hm6"GWDq9@#@&ddnDP:Md'1Wx	 n6m!Yn`EdVnmD~:m6c.KWYr[*PoDKh,2Ut+	EZ^lkdJ*@#@&dtla]KWY&9'sD/vTb_8@#@&dv先将当前栏目移至最后，包括子栏目@#@&71WUxcn6m;O`J;29lO+,3	Hn	E;Vlk/,d+DP]WKY(9{JPL~Hm6IKGOqGPLPEPA4DnP"GWDq9xrP[~^"WGY&9*@#@&i@#@&dB然后将位于当前栏目以下的栏目的"WKOqG依次减一，范围为要下降的数字@#@&dk;s6MN+MxJk+V^OPCPwDG:~3	Hnx!/Vm/d~St+.n,nCDUDq9{!,lx9P"GWDq9@*rP'~1IWKOqGP[,E~WMND~4z~"WGY&9J@#@&7dYP.d}D[+Mxk+.7+MRZM+mO+}4%+1YcEmNW98RM+mK.[/Yr#@#@&7.kr.N.RKwnU,/;s6MNnDB^KxUBFB&@#@&dbWPM/6D9+. (W0,Cx9PDk6.NDc+G0~O4+U@#@&7d6rO,/E8~,P~P,~,B如果当前栏目已经在最下面，则无需移动@#@&dxN,k6@#@&ikxF@#@&7[KPh4rVPxKO~DkrMNnD nK0@#@&i7Y"WGO&f'.d}D[+McrIGKY&fJ*P,~P,P~B得到要提升位置的"WGO&f，包括子栏目@#@&7d1Wx	 n6m!Yn`E;aNCY~2	HnU!ZVCdkPd+D~"WGDqG'IKWD(f F~h4+.n,IWKOqG'J,'~Y"WKY(fb@#@&d7k{r_8@#@&7ik0~r@*HG\H!:~Dtx@#@&di7Dkr.NDcEg+6D(fr#';sC/kqG@#@&d77M/6D9nDcE2[mY+@#@&id7mKU	RnX+1EY`r;w9lO+,2U\xE;slk/PknOPhD\(fxE,[~Dk6D9+.crZVCdkq9J*~LPE,h4+DP;slk/(f{J~',ZVmd/&f#@#@&7di+XkOP[G@#@&7dUN,kW@#@&dd.d}D[+M sW-x6Y@#@&isWKw@#@&iDd6MN+M :K\+	naY@#@&ikWP.d}D[+M +K0~O4+x@#@&id^W	Uc+am!Y+vJ!2NmYnPAx\n	EZ^C/kP/O~16Dq9'T~StnD~Z^ldd&f'E~LP/Vmdkq9*@#@&d+^/@#@&id./}D[nM`Jh.+7qfrbxZ^lk/(f@#@&id./}.ND ;aNlOn@#@&7d1G	x 6mED+vEEaNCYP3Ut+x!/Vm//,dnY,16Oq9xrP'PMdrMNn.vJZsCk/(frb,[~rPSt+M+,/Vm/dqG'E~LPZ^C/kqf*@#@&dx9Pr07@#@&d./}.ND ^^W/n@#@&dd+D~M/6MND'	WD4k	o@#@&i@#@&7E然后再将当前栏目从最后移到相应位置，包括子栏目@#@&i^W	xRanm!Y`EE2[mYnPAUHx;/^l/d~k+OP"GKY(G'rP[,Y"GWDq9PLPE~St+MnP"WWD(9'rPLP\la]KWOqGb@#@&d^C^VP/sK/nZKU	`b@#@&iD+kwKU/R]+9k.n1YPr)Nskx|/slk/|Hnx;3	RC/a_b1YrG	'r.[DE@#@&n	N~kE(@#@&@#@&k;4,j2rMNn.g`#@#@&d9k:,d5V}D9+.~.d}D[+MSHK\nH!:~/sm/dqGSb@#@&iNb:PhlMnxDq9~}D[nMqfBKlM+xDKCY4~;trV[ShDn\&9~g+aO&f@#@&7;VC/k(G'PMks`D;!n/D`EZ^ldd&fJ*b@#@&dHK-n1!:{Y.khcM+5EdYvJ\G7+1;hr#b@#@&7b0~;Vm//&f{EJ,Y4+	@#@&7isW!UNADD{O.E@#@&d72..t/L'A.Dt/L~LPJ@!8M@*@!Vb@*错误参数！@!zsb@*r@#@&i+^d+@#@&7d;VCdkqf{/S	o`;sC/kqG#@#@&7n	N~k6@#@&ikW~tW\nH!:xJr~Dtn	@#@&ddwW!UNAD.'DD;n@#@&di3DMH/Tx3DMHko~[~E@!4.@*@!sk@*错误参数！@!&sb@*J@#@&i+s/@#@&d7tW7+1!:{/k	YcHK\nH!:#@#@&dik0,\G\1!:x!~O4+U@#@&7disG;	N2..{K.E@#@&d7i2MDHko{3DMHdo,[~E@!4D@*@!Vb@*请选择要提升的数字！@!Jsr@*r@#@&d7+U[,kW@#@&7+	N~r6@#@&7r6PoW!U92.M':DEPD4+	@#@&di+arDP/!8@#@&d+	[~k6@#@&@#@&d[rsPd;^SDk~Gs9WD[nM/SkbSDDdBY}DND&9@#@&dv要移动的栏目信息@#@&ddnDPDkxmKxxcna+1ED+cJdn^+^Y,KlM+UO&f~6.9+.qGShl.xDnlDtB^tbV[~hDn-&f~gn6Dqf,o.WsPAx\+U;;VC/k~h4+.n,ZVCdkq9'r';VCk/&f#@#@&iKlM+UY&fx.k`!*@#@&irD9n.qG'M/cFb@#@&dKlMnxDnCO4'Ddcy#~[,EBJ~LP;Vlk/&9@#@&d^tbV[xM/`2b@#@&dnMn-qG'M/ccb@#@&dH+XOqG'.dv*#@#@&iDdR1sK/n@#@&i/+DPMd'	WOtbxL@#@&dk6~m4kV9@*TPDtx@#@&77k+OPMd'1WUUc+6n^!Yn`rdVn1Y,mW!xDce*PoDK:~3	H+	;Z^l/k~AtDPKl.n	YKlD4P^kVn,B]E'hl.+	OhlO4[r]Br#@#@&diWsNKD[nM/'Md`Z#@#@&77DkR1VG/n@#@&d7/OPM/xUKYtrUT@#@&dsk+@#@&diWV9WM[+M/x!@#@&7n	NPbW@#@&dB先修改上一栏目的gnaY&f和下一栏目的hDn\(9@#@&7k6~nM+-(G@*!~O4+U@#@&7imG	xc+6m!O+,J;w9lOn,2xtnx!ZVmddPk+DPH+aO&fxJ,'Pg+aO&fP'~rPAt.P/^lk/qG'r~[,n.+7q9@#@&d+	[Pb0@#@&7r0,16Oq9@*ZPOtU@#@&d7^Kxx nX+^EDn,J;aNmY+,2	\+	E/Vm/d~k+Y,KD\qGxEPLPhDn\(9,[~J,AtDn~;Vldd&fxJ,',1nXY&f@#@&dUN,kW@#@&d@#@&iB和该栏目同级且排序在其之上的栏目O RO O更新其排序，范围为要提升的数字@#@&7d;^'r/nVn^DP/Vmd/&fS6MN+.(G~^tbs9~KmDxYhlD4~hDn\&fSH6Y&9PwDWs~3xt+	E/VCdkPAt.+,nC.xY(9{J'nm.xO&fLJPmx9~rMNnD&f@!ELrD9nD&f[r~GD9+MP8X~6MNnD&9P9+d^r@#@&7dY~Dkxk+.7+MRZM+mO+}4%+1YcEmNW98RM+mK.[/Yr#@#@&7.kRGwUPk;sS1WxUS8~f@#@&7b'q@#@&iNW,h4rVPUWDP.dc+W6@#@&idY}.[+MqG'./cq*@#@&di^W	x nX+m;O`EEa[mYn,2	H+	E;slk/~/Y~6MN+M(f{J[D6.ND&fQWs[KD[+Md_b[E~St+.n,Zslkd&fxr[M/`Z#*@#@&idr0,Ddcy#@*Z~Y4+x@#@&7dikb'r_q@#@&d7dknY,Y.d{mWUUc+a+1;D+cr/V+1Y,/Vm/dqG~6.9+D&9PwDWs~3xt+	E/VCdkPAt.+,nC.xYKCDt~VbVPvuJLD/v&*'JBJ'Dk`TbLJ]E~WMN+M~8X,rMNnD(9r#@#@&i7db0~UKYPcOM/ +KW,lU9PDD/c4KW#,Y4+	@#@&7idd9GPStk^n~xKY,Y./ nK0@#@&i7did^G	xRnam;YcrE29lD+PAxtnx!Zslk/~dYP}.NDqGxE[DrMNnD(93WsNK.NDdQbk[E~StnD~;VCk/&f'r[D./v!b#@#@&77iddbr'bk_8@#@&dididODd sW-+	n6D@#@&7iddsGKw@#@&i7i+U9Pb0@#@&di7YM/ m^Wdn@#@&di7/YPD.d'	WDtrxL@#@&d7+	[Pb0@#@&idkxr3F@#@&i7b0~b@*tW\1!hPDtnx@#@&77iD/v*#{ZVmddqG@#@&d7d.dcE2NmO+@#@&77imWUUc+a+1;D+crEaNlD+,3xt+UE;VCdkP/OPg+6D(9'rPLP./cT*P'Pr~h4+.n,ZVCdkq9'r~LP/^lk/qG#i7@#@&d7d6rO,NW@#@&di+x9~r0@#@&id./ hK\nxaY@#@&7sKWw@#@&iDdRsG7+U6D@#@&ik6~DkRnW6PO4x@#@&7d1Wx	 n6m!Yn`E;aNCY~2	HnU!ZVCdkPd+D~hDn7qG'!,h4nDP/Vm/d(G'J,'P;Vlkd(f*@#@&dnVdn@#@&7dMd`l#x/^l/d(G@#@&di.kR;aNmY+@#@&i7mKxUR6n^!Y+vEEaNlDn~2	Hx;ZsCk/~/OPhDn-&f'E~LP./vT*P',J,htD~Z^ld/&fxE,[P;slk/qGb@#@&i+	N~kW7@#@&7Dk m^Wdn@#@&ddnDP./{UKY4bxT@#@&i@#@&7B更新所要排序的栏目的序号@#@&7mKxU 6+1;Y`J!2[lD+,2UHnU!ZslkdPk+O~}DNn.&fxJLO}D[D&f[rPS4+M+~Z^ldd&f'r'Z^l/k(9#@#@&iB如果有下属栏目，则更新其下属栏目排序@#@&7r6P^tbsN@*!~O4+x@#@&idr'8@#@&d7k+DPDk'1Gx	Rn6m;O`JknVmY,/slk/&f~s.GsP3xtnx!ZsCk/PA4DnPhCM+UDnmYt,VbV+,BYJLnC.xYhCY4[Juv~WMND~4z~}D[+M(fr#@#@&idNG~StrV~	WO,DkR+K0@#@&did^W	x nX+m!O+vJEa[CYPAx\+U;;VC/k~/Y~6MN+.(G'E[D6MNnMqG_kLJ,AtDnP;VCdkqf{E[M/`Zbb@#@&didr'rQ8@#@&di7DkRhG7+xnaD@#@&disKW2@#@&idDkR1sWk+@#@&iddnDPDkxxKYtbUL@#@&dx[PrW@#@&7mmsV,ZsGk+ZGU	`b@#@&7M+daW	/+cI[kM+^Y,J)[skx|/Vm//|\nx!2	RC/2_zmOkKU'}D[nM1J@#@&x[Pk;(@#@&@#@&kE4,fKAx}D[+M1cb@#@&d9r:,/;^6.NDBDdr.[DSHK-+gEhS;Vldd&fSk@#@&iNrsPhlDxD(fBr.ND(9BnlMnxDnlD4SZ4k^NSn.n7q9~gn6Dq9@#@&dZsCk/(f{PMkhvD;E/DcJ;VC/kq9E*#@#@&7HK\+g;h'DDb:cDn5!+dYvEHK\nH!:Jbb@#@&7k6~;VCk/&f'rJ,Otx@#@&idoG!xNA.D{YD!n@#@&idAD.HdL{2.Dtdo,[~E@!4D@*@!^k@*错误参数！@!Jsb@*E@#@&id+XkD~/!4@#@&i+sd@#@&i7Z^l/k(9';k	YcZsCk/(f*@#@&i+U[,k0@#@&ikWPtG7+H!:{JJ,Y4nx@#@&7dwW;U92DMxYME+@#@&7dADMHdox3MD\/T~[,J@!8M@*@!sr@*错误参数！@!&Vb@*r@#@&id6kDPk;4@#@&7+^/n@#@&ddtG\1Esx/k	YvHG\nH!:b@#@&7db0~\K\+H;s'TPD4x@#@&didsKE	[2MDxKMEn@#@&ddi3DMH/Tx3DMHko~[~E@!4.@*@!sk@*请选择要下降的数字！@!&sb@*J@#@&id7+XrDPd!4@#@&di+	[Pb0@#@&i+U[,k0@#@&@#@&dNbh~/$VBDd~Gs9W.N./BkrSDD/SO}D[+M(G@#@&iB要移动的栏目信息@#@&dk+D~Dk'^W	x nX+m!O+vJ/snmDPhl.+UO&fSrM[+Mq9ShlDnUDnCY4S1tr^NBnD\&9~g+aY&f~oMW:,3xt+x!/slk/,h4+.n,ZslkdqG'E';Vldd&fb@#@&7hl.xDqf{Dkc!*@#@&d}D[nMqf{./vF#@#@&7nmDxOnCO4'./v+#,[~EBJP'~;VC/k(G@#@&im4kV9'Md`2#@#@&in.n7qf{./vc#@#@&716Dq9'.dv*b@#@&7DkR^sK/+@#@&i/nY,.k'UKY4kxT@#@&@#@&iB先修改上一栏目的H+XY(9和下一栏目的hD+7(f@#@&dbW~nM+7q9@*T~Dtnx@#@&dimGU	R+an1EO+,E!w[mYP2	HUE;VC/kPdnDP1aY&f'r~'Pg+XY(f~',J~h4nDP/sm//(9{J~[,KM+-&f@#@&dx9~k6@#@&db0~H6Y&9@*ZPY4nU@#@&dimGxU 6nm!O+,J;29lYn~Ax\+	;;VCk/,/+DPh.+7q9'rP'~hD+7(f,[Pr~AtDP/VCdkq9'r~[,1naDqf@#@&i+UN,r6@#@&i@#@&dB和该栏目同级且排序在其之下的栏目 O RO 更新其排序，范围为要下降的数字@#@&dk;sxr/+^nmDPZ^Cd/&fBr.Nn.&fSm4rV9~KCM+xOKmY4~h.\(G~g+6DqG~sMWhPAx\n	EZ^C/kPh4n.+,nmDnxO(G'E[hCDxO(G[J~C	N~rM[D(G@*r[rMN.qG[EPKD[nMP4H~rMN+M(9J@#@&i/nY~.k'd+M-+MR/.lYn6(LnmDcrl[KN(RDmK.Nk+OJ*@#@&7M/RK2+	P/$sSmKx	~q~f@#@&dr'Z~P,P~~E同级栏目@#@&7rb'TP,~,Pv同级栏目和子栏目@#@&iNW,h4rVPUWDP.dc+W6@#@&idmKUUR6m;YncrE2NmO+,2U\xE/sm/dPknDP6MNDqG'r'rMNnD&fQrb[J,AtD+,/slk/&fxJ'.k`T#*@#@&idrW,D/c+*@*TPD4x@#@&did/Y,ODk'^W	x nX+m!O+vJ/snmDP;VC/d(G~6D9nD&f~oMW:~3	Hnx!/^ldkPSt+M+,KlM+UYhlO4,Vk0nPE]JL.d`2#LJSJ'.k`T#LE]EPG.9+D~8HP6D9nMq9r#@#@&didbWP	WOPvY.dc+W6~l	NPD.dR(W6#~Y4n	@#@&di7d9W~A4kVn~	WOPD.kRnK0@#@&didi7kb'rk3F@#@&iddi7mKxxcna+1ED+cJ;29lO+,3xt+U;;Vldd,/nY,6MNnMqG'JLrM[+Mq9_bk'E,ht.+,ZVmddqG'r[ODdcZ#b@#@&7did7OM/RhG7+U+XO@#@&7idiVWKw@#@&didnx9PrW@#@&di7YM/R1sG/@#@&d7ddnDPODkxxKY4r	o@#@&7i+UN,r6@#@&idbk'bk3q@#@&d7k{kQq@#@&dir0,k@*{\G\1!:~Y4n	@#@&di7Dk`Xb{ZVCdkq9@#@&7id.kR!wNmY@#@&id7mKxU 6+1;Y`J!2[lD+,2UHnU!ZslkdPk+O~hD+-(G'EPL~M/cZ#,[PrPS4+M+~Z^ldd&f'r~[,ZVmddqG#id@#@&77i+akD~NK@#@&7i+x[~b0@#@&i7M/ sW7+x6D@#@&iVGWa@#@&7M/RsG\x+XO@#@&ik6P./ nK0~Y4nx@#@&771WxU 6nm!O`E!w9lYPAUHx;Z^ldd,/+D~16Y&9x!,h4+.+~/^ld/&9'rP'~;Vldd&fb@#@&7Vd@#@&ddM/v*#{Zslk/(9@#@&di./cEw9CO+@#@&id^WUUc+a+1;Y`E;aNlOn,2UHU!Zsm/kP/Y,H+XY(f{J~',D/vT#,[Pr~AtDP/VCdkq9'r~[,ZsCk/q9b@#@&7+	[,kWi@#@&dDkR1sWk+@#@&i/nO,D/{UWDtk	L@#@&i@#@&dv更新所要排序的栏目的序号@#@&71WUxcn6m;O`J;29lO+,3	Hn	E;Vlk/,d+DP6D9+.(G'JL6D9+D&9Qkb[rPAtn.P/Vmd/&fxELZVCdkq9#@#@&iB如果有下属栏目，则更新其下属栏目排序@#@&db0P1tbsN@*!~Y4+U@#@&ddbxF@#@&didnY,Dk'^WUUc+a+1;Y`EdV+^O,Zslkd&f~wDK:PAxtnx!Zslk/~A4+D~nmD+	OKlDt,Vr3n~E]E[hCDxOKmYt'EuB~WM[D~(X,rD9+M(fr#@#@&id[G,htbs+,xWD~./c+K0@#@&77imGx	 +X+^;D+`E;aNCY~Ax\x!ZVm/k~/Y~rMNn.&f'r'rMN+M(9_bk3k'J~A4+.+,/Vm/d(G'J'.k`T#*@#@&d7ik{k_8@#@&7diDdRsW-n	+6D@#@&idVKG2@#@&diDdR^sK/n@#@&7dk+O~M/'UGDtrxT@#@&dn	N,k0@#@&i^l^V~Z^Wdn;Wx	c#@#@&dMndwKxk+ In[bDnmD~JzNhr	{ZsCk/mHU!2UclkwgzmDrW	'6D9+.Hr@#@&UN,/E(@#@&@#@&/!4~?C-In/O`*@#@&79k:~rB/5VB.k~j!m1+/kHkL~bZGE	YSKM+\&9~g+6D(9@#@&dk;s'EdVnmD~Z^ldd&fPo.K:~2	\x;;Vm//,WM[+MP8X,IGGDqfB6D9+D&9E@#@&dk+OP.d{/nD7nDcZ.nmY+68N+^YvEmNG94cD+1WM[/YE#@#@&7.kRWanx,/;^S^W	xBFSF@#@&ik/W!UY{Dd M+mG.9mGE	O@#@&7b'8@#@&inMn\&fx!@#@&7[KPh4rVPxKO~DkRWW@#@&7iDdRsG\xnaD@#@&77b0~Dk WW,Y4+x@#@&i7dg+aY&fxT@#@&dinVk+@#@&77dg+XY(fx.k`T#@#@&di+U[,k0@#@&id./chK\naD\kKEk@#@&id^W	x nX+m!O+vJEa[CYPAx\+U;;VC/k~/Y~]KWY(9{J~[,r,[~r~}DND&9'Z~KlM+UO&f'ZSZ4kV9xT~hlM+UYKCDtxBZv~G+2O4'!SKM+-qGxrP',nM+\&f,'Pr~H+XY(9{JPL~16Y&9~[,J,h4+.n,ZslkdqG'E~LPDdcZ#b@#@&7in.\&f'M/vT#@#@&7db'rQ8@#@&i7DkR:K-nx6D@#@&dsGKw@#@&i./cmsGk+@#@&7k+OPMd{xGDtbxoi@#@&7@#@&djE1mndkH/TxJ复位成功！请返回@!lP4.n0{BzNhkUm;VC/kmHx;3	Rld2E@*栏目管理首页@!zm@*做栏目的归属设置。r@#@&immVV,MrY?;m1+ddt/ovjE1m+kd\/T#@#@&nx[~kE8@#@&@#@&kE8~Ul\ni	kO+vb@#@&79ksPZ^lkdqG~PlMonO;VlkdqG~nm.nxDnmY4~rKmDnxDKlDtS9wY4SbnCDUDq9BZ4kV9~h.+7q9~g+aO&f@#@&7Nb:PMdSYM/BkS?;^1+d/tdo@#@&7/^l/d(G'ODbhvDn$E/YvJ;slk/(fr#b@#@&dKm.oYZ^Cd/&f{Y.khcM+5EdYvJPCMo+O/^ld/&9r#b@#@&ik0,Z^C/kq9'rJ~O4+x@#@&disW!U[2MD{K.En@#@&d72M.Hkox3MDHdL,[~J@!8M@*@!^k@*请指定要合并的栏目！@!z^k@*E@#@&dnVk+@#@&idZ^C/kqf{/JxT`;VC/d(G#@#@&inx9PrW@#@&drW,KCDTnDZsm/kqf{Jr~Y4+U@#@&d7oKEx93DM'KM;n@#@&di2.D\dT'3DM\/TP'~r@!4.@*@!Vr@*请指定目标栏目！@!&^k@*r@#@&d+^/@#@&idPlMonO;VlkdqG'ZdUL`:lMonY/sm/dqGb@#@&dnU9PkW@#@&dr0,/^ldkqG'KmDTnY;VC/kq9~Dt+	@#@&idsK;UNADM'PD;n@#@&7dA.Dt/LxADD\dTP'Pr@!(D@*@!Vb@*请不要在相同栏目内进行操作@!JVb@*J@#@&7+	N~r6@#@&ir0,sW!U[2MD{K.En~Dtnx@#@&di+arDP/;8@#@&7+	[,kW@#@&iB判断目标栏目是否有子栏目，如果有，则报错。@#@&dknY,Dd'1WUUc+6^ED+`rdnVmDP/trs9PWDKhPAx\n	EZsCk/~h4nM+~;Vm//&f{EPLPPlMonO;VlkdqG#@#@&7r0,DkR8WW~mx[PMdRWW~Dt+U@#@&d7sK;	N3MD{KD!+@#@&di2.Dt/LxADDtdo,[Pr@!8D@*@!^k@*目标栏目不存在，可能已经被删除！@!&sb@*E@#@&7+^/n@#@&ddrW,Dd`Zb@*!~Dtx@#@&di7sKEUNAD.x:DE@#@&iddA..Hko{2.D\dTP'Pr@!4M@*@!sb@*目标栏目中含有子栏目，不能合并！@!&sb@*E@#@&7i+U9Pb0@#@&dUN,kW@#@&drW,sW!UNADD{P.EPDtnx@#@&idn6bOPkE8@#@&d+U[,kW@#@&@#@&dv得到当前栏目信息@#@&i/+DPMd'1WUxc+an1EYcJk+V^OP;Vm/dq9Shl.+	OqG~KCM+xOKmY4~h.\(G~g+6DqGSfwOt,0.GsP2	\+	EZ^Cd/,h4+.+~/^ld/&9'r[/sm//(9*@#@&dbKmDn	Y&f'M/vq#@#@&7fwO4{D/vX#@#@&dbW~khlM+UY(9{!~Y4nx@#@&77hlDnUDnCY4xM/cZ#@#@&dVkn@#@&d7nmDnUDnlD4'M/`yb~[,JBJ~[~.k`T#@#@&dx[~b0@#@&7bnCDUDnCDt{D/v!*@#@&in.+7q9xM/`2b@#@&d1aOqG'M/ccb@#@&d@#@&iv判断是否是合并到其下属栏目中@#@&ddnDPDdx1WUxcnX+^!Y`Jk+^nmDP/Vm/d(GP0MG:,2xtnUE;Vm/dPA4DnP;slk/(9{J[PCMonY;sm/d&fLJPmx9~nmDnxDnCO4PVbV+,BJLKCDxDnCY4'r]vJ*@#@&ikW~	WY~cM/ +KW,lU9PM/R(W6bPDtnx@#@&77wWE	[2MD'D.;+@#@&id3D.\kox2M.Hko~',J@!8.@*@!sk@*不能将一个栏目合并到其下属子栏目中@!JVr@*J@#@&di+XrY,/;4@#@&7n	NPbW@#@&d@#@&7v得到当前栏目的下属栏目qG@#@&dd+O~M/xmKUxc+an1EYncr/nV^DP/^lk/qGP6.WsP3xt+U;;VlkdPSt+Mn~nmDxOnCO4Psk0nPEJ'KmD+UOhlOtLEuBE*@#@&dk{!@#@&db0~xKY~cM/RG0,lx9~./c4K0bPO4x@#@&i7NKPA4bV+~UKY~Dk WW@#@&iddbnm.+	YKlDtxrhlDUYhlY4~'Pr~rP'P.dv!b@#@&7dikxr3F@#@&7id./chK\n	+XY@#@&disWKw@#@&i+U[,k0@#@&db0Pb@*TPDtx@#@&77hl.+	OnmY4xbnl.n	YKlD4@#@&7Vk+@#@&diKlM+UYhlO4{ZVmd/&f@#@&7nx9Pb0@#@&7@#@&dv先修改上一栏目的1aY&f和下一栏目的K.\q9@#@&dr0,KM+-&f@*!PDtU@#@&d7mKxU 6+1;YPJ!2[lD+,2UHnU!ZslkdPk+O~g+6O(G'EPL~g+aDqGP[,J,AtDnP;VCdkqf{EPLPnMn-qG@#@&dnx[~b0@#@&ir0,1naDqf@*T,Y4+	@#@&d71W	xR6^ED+~J!w[CD+PAUHxE;sC/kPk+OPK.\(f{EPLPK.\q9~LPEPS4Dn,Z^l/kqGxJ,[~16O(G@#@&inx9Pk67@#@&i@#@&dv删除被合并栏目及其下属栏目@#@&71WUxcn6m;O`J[n^+O+,WMWh,2	H+	E;slk/~h4+.n,ZVmd/&fPbU~`r[hl.+UOhlOtLE#r#@#@&i@#@&7v更新其原来所属栏目的子栏目数，排序相当于剪枝而不需考虑@#@&7k6~G+2Dt@*!PDtU@#@&d7mKxU 6+1;Y`J!2[lD+,2UHnU!ZslkdPk+O~;tks[{Z4k^[ F~StD+,Z^C/kq9'r[rKmD+	OqG#@#@&7nx9Pb0@#@&7@#@&djE1^+k/\dT'J栏目合并成功！已经将被合并栏目及其下属子栏目的所有数据转入目标栏目中。@!8M@*@!4M@*同时删除了被合并的栏目及其子栏目。r@#@&immVV,MrY?;m1+ddt/ovjE1m+kd\/T#@#@&7/nO,Dd'	GY4kUL@#@&ddnDPODkx	WO4k	o@#@&+	[PkE8@#@&@#@&izMjAA==^#~@%> 
