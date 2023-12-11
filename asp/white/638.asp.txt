<%@LANGUAGE="VBSCRIPT" CODEPAGE="936"%>
<html>
<head>
<meta http-equiv="Content-Language" content="en-us">
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<title>单元格属性</title>
<link rel="stylesheet" href="editor_dialog.css" type="text/css">
<SCRIPT LANGUAGE="JScript.Encode">
var tempbordercolor="Default";
var tempbackgroundcolor="Default";
var color_value="";
var color_status=0;
function getcolorfrommoondowner(currentcolor){
	color_value=currentcolor;
	window.showModalDialog("color.htm",window,"dialogHeight: 250px; dialogWidth: 520px; help: no;scroll: no; status: no");
	var x=color_value;return x;
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
	var alignindex=window.document.moondownerform.horizontalalign.selectedIndex;
	x.tablealign=window.document.moondownerform.horizontalalign.options[alignindex].value;
	var valignindex=window.document.moondownerform.verticalalign.selectedIndex;
	x.tablevalign=window.document.moondownerform.verticalalign.options[valignindex].value;
	var tablestyleindex=window.document.moondownerform.cellstyle.selectedIndex;
	x.tablecellclass="";
	if(tablestyleindex!=0){x.tablecellclass=window.document.moondownerform.cellstyle.options[tablestyleindex].value;}
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
	}else{
		window.document.moondownerform.widthtype[0].checked=true;
	}
	if(x.tablewidth!=""&&isNaN(parseInt(x.tablewidth))!=true){
		window.document.moondownerform.tablewidth.value=parseInt(x.tablewidth);
	}else{
		window.document.moondownerform.tablewidth.value="";
	}
	var s2=""+x.tablealign;s2=s2.toUpperCase();
	if(s2=="LEFT"){
		window.document.moondownerform.horizontalalign.options[1].selected=true;
	}else if(s2=="CENTER"){
		window.document.moondownerform.horizontalalign.options[2].selected=true;
	}else if(s2=="RIGHT"){
		window.document.moondownerform.horizontalalign.options[3].selected=true;
	}else if(s2=="JUSTIFY"){
		window.document.moondownerform.horizontalalign.options[4].selected=true;
	}else{
		window.document.moondownerform.horizontalalign.options[0].selected=true;
	}
	var s3=""+x.tablevalign;s3=s3.toUpperCase();
	if(s3=="TOP"){
		window.document.moondownerform.verticalalign.options[1].selected=true;
	}else if(s3=="MIDDLE"){
		window.document.moondownerform.verticalalign.options[2].selected=true;
	}else if(s3=="BASELINE"){
		window.document.moondownerform.verticalalign.options[3].selected=true;
	}else if(s3=="BOTTOM"){
		window.document.moondownerform.verticalalign.options[4].selected=true;
	}else{
		window.document.moondownerform.verticalalign.options[0].selected=true;
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

function tcolor(){window.document.all.item("bordercolortable").bgColor="#FF0000";}
</SCRIPT>
</head>

<body onLoad="initmoondowner();" onunload="clearme();" bgcolor=menu>
<form method="POST" name="moondownerform">
<table border="0" cellspacing="10" cellpadding="0" width="100%" align="center">
<tr>
 <td>
      <fieldset><legend>布局</legend><table border="0" width="100%" cellspacing="0" cellpadding="2">
                <tr> 
                  <td width="80" valign="middle" align="right">水平对齐：
                  </td>
                  <td width="80" valign="middle"> 
                    <select size="1" name="horizontalalign">
                      <option value="Default">默认</option>
                      <option value="Left">居左</option>
                      <option value="Center">居中</option>
                      <option value="Right">居右</option>
                    </select>
                  </td>
                  <td valign="middle" align="right"> 
                    <input type="checkbox" name="widthspecified" value="ON" checked>
                  </td>
                  <td width="130" valign="middle" align="left">指定单元格宽度</td>
                </tr>
                <tr> 
                  <td width="80" align="right" height="19">垂直对齐：
                  </td>
                  <td width="80" height="19"> 
                    <select size="1" name="verticalalign">
                      <option value="Default">默认</option>
                      <option value="Top">顶端</option>
                      <option value="Middle">居中</option>
                      <option value="Baseline">基线</option>
                      <option value="Bottom">底部</option>
                    </select>
                  </td>
                  <td align="right" height="19"> 
                    <input type="text" name="tablewidth" size="4" value="100">
                  </td>
                  <td width="130" valign="middle" align="left" height="19"> 
                    <input type="radio" value="pixels" name="widthtype">
                    像素
                    <input type="radio" value="percentage" checked name="widthtype">
                    百分比 </td>
                </tr>
              </table></fieldset>
</td><tr><td><fieldset><legend>颜色</legend>
              
        <table border="0" width="100%">
          <tr> 
                  
            <td valign="middle"> 
              <table width="100%" border="0" cellspacing="0" cellpadding="0">
                <tr>
                  <td width="25%">边框颜色：</td>
                  <td> 
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
                  <td width="25%">背景颜色：</td>
                  <td> 
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
              </table></td>
                </tr>
        </table>
			  </fieldset>
            </td>
          </tr>
          <tr> 
            <td><fieldset><legend>样式：</legend>单元格样式：
              <select size="1" name="cellstyle">
                <option value="Default">默认</option>
              </select>
			  </fieldset>
            </td>
</tr>
<tr>
<td align="center">
      <input name="btn_insert" type="button" id="btn_insert" onClick="setattributes();" value="  确定  ">
       &nbsp;
       <input type="button" value="  取消  " name="btn_cancel" onClick="bye();">
    </td>
</tr>
</table>
</form>
</body>
</html>