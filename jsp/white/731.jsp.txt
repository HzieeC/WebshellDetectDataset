<%@ page language="java" import="java.util.*" pageEncoding="GB2312"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>My JSP 'bank2.jsp' starting page</title>
    
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
<FORM  name="order" METHOD=POST 

ACTION="https://mybank.icbc.com.cn/servlet/ICBCINBSEBusinessServlet">

1、订单只能使用POST方式提交；使用https协议通讯；

2、接收servlet名称固定为：/servlet/ICBCINBSEBusinessServlet

3、银行地址：如果是生产则为“mybank.icbc.com.cn”，若为模拟测试环境则为“mybank.dccnet.com.cn”

<INPUT NAME="interfaceName" TYPE="text" value="ICBC_PERBANK_C2C" >

接口名称固定为“ICBC_PERBANK_C2C”

<INPUT NAME="interfaceVersion" TYPE="text" value="1.0.0.0">  
  
接口版本目前为&ldquo;1.0.0.0&rdquo;  
  
<INPUT NAME="orderID" TYPE="text" value="001">

订单号商户端产生，一天内不能重复。

<INPUT NAME="amount" TYPE="text" value="2000">

金额以分为单位

<INPUT NAME="merFeeAmt" TYPE="text" value="100">

金额以分为单位

<INPUT NAME="curType" TYPE="text" value="001">

币种目前只支持人民币，代码为“001”

<INPUT NAME="merID" TYPE="text" value="0200EC30000011" >

银行提供

<INPUT NAME="merAcct" TYPE="text" value="0200029109000030079">

银行提供

<INPUT NAME="venderCardNum" TYPE="text" value="9558800200100131909">

工行牡丹卡

<INPUT NAME="venderName" TYPE="text" value="张三">

中文使用GBK编码

<INPUT NAME="verifyJoinFlag" TYPE="text" value="0" >

“1”判断该客户是否与商户联名；取值“0”不检验客户是否与商户联名。

<INPUT NAME="notifyType" TYPE="text" value="HS">

HS方式实时发送通知；AG方式不发送通知；

<INPUT NAME="merURL" TYPE="text"  value="http://127.0.0.1/c2c.jsp">

接收银行通知地址，目前只支持http协议80端口

<INPUT NAME="resultType" TYPE="text"  value="0">

当通知方式是“HS”是有效；

取值“0”：无论支付成功或者失败，银行都向商户发送交易通知信息；取值“1”，银行只向商户发送交易成功的通知信息。

<INPUT NAME="orderDate" TYPE="text" value="20050801152744">

14位时间戳

<INPUT NAME="merSignMsg" TYPE="text" value="Jhlbbmzl338HsaZpx0ESDIVy4sYSBdWeKj8AB9IWitTZkWSmggvin/2RpGXfj0kSjBiXp5oqbJi5...(skip)">

商户签名数据BASE64编码

<INPUT NAME="merCert" TYPE="text" value="MIICVTCCAb6gAwIBAgIKI9fKEDP6AAANhTANBgkqhkiG9w0BAQUFADA0MRgwFgYDVQQDEw9wYmou..(skip)">

商户证书公钥BASE64编码

      <INPUT NAME="goodsID" TYPE="text" value="123">

      <INPUT NAME="goodsName" TYPE="text" value="小毡帽">

      <INPUT NAME="goodsNum" TYPE="text" value="1" >

      <INPUT NAME="carriageAmt" TYPE="text" value="100">

<INPUT NAME="merHint" TYPE="text" value="优惠促销，折上折！"  size="60">

以上五个字段用于客户支付页面显示

<INPUT NAME="remark1" TYPE="text" value="ABCDEFG">

备注字段

<INPUT NAME="remark2" TYPE="text" value="1234567">

备注字段

<INPUT NAME=" Language " TYPE="text" value="EN_US">

Language, EN_US为英文版

         <INPUT TYPE="submit" value=" 提 交 订 单 " >

      注意商户提交订单数据不能提交接口中没有定义的字段，提交按钮不能设置name属性，如果设置了，提交按钮的值将作为一个变量提交，可能造成数据检查错误。

</form>


  </body>
</html>
