<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE HTML>
<html>
  <head>
    <base href="<%=basePath%>">
    <jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
 	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
 	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/qrcode.js"></script>
    <title>支付确认页面</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
  </head>
  
  <body>
  <style>
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
		    background:#f9f9f9;
		    min-height:100%;
		}
		.cont_pay{width:800px;margin:50px auto; border:3px solid #ccc;height:300px;background:#fff;}
		.cont_pay p{ line-height:30px;font-size:20px;margin:20px 0 0;width:100%;}
		.cont_pay img{width:200px;height:200px;}
	</style>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
  		<div class="cont_pay">
	  		<p>${html}</p>
			<div align="center" id="qrcode">
				
			</div>
		</div>
		<script type="text/javascript">
			function wx_native(){
				var order_id="${order_id}";
				var billNo="${billNo}";
				var goods_id="${goods_id}";
				$.ajax({
					url:"<%=basePath%>pay/wxPolling", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:{billNo:billNo,order_id:order_id,goods_id:goods_id},//要传递的数据
					success:function(data){
						if(data.status==200){
							window.location.href="<%=basePath%>pay/payTip";
						}
					}
				}); 
		    }
		    
		    function makeqrcode() {
		        var qr = qrcode(10, 'L');
		        qr.addData(codeUrl);
		        qr.make();
		        var wording = document.createElement('p');
		        wording.innerHTML = "扫我，扫我";
		        var code = document.createElement('DIV');
		        code.innerHTML = qr.createImgTag();
		        var element = document.getElementById("qrcode");
		        element.appendChild(wording);
		        element.appendChild(code);
		    }
		    var type = '${type}';
		    var codeUrl;
		    var success = '${success}'
		    if (type == 'WX_NATIVE') {
		        codeUrl = '${codeUrl}';
		    }
		
		    if (type == 'WX_NATIVE' || 'true' == success) {
		        makeqrcode();
		        setInterval('wx_native()',1000);
		    }
		   
		</script>
		
		<script type="text/javascript">
		    callpay();
		    function jsApiCall() {
		        var data = {
		            //以下参数的值由BCPayByChannel方法返回来的数据填入即可
		            "appId": "${jsapiAppid}",
		            "timeStamp": "${timeStamp}",
		            "nonceStr": "${nonceStr}",
		            "package": "${jsapipackage}",
		            "signType": "${signType}",
		            "paySign": "${paySign}"
		        };
		        alert(JSON.stringify(data));
		        WeixinJSBridge.invoke(
		                'getBrandWCPayRequest',
		                data,
		                function (res) {
		                    alert(res.err_msg);
		                    alert(JSON.stringify(res));
		                    WeixinJSBridge.log(res.err_msg);
		                }
		        );
		    }
		
		    function callpay() {
		        if (typeof WeixinJSBridge == "undefined") {
		            if (document.addEventListener) {
		                document.addEventListener('WeixinJSBridgeReady', jsApiCall, false);
		            } else if (document.attachEvent) {
		                document.attachEvent('WeixinJSBridgeReady', jsApiCall);
		                document.attachEvent('onWeixinJSBridgeReady', jsApiCall);
		            }
		        } else {
		            jsApiCall();
		        }
		    }
		</script>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
    
	
  </body>
</html>