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
 	<jsp:useBean id="slide" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 幻灯片 -->
 	<jsp:useBean id="active" scope="page" class="com.weishang.my.bean.MyBean"/><!-- 精彩活动-->
 	<jsp:useBean id="news" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 精彩活动-->
 	
 	<jsp:useBean id="bussinescar" scope="page" class="com.weishang.my.bean.MyBean"/><!--商务用车 -->
 	<jsp:useBean id="marrycar" scope="page" class="com.weishang.my.bean.MyBean"/><!--婚礼用车 -->
 	<jsp:useBean id="travelcar" scope="page" class="com.weishang.my.bean.MyBean"/><!--旅游用车 -->
 	<jsp:useBean id="planecar" scope="page" class="com.weishang.my.bean.MyBean"/><!--接送机用车 -->
 	
 	<jsp:useBean id="auntList" scope="page" class="com.weishang.my.bean.MyBean"/><!--师机 -->
 	
    <title>${recept.title}</title>
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="${recept.keywords}">
	<meta http-equiv="description" content="${recept.descript}">
	<link rel="stylesheet" type="text/css" href="<%=basePath%>template/${folder.tpl.folder}/css/index.css">
  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head.jsp"/>
  	<div class="header_banner">
			<div class="header_banner_box">
			  <div class="bd">
					<ul class="header_banner_list">
						<c:forEach items="${slide.slideList}" var="slide" varStatus="status">
							<li>
								<a href="${slide.url}" style="background: url(<%=basePath%>${slide.img}) no-repeat center top;"></a>
							</li>
						</c:forEach>
					</ul>
				</div>
			  <div class="hd"><ul></ul></div>
			</div>
			<form action="<%=basePath%>front?tag=goFastOrder" method="post" name="fast_form" id="fast_form">
				<div class="header_search text-center">
				
					<input type="hidden" name="store_id" id="store_id" value=""/>
					<input type="hidden" name="type_id" id="type_id" value=""/>
					<ul>
						<li class="left search_info">
							<div class="left down_list1">选择门店<!--<span></span>--></div> 
							<div class="left down_list2" onClick="showList()">选择服务类型</div>
							<input type="text" name="time" id="time" class="left down_list3" onClick="WdatePicker()" placeholder="选择时间">
							<ul class="list_store">
								<jsp:useBean id="storeList" scope="page" class="com.weishang.my.bean.MyBean"/><!--商务用车 -->
								<c:forEach items="${storeList.storeList}" var="store" varStatus="status">
									<li var="${store.id}">${store.name}</li>
								</c:forEach>
							</ul>
							<ul class="list_type">
								<jsp:useBean id="catList" scope="page" class="com.weishang.my.bean.MyBean"/><!--商务用车 -->
								<c:forEach items="${catList.catList}" var="cat" varStatus="status">
									<li var="${cat.id}">${cat.name}</li>
								</c:forEach>
							</ul>
						</li>
						
						<buttun class="left btn_search text-center" id="search_submit" onclick="form_smt()">快速订车</buttun>
					</ul>
				</div>
			</form>
		</div>
		<ul class="header_discribe">
			<li>
				<img class="left" src="<%=basePath%>template/${folder.tpl.folder}/images/discribe1.jpg" />
				<a href="<%=basePath%>goods?cat_id=17&menuId=7" class="left header_discribe_info">
					<h3>接送机场</h3>
					<p>无需任何手续，方便快捷提前到达指定地点等候</p>
				</a>
			</li>
			<li>
				<img class="left" src="<%=basePath%>template/${folder.tpl.folder}/images/discribe2.jpg" />
				<a href="<%=basePath%>service/8.html" class="left header_discribe_info">
					<h3>企业服务</h3>
					<p>承接各种大型会议用车专业商务接待团队</p>
				</a>
			</li>
			<li>
				<img class="left" src="<%=basePath%>template/${folder.tpl.folder}/images/discribe3.jpg" />
				<a href="<%=basePath%>goods?cat_id=15&menuId=7" class="left header_discribe_info">
					<h3>婚期豪车</h3>
					<p>多款豪华跑车供您选择高中低档搭配套餐</p>
				</a>
			</li>
			<li>
				<img class="left" src="<%=basePath%>template/${folder.tpl.folder}/images/discribe4.jpg" />
				<a href="<%=basePath%>goods?cat_id=16&menuId=7" class="left header_discribe_info">
					<h3>旅游越野</h3>
					<p>为您量身打造私人线路领略不同旅途风景</p>
				</a>
			</li>
		</ul>
	</div>
	<div class="clear contain">
		<div class="contain_models" style="height:700px;">
			<div class="contain_models_bg">
				<label class="left">热租车型</br><em>HOT RENT CAR</em></label>
				<ul class="contain_models_nav right">
                    <li><a href="javascript:void(0)" class="contain_models_action">商务用车</a></li>
					<li><a href="javascript:void(0)">婚庆用车</a></li>
					<li><a href="javascript:void(0)">旅游用车</a></li>
					<li><a href="javascript:void(0)">接机送机</a></li>
				</ul>
				<div class="contain_models_leftbg"></div>
				<div class="contain_models_rightbg"></div>
			</div>
			<div class="parBd">
				<div class="models_slidBox">
					<a href="javascript:void(0)" class="prev"></a>
					<div class="slide">
						<ul class="contain_models_list">
							<jsp:setProperty property="pageNo" value="1" name="bussinescar"/>
							<jsp:setProperty property="pageSize" value="6" name="bussinescar"/>
							<jsp:setProperty property="cat_id" value="1" name="bussinescar"/>
							<c:forEach items="${bussinescar.goodsListByCat}" var="goods" varStatus="status">
								<li>
									<a href="<%=basePath%>front/goodsDetail/${goods.goodsId}.html">
				                        <div class="p1z">${goods.shopPrice}${goods.category.measure_unit}</div>
				                        <div class="img_zs"><img src="<%=basePath%>${goods.originalImg}" /></div>
				                        <div class="em_z">${goods.goodsName}</div>
			                        </a>
		                        </li>
	                        </c:forEach>
						</ul>
					</div>
					<a href="javascript:void(0)" class="next"></a>
				</div>
				<div class="models_slidBox">
					<a href="javascript:void(0)" class="prev"></a>
					<div class="slide">
						<ul class="contain_models_list">
	                        <jsp:setProperty property="pageNo" value="1" name="marrycar"/>
							<jsp:setProperty property="pageSize" value="6" name="marrycar"/>
							<jsp:setProperty property="cat_id" value="15" name="marrycar"/>
							<c:forEach items="${marrycar.goodsListByCat}" var="goods" varStatus="status">
								<li>
									<a href="<%=basePath%>front/goodsDetail/${goods.goodsId}.html">
				                        <div class="p1z">${goods.shopPrice}${goods.category.measure_unit}</div>
				                        <div class="img_zs"><img src="<%=basePath%>${goods.originalImg}" /></div>
				                        <div class="em_z">${goods.goodsName}</div>
			                        </a>
		                        </li>
	                        </c:forEach>
						</ul>
					</div>
					<a href="javascript:void(0)" class="next"></a>
				</div>
				<div class="models_slidBox">
					<a href="javascript:void(0)" class="prev"></a>
					<div class="slide">
						<ul class="contain_models_list">
							<jsp:setProperty property="pageNo" value="1" name="travelcar"/>
							<jsp:setProperty property="pageSize" value="6" name="travelcar"/>
							<jsp:setProperty property="cat_id" value="16" name="travelcar"/>
							<c:forEach items="${travelcar.goodsListByCat}" var="goods" varStatus="status">
								<li>
									<a href="<%=basePath%>front/goodsDetail/${goods.goodsId}.html">
				                        <div class="p1z">${goods.shopPrice}${goods.category.measure_unit}</div>
				                        <div class="img_zs"><img src="<%=basePath%>${goods.originalImg}" /></div>
				                        <div class="em_z">${goods.goodsName}</div>
			                        </a>
		                        </li>
	                        </c:forEach>
						</ul>
					</div>
					<a href="javascript:void(0)" class="next"></a>
				</div>
				<div class="models_slidBox">
					<a href="javascript:void(0)" class="prev"></a>
					<div class="slide">
						<ul class="contain_models_list">
							<jsp:setProperty property="pageNo" value="1" name="planecar"/>
							<jsp:setProperty property="pageSize" value="6" name="planecar"/>
							<jsp:setProperty property="cat_id" value="17" name="planecar"/>
							<c:forEach items="${planecar.goodsListByCat}" var="goods" varStatus="status">
								<li>
									<a href="<%=basePath%>front/goodsDetail/${goods.goodsId}.html">
				                        <div class="p1z">${goods.shopPrice}${goods.category.measure_unit}</div>
				                        <div class="img_zs"><img src="<%=basePath%>${goods.originalImg}" /></div>
				                        <div class="em_z">${goods.goodsName}</div>
			                        </a>
		                        </li>
	                        </c:forEach>
						</ul>
					</div>
					<a href="javascript:void(0)" class="next"></a>
				</div>
				
			</div>
		</div>
		<div class="clear contain_activity">
			<div class="contain_activity_bg">
				<label class="left">精彩活动</br><em>ACTIVITIES</em></label>
				<span class="right nav_font">多种优惠套餐 让你快乐出行</span>
				<div class="contain_activity_leftbg"></div>
				<div class="contain_activity_rightbg"></div>
			</div>
			<ul class="contain_activity_list">
			    <jsp:setProperty property="pageNo" value="1" name="active"/>
				<jsp:setProperty property="pageSize" value="6" name="active"/>
				<jsp:setProperty property="flag" value="2" name="active"/>
				<c:forEach items="${active.thematicList}" var="activity" varStatus="status">
					<li class="list-bg_z">
		                <a href="<%=basePath%>huodong/${activity.id}.html">
			                <img src="<%=basePath%>${activity.img}" />
			                <span><font class="font_zs">${activity.name}</font><i></i></span>
			            </a>
		             </li>
               </c:forEach>
			</ul>
		</div>
		<div class="clear contain_news">
			<div class="contain_news_tag text-center">
				<div class="contain_news_ico">
					<p class="icon_name_c">新闻中心</p>
					<p class="icon_name_e">NEWS CENTER</p>
					<span>&nbsp;1 </span>
				</div>
				<ul class="contain_news_list">
					<jsp:setProperty property="pageNo" value="1" name="news"/>
					<jsp:setProperty property="pageSize" value="3" name="news"/>
					<jsp:setProperty property="ids" value="74" name="news"/>
					<c:forEach items="${news.cmsList}" var="cms" varStatus="status">
						<c:set value="${fn:split(cms.time,'-')}" var="time1" />
						<li>
							<a href="<%=basePath%>page/${cms.id}_1.html">
								<div class="left news_time news_action"><span>${time1[1]}.${time1[2]}</span></br>${time1[0]}</div>
								<div class="right news_btn">点击查看</div>
								<p class="left news_til">${cms.name}</p>
								<p class="left news_info">${cms.description}</p>
							</a>
						</li>
					</c:forEach>
				</ul>
				<div class="clear"></div>
				<div class="news_more"><a href="<%=basePath%>auto/21.html">查看更多</a></div>
			</div>
		</div>
		<div class="contain_mien">
			<div class="contain_mien_bg">
				<label class="left">司机风采</br><em>THE DRIVER</em></label>
				<span class="right nav_font">多年驾龄技巧，让您放心乘坐！</span>				
				<div class="contain_models_leftbg"></div>
				<div class="contain_models_rightbg"></div>
			</div>
			<div class="mien_slidBox">
				<a href="javascript:void(0)" class="prev"></a>
				<div class="slide">
					<ul class="contain_mien_list">
						<jsp:setProperty property="pageNo" value="1" name="auntList"/>
						<jsp:setProperty property="pageSize" value="16" name="auntList"/>
						<c:forEach items="${auntList.auntList}" var="aunt" varStatus="status">
							<li>
								<img src="<%=basePath%>${aunt.head}" />
								<div class="name_c">姓名:${aunt.name}</div>
								<div class="speak">${aunt.desc}</div>
								<div class="xingp">
									<c:forEach begin="0" end="${aunt.score}" var="i" varStatus="status">
										<c:if test="${aunt.score!=0 && i!=0}">
											<img src="<%=basePath%>template/${folder.tpl.folder}/images/news_icon_x1.jpg"/>
										</c:if>
									</c:forEach>
									<c:forEach begin="0" end="${5-aunt.score}" var="i" varStatus="status">	
										<c:if test="${i!=0}">
											<img src="<%=basePath%>template/${folder.tpl.folder}/images/news_icon_x2.jpg"/>
										</c:if>
									</c:forEach>
								</div>
								<div class="clear"></div>
								<a href="<%=basePath%>aunt/${aunt.id}.html" class="contain_mien_btn">点击查看</a>
							</li>
						</c:forEach>
					</ul>
				</div>
				<a href="javascript:void(0)" class="next"></a>
			</div>
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
			
			<div class="clear"></div>
		</div>
	</div>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/> 
    <script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/My97DatePicker/calendar.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/My97DatePicker/WdatePicker.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.SuperSlide.2.1.1.js"></script>
	<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/main_js.js"></script>
	<script type="text/javascript">
		function form_smt(){
			var store_id=document.getElementById("store_id").value;
			var type_id=document.getElementById("type_id").value;
			var time=document.getElementById("time").value;
			if(store_id==""||store_id==null){
	         	alert("请选择门店！");
	        }else if(type_id==""||type_id==null){
	         	alert("请选择服务类型！");
	        }else if(time==""||time==null){
	         	alert("请选择时间！");
	        }else{
	        	document.forms["fast_form"].submit();
	        }
		}
	</script>
  </body>
</html>
