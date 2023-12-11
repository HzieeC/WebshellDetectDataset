<%@LANGUAGE="VBSCRIPT" CODEPAGE="936"%>
<html>
<head>
<meta http-equiv="Content-Language" content="en-us">
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<title>表格属性</title>
<link rel="stylesheet" href="editor_dialog.css" type="text/css">
<SCRIPT LANGUAGE="JScript.Encode">
var tempbordercolor="Default";
var tempbackgroundcolor="Default";
var color_value="";
var color_status=0;
function getcolorfrommoondowner(currentcolor){
	color_value=currentcolor;
	window.showModalDialog("color.htm",window,"dialogHeight: 250px; dialogWidth: 520px; help: no;scroll: no; status: no");
	var x=color_value;
	return x;
}

function showCol(i) {
  col = websn_getTblCol();
  if (col != "" && col != null) {
  	if (i==1){
 		 window.document.all.item("bordercolortable").bgColor=col;
		 tempbordercolor=col;
	}else{
		window.document.all.item("backgroundcolortable").bgColor=col;
		tempbackgroundcolor=col;
	}
  }
}

function websn_getTblCol() {
var retCol
  bascol = showModalDialog("editor_color.asp",window,"center:yes;dialogHeight:320px;dialogWidth:300px;help:no;status:no");
  if(bascol!=null){retCol=bascol.split("*")[0]}else{retCol=""}
  return retCol;
}

function setattributes(){
	var x=window.dialogArguments;
	x.tablewidthspecified="yes";
	if(window.document.moondownerform.widthspecified.checked==false){
		x.tablewidthspecified="no";
	}
	x.tablewidth="";
	if(isNaN(parseInt(window.document.moondownerform.tablewidth.value))==false){x.tablewidth=parseInt(window.document.moondownerform.tablewidth.value);}
	var tempx="";
	if(window.document.moondownerform.widthtype[0].checked==true){tempx="pixels";}
	if(window.document.moondownerform.widthtype[1].checked==true){tempx="percentage";}
	x.tablewidthtype=tempx;
	var alignindex=window.document.moondownerform.align.selectedIndex;
	x.tablealign=window.document.moondownerform.align.options[alignindex].value;
	x.tablebordersize="1";
	if(isNaN(parseInt(window.document.moondownerform.bordersize.value))==false){x.tablebordersize=parseInt(window.document.moondownerform.bordersize.value)+1;}
	x.tablecellpadding="1";
	if(isNaN(parseInt(window.document.moondownerform.cellpadding.value))==false){x.tablecellpadding=parseInt(window.document.moondownerform.cellpadding.value);}
	x.tablecellspacng="1";
	if(isNaN(parseInt(window.document.moondownerform.cellspacing.value))==false){x.tablecellspacing=parseInt(window.document.moondownerform.cellspacing.value);}
	var tablestyleindex=window.document.moondownerform.tablestyle.selectedIndex;
	x.tableclass="";
	if(tablestyleindex!=0){x.tableclass=window.document.moondownerform.tablestyle.options[tablestyleindex].value;}
	x.tablebordercolor=tempbordercolor;
	x.tablebackgroundcolor=tempbackgroundcolor;
	x.tableiscancel="no";
	x.tableisinsert="yes";
	x.table_status=0;
	x.tablemoondowneropen=0;window.close();
}

function bye(){
	var x=window.dialogArguments;
	x.table_status=0;
	x.tableiscancel="yes";
	x.tableisinsert="no";
	window.close();
}

function clearme(){
	var x=window.dialogArguments;
	x.table_status=0;
}

function initmoondowner(){
	var x=window.dialogArguments;
	if(x.tablewidthspecified=="yes"){
		window.document.moondownerform.widthspecified.checked=true;
	}else{
		window.document.moondownerform.widthspecified.checked=false;
	}
	var s1=""+x.tablewidth;
	if(s1.indexOf("%")!=-1){
		window.document.moondownerform.widthtype[1].checked=true;
	}
		else{window.document.moondownerform.widthtype[0].checked=true;
	}
	if(x.tablewidth!=""&&isNaN(parseInt(x.tablewidth))!=true){window.document.moondownerform.tablewidth.value=parseInt(x.tablewidth);}else{window.document.moondownerform.tablewidth.value="";}
	var s2=""+x.tablealign;
	s2=s2.toUpperCase();
	if(s2=="LEFT"){
		window.document.moondownerform.align.options[1].selected=true;
	}else if(s2=="CENTER"){
		window.document.moondownerform.align.options[2].selected=true;
	}else if(s2=="RIGHT"){
		window.document.moondownerform.align.options[3].selected=true;
	}else{
		window.document.moondownerform.align.options[0].selected=true;
	}
	if(x.tablebordersize!=""&&x.tablebordersize!=null){
		window.document.moondownerform.bordersize.value=(parseInt(x.tablebordersize)-1);
	}else{
		window.document.moondownerform.bordersize.value="";
	}
	if(x.tablecellpadding!=""&&x.tablecellpadding!=null){
		window.document.moondownerform.cellpadding.value=x.tablecellpadding;
	}else{
		window.document.moondownerform.cellpadding.value="";
	}
	if(x.tablecellspacing!=""&&x.tablecellspacing!=null){
		window.document.moondownerform.cellspacing.value=x.tablecellspacing;
	}else{
		window.document.moondownerform.cellspacing.value="";
	}
	if(x.tablebordercolor!=""&&x.tablebordercolor!=null){
		tempbordercolor=x.tablebordercolor;
		window.document.all.item("bordercolortable").bgColor=x.tablebordercolor;
	}
	if(x.tablebackgroundcolor!=""&&x.tablebackgroundcolor!=null){
		tempbackgroundcolor=x.tablebackgroundcolor;
		window.document.all.item("backgroundcolortable").bgColor=x.tablebackgroundcolor;
	}
	
}

