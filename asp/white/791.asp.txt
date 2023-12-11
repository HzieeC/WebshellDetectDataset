<HTML><HEAD><TITLE>插入图片URL</TITLE>
<META content="text/html; charset=gb2312" http-equiv=Content-Type>
<STYLE type=text/css>TD {
FONT-SIZE: 9pt
}
BODY {
FONT-SIZE: 9pt
}
INPUT{
FONT-SIZE: 9pt
}
SELECT{
FONT-SIZE: 9pt
}

</STYLE>
<script language="JavaScript">
function checkchange()
{
if  (chkSize.checked==true){
    width.disabled=false;
	height.disabled=false;
	}
else{	
   width.disabled=true;
   height.disabled=true;
   }
}

function image(){
var imghtml="";
var strurl=imgurl.value;
if (strurl==""||strurl=="http://") return;
strtype=strurl.substring(strurl.length-3,strurl.length);
strtype=strtype.toLowerCase();
if (strtype=="jpg"||strtype=="gif"){
  imghtml=imghtml+"<img src='"+imgurl.value+"' align='"+aligntype.value+"'";
  if (alttext.value!="") imghtml=imghtml+" alt='"+alttext.value+"'";
  if (border.value!="") imghtml=imghtml+" border='"+border.value+"'";
  if (chkSize.checked==true){
    if (width.value!="") imghtml=imghtml+" width='"+width.value+"'";
	if (height.value!="") imghtml=imghtml+" height='"+height.value+"'";
	}
  imghtml=imghtml+">";
  window.returnValue = imghtml;
  }
window.close();
}
</script>

<META content="MSHTML 5.00.2920.0" name=GENERATOR></HEAD>
<BODY bgColor=menu>
<br>
<table width="400" border="0" align="center">
  <tr> 
    <td width="76" height="30" align="right">图片URL：</td>
    <td height="30" colspan="2"><input name="imgurl" type="text" id="imgurl" value="http://" size="48"></td>
  </tr>
  <tr> 
    <td height="30" align="right">替换文字：</td>
    <td width="131" height="30"><input name="alttext" type="text" id="alttext" size="18"></td>
    <td width="179" height="30">对齐方式： 
      <select name="aligntype" id="select">
        <option value="">默认</option>
        <option value="left" selected>左对齐</option>
        <option value="right">右对齐</option>
        <option value="top">顶边对齐</option>
        <option value="texttop">文字上方</option>
        <option value="middle">相对垂直居中</option>
        <option value="absmiddle">绝对垂直居中</option>
        <option value="baseline">基线</option>
        <option value="bottom">相对底边对齐</option>
        <option value="absbottom">绝对底边对齐</option>
        <option value="center">居中</option>
      </select></td>
  </tr>
  <tr> 
    <td height="30" align="right">图片大小：</td>
    <td height="30"><input name="chkSize" type="checkbox" id="chkSize" value="checkbox" onChange="checkchange();">
      指定大小</td>
    <td height="30">边框粗细： 
      <input name="border" type="text" id="border" value="0" size="5">
      像素</td>
  </tr>
  <tr> 
    <td height="30" align="right">&nbsp;</td>
    <td height="30">宽： 
      <input name="width" type="text" id="width" size="5" disabled>
      像素 </td>
    <td>高： 
      <input name="height" type="text" id="height" size="5" disabled>
      像素</td>
  </tr>
  <tr align="center"> 
    <td height="5" colspan="3"><hr></td>
  </tr>
  <tr align="right">
    <td height="30" colspan="3"><input name="cmdOK" type="button" id="cmdOK" value=" 确定 " onClick="image();"> 
      &nbsp; <input name="cmdCancel" type="submit" id="cmdCancel" value=" 取消 " onClick="window.close();"> 
      &nbsp;&nbsp;&nbsp;&nbsp;</td>
  </tr>
</table>
</BODY></HTML>