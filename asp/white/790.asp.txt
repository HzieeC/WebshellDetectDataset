<title>颜色调解器</title><link rel="stylesheet" type="text/css" href="editor_dialog.css">
<body bgcolor=D4D0C8 topmargin=0 leftmargin=0 onload="onloads()">
<center>&nbsp;<br>
<table width=255 border=0 cellspacing=2 cellpadding=0 style='font-size:25px;' id=tds><Tr><td bgcolor=white onmouseup='mov(a)'>
<table width=20 height=100% style='position:relative;' id=a><Tr><Td bgcolor=red>&nbsp;</td></tr></table>
</td></tr><tr><td bgcolor=white onmouseup='mov(b)'>
<table width=20 height=100% style='position:relative;' id=b><Tr><Td bgcolor=green>&nbsp;</td></tr></table>
</td></tr><Tr><Td bgcolor=white onmouseup='mov(c)'>
<table width=20 height=100% style='position:relative;' id=c><Tr><Td bgcolor=blue>&nbsp;</td></tr></table>
</td></tr></table><br>
<table width=255 height=50 border=0 bgcolor="#FFFFFF">
  <tr><Td id=d bgcolor=black>&nbsp;</tD></tr></table>
<div id=e><font color=red>红:0 </font><font color=green>绿:0 </font><font color=blue>蓝:0 </font></div>
<p>
<input id=txt1 value=#000000 style='width:198' readonly> 应用<input type=radio name=radio1 checked><br>
<input id=txt2 value=rgb(0,0,0) style='width:198' readonly> 应用<input type=radio name=radio1><br>
<input id=txt3 value=rgb(0%,0%,0%) style='width:198' readonly> 应用<input type=radio name=radio1><br>
<input type=button onclick="ok()" value="确认输入">

<script>
function mov(who){
who.style.posLeft=event.offsetX
d.bgColor='rgb('+a.style.posLeft+','+b.style.posLeft+','+c.style.posLeft+')'
e.innerHTML="<font color=red>红:"+a.style.posLeft+" </font><font color=green>绿:"+b.style.posLeft+" </font><font color=blue>蓝:"+c.style.posLeft
txt1.value=d.bgColor
txt2.value='rgb('+a.style.posLeft+','+b.style.posLeft+','+c.style.posLeft+')'
txt3.value='rgb('+Math.round(a.style.posLeft/2.55)+'%,'+Math.round(b.style.posLeft/2.55)+'%,'+Math.round(c.style.posLeft/2.55)+'%)'
}
function ok(){
if(document.all("radio1")[0].checked==true){txtstr=txt1.value}
if(document.all("radio1")[1].checked==true){txtstr=txt2.value}
if(document.all("radio1")[2].checked==true){txtstr=txt3.value}
returnValue=txtstr+"*"+a.style.posLeft+"*"+b.style.posLeft+"*"+c.style.posLeft
window.close()
}
function onloads(){
a.style.posLeft=dialogArguments.color1
b.style.posLeft=dialogArguments.color2
c.style.posLeft=dialogArguments.color3
d.bgColor='rgb('+a.style.posLeft+','+b.style.posLeft+','+c.style.posLeft+')'
e.innerHTML="<font color=red>红:"+a.style.posLeft+" </font><font color=green>绿:"+b.style.posLeft+" </font><font color=blue>蓝:"+c.style.posLeft
txt1.value=d.bgColor
txt2.value='rgb('+a.style.posLeft+','+b.style.posLeft+','+c.style.posLeft+')'
txt3.value='rgb('+Math.round(a.style.posLeft/2.55)+'%,'+Math.round(b.style.posLeft/2.55)+'%,'+Math.round(c.style.posLeft/2.55)+'%)'
}
</script>