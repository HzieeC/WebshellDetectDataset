<table width="99%" border="0" align=center cellpadding="0" cellspacing="0" bgcolor="#C6BEC6" style="font-family: 宋体;font-size:10pt">
              <tr >
                <td height="23" style='BORDER-BOTTOM:#666666 1px solid '><strong>&nbsp;客户信息反馈管理</strong> <font color="#0000FF"><strong>注意 </strong>：删除目录同时将删除该目录下所有内容！ </font></td>
              </tr>
</table>
<script language=javascript> 
var DS_x,DS_y; 

function dateSelector()  //构造dateSelector对象，用来实现一个日历形式的日期输入框。 
{ 
  var myDate=new Date(); 
  this.year=myDate.getFullYear();  //定义year属性，年份，默认值为当前系统年份。 
  this.month=myDate.getMonth()+1;  //定义month属性，月份，默认值为当前系统月份。 
  this.date=myDate.getDate();  //定义date属性，日，默认值为当前系统的日。 
  this.inputName='';  //定义inputName属性，即输入框的name，默认值为空。注意：在同一页中出现多个日期输入框，不能有重复的name！ 
  this.display=display;  //定义display方法，用来显示日期输入框。 
} 

function display()  //定义dateSelector的display方法，它将实现一个日历形式的日期选择框。 
{ 
  var week=new Array('日','一','二','三','四','五','六'); 

  document.write("<style type=text/css>"); 
  document.write("  .ds_font td,span  { font: normal 12px 宋体; color: #000000; }"); 
  document.write("  .ds_border  { border: 1px solid #000000; cursor: hand; background-color: #DDDDDD }"); 
  document.write("  .ds_border2  { border: 1px solid #000000; cursor: hand; background-color: #DDDDDD }"); 
  document.write("</style>"); 

  document.write("<input style='text-align:left;'size='10' id='DS_"+this.inputName+"' name='"+this.inputName+"' value='"+this.year+"-"+this.month+"-"+this.date+"' title=双击可进行编缉 ondblclick='this.readOnly=false;this.focus()' onblur='this.readOnly=true' readonly>"); 
  document.write("<button style='width:60px;height:18px;font-size:12px;margin:1px;border:1px solid #A4B3C8;background-color:#DFE7EF;' type=button onclick=this.nextSibling.style.display='block' onfocus=this.blur()>选择日期</button>"); 

  document.write("<div style='position:absolute;display:none;text-align:center;width:0px;height:0px;overflow:visible' onselectstart='return false;'>"); 
  document.write("  <div style='position:absolute;left:-60px;top:20px;width:142px;height:165px;background-color:#F6F6F6;border:1px solid #245B7D;' class=ds_font>"); 
  document.write("    <table cellpadding=0 cellspacing=1 width=140 height=20 bgcolor=#CEDAE7 onmousedown='DS_x=event.x-parentNode.style.pixelLeft;DS_y=event.y-parentNode.style.pixelTop;setCapture();' onmouseup='releaseCapture();' onmousemove='dsMove(this.parentNode)' style='cursor:move;'>"); 
  document.write("      <tr align=center>"); 
  document.write("        <td width=12% onmouseover=this.className='ds_border' onmouseout=this.className='' onclick=subYear(this) title='减小年份'><<</td>"); 
  document.write("        <td width=12% onmouseover=this.className='ds_border' onmouseout=this.className='' onclick=subMonth(this) title='减小月份'><</td>"); 
  document.write("        <td width=52%><b>"+this.year+"</b><b>年</b><b>"+this.month+"</b><b>月</b></td>"); 
  document.write("        <td width=12% onmouseover=this.className='ds_border' onmouseout=this.className='' onclick=addMonth(this) title='增加月份'>></td>"); 
  document.write("        <td width=12% onmouseover=this.className='ds_border' onmouseout=this.className='' onclick=addYear(this) title='增加年份'>>></td>"); 
  document.write("      </tr>"); 
  document.write("    </table>"); 

  document.write("    <table cellpadding=0 cellspacing=0 width=140 height=20 onmousedown='DS_x=event.x-parentNode.style.pixelLeft;DS_y=event.y-parentNode.style.pixelTop;setCapture();' onmouseup='releaseCapture();' onmousemove='dsMove(this.parentNode)' style='cursor:move;'>"); 
  document.write("      <tr align=center>"); 
  for(i=0;i<7;i++) 
   document.write("      <td>"+week[i]+"</td>"); 
  document.write("      </tr>"); 
  document.write("    </table>"); 

  document.write("    <table cellpadding=0 cellspacing=2 width=140 bgcolor=#EEEEEE>"); 
  for(i=0;i<6;i++) 
  { 
    document.write("    <tr align=center>"); 
   for(j=0;j<7;j++) 
      document.write("    <td width=10% height=16 onmouseover=if(this.innerText!=''&&this.className!='ds_border2')this.className='ds_border' onmouseout=if(this.className!='ds_border2')this.className='' onclick=getValue(this,document.all('DS_"+this.inputName+"'))></td>"); 
    document.write("    </tr>"); 
  } 
  document.write("    </table>"); 

  document.write("    <span style=cursor:hand onclick=this.parentNode.parentNode.style.display='none'>【关闭】</span>"); 
  document.write("  </div>"); 
  document.write("</div>"); 

  dateShow(document.all("DS_"+this.inputName).nextSibling.nextSibling.childNodes[0].childNodes[2],this.year,this.month) 
} 

