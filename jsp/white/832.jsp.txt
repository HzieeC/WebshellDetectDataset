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
	<link href="<%=basePath%>template/${folder.tpl.folder}/css/page.css" rel="stylesheet">
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js" type="text/javascript"></script>
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.DB_gallery.js" type="text/javascript"></script>
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/public_pc.js" type="text/javascript"></script>
	<script src="<%=basePath%>js/layer/layer.js" type="text/javascript"></script>
  </head>
  
  <body>
  	<jsp:include  page="/template/${folder.tpl.folder}/page/head_nav_yes.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/head.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/left.jsp"/>
    <div class="user-bd-main_r">
     <!-- 内容 我的余额 -->
        <div class="user-remainder">
    <!-- 单元内容 新增地址 -->
                <div class="user-bd-main">
                    <!-- 标题 -->
                    <div class="user-bd-main-title">
                    	<span class="lname">我的积分</span>                
                    </div>
                    
                    <!-- 内容 我的余额 -->
                    <div class="content">
                        <div class="title">
                            <span class="left">积分: <span class="c-red">${ordinary_user.integel}</span>分</span>
                        </div>
                        <table>
                            <tr>
                                <th>积分</th>
                                <th>时间</th>
                                <th>原因</th>
                                <th>方向</th>
                            </tr>
                            <c:forEach items="${integelList}" var="integel" varStatus="status">
	                            <tr>
	                                <td>${integel.integel}</td>
	                                <td>${integel.date}元</td>
	                                <td>${integel.desc}</td>
	                                <td>
	                                	<c:if test="${integel.dericte==1}">
	                                		增加
	                                	</c:if>
	                                	<c:if test="${integel.dericte==2}">
	                                		扣除
	                                	</c:if>
	                                </td>
	                            </tr>
                            </c:forEach>
                        </table>
                         <!-- 分页 -->
						    <jsp:useBean id="paging" scope="page" class="com.weishang.bean.Page"/>
							<jsp:setProperty property="user" value="user" name="paging"/>
							<jsp:setProperty property="crrent" value="${pageNo}" name="paging"/>
							<jsp:setProperty property="suffix" value="" name="paging"/>
							<jsp:setProperty property="sumPage" value="${sum}" name="paging"/>
							<jsp:setProperty property="color" value="blue" name="paging"/>
							<jsp:setProperty property="url" value="/user/commonUserAction?tag=goUserIntegel" name="paging"/>
							${paging.pageString}
                    </div>
                </div>
            </div>
           
	        <!-- end 内容 -->
    </div>
    <div class="clear"></div>
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/foot.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/>
    <script>
    	$(".lnachong").on('click', function(){
    		layer.open({
			  type: 1,
			  skin: 'layui-layer-rim', //加上边框
			  area: ['420px', '240px'], //宽高
			  content:$('#chongzhi_c')
			});
    	});
    </script>
  </body>
</html>