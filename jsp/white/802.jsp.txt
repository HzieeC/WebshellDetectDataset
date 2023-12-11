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
 	 	
    <title>${goods.goodsName}</title>
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
					<div class="left order_car_img"><img src="<%=basePath%>${goods.goodsThumb}" /></div>
					<div class="left car_info_cs">
						<div class="b_name car_info_til">${goods.goodsName}</div>
						<div class="car_info_cs_li">
							<ul>
								<c:forEach items="${goods.attrList}" var="attr" varStatus="status">
									<c:if test="${fn:length(attr.value)>10}">
										<li><span class="b_name">${attr.name}</span>${attr.value}</li>
									</c:if>
									<c:if test="${fn:length(attr.value)<10}">
										<li class="lang_box"><span class="b_name">${attr.name}</span>${attr.value}</li>
									</c:if>
								</c:forEach>
							</ul>
						</div>
					</div>
					<div class="clear"></div>
				</div>
				<div class="add_renter_tit"><span>选择租车人信息</span></div>
				<div class="clear"></div>
				<div class="address_list">
					<ul id="address_list">
						<c:forEach items="${addressList}" var="address" varStatus="status">
							<li><input name="tem_address_id" type="radio" value="${address.id}">${address.name}-联系电话：${address.tel}-身份证号码：${address.address}</li>
						</c:forEach>
					</ul>
				</div>
				<div class="clear"></div>
				
				<div class="clear"></div>
				<div class="add_renterinfo">
					<div class="left">
					<form action="javascript:void(0)" name="addressForm" id="addressForm" method="post">
						姓名：<input name="name" type="text" class="renter_name"/>
						身份证号码：<input type="text" name="address" class="idcard"/>
						联系电话：<input type="text" name="tel"  class="renter_type"/>
					</form>
					</div>
					<div class="add_renter_sub" onclick="addAddress()">添加租车人信息</div>
				</div>
				<div class="clear"></div>
			</div>
			
			<form action="javascript:void(0)" name="orderForm" id="orderForm" method="post">
				<input name="address_id" id="address_id" type="hidden" value=""/>
				<input name="goods_id" id="goods_id" type="hidden" value="${goods.goodsId}"/>
				
				<input type="hidden" name="content" id="content" value=""/>
				<input type="hidden" name="desc" id="desc" value=""/>
				
				<c:if test="${special!=null && special=='x'}">
					<div class="order_car_infobox " style="margin-top:20px;">
						<div class="left order_car_til">接送机</div>
						<div class="left order_car_full"></div>
						<div class="clear"></div>
						<div class="order_car_info order_car_inplay">
							<input name="inner_price" id="inner_price" type="hidden" value="${goods.shopPrice}">
							<input name="outer_price" id="outer_price" type="hidden" value="${goods.marketPrice}">
							<div class="clear play_radio">
								<select name="tem_desc" id="tem_desc" onchange="getOuterOrInner()">
									<option value="">--接送机请选择--</option>
									<option value="1">三环内</option>
									<option value="2">三环外</option>
								</select>
								只有接送机的项目可以选择
							</div>
						</div>
					</div>
				</c:if>
				<div class="order_car_infobox " style="margin-top:20px;">
					<div class="left order_car_til">优惠活动</div>
					<div class="left order_car_full"></div>
					<div class="clear"></div>
					<div class="order_car_info order_car_inplay">
						<c:forEach items="${couponList}" var="coupon" varStatus="status">
							<div class="left play_yh_btn play_yh_action">${coupon.title}（${coupon.price}）</div>
						</c:forEach>
						<div class="clear play_radio">
							使用 <input name="play_yh" class="renter_name" id="crrent_integel" value=""> 积分（当前积分：<font color="red">${ordinary_user.integel}分</font>，此商品可以使用${goods.integral}）${integel}个积分相当于1元钱<br />
						</div>
					</div>
				</div>
				
				<div class="order_car_infobox " style="margin-top:20px;">
					<div class="left order_car_til">费用明细</div>
					<div class="left order_car_full"></div>
					<div class="clear"></div>
					<!-- 商务用车和旅游用车 -->
					<c:if test="${special==null || special==''}">
						<div class="order_car_infos">
							<div class="order_car_moninfo" style="line-height:35px;">车辆租赁费<span class="mx_zhus">（注：该费用只包含车辆租赁费和基本保险费用）</span></div>
							<div class="order_car_moninfo"><input value="配司机" name="add_items" id="peishiji" type="checkbox" />配司机<span class="mx_zhus">（注：司机每日工作8小时，超出按30元/小时收费，行程结束后补差价）</span></div>
							<div  class="order_car_moninfo">
								<input type="checkbox" value="送车收车" id="songchengjieche" name="add_items"/>送车收车<span class="mx_zhus">（注：成都市三环内免费送车收车，三环外绕城内80元/1次）</span>
							</div>
							<div class="order_car_moninfo"><input name="add_items" id="xuyaofapiao" value="需要发票" type="checkbox" name="add_items"/>需要发票<span class="mx_zhus">（注：开发票加收17%税金，费用结算后为您寄出）</span></div>
							<div class="order_car_moninfo">备注信息</div>
							<div class="order_car_moninfo"><textarea name="ext_text" id="ext_text" class="remark_area"></textarea></div>
						</div>
					</c:if>
					<!-- 接机 -->
					<c:if test="${special!=null && special=='x'}">
						<div class="order_car_infos">
							<div class="order_car_moninfo" style="line-height:35px;">车辆租赁费<span class="mx_zhus">（注：包含车辆租赁使用费、司机服务费、油费、机场高速过路费，免费超时等待30分钟，若超出等待时间收费80元/小时，行程结束后收取）</span></div>
							<div class="order_car_moninfo"><input value="夜间服务" name="add_items" id="yejian" type="checkbox" />夜间服务<span class="mx_zhus">（注：夜间服务时间为21:00-8:00，收取费用为100元，行程结束前收取）</span></div>
							<div class="order_car_moninfo"><input name="add_items" id="xuyaofapiao" value="需要发票" type="checkbox" name="add_items"/>需要发票<span class="mx_zhus">（注：开发票加收17%税金，费用结算后为您寄出）</span></div>
							<div class="order_car_moninfo">备注信息</div>
							<div class="order_car_moninfo"><textarea name="ext_text" id="ext_text" class="remark_area"></textarea></div>
						</div>
					</c:if>
					
					<c:if test="${special!=null && special=='h'}">
						<div class="order_car_infos">
							<div class="order_car_moninfo" style="line-height:35px;">车辆租赁费<span class="mx_zhus">（注：包含成都市区三环内婚庆车辆租赁（5小时内）使用费、司机服务费、油费，超出用车时间费用另算）</span></div>
							<div class="order_car_moninfo"><input name="add_items" id="xuyaofapiao" value="需要发票" type="checkbox" name="add_items"/>需要发票<span class="mx_zhus">（注：开发票加收17%税金，费用结算后为您寄出）</span></div>
							<div class="order_car_moninfo">备注信息</div>
							<div class="order_car_moninfo"><textarea name="ext_text" id="ext_text" class="remark_area"></textarea></div>
						</div>
					</c:if>
					
					<div class="order_sub">
						<div>
							<c:if test="${special=='h'}">
								预付金额：
							</c:if>
							<c:if test="${special!='h'}">
								应付金额：
							</c:if>
							<span>￥<span class="shiji_order" id="shiji_order">${goods.shopPrice}</span></span>
						</div>
						
						<input type="hidden" name="price" id="price" value="${goods.shopPrice}"/>
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
			var goods_integral=${goods.integral};//本商品最多可以用多少积分兑换
			var integel=${integel};//多少积分相当于一元
			var crrent_integel=$("#crrent_integel").val();//当前用户填写的积分
			var Regex =/^[0-9]*[1-9][0-9]*$/;//验证是否是整数
			if(crrent_integel!="" && crrent_integel!=null){
				if(!Regex.test(crrent_integel)){
					alert("使用积分必须是整数");
				}else if(crrent_integel>my_accunt){
					alert("您填写的积分已经大于自己的积分,请重填");
				}else if(crrent_integel>goods_integral){
					alert("您填写的积分已经大于当前商品要求使用的最高积分,请重填");
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
		
		
		$("#yejian").click(function(){
			if($("#yejian").is(":checked")){
				$("#content").val($("#content").val()+"夜间服务+");
			}else{
				$("#content").val($("#content").val().replace("夜间服务+",""));
			}
		});
		
		$("#ext_text").change(function(){
			$("#content").val($("#content").val()+$("#ext_text").val()+"+");
		});
		
		function getOuterOrInner(){
			var tem_desc=$("#tem_desc").val();//获取地址或者距离
			if(tem_desc=='1'){//三环内
				$("#desc").val("三环内接送机");
				$("#price").val($("#inner_price").val());
				$("#shiji_order").html($("#inner_price").val());
			}else if(tem_desc=='2'){//三环外
				$("#desc").val("三环外接送机");
				$("#price").val($("#outer_price").val());
				$("#shiji_order").html($("#outer_price").val());
			}
		}
		
		
	</script>
  </body>
</html>