function subYear(obj)  //减小年份 
{ 
  var myObj=obj.parentNode.parentNode.parentNode.cells[2].childNodes; 
  myObj[0].innerHTML=eval(myObj[0].innerHTML)-1; 
  dateShow(obj.parentNode.parentNode.parentNode.nextSibling.nextSibling,eval(myObj[0].innerHTML),eval(myObj[2].innerHTML)) 
} 

function addYear(obj)  //增加年份 
{ 
  var myObj=obj.parentNode.parentNode.parentNode.cells[2].childNodes; 
  myObj[0].innerHTML=eval(myObj[0].innerHTML)+1; 
  dateShow(obj.parentNode.parentNode.parentNode.nextSibling.nextSibling,eval(myObj[0].innerHTML),eval(myObj[2].innerHTML)) 
} 

function subMonth(obj)  //减小月份 
{ 
  var myObj=obj.parentNode.parentNode.parentNode.cells[2].childNodes; 
  var month=eval(myObj[2].innerHTML)-1; 
  if(month==0) 
  { 
    month=12; 
    subYear(obj); 
  } 
  myObj[2].innerHTML=month; 
  dateShow(obj.parentNode.parentNode.parentNode.nextSibling.nextSibling,eval(myObj[0].innerHTML),eval(myObj[2].innerHTML)) 
} 

function addMonth(obj)  //增加月份 
{ 
  var myObj=obj.parentNode.parentNode.parentNode.cells[2].childNodes; 
  var month=eval(myObj[2].innerHTML)+1; 
  if(month==13) 
  { 
    month=1; 
    addYear(obj); 
  } 
  myObj[2].innerHTML=month; 
  dateShow(obj.parentNode.parentNode.parentNode.nextSibling.nextSibling,eval(myObj[0].innerHTML),eval(myObj[2].innerHTML)) 
} 

function dateShow(obj,year,month)  //显示各月份的日 
{ 
  var myDate=new Date(year,month-1,1); 
  var today=new Date(); 
  var day=myDate.getDay(); 
  var selectDate=obj.parentNode.parentNode.previousSibling.previousSibling.value.split('-'); 
  var length; 
  switch(month) 
  { 
    case 1: 
    case 3: 
    case 5: 
    case 7: 
    case 8: 
    case 10: 
    case 12: 
      length=31; 
      break; 
    case 4: 
    case 6: 
    case 9: 
    case 11: 
      length=30; 
      break; 
    case 2: 
      if((year%4==0)&&(year%100!=0)||(year%400==0)) 
        length=29; 
      else 
        length=28; 
  } 
  for(i=0;i<obj.cells.length;i++) 
  { 
    obj.cells[i].innerHTML=''; 
    obj.cells[i].style.color=''; 
    obj.cells[i].className=''; 
  } 
  for(i=0;i<length;i++) 
  { 
    obj.cells[i+day].innerHTML=(i+1); 
    if(year==today.getFullYear()&&(month-1)==today.getMonth()&&(i+1)==today.getDate()) 
      obj.cells[i+day].style.color='red'; 
    if(year==eval(selectDate[0])&&month==eval(selectDate[1])&&(i+1)==eval(selectDate[2])) 
      obj.cells[i+day].className='ds_border2'; 
  } 
} 

function getValue(obj,inputObj)  //把选择的日期传给输入框 
{ 
  var myObj=inputObj.nextSibling.nextSibling.childNodes[0].childNodes[0].cells[2].childNodes; 
  if(obj.innerHTML) 
    inputObj.value=myObj[0].innerHTML+"-"+myObj[2].innerHTML+"-"+obj.innerHTML; 
  inputObj.nextSibling.nextSibling.style.display='none'; 
  for(i=0;i<obj.parentNode.parentNode.parentNode.cells.length;i++) 
    obj.parentNode.parentNode.parentNode.cells[i].className=''; 
  obj.className='ds_border2' 
} 

function dsMove(obj)  //实现层的拖移 
{ 
  if(event.button==1) 
  { 
    var X=obj.clientLeft; 
    var Y=obj.clientTop; 
    obj.style.pixelLeft=X+(event.x-DS_x); 
    obj.style.pixelTop=Y+(event.y-DS_y); 
  } 
} 
</script>
