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
                <!-- 单元内容 预约下单 -->
                <div class="user-bd-main">
                	<form action="<%=basePath%>pc/pcUserAddOrderTwo" method="post">
                		<input name="goodsTypeFlag" value="${goodsTypeFlag}" type="hidden"/>
                		<input name="goodsTypeId" value="${goodsTypeId}" type="hidden"/>
                		<input name="orderPrice" value="${orderPrice}" type="hidden"/>
                		<input name="gradeName" value="${gradeName}" type="hidden"/>
	                    <!-- 进度 -->
	                    <div class="user-order-progress clearfix">
	                        <span class="first complete"></span>
	                        <span class="second"></span>
	                        <span class="third"></span>
	                        <span class="fourth"></span>
	                    </div>
	
	                    <!-- 标题 -->
	                    <c:if test="${goodsTypeFlag==1}">
	                    	<div class="user-bd-main-title">预约下单</div>
	                    </c:if>
	                    <c:if test="${goodsTypeFlag==2}">
	                    	<div class="user-bd-main-title">下单</div>
	                    </c:if>
	
	                    <!-- 时间 -->
	                    <div class="user-order-time clearfix">
	                        <div class="service">开始时间：<input name="starttime" id="fwsj" type="text" onClick="WdatePicker({dateFmt: 'yyyy-MM-dd HH',minDate:'%y-%M-%d-{%H+2}'})" placeholder="请选择服务时间"></div>
	                        <div class="order">结束时间：<input name="endtime" id="xdsj" type="text" onClick="WdatePicker({dateFmt:'yyyy-MM-dd HH',minDate:'#F{$dp.$D(\'fwsj\',{H:1})}'})" placeholder="请选择下单时间"></div>
	                    </div>
	                    
	
	                    <!-- 服务地点 -->
	                    <div class="user-order-address clearfix">
	                        <div class="clearfix">
	                            <div class="title">服务地点：</div>
	                            <div class="address clearfix">
	                            	<c:forEach items="${addressList}" var="address" varStatus="status">
		                                <ul _val="${address.id}" >
		                                    <li>${address.name}</li>
		                                    <li>${address.address}<br>${address.tel}</li>
		                                </ul>
	                                </c:forEach>
	                                <input type="hidden" id="address_id" name="address_id" value='0'/>
	                            </div>
	                        </div>
	                        <a class="manageAddress" target="_blank" href="<%=basePath%>pc/pcUserAddress">管理地址</a>
	                    </div>
	
	                    <c:if test="${goodsTypeFlag==1}">
	                    	<div class="user-order-remark clearfix">
		                        <div class="title">备注：</div>
		                        <div class="formRow"><textarea name="desc"></textarea></div>
		                        <div class="formRow btnBox"><input class="btn-sub order_sub" type="submit" value="立即预约"></div>
		                    </div>
	                    </c:if>
	                    <c:if test="${goodsTypeFlag==2}">
	                    	<div class="user-order-remark clearfix">
		                        <div class="title">备注：</div>
		                        <div class="formRow"><textarea name="desc"></textarea></div>
		                        <div class="formRow btnBox"><input class="btn-sub order_sub" type="submit" value="开始下单"></div>
		                    </div>
	                    </c:if>
	                    <!-- 备注 -->
	                    
                    </form>
                </div>
            </div>
        </div>
        <!-- end 内容 -->
        <!-- 底部 -->
        <!-- end底部 -->
    </div>
    <!-- end 网页背景层 -->
     <jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/>
  </body>
</html>
