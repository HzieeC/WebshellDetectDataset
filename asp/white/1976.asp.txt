<!--#include file="conn.asp"-->
<!--#include file="inc/const.asp"-->
<!--#include file="inc/dv_clsother.asp"-->
<!--#include file="inc/plus_check.asp"-->
<%
Dv_plus.name="myplus"'定义插件ID，并提取设置。
Dv_plus.checklogin'验证用户进入的权限。
Dvbbs.LoadTemplates("index")'提取模板数据，如果你添加了自己的模板可以这样调用:Dvbbs.LoadTemplates("名称")
Dvbbs.Stats="插件开发示范说明"
Dvbbs.Nav()
Dvbbs.Head_var 0,"","插件开发帮助","myplus.asp"
'由于本页面是示范页面，故不采用页面模板。样式全部在代码中输出。
%>
<table border="0" cellspacing="1" cellpadding="3" width="100%" class=tableborder2 align=center>
<tr><th height="22">示范代码</th></tr>
<tr>
  <td align=center width="98%" class=tablebody1> 
    <TEXTAREA id="edittxt" ROWS="20" COLS="120" style="width:98%;overflow-y:visible; border:0px; overflow:visible; border-width:1px;border-style: dotted;color: #000000;" >
&lt;!--#include file=&quot;conn.asp&quot;--&gt;
&lt;!--#include file=&quot;inc/const.asp&quot;--&gt;
&lt;!--#include file=&quot;inc/plus_check.asp&quot;--&gt;
&lt;%
Dv_plus.name=&quot;myplus&quot;
Dv_plus.checklogin()
Dvbbs.LoadTemplates(&quot;&quot;)
Dvbbs.Stats=&quot;插件开发示范代码&quot;
Dvbbs.Nav()
Dvbbs.Head_var 0,&quot;&quot;,&quot;插件开发示范&quot;,&quot;myplus.asp&quot;
Set Dv_plus=Nothing 
Dvbbs.Footer
%&gt;
</TEXTAREA></td></tr>
</table>
<br>
<table border="0" cellspacing="1" cellpadding="3" width="100%" class=tableborder2 align=center>
<tr><th height="22">变量说明</th></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(0) 是否定时间开放。缺省值为：<%=Dv_plus.Mian_settings(0)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(1) 定时开放起止时间。缺省值为：<%=Dv_plus.Mian_settings(1)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(2) 可使用插件的用户组列表，分隔符号为“@”。缺省值为：<%=Dv_plus.Mian_settings(2)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(3) 管理人员,分隔符号为“|”。缺省值为：<%=Dv_plus.Mian_settings(3)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(4) 能使用插件的最少文章。缺省值为：<%=Dv_plus.Mian_settings(4)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(5) 能使用插件的最低金钱。缺省值为：<%=Dv_plus.Mian_settings(5)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(6) 能使用插件的最低积分。缺省值为：<%=Dv_plus.Mian_settings(6)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(7) 能使用插件的最低魅力。缺省值为：<%=Dv_plus.Mian_settings(7)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(8) 能使用插件的最低威望。缺省值为：<%=Dv_plus.Mian_settings(8)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(9) 每次使用插件金钱变化。缺省值为：<%=Dv_plus.Mian_settings(9)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(10) 每次使用插件积分变化。缺省值为：<%=Dv_plus.Mian_settings(10)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(11) 每次使用插件魅力变化。缺省值为：<%=Dv_plus.Mian_settings(11)%></td></tr>
<tr><td height="22"><li>Dv_plus.Mian_settings(12) 每次使用插件威望变化。缺省值为：<%=Dv_plus.Mian_settings(12)%></td></tr>
<tr><td height="22"><li>Dv_plus.plus_Settings 为数组变量，这是扩展的设置变量，由插件作者自己定义。</td></tr>
<tr><td height="22"><li>Dv_plus.plus_Settingnames 为数组变量，这是扩展的设置变量的含义，由插件作者自己定义。</td></tr>
<tr><td height="22"><li>重要方法Dv_plus.name获得所有插件的设置。必须首先调用。</td></tr>
<tr><td height="22"><li>方法Dv_plus.checklogin验证用户使用插件的权限。</td></tr>
<tr><td height="22"><li>变量Dv_plus.plus_Copyright存储插件的版权信息</td></tr>
<tr><td height="22"><li> 变量Dv_plus.Plus_Name存储插件在菜单上显示的名称</td></tr>
<tr><td height="22"><li>变量Dv_plus.plus_master是布尔变量，为True的时候表示用户是插件的管理员。如果是论坛管理员，则自动是插件的管理员。</td></tr>
<tr><td height="22"><li>方法Dv_plus.updateuser() 可自动根据设置更新用户的金钱、积分、魅力、威望.插件开发者可根据自己的需要调用。</td></tr>
</table>
<%
Set Dv_plus=Nothing 
Dvbbs.Footer
Dvbbs.PageEnd()
%>