function tcolor(){
	window.document.all.item("bordercolortable").bgColor="#FF0000";
}

function checkkey(x){if(x=="13"){setattributes();}}

</SCRIPT>
</head>

<body onLoad="initmoondowner();" onunload="clearme();" onkeydown="return checkkey(event.keyCode);" bgcolor=menu>
<form method="POST" name="moondownerform">
  <table border="0" width="100%" cellspacing="10" cellpadding="0" align="center">
    <tr> 
      <td width="100%" height="96"><fieldset><legend>布局</legend>
        <table border="0" width="100%" cellspacing="0" cellpadding="2">
          <tr>
            <td width="50%" valign="middle" align="right">
              <table border="0" width="100%" cellpadding="1" cellspacing="0">
                <tr>
                  <td width="50%" valign="middle" align="right">对齐方式：</td>
                  <td width="50%" valign="middle" align="left">
                    <select size="1" name="align">
                      <option value="Default" selected>默认</option>
                      <option value="Left">居左</option>
                      <option value="Center">居中</option>
                      <option value="Right">居右</option>
                    </select>
                  </td>
                </tr>
                <tr>
                  <td width="50%" valign="middle" align="right">单元格边距：</td>
                  <td width="50%" valign="middle" align="left">
                    <input type="text" name="cellpadding" size="7" value="1">
                  </td>
                </tr>
                <tr>
                  <td width="50%" valign="middle" align="right">单元格间距：</td>
                  <td width="50%" valign="middle" align="left">
                    <input type="text" name="cellspacing" size="7" value="1">
                  </td>
                </tr>
              </table>
            </td>
            <td width="50%">
              <table width="100%" border="0" cellspacing="0" cellpadding="0">
                <tr>
                  <td width="8%">
                    <input type="checkbox" name="widthspecified" value="ON" checked>
                  </td>
                  <td width="92%">指定表格宽度</td>
                </tr>
                <tr>
                  <td width="8%">
                    <input type="text" name="tablewidth" size="4" value="100">
                  </td>
                  <td width="92%"><input type="radio" value="pixels" name="widthtype">                    像素
                      &nbsp;&nbsp;
                      <input type="radio" value="percentage" checked name="widthtype">
                  百分比</td>
                </tr>
              </table>
            </td>
          </tr>
        </table>
      </fieldset></td>
    </tr>
    <tr> 
      <td width="100%"><fieldset><legend>边框</legend><table width="100%" border="0" align="center" cellpadding="3" cellspacing="0">
    <tr>
      <td height="26">边框大小：
              <input type="text" name="bordersize" size="7" value="0">
      </td>
      </tr>
    <tr>
      <td >
              <table width="100%" border="0" cellspacing="0" cellpadding="0">
                <tr>
                  <td width="21%">边框颜色：</td>
                  <td width="29%"> 
                    <table id=bordercolortable cellspacing=0 cellpadding=0 border=0>
                      <tbody> 
                      <tr> 
                        <td onClick=showCol(1); width="100%"><img height=20 
                  src="images/clrbox.gif" width=54 
              border=0></td>
                      </tr>
                      </tbody>
                    </table>
                  </td>
                  <td width="21%">背景颜色：</td>
                  <td width="29%"> 
                    <table id=backgroundcolortable cellspacing=0 cellpadding=0 
              border=0>
                      <tbody> 
                      <tr> 
                        <td onClick=showCol(0); width="100%"><img height=20 
                  src="images/clrbox.gif" width=54 
              border=0></td>
                      </tr>
                      </tbody>
                    </table>
                  </td>
                </tr>
              </table>
              </td>
      </tr>
  </table>
      </fieldset></td>
    </tr>
    <tr> 
      <td><fieldset><legend>表格样式</legend> 
          <div align="center">
            <select size="1" name="tablestyle">
              <option value="Default">默认</option>
            </select>
          </div>
      </fieldset>
      </td>
    </tr>
    <tr> 
      <td align="center">
        <input type="button" value="确定" name="btn_insert" onClick="setattributes();" style="width:80px;">
        &nbsp;
<input type="button" value="取消" name="btn_cancel" style="width:80px;" onClick="bye();">
      </td>
    </tr>
  </table>
</form>
</body>
</html>