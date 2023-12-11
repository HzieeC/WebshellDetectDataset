<HTML><HEAD><TITLE>简易计算器</TITLE>
<META http-equiv=Pragma content=no-cache>
<META http-equiv=Content-Language content=zh-cn>
<META http-equiv=Content-Type content="text/html; charset=gb2312">

<SCRIPT language=JavaScript>
function computetoedit(obj) 
{
return eval(obj.expr.value)
}
function compute(obj) 

   {obj.expr.value = eval(obj.expr.value)}

var one = '1'

var two = '2'

var three = '3'

var four = '4'

var five = '5'

var six = '6'

var seven = '7'

var eight = '8'

var nine = '9'

var zero = '0'

var plus = '+'

var minus = '-'

var multiply = '*'

var divide = '/'

var decimal = '.'

function enter(obj, string) 

   {obj.expr.value += string}


</SCRIPT>

<STYLE type=text/css>INPUT {
	FONT-SIZE: 9pt
}
</STYLE>
<SCRIPT event=onclick for=Ok language=JavaScript>
  a.value = computetoedit(this.form);
  window.returnValue = a.value+"*"+b.value
  window.close();
</SCRIPT>
<script>
function IsDigit()
{
  return ((event.keyCode >= 48) && (event.keyCode <= 57));
}
</script>
</HEAD>
<BODY bgColor=menu leftMargin=0 topMargin=0 oncontextmenu=window.event.returnValue=false>
<FORM name=calc>
<TABLE borderColor=#efefef width=152 align=center border=1>
  <TBODY>
  <TR>
    <TD width=202 bgColor=#000000 colSpan=4><INPUT 
      style="BORDER-RIGHT: #000000 1px inset; BORDER-TOP: #000000 1px inset; FONT-SIZE: 18pt; BORDER-LEFT: #000000 1px inset; COLOR: #ffff00; BORDER-BOTTOM: #000000 1px inset; BACKGROUND-COLOR: #000000" 
      size=14 name=expr id=a action="compute(this.form)" ONKEYPRESS="event.returnValue=IsDigit();"> 
  <TR>
    <TD width=37 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, seven)" type=button value=" 7 "> 
      </P>
    <TD width=50 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, eight)" type=button value=" 8 "> 
      </P>
    <TD width=47 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, nine)" type=button value=" 9 "> 
      </P>
    <TD width=50 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, divide)" type=button value=" / "> 
      </P>
  <TR>
    <TD width=37 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, four)" type=button value=" 4 "> 
      </P>
    <TD width=50 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, five)" type=button value=" 5 "> 
      </P>
    <TD width=47 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, six)" type=button value=" 6 "> 
      </P>
    <TD width=50 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, multiply)" type=button value=" * "> 
      </P>
  <TR>
    <TD width=37 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, one)" type=button value=" 1 "> 
      </P>
    <TD width=50 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, two)" type=button value=" 2 "> 
      </P>
    <TD width=47 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, three)" type=button value=" 3 "> 
      </P>
    <TD width=50 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, minus)" type=button value=" - "> 
      </P>
  <TR>
    <TD width=37 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, decimal)" type=button value=" . "> 
      </P>
    <TD width=50 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, zero)" type=button value=" 0 "> 
      </P>
    <TD width=47 bgColor=#c0c0c0>
      <P align=center><INPUT type=reset size=3 value=" AC"> 
      </P>
    <TD width=50 bgColor=#c0c0c0>
      <P align=center><INPUT onclick="enter(this.form, plus)" type=button value=" + "> 
      </P>
  <TR>
    <TD width=196 bgColor=#c0c0c0 colSpan=4><INPUT onclick=compute(this.form) type=button value="   =   " id=b> <INPUT type=button value=计算并插入 style="width:70px" id=Ok> <INPUT type=button value="关闭" onclick="javascript:window.close()">
  </TR></TBODY>
</TABLE>
</FORM>
</BODY>
</HTML>
