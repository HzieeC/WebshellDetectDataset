<%@ LANGUAGE = VBScript.Encode %>
<!--#include file="conn.asp"-->
<!--#include file="admin.asp"-->

<%#@~^LgAAAA==r6P];!+/DR5;+MXjYMkULvJ:m.3r#'rdGEDtbN^J~O4+UhhAAAA==^#~@%>
<%#@~^DwEAAA==@#@&C.gls+':Dbh`"+5E/OcrCDgC:J#*@#@&CMI;;k.ngEh':.ks`]n$E+dOvJuD"n$ErM+gE:r#*@#@&_D)N9Dndk'KMr:vI+$;n/D`rC.b[[M+d/rb#@#@&u.UlVC.H'PDbhvIn$E/YvJ_.?mVCDHJbb@#@&CM9lD+':.r:vI;;+dOvJuDGCYJbb@#@&C.#mVrNGCD+x:Db:`"+$;+kYcJ_D#C^kNGCYJ#*@#@&CMfYCksx"+5EdYvJ/G	Y+UOr#@#@&W1QAAA==^#~@%>
<%#@~^cQUAAA==@#@&qW,CM1ls+{EJ,K4+	@#@&./wKU/RhMrO+,JUr]Ie~@!4.@*r@#@&M+d2Kx/n SDrY~r招聘对象内容不能为空"@!mP4D+6'rELm\C/1Dr2D)tbdYKDXcLG` F*JE@*返回重输@!&m@*E@#@&.+kwGUk+RnU9@#@&+	[,kW@#@&&0P_D"n;!k.+gEhxrJP:4+	@#@&MndwKxk+ h.rD+~JU6I"5~@!(D@*E@#@&Dn/aG	/nchMkYPr招聘人数内容不能为空Z@!mP4D0xErLl7C/1DkaOltb/DW.X LK`RF*EJ@*返回重输@!&C@*J@#@&./2W	dRn	N@#@&+	N,r0@#@&(0,C.)9NDd/{JJ,P4+	@#@&Dn/2G	/nRS.kD+~EUrI]e,@!8D@*E@#@&./aWxk+cADbYnPr工作地点内容不能为空"@!C,tDW'rJLm-C/1DbwO)4rkYGDH oK`Rq*JJ@*返回重输@!Jl@*J@#@&M+daW	/+c+	[@#@&+UN,kW@#@&q0,uDUlVm.z'rJ,K4+U@#@&Dn/aGxk+ AMkYn~r?6I"e,@!8M@*r@#@&M+k2W	/nRSDrOPJ工资待遇内容不能为空e@!l,tDWxJrLm\C/^.bwO)4r/DW.zcoWcR8#EJ@*返回重输@!Jl@*r@#@&D+kwKU/Rnx9@#@&n	NPbW@#@&q0,u..mVbN9lOn{JEP:4+	@#@&./wGUk+ hMrD+~r?}IIIP@!8D@*J@#@&M+d2Kx/ hMkY~E有效期限内容不能为空"@!l,t.+WxrJ%l7C/1Dr2D)trdDW.XcLK`R8#rJ@*返回重输@!zm@*J@#@&.+kwGUk+RUN@#@&+	[~k6@#@&qWPu.G+Olbs'rJ~P4+x@#@&M+dwKUk+ SDbY+,JU6I"5~@!(D@*E@#@&DdwKx/ ADbYPE招聘要求内容不能为空"@!C,t.+6xJrLC-m/m.raYltbdDW.HRTW` F*EJ@*返回重输@!&l@*J@#@&M+/aGxk+RU[@#@&+	N~kW@#@&@#@&?OPM/~x,?+.-D ZMnmYn}4N+mD`r)f}f$R"+^GMN/OJ*@#@&k5s'r/VnmO~CPWDKhP_D9nslx[~r@#@&Dk Kwn	Pk;VBmKUxBFS&@#@&.dclN9U+S@#@&MdcJ_Dglh+Eb{C.1mh+@#@&.dvJC.];;kMngEhr#{CD"+$;kM+HEs@#@&.k`J_.b9NDddJ*'_D)N[./d@#@&./vJu.UlVC.HJb'_.UlsmDH@#@&M/vECMfCYJbx_DfmO+@#@&DkcECM.mVrN9CD+E#{uDjlsr9flOn@#@&./vE_D9YmkVr#{uDG+OlbV@#@&M/R!2NmY+@#@&./cm^Wd+@#@&M+dwKU/R.n9kDn^DPEb9hbxm_DG+:mx9 lkwE@#@&+U[,k0@#@&13gBAA==^#~@%>
<!-- #include file="Inc/Head.asp" -->
<table width="100%" height="100%" border="0" cellpadding="0" cellspacing="0">
  <tr> 
    <td align="center" valign="top"> <br> 
      <table width="650" border="0" cellpadding="2" cellspacing="1" class="table_southidc">
        <tr> 
          <td height="25" class="back_southidc"> 
            <div align="center"><strong>发布招聘信息 <br>
              </strong></div></td>
        </tr>
        <tr> 
          <form method="post" action="Admin_HrDemandAdd.asp?mark=southidc">
            <td><div align="center"> 
                <table width="100%" border="0" cellspacing="1" cellpadding="0">
                  <tr bgcolor="#ECF5FF"> 
                    <td width="19%" height="25"> 
                      <div align="center">招聘对象:</div></td>
                    <td width="81%"> 
                      <input name="HrName" type="text" id="HrName" size="30" maxlength="30"></td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="22"> 
                      <div align="center">招聘人数:</div></td>
                    <td> 
                      <input name="HrRequireNum" type="text" id="HrRequireNum" size="5" maxlength="30">
                      人</td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="22"> 
                      <div align="center">工作地点:</div></td>
                    <td height="-4"> 
                      <input name="HrAddress" type="text" id="HrAddress" size="50" maxlength="60"> 
                    </td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="22"> 
                      <div align="center">工资待遇:</div></td>
                    <td height="-1" bgcolor="#ECF5FF"> 
                      <input name="HrSalary" type="text" id="HrSalary" size="20" maxlength="50">
                    </td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="22"> 
                      <div align="center">发布日期:</div></td>
                    <td>                    <input name="HrDate" type="text" id="HrDate" value="<%=#@~^BgAAAA==[mYnv#7wEAAA==^#~@%>" size="12" maxlength="50"></td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="22"> 
                      <div align="center">有效期限:</div></td>
                    <td height="0"> 
                      <input name="HrValidDate" type="text" id="HrValidDate" size="5" maxlength="30">
                      天</td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="22"> 
                      <div align="center">招聘要求:</div></td>
                    <td height="10"> 
                       <input type="hidden" name="content" value=""> <iframe ID="eWebEditor1" src="Southidceditor/ewebeditor.asp?id=content&style=southidc" frameborder="0" scrolling="no" width="550" HEIGHT="400"></iframe> </td>
                  </tr>
                  <tr bgcolor="#ECF5FF"> 
                    <td height="25" colspan="2"> 
                      <div align="center"> 
                        <input type="submit" value="确定">
                        &nbsp; 
                        <input type="reset" value="取消">
                      </div></td>
                  </tr>
                </table>
              </div></td>
          </form>
        </tr>
      </table>
      <br> <br> </td>
  </tr>
</table>
<!-- #include file="Inc/Foot.asp" -->