<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
 <jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
<!DOCTYPE HTML>
<html>
  <head>
    <base href="<%=basePath%>">
    <link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
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
		    background:#f9f9f9;
		    min-height:100%;
		}
		.cont_pay{width:800px;margin:50px auto; border:3px solid #ccc;height:300px;background:#fff;}
		.title_s{width:100%;height:50px; line-height:50px;border-bottom:1px solid #ccc;}
		.title_s span{margin-left:20px;font-size:20px;}
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
		    margin:15px 0 0 12px;
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
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
  </head>
  	<body>
  		<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
	   		<form action="<%=basePath%>pay" method="POST">
	   			<input name="billNo" value="${billNo}" type="hidden"/>
	   			<input name="goodsId" value="${goods.goodsId}" type="hidden"/>
	   			<input name="goodsSn" value="${goods.goodsSn}" type="hidden"/>
	   			<input name="goodsName" value="${goods.goodsName}" type="hidden"/>
	   			<input name="order_id" value="${order_id}" type="hidden"/>
	   			<input name="play_yh" value="${play_yh}" type="hidden"/>
	   			<input name="price" value="${price}" type="hidden"/>
	   			
			    <div class="cont_pay">
			    	<div class="title_s"><span>选择支付方式</span></div>
			        <ul class="clear" style="margin-top:20px">
			            <li class="clicked" onclick="paySwitch(this)">
			                <input type="radio" value="ALI_WEB" name="paytype" checked="checked">
			                <img src="http://beeclouddoc.qiniudn.com/ali.png" alt="">
			            </li>
			            <li onclick="paySwitch(this)">
			                <input type="radio" value="WX_NATIVE" name="paytype">
			                <img src="http://beeclouddoc.qiniudn.com/wechats.png" alt="">
			            </li>
			        </ul>
			        <div class="clear"></div>
			        <input type="submit" class="button" value="确认付款">
			    </div>
			</form>
		<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
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
