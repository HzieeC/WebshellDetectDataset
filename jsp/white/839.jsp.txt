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
 	
    <title>${recept.title}</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="${recept.keywords}">
	<meta http-equiv="description" content="${recept.descript}">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/page.css">
  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
	<div class="clear contain">			
		<ul class="clear rent_flow">
			<li>
				<span class="left num_icon">1</span>
				<p class="left mien_tit">预订车辆</br><font>提前为您预留</font></p>					
			</li>
			<li style="border-left:0px solid red">
				<span class="left num_icon">2</span>
				<p class="left mien_tit">签订合同</br><font>双方共同验车</font></p>
			</li>
			<li style="border-left:0px solid red">
				<span class="left num_icon">3</span>
				<p class="left mien_tit">开心旅途</br><font>一路为您保驾护航</font></p>
			</li>
			<li style="border-left:0px solid red">
				<span class="left num_icon">4</span>
				<p class="left mien_tit">退还车辆</br><font>完成租车使用</font></p>
			</li>
		</ul>
		<div class="clear sales">
			<div class="left cartype_contain">
				<div class="car_type">
					<div class="cartype_list">
						<c:forEach items="${cat.catList}" var="cate" varStatus="status">
							<c:if test="${cate.id==cat_id}">
								<a class="car_type_t car_type_action" href="<%=basePath%>goods?cat_id=${cate.id}&menuId=${menuId}">${cate.name}</a>
							</c:if>
							<c:if test="${cate.id!=cat_id}">
								<a class="car_type_t" href="<%=basePath%>goods?cat_id=${cate.id}&menuId=${menuId}">${cate.name}</a>
							</c:if>
						</c:forEach>
					</div>
					<div class="cartype_list1">
						<label class="left type_til">车&nbsp;&nbsp;型</label>
						<ul class="left car_type_list">
							<c:if test="${type_id==null || type_id==''}">
								<li><a class="curs" href="<%=basePath%>goods?brand_id=${brand_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">全部</a></li>
								<c:forEach items="${type.typeList}" var="type" varStatus="status">
									<c:if test="${fn:indexOf(type_id,type.id)>=0}">
										<li><a class="curs" href="<%=basePath%>goods?type_id=${type.id}&brand_id=${brand_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">${type.name}</a></li>
									</c:if>
									<c:if test="${fn:indexOf(type_id,type.id)<0}">
										<li><a href="<%=basePath%>goods?type_id=${type.id}&brand_id=${brand_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">${type.name}</a></li>
									</c:if>
								</c:forEach>
							</c:if>
							<c:if test="${type_id!=null && type_id!=''}">
								<li><a href="<%=basePath%>goods?brand_id=${brand_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">全部</a></li>
								<c:forEach items="${type.typeList}" var="type" varStatus="status">
									<c:if test="${fn:indexOf(type_id,type.id)>=0}">
										<li><a class="curs" href="<%=basePath%>goods?type_id=${type.id},${type_id}&brand_id=${brand_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">${type.name}</a></li>
									</c:if>
									<c:if test="${fn:indexOf(type_id,type.id)<0}">
										<li><a href="<%=basePath%>goods?type_id=${type.id},${type_id}&brand_id=${brand_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">${type.name}</a></li>
									</c:if>
								</c:forEach>
							</c:if>
						</ul>
					</div>
					<div class="cartype_list1">
						<label class="left type_til">品&nbsp;&nbsp;牌</label>
						<ul class="left car_type_list">
							<c:if test="${brand_id==null || brand_id==''}">
								<li><a class="curs" href="<%=basePath%>goods?type_id=${type_id}&brand_id=&cat_id=${cat_id}&menuId=${menuId}&price=${price}">全部</a></li>
								<c:forEach items="${brand.brandList}" var="brand" varStatus="status">
									<c:if test="${fn:indexOf(brand_id,brand.id)>=0}">
										<li><a class="curs" href="<%=basePath%>goods?brand_id=${brand.id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">&nbsp;${brand.name}</a></li>
									</c:if>
									<c:if test="${fn:indexOf(brand_id,brand.id)<0}">
										<li><a href="<%=basePath%>goods?brand_id=${brand.id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">&nbsp;${brand.name}</a></li>
									</c:if>
								</c:forEach>
							</c:if>
							<c:if test="${brand_id!=null && brand_id!=''}">
								<li><a href="<%=basePath%>goods?brand_id=&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">全部</a></li>
								<c:forEach items="${brand.brandList}" var="brand" varStatus="status">
									<c:if test="${fn:indexOf(brand_id,brand.id)>=0}">
										<li><a class="curs" href="<%=basePath%>goods?brand_id=${brand.id},${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">&nbsp;${brand.name}</a></li>
									</c:if>
									<c:if test="${fn:indexOf(brand_id,brand.id)<0}">
										<li><a href="<%=basePath%>goods?brand_id=${brand.id},${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">&nbsp;${brand.name}</a></li>
									</c:if>
								</c:forEach>
							</c:if>
						</ul>
					</div>
					
					<div class="cartype_list1">
						<label class="left type_til">价&nbsp;&nbsp;格</label>
						<ul class="left car_type_list">
							<c:if test="${price==null || price=='x'}">
								<li><a class="curs" href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=x">全部</a></li>
								<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=0-300">&nbsp;0-300</a></li>
								<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=300-500">&nbsp;300-500</a></li>
								<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=500-1000">&nbsp;500-1000</a></li>
								<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=1000-2000">&nbsp;1000-2000</a></li>
								<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=2000-x">&nbsp;2000以上</a></li>
							</c:if>
							<c:if test="${price!=null && price!='x'}">
								<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=x">全部</a></li>
								<c:if test="${price=='0-300'}">
									<li><a class="curs" href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=0-300">&nbsp;0-300</a></li>
								</c:if>
								<c:if test="${price!='0-300'}">
									<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=0-300">&nbsp;0-300</a></li>
								</c:if>
								<c:if test="${price=='300-500'}">
									<li><a class="curs" href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=300-500">&nbsp;300-500</a></li>
								</c:if>
								<c:if test="${price!='300-500'}">
									<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=300-500">&nbsp;300-500</a></li>
								</c:if>
								<c:if test="${price=='500-1000'}">
									<li><a class="curs" href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=500-1000">&nbsp;500-1000</a></li>
								</c:if>
								<c:if test="${price!='500-1000'}">
									<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=500-1000">&nbsp;500-1000</a></li>
								</c:if>
								<c:if test="${price=='1000-2000'}">
									<li><a class="curs" href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=1000-2000">&nbsp;1000-2000</a></li>
								</c:if>
								<c:if test="${price!='1000-2000'}">
									<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=1000-2000">&nbsp;1000-2000</a></li>
								</c:if>
								<c:if test="${price=='2000-x'}">
									<li><a class="curs" href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=2000-x">&nbsp;2000以上</a></li>
								</c:if>
								<c:if test="${price!='2000-x'}">
									<li><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=2000-x">&nbsp;2000以上</a></li>
								</c:if>
							</c:if>
						</ul>
					</div>
				</div>
				<div class="clear"></div>
				<div class="cartype_lis">
					<c:if test="${order==null || order==''}">
						<div class="cartype_tab_sort"><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">默认排序</a></div>
						<div class="cartype_tab"><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}&order=xs">按租金</a></div>
						<div class="cartype_tab"><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}&order=extension">按销量</a></div>
					</c:if>
					<c:if test="${order=='xs'}">
						<div class="cartype_tab"><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">默认排序</a></div>
						<div class="cartype_tab_sort"><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}&order=xs">按租金</a></div>
						<div class="cartype_tab"><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}&order=extension">按销量</a></div>
					</c:if>
					<c:if test="${order=='extension'}">
						<div class="cartype_tab"><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}">默认排序</a></div>
						<div class="cartype_tab"><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}&order=xs">按租金</a></div>
						<div class="cartype_tab_sort"><a href="<%=basePath%>goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}&order=extension">按销量</a></div>
					</c:if>
					
					<div class="cartype_tab cartype_tab_full"></div>
					<div class="clear"></div>
					<!--循环-->
					<c:forEach items="${goodsList}" var="goods" varStatus="status">
						<div class="clear cartype_lis1">
							<a href="<%=basePath%>front/goodsDetail/${goods.goodsId}.html">
								<img class="left cartype_lis_img" src="<%=basePath%>${goods.goodsThumb}" />
							</a>
							<div class="left car_right_c">
								<a href="<%=basePath%>front?tag=goodsDetail&id=${goods.goodsId}&sp=${sp}">
									<div class="left cartype_lis_name">${goods.goodsName}</div>
								</a>
								<div class="clear"></div>
								<ul class="left cartype_info">
									<li>
										<div>
										<c:forEach items="${goods.attrList}" var="attr" varStatus="status">
											<c:if test="${attr.name=='排量'}">
												${attr.value}/
											</c:if>
											
											<c:if test="${attr.name=='变速箱'}">
												${attr.value}
											</c:if>
											<c:if test="${attr.name=='座位数' || attr.name=='座位'}">
												${attr.value}
											</c:if>
										</c:forEach>
										</br>${goods.goodsType.name}</div>
									</li>
									<c:if test="${sp=='x'}">
										<li class="oncl_mon_wid">
											<div class="oncl_border_1 oncl_mon"><span class="or_blue span_cont">￥<span class="money_big">${goods.shopPrice}</span>/三环内<a class="ren_sjx"></a></span></div>	
											<div class="mon_week">
													<div class="left">三环内：<span class="orange">￥${goods.shopPrice}</span>&nbsp;&nbsp;三环外：<span class="orange">￥${goods.marketPrice}</span></div>
											</div>			
										</li>
									</c:if>
									<c:if test="${sp=='h'}">
										<li class="oncl_mon_wid">
											<div class="oncl_mon_1"><span class="or_blue span_cont">￥<span class="money_big">${goods.shopPrice}</span>/次</span></div>	
										</li>
									</c:if>
									<li class="rig_b">
										<a class="order_btn" href="<%=basePath%>front?tag=goodsDetail&id=${goods.goodsId}&sp=${sp}">预 定</a>
									</li>
								</ul>
							</div>				
						</div>
					</c:forEach>
					<!--循环结束-->
					<div class="clear"></div>
				</div>
				<div class="paginationBox blue">
                    <jsp:useBean id="paging" scope="page" class="com.weishang.bean.Page"/>
					<jsp:setProperty property="user" value="user" name="paging"/>
					<jsp:setProperty property="crrent" value="${pageNo}" name="paging"/>
					<jsp:setProperty property="suffix" value="" name="paging"/>
					<jsp:setProperty property="sumPage" value="${sum}" name="paging"/>
					<jsp:setProperty property="color" value="red" name="paging"/>
					<jsp:setProperty property="url" value="goods?brand_id=${brand_id}&type_id=${type_id}&cat_id=${cat_id}&menuId=${menuId}&price=${price}&order=${order}" name="paging"/>
					${paging.pageString}
                </div>
			</div>
			<!--右侧模块-->
			<jsp:include  page="/template/${folder.tpl.folder}/page/web_right.jsp"/>
		</div>
	</div>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
    <script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
	<script>
	/*列表交互*/
	+(function(){
		$(function(){
			$(".cartype_lis1").find(".oncl_mon_wid").hover(function(){
				//alert();
				_this = $(this);
				_this.find(".oncl_mon").removeClass("oncl_border_1");
				_this.find(".oncl_mon").addClass("oncl_border");
				_this.find(".mon_week").show();
				
			},function(){
				_this.find(".oncl_mon").removeClass("oncl_border");
	            _this.find(".oncl_mon").addClass("oncl_border_1");
				_this.find(".mon_week").hide();
			})
			
		/*
		$(".cartype_lis1").find(".oncl_mon").hover(function(){
				$ = $(this);
				$(".oncl_mon").removeClass("oncl_border_1");
	            $(".oncl_mon").addClass("oncl_border");
				$(".mon_week").css("display","block");
	        },function(){
	            $(".oncl_mon").removeClass("oncl_border");
	            $(".oncl_mon").addClass("oncl_border_1");
				$(".mon_week").css("display","none");
	        });
		*/
			
		});
	})();
	
	</script>
  </body>
</html>