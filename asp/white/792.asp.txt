<html>
<head>
<title>插入表格</title>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<link rel="stylesheet" type="text/css" href="editor_dialog.css">
<script language="JavaScript">
function checkchange()
{
if  (checkbox.checked==true){
    width.disabled=false;
	}
else{	
   width.disabled=true;
   }
}	 


function table(){
var tablehtml="";
tablehtml=tablehtml+"<table ";
if (align.value=="left"||align.value=="center"||align.value=="right")
 tablehtml=tablehtml+"align="+align.value+" ";
if (border!="")
 tablehtml=tablehtml+"border="+border.value+" "
 else
 tablehtml=tablehtml+"border=1 "
if(cellpad.value!="")
  tablehtml=tablehtml+"cellpadding="+cellpad.value+" " 
 if(cellspace.value!="")
  tablehtml=tablehtml+"cellspacing="+cellspace.value+" "
  if(bordercolor.value!="")
  tablehtml=tablehtml+"bordercolor="+bordercolor.value+" " 
  if(bgcolor.value!="")
  tablehtml=tablehtml+"bgcolor="+bgcolor.value+" "
  if  (checkbox.checked==true)
    {
	if(width.value!="")
    { tablehtml=tablehtml+"width="+width.value
	     tablehtml=tablehtml+WidthUnit.value+""
	}	 
	} 	
	 tablehtml=tablehtml+">"  
for (i=1;i<=rows.value;i++)
{
tablehtml=tablehtml+"<tr>";
 for (j=1;j<=columns.value;j++)
   tablehtml=tablehtml+"<td>&nbsp;</td>";
 }  
 tablehtml= tablehtml+"</table>"; 
window.returnValue = tablehtml;
 window.close();
}
</script>

</head>
<body bgcolor=menu topmargin="15" leftmargin="15">
<table width="100%" border="0" cellspacing="2" cellpadding="0">
  <tr>
    <td><fieldset><legend>表格大小</legend>
      <table border="0" cellspacing="3" cellpadding="0">
        <tr>
          <td>表格行数：
            <input type="text" name="rows" size="8" maxlength="2">
          </td>
          <td>表格列数：
            <input type="text" name="columns" size="8" maxlength="2">
          </td>
        </tr>
        <tr>
          <td align="right"><input name="checkbox" type="checkbox" onClick="checkchange();" value="1">指定表格的宽度</td>
          <td align="right"><input name="width" type="text" value="100" size="5" maxlength="3" disabled>
            <select name="WidthUnit">
              <option>像素</option>
              <option value="%" selected>百分比</option>
            </select></td>
        </tr>
        <tr>
          <td>边框粗细：
          <input name="border" type="text" value="1" size="8" maxlength="2"></td>
          <td>对齐方式：
            <select name="align">
              <option selected>默认</option>
              <option value="left">居左</option>
              <option value="center">居中</option>
              <option value="right">居右</option>
            </select></td>
        </tr>
        <tr>
          <td>单元格边距：
          <input type="text" name="cellpad" size="8" maxlength="2"></td>
          <td>单元格间距：
          <input type="text" name="cellspace" size="8" maxlength="2"></td>
        </tr>
        <tr>
          <td>边框颜色：
            <input name='bordercolor' type='text' style='cursor:text' onClick='p=showModalDialog("editor_color.asp",window,"center:yes;dialogHeight:320px;dialogWidth:300px;help:no;status:no");if(p!=null){this.value=p.split("*")[0]}else{this.value=""}' size="8" maxlength="20" readonly>
          </td>
          <td>单元格背景：
            <input name='bgcolor' type='text' style='cursor:text' onClick='p=showModalDialog("editor_color.asp",window,"center:yes;dialogHeight:320px;dialogWidth:300px;help:no;status:no");if(p!=null){this.value=p.split("*")[0]}else{this.value=""}' size="8" maxlength="20" readonly>
          </td>
        </tr>
    </table>
	</fieldset>
	</td>
    <td align="center"><input class=button type=submit onClick='table();' value="确　定" name="submit">
      <br>
      <br>
      <input class=button type=button onClick='window.close();' value="取　消" name="button">
</td>
  </tr>
</table>
</body>
</html>