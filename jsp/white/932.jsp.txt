<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!doctype html>
<html>
<head>
	<link href="../css/demo.css" rel="stylesheet" type="text/css"/>
    <meta charset="utf-8">
    <title>支付页面</title>
    <style>
    	.clear:after {
    content: ".";
    display: block;
    clear: both;
    visibility: hidden;
    line-height: 0;
    height: 0;
}

html {
    width: 100%;
}

body {
    margin: 0;
    padding: 0;
    width: 100%;
    color: #111;
    font-family: "PingHei", STHeitiSC-Light, "Lucida Grande",
    "Lucida Sans Unicode", Helvetica, Arial, Verdana, sans-serif;
    font-size: 1em;
}

ul {
    list-style: none;
    padding: 0;
    margin: 0;
    width: 100%;
}

ul li {
    float: left;
    margin: 0 1em;
}

ul li img {
    cursor: pointer;
    width: 158px;
    border: rgba(0, 0, 0, 0.2) 2px solid;
}

ul li img:hover {
    box-shadow: 0 0 2px #0CA6FC;
    border: #0CA6FC 2px solid;
}

.button {
    cursor: pointer;
    display: block;
    line-height: 45px;
    text-align: center;
    width: 158px;
    height: 45px;
    margin-top: 1.5em;
    border: rgba(123, 170, 247, 1) 1px solid;
    color: #fff;
    font-size: 1.2em;
    border-top-color: #1992da;
    border-left-color: #0c75bb;
    border-right-color: #0c75bb;
    border-bottom-color: #00589c;
    -webkit-box-shadow: inset 0 1px 1px 0 #6fc5f5;
    -moz-box-shadow: inset 0 1px 1px 0 #6fc5f5;
    box-shadow: inset 0 1px 1px 0 #6fc5f5;
    background: #117ed2;
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#37aaea",
    endColorstr="#117ed2");
    background: -webkit-gradient(linear, left top, left bottom, from(#37aaea),
    to(#117ed2));
    background: -moz-linear-gradient(top, #37aaea, #117ed2);
    background-image: -o-linear-gradient(top, #37aaea 0, #117ed2 100%);
    background-image: linear-gradient(to bottom, #37aaea 0, #117ed2 100%);
}

.button:hover {
    background: #1c5bad;
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#2488d4",
    endColorstr="#1c5bad");
    background: -webkit-gradient(linear, left top, left bottom, from(#2488d4),
    to(#1c5bad));
    background: -moz-linear-gradient(top, #2488d4, #1c5bad);
    background-image: -o-linear-gradient(top, #2488d4 0, #1c5bad 100%);
    background-image: linear-gradient(to bottom, #2488d4 0, #1c5bad 100%);
    -webkit-box-shadow: inset 0 1px 1px 0 #64bef1;
    -moz-box-shadow: inset 0 1px 1px 0 #64bef1;
    box-shadow: inset 0 1px 1px 0 #64bef1;
}

li.clicked img {
    box-shadow: 0 0 2px #0CA6FC;
    border: #0CA6FC 2px solid;
}

input {
    display: none;
}
    </style>

</head>
<body>
<div>
    <h2>应付总额： ¥0.01</h2>

    <p>请选择支付方式</p>
</div>
<div>
    支付平台
</div>
<form action="<%=basePath%>pay" method="POST" target="_blank">
    <div>
        <ul class="clear" style="margin-top:20px">
            <li class="clicked" onclick="paySwitch(this)">
                <input type="radio" value="ALI_WEB" name="paytype" checked="checked">
                <img src="http://beeclouddoc.qiniudn.com/ali.png" alt="">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="WX_NATIVE" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/wechats.png" alt="">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="WX_JSAPI" name="paytype">
                <img src="http://7xavqo.com1.z0.glb.clouddn.com/wechatgzh.png" alt="">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="UN_WEB" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/unionpay.png" alt="">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="ALI_QRCODE" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/alis.png" alt="">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="ALI_WAP" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/aliwap.png" alt="">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="YEE_WAP" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/ybwap.png" alt="YEE WAP">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="YEE_WEB" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/yb.png" alt="YEE WEB">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="YEE_NOBANKCARD" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/ybcard.png" alt="YEE WEB">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="JD_WAP" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/jdwap.png" alt="JD　WAP">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="JD_WEB" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/jd.png" alt="JD　WEB">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="KUAIQIAN_WAP" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/kqwap.png" alt="KUAIQIAN WAP">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="KUAIQIAN_WEB" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/kq.png" alt="KUAIQIAN WEB">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="BD_WEB" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/bd.png" alt="KUAIQIAN WEB">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="BD_WAP" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/bdwap.png" alt="KUAIQIAN WEB">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="PAYPAL_PAYPAL" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/paypal.png" alt="PAYPAL PAYPAL">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="PAYPAL_CREDITCARD" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/icon_paypal_credit.png" alt="PAYPAL CREDITCARD WEB">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="PAYPAL_SAVED_CREDITCARD" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/icon_paypal_kuaijiezhifu.png" alt="PAYPAL SAVED CREDITCARD">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="BC_GATEWAY" name="paytype">
                <img src="http://7xavqo.com1.z0.glb.clouddn.com/icon_gateway.png" alt="BC GATEWAY">
            </li>
            <li onclick="paySwitch(this)">
                <input type="radio" value="BC_EXPRESS" name="paytype">
                <img src="http://beeclouddoc.qiniudn.com/icon_BcExpress.png" alt="BC EXPRESS">
            </li>
        </ul>
    </div>
    <div style="clear: both;">
        <input type="submit" class="button" value="确认付款">
    </div>
</form>
<hr/>
</body>
<script type="text/javascript">
    function paySwitch(that) {
        var li = that.parentNode.children;
        for (var i = 0; i < li.length; i++) {
            li[i].className = "";
            li[i].childNodes[1].removeAttribute("checked");
        }
        console.log(li);
        that.className = "clicked";
        that.childNodes[1].setAttribute("checked", "checked");
    }
    function querySwitch(that) {
        var li = that.parentNode.children;
        for (var i = 0; i < li.length; i++) {
            li[i].className = "";
            li[i].childNodes[1].removeAttribute("checked");
        }
        console.log(li);
        that.className = "clicked";
        that.childNodes[1].setAttribute("checked", "checked");
    }
</script>
</html>