<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE html>
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
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/public_pc.js" type="text/javascript"></script>

  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head_nav_yes.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/head.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/left.jsp"/>
    <div class="user-bd-main_r">
                <!-- 优惠券 -->
                <div class="user-bd-main">
                    <!-- 标题 -->
                    <div class="user-bd-main-title">领取优惠券</div>
                    
                    <!-- 内容 优惠券 -->
                    <div class="content coupon">
                        <div class="all_pri">
	                        <ul>
	                        	<c:forEach items="${couponListTemp}" var="coupon" varStatus="status">
									<c:if test="${coupon.userName==null || coupon.userName==''}">
										<li>
											<img class="privi_img" src="<%=basePath%>template/${folder.tpl.folder}/images/privi_can.png">
											<div class="privi_title">${coupon.title}</div>
											<div class="face_value">${coupon.price}</div>
											<div class="time_info">
												<div class="time_top">使用期限</div>
												<div class="time_data">
													<p>${coupon.date}</p>
													<c:if test="${coupon.end==1}">
														永久
													</c:if>
													<c:if test="${coupon.end==2}">
														<p>${coupon.enddate}</p>
													</c:if>
												</div>
												<a href="javascript:void(0)" onclick="updateUser(${coupon.id})"><div class="time_state">我要领取</div></a>
											</div>
										</li>
									</c:if>
									<c:if test="${coupon.userName!=null && coupon.userName!='' }">
										<li>
											<img class="privi_img" src="<%=basePath%>template/${folder.tpl.folder}/images/privi_timeout.png">
											<div class="title">${coupon.title}</div>
											<div class="face_value">${coupon.price}</div>
											<div class="time_info other">
												<div class="time_top">使用期限</div>
												<div class="time_data">
													<p>${coupon.date}</p>
													<c:if test="${coupon.end==1}">
														永久
													</c:if>
													<c:if test="${coupon.end==2}">
														<p>${coupon.enddate}</p>
													</c:if>
												</div>
												<div class="time_state">已领取</div>
											</div>
										</li>
									</c:if>
								</c:forEach>
	                        </ul>
	                    </div>
                    </div>
                </div>
            </div>
            <div class="clear"></div>
        </div>
        <!-- end 内容 -->
    	<div class="clear"></div>
    </div>
	<script type="text/javascript">
		function updateUser(coupon_id){
			$.ajax({
					url:"<%=basePath%>wx/wxReceiveCoupon", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:{coupon_id:coupon_id},         //要传递的数据
					success:function(data){
						alert(data.tip);
						window.location.reload();
					}
			}); 
		}
	</script>
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/foot.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/>
  </body>
</html>