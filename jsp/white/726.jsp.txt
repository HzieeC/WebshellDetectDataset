<%@ page language="java" import="java.util.*" pageEncoding="GB2312"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'bank.jsp' starting page</title>
    
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
	<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->

  </head>
  
  <body>
<form name=" sendOrder " method="post" 


action="http://工行网上银行支付服务器IP地址/ servlet/com.icbc.inbs.b2c.pay.B2cMerPayReqServlet ">


<input type="text" name="merchantid" value="020000010001" >    --商城代码


<input type="text" name="interfaceType" value="HS" >  --接口类型



























































        --接收工行支付结果信息的程序名称和地址


<input type="text" name="orderid" value="12345678912345" >  --订单号


<input type="text" name="amount" value="10056" >   --订单总金额（以分为单位）


<input type="text" name="curType" value="001" >   --币种


<input type="text" name="hsmsgType" value="0" >   --信息发送类型


<input type="text" name="signMsg" value="LKJDFKF#$LKJFDA090980LKJAFK" >   --BASE64编码后的交易数据签名信息


<input type="text" name="cert" value="%$#%#KLLK4LKJLJ67LJ8L54LH67L" >     --BASE64编码后的商户证书


<input type="text" name="comment1" value=" " >   --备注字段2


<input type="text" name="comment2" value=" " >   --备注字段3


<input type="submit" value="工行支付">


</form>

  </body>
</html>
