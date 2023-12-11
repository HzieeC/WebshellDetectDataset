<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>  
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>å¾çåè¡¨æ»å¨jqueryéé¡¹å¡ä»£ç  - ç«é¿ç´ æ</title>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<link rel="stylesheet" type="text/css" href="./css/style3.css">


<script type="text/javascript" src="./js/jquery.js"></script>
<script type="text/javascript">
$(function(){
	$(".yScrollListTitle h1").click(function  () {
		var index=$(this).index(".yScrollListTitle h1");
		$(this).addClass("yth1click").siblings().removeClass("yth1click");
		$($(".yScrollListInList")[index]).show().siblings().hide();
	})
	$(".yScrollListInList1 ul").css({width:$(".yScrollListInList1 ul li").length*(160+84)+"px"});
	$(".yScrollListInList2 ul").css({width:$(".yScrollListInList2 ul li").length*(160+84)+"px"});
	var numwidth=(160+84)*5;
	$(".yScrollListInList .yScrollListbtnl").click(function(){
		var obj=$(this).parent(".yScrollListInList").find("ul");
		if (!(obj.is(":animated"))) {
			var lefts=parseInt(obj.css("left").slice(0,-2));
			if(lefts<30){
				obj.animate({left:lefts+numwidth},1000);
			}
		}
	})
	$(".yScrollListInList .yScrollListbtnr").click(function(){
		var obj=$(this).parent(".yScrollListInList").find("ul");
		var objcds=-(30+(Math.ceil(obj.find("li").length/5)-2)*numwidth);
		if (!(obj.is(":animated"))) {
			var lefts=parseInt(obj.css("left").slice(0,-2));
			if(lefts>objcds){
				obj.animate({left:lefts-numwidth},1000);
			}
		}
	})
})
</script>

</head>
<body>

<div class="yScrollList">
	<div class="yScrollListTitle"><h1 class="yth1click">推荐产品</h1><!--  <h1 class="ytitleh12">最新产品</h1>--></div>
	<div class="yScrollListIn">
		<div class="yScrollListInList yScrollListInList1" style="display:block;">
		    
		
			<ul>
			   <s:iterator value="pageBean.list8">         
				<li>
				<table>
				<tr><td align="center"><s:property value="id"/></td></tr>
				<tr><td>   <s:set name="pic" value="pic" />
            <s:if test="#pic==''">    
             <img src="./images/nopic_small.jpg" width="150" height="150" border="0"/>
        </s:if>
         <s:else>     <a href="<s:property value="pic"/>"><img src="<s:property value="pic"/>" align="middle"/> </a> </s:else>
				</td></tr>
				<tr><td align="center"><a href="showProduct!get?id=<s:property value="id"/>"><s:property value="productname"/></a>
				</td></tr>
				</table>
				</li>
				   </s:iterator>
			</ul>
			<div class="yScrollListbtn yScrollListbtnl"></div>
			<div class="yScrollListbtn yScrollListbtnr"></div>
		</div>
		<!--  <div class="yScrollListInList yScrollListInList2">
			<ul>
			  <s:iterator value="pageBean.list2">         
				<li>
				      
						    <a href="<s:property value="pic"/>">  <img src="<s:property value="pic"/>" width="50" height="50"/> </a>
						<p align="center"><a href="showProduct!get?id=<s:property value="id"/>"><s:property value="productname"/></a></p>
					</a>
				</li>
				   </s:iterator>
			</ul>
			<div class="yScrollListbtn yScrollListbtnl"></div>
			<div class="yScrollListbtn yScrollListbtnr"></div>
		</div>-->
	</div>
</div>

</body>
</html>