<HTML>
<HEAD>
<title>插入水平线</title>
<META content="text/html; charset=gb2312" http-equiv=Content-Type>
<link rel="stylesheet" type="text/css" href="editor_dialog.css">
<script language="JavaScript">
function OK(){
  var str1;
  str1="<hr color='"+color.value+"' size="+size.value+"' "+shadetype.value+" align="+align.value+" width="+width.value+">"
  window.returnValue = str1
  window.close();
}
function IsDigit()
{
  return ((event.keyCode >= 48) && (event.keyCode <= 57));
}
</script>
</head>
<BODY bgColor=menu topmargin=15 leftmargin=15 >
<table width=100% border="0" cellpadding="0" cellspacing="2">
  <tr><td>
<FIELDSET align=left>
<LEGEND align=left><strong>输入水平线参数</strong></LEGEND>
      <table border="0" cellpadding="0" cellspacing="3">
        <tr> 
          <td>线条颜色：
            <input name="color" id=color style='cursor:text' onclick='p=showModalDialog("editor_color.asp",window,"center:yes;dialogHeight:320px;dialogWidth:300px;help:no;status:no");if(p!=null){this.value=p.split("*")[0]}else{this.value=""}' readonly>
          </td>
        </tr>
        <tr>
          <td>线条粗度：
            <input name="size"  id=size onKeyPress="event.returnValue=IsDigit();" value="2" size="4" maxlength=3>
必须是数字，范围建议在1-100之间</td>
        </tr>
        <tr> 
          <td> 页面对齐：
            <select name="align"  id=align>
              <option value="left" selected>默认对齐</option>
              <option value="left">左对齐 </option>
              <option value="center">中对齐 </option>
              <option value="right">右对齐 </option>
            </select>
            &nbsp;&nbsp;阴影效果；
            <select name="shadetype"  id=shadetype>
              <option value=noshade selected>无 
              <option value=''>有 
            </select>
          </td>
        </tr>
        <tr> 
          <td> 水平宽度：
            <input name="width" id=width ONKEYPRESS="event.returnValue=IsDigit();" value="400" size="6" maxlength=3>
            必须是数字，范围建议在1-999之间</td>
        </tr>
      </table>
</fieldset></td>
    <td width=80 align="center"><input name="cmdOK" type="button" id="cmdOK" value="  确定  " onClick="OK();">
      <br>
      <br>
      <input name="cmdCancel" type=button id="cmdCancel" onclick="window.close();" value='  取消  '></td>
  </tr></table>
</body>
</html>