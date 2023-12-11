<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML>
<html>
  <head>
    <base href="<%=basePath%>">
    <jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
    <jsp:useBean id="net" scope="page" class="com.weishang.bean.ReceptBean"/><!--网站基本信息-->
    <title>${net.net.company}</title>
    
	<link href="<%=basePath%>template/${folder.tpl.folder}/css/style_pc.css" rel="stylesheet">
	<link href="<%=basePath%>template/${folder.tpl.folder}/css/index.css" rel="stylesheet">
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js" type="text/javascript"></script>
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.DB_gallery.js" type="text/javascript"></script>
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.SuperSlide.2.1.1.js" type="text/javascript"></script>
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/My97DatePicker/WdatePicker.js"></script>
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/public_pc.js" type="text/javascript"></script>

  </head>
  
  <body>
   	<jsp:include  page="/template/${folder.tpl.folder}/page/head_nav_yes.jsp"/>
    	  <!-- 网页背景层 -->
    <div class="wrapperBg">
        <!-- 头部 -->
        <!-- end 头部 -->


        

        <!-- 内容 预约下单 -->
        <div class="container user-order wrap">
            <!-- 内容主体 -->
            <div class="user-bd clearfix">
                <form action="<%=basePath%>pc/pcUserAddOrderThree" method="post">
                	<input name="goodsTypeFlag" value="${goodsTypeFlag}" type="hidden"/>
                	<input name="goodsTypeId" value="${goodsTypeId}" type="hidden"/>
                	<input name="orderPrice" value="${orderPrice}" type="hidden"/>
                	<input name="gradeName" value="${gradeName}" type="hidden"/>
                	<input name="starttime" value="${starttime}" type="hidden"/>
                	<input name="endtime" value="${endtime}" type="hidden"/>
                	<input name="address_id" value="${address.id}" type="hidden"/>
	                <!-- 单元内容 预约下单 -->
	                <div class="user-bd-main">
	                    <!-- 进度 -->
	                    <div class="user-order-progress clearfix">
	                        <span class="first complete"></span>
	                        <span class="second complete"></span>
	                        <span class="third"></span>
	                        <span class="fourth"></span>
	                    </div>
	
	                    <!-- 标题 -->
	                    <div class="user-bd-main-title">订单详情</div>
	
	                    <!-- 内容 订单详情 -->
	                    <div class="user-pay-order">
	                        <div class="project">服务类型：  ${goodsType.name}</div>
	                        <div class="orderAttr">
	                            <span class="ffyh">开始时间：${starttime}时</span>
	                            <span>结束时间：${endtime}时</span>
	                        </div>
	                        <div class="orderDetail">
	                            <div class="clearfix">
	                                <span class="left">联 系 人：</span>
	                                <span class="right">${address.name}</span>
	                            </div>
	                            <div class="clearfix">
	                                <span class="left">联系号码：</span>
	                                <span class="right">${address.tel}</span>
	                            </div>
	                            <div class="clearfix">
	                                <span class="left">服务地址：</span>
	                                <span class="right">${address.address}</span>
	                            </div>
	                            <div class="clearfix">
	                                <span class="left">定制要求：</span>
	                                <span class="right">${goodsType.name}</span>
	                            </div>
	                            <div class="clearfix">
	                                <span class="left">备　　注：</span>
	                                <span class="right">${desc}</span>
	                            </div>
	                        </div>
	                    </div>
						<c:if test="${ goodsType.flag==2}">
		                    <!-- 标题 -->
		                    <div class="user-bd-main-title">支付</div>
		
		                    <!-- 内容 支付方式 -->
		                    <div class="user-pay-method">
		                        <div class="preferential">
		                            <div class="preferential-content">
		                                <input type="hidden" value="1">
		                                <div class="count">
								            <div class="radioBox_jin_lf">总金额：<span class="c-red">￥${orderPrice}</span></div>
								            <div class="radioBox_jin_lf_r">
								            <c:if test="${myMoneyFlag==1}">
				                            	<div class="radioBox yezf checked" _val="1"><span>余额 ${ordinary_user.money}元</span></div>
				                            </c:if>
				                            <c:if test="${myMoneyFlag==0}">
				                            	 <div class="radioBox yezf checked" val="1"><span>余额 ${ordinary_user.money}元</span></div>
				                            </c:if>
				                            </div>
				                            <div class="clear"></div>	                              
		                                </div>
		                            </div>
		                        </div>
							</div>
							
							<div class="user-bd-main-title">选择充值方式</div>
							<div class="user-pay-method" style="padding-bottom:50px;">
		                        <div class="method">
		                            <a class="radioBox zfb" target="_blank" href="<%=basePath%>pc/goUserRecharge"></a>
		                            
		                            <!--  
			                            <div class="checkedBox wxzf" _val="2"></div>
			                            <div class="checkedBox jfdf" _val="4"><span>我的积分 100分</span></div>
		                            -->
		                            
		                        </div>
		                        <input type="hidden" name="pay" value="1">
		                        <input class="btn-sub" type="submit" value="确认支付">
		                    </div>
		                    
	                    </c:if>
	                    <c:if test="${ goodsType.flag==1}">
							<input class="btn-sub_33" type="submit" value="开始下单">
	                    </c:if>
	                </div>
                </form>
            </div>
        </div>
        <!-- end 内容 -->
    	</div>
    <!-- end 网页背景层 -->
     <jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/>
  </body>
</html>
