<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE HTML>
<html>
  <head>
    <base href="<%=basePath%>">
    <jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
    <jsp:useBean id="active" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 精彩活动-->
 	<jsp:useBean id="news" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 新闻-->
 	<jsp:useBean id="brand" scope="page" class="com.weishang.my.bean.MyBean"/><!-- 品牌-->
 	<jsp:useBean id="type" scope="page" class="com.weishang.my.bean.MyBean"/><!-- 类型-->
 	<jsp:useBean id="cat" scope="page" class="com.weishang.my.bean.MyBean"/><!-- 分类-->
 	<jsp:useBean id="promt" scope="page" class="com.weishang.my.bean.MyBean"/><!--活动-->
 	 	
    <title>${market.title}</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="${goods.goodsName}">
	<meta http-equiv="description" content="${goods.goodsName}">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
	<div class="clear sales">
		<div class="left order_car_contain">
			<div class="order_car_infobox">
				<div class="left order_car_til">基本信息</div>
				<div class="left order_car_full"></div>
				<div class="clear"></div>
				<div class="order_car_info">
					<div class="left order_car_img"><img src="<%=basePath%>${market.img}" /></div>
					<div class="left car_info_cs" style="height:210px;">
						<div class="b_name car_info_til">${market.title}</div>
						
						
					</div>
					<div class="clear"></div>
				</div>
				<div class="add_renter">选择租车人信息</div>
				<div class="clear"></div>
				<div class="address_list">
					<ul id="address_list">
						<c:forEach items="${addressList}" var="address" varStatus="status">
							<li><input name="tem_address_id" type="radio" value="${address.id}">${address.name}-联系电话：${address.tel}-身份证号码：${address.address}</li>
						</c:forEach>
					</ul>
				</div>
				<div class="clear"></div>
				<div class="add_renter" onclick="addAddress()">添加租车人信息</div>
				<div class="clear"></div>
				<div class="add_renterinfo">
					<form action="javascript:void(0)" name="addressForm" id="addressForm" method="post">
						姓&nbsp;&nbsp;&nbsp;名：<input name="name" type="text" class="renter_name"/>
						身份证号码：<input type="text" name="address" class="idcard"/>
						联系电话：<input type="text" name="tel"  class="renter_type"/>
					</form>
				</div>
				<div class="clear"></div>
			</div>
			
			<form action="javascript:void(0)" name="orderForm" id="orderForm" method="post">
				<input name="address_id" id="address_id" type="hidden" value=""/>
				<c:forEach items="${market.goodsList}" var="goods" varStatus="status">
					<input name="goods_id" type="hidden" value="${goods.goodsId}"/>
				</c:forEach>
				<input type="hidden" name="content" id="content" value=""/>
				
				<div class="order_car_infobox " style="margin-top:20px;">
					<div class="left order_car_til">费用明细</div>
					<div class="left order_car_full"></div>
					<div class="clear"></div>
					<div class="order_car_infos">
						<div class="order_car_moninfo" style="line-height:35px;">车辆租赁费<span class="mx_zhus">（注：该费用只包含车辆租赁费和基本保险费用）</span></div>
						<div class="order_car_moninfo"><input value="配司机" name="add_items" id="peishiji" type="checkbox" />配司机<span class="mx_zhus">（注：司机每日工作8小时，超出按30元/小时收费，行程结束后补差价）</span></div>
						<div  class="order_car_moninfo">
							<input type="checkbox" value="送车收车" id="songchengjieche" name="add_items"/>送车收车<span class="mx_zhus">（注：成都市三环内免费送车收车，三环外绕城内80元/1次）</span>
						</div>
						<div class="order_car_moninfo"><input name="add_items" id="xuyaofapiao" value="需要发票" type="checkbox" name="add_items"/>需要发票<span class="mx_zhus">（注：开发票加收7.6%税金，费用结算后为您寄出）</span></div>
						<div class="order_car_moninfo">备注信息</div>
						<div class="order_car_moninfo"><textarea name="ext_text" id="ext_text" class="remark_area"></textarea></div>
					</div>
					<div class="order_sub">
						<div>应付金额：<span>￥<font id="shiji_order">${market.price}</font></span></div>
						<input type="hidden" name="price" value="${market.price}"/>
						<input type="button" value="提交订单" onclick="javascript:submit_order()" class="right order_sub_btn"/>
						<div class="clear"></div>
					</div>
					<div class="clear"></div>
				</div>
			</form>
		</div>
		
		<!--右侧模块-->
			<jsp:include  page="/template/${folder.tpl.folder}/page/web_right.jsp"/>
	</div>
</div>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
    <script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
	<script type="text/javascript">
		function addAddress(){
			var params= $('#addressForm').serialize();
			$.ajax({
				url:"<%=basePath%>front?tag=addAddress", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					if(data.status==200){
						$("#address_list").append("<li><input name=tem_address_id type=radio value="+data.addressId+">"+data.name+"-联系电话："+data.tel+"-身份证号码："+data.address+"</li>");
						$("#addressForm")[0].reset();
					}else{
						alert(data.tip);
					}
				}
			}); 
		}
		//选择租车人信息
		(function(){
			var $address_radio = $('#address_list').find('input[type=radio]');
			var $addressID =  $('#address_id');
		    $addressID.val($('#address_list').find('input[type=radio]:checked').val());
			$address_radio.click(function(){
				$addressID.val($(this).val())
			})
		})()
		
		//加师机、接送车、
		
		function submit_order(){
			var address_id=$("#address_id").val();
			var my_accunt=${ordinary_user.integel};//我的积分
			//var goods_integral=${goods.integral};//本商品最多可以用多少积分兑换
			var integel=${integel};//多少积分相当于一元
			var crrent_integel=$("#crrent_integel").val();//当前用户填写的积分
			var Regex =/^[0-9]*[1-9][0-9]*$/;//验证是否是整数
			if(crrent_integel!=null && crrent_integel!="" ){
				if(!Regex.test(crrent_integel)){
					alert("使用积分必须是整数");
				}else if(crrent_integel>my_accunt){
					alert("您填写的积分已经大于自己的积分,请重填");
				}else if(crrent_integel%integel!=0){
					alert("您填写的积分必须是“"+integel+"”的倍数,请重填");
				}
			}else if(address_id==null || address_id==""){
				alert("地址没有选择");
			}else{
				$("#content").val($("#content").val().substring(0,$("#content").val().length-1));
				$("#orderForm").attr("action","<%=basePath%>payPage");
				$("#orderForm").submit();
			}
		}
		
		$("#peishiji").click(function(){
			if($("#peishiji").is(":checked")){
				$("#content").val($("#content").val()+"配师机+");
			}else{
				$("#content").val($("#content").val().replace("配师机+",""));
			}
		});
		
		$("#songchengjieche").click(function(){
			if($("#songchengjieche").is(":checked")){
				$("#content").val($("#content").val()+"送车接车+");
			}else{
				$("#content").val($("#content").val().replace("送车接车+",""));
			}
		});
		
		$("#xuyaofapiao").click(function(){
			if($("#xuyaofapiao").is(":checked")){
				$("#content").val($("#content").val()+"需要发票+");
			}else{
				$("#content").val($("#content").val().replace("需要发票+",""));
			}
		});
		
		$("#ext_text").change(function(){
			$("#content").val($("#content").val()+$("#ext_text").val()+"+");
		});
		
		
		
		
	</script>
  </body>
</html>