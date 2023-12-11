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
    	<div class="user-myOrder">
                <!-- 单元内容 新增地址 -->
                <div class="user-bd-main">
                    <!-- 标题 -->
                    <table class="content">
                    	<tr class="tHead">
	                       <th width="15%">优惠券编号</th>
	                       <th width="15%">标题</th>
	                       <th width="8%">金额</th>
	                       <th width="8%">结束时间</th>
	                       <th width="8%">状态</th>
	                     </tr>
                       
                        <c:forEach items="${couponList}" var="coupon" varStatus="status">
                        	<tr>
		                        <td class="pl20">${coupon.no}</td>
		                        <td align="center">${coupon.title}</td>
		                        <td align="center">${coupon.price}</td>
		                        <td>${coupon.enddate}</td>
		                        <td align="center">
		                        	<c:if test="${coupon.flag==1}"><!-- 未使用 -->
			                         	 可以使用
		                            </c:if>
		                           	<c:if test="${coupon.flag==3}"><!-- 过期 -->
			                           	过期
		                            </c:if>
		                            <c:if test="${coupon.flag==2}"><!--已使用 -->
			                             	已使用
		                            </c:if>
		                        </td>
		                   </tr>
                        </c:forEach>
                    </table>
          </div>
     </div>
     <div class="clear"></div>
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/foot.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/>
  </body>
</html>