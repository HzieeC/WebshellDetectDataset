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
    	<div class="user-myOrder">
                <div class="user-bd-main-title">订单管理</div>
                
                <div class="user-bd-main" style="margin-top:10px;">
                    <!-- 标题 -->
                    <table class="content">
		                <tr class="tHead">
		                	<!-- <th width="21%">订单编号</th> -->
			                
			                <th width="8%">商品</th>
			                <th width="8%">金额</th>
			                <th width="10%">服务时间</th>
			                <th width="8%">联系人</th>
			                <th width="15%">身份证</th>
			                <th width="10%">电话</th>
			                <th width="7%">状态</th>
			                <th width="22%">操作</th>
		                </tr>
                       
                      
                        <c:forEach items="${OrderList}" var="order" varStatus="status">
			                <tr>
			                	<div id="content_${order.id}" style="display:none;padding:10px; line-height:2em">
			                		${order.content}
			                	</div>
			                	<!-- <td class="pl20">${order.no}</td>		 -->
				                              
				                <td >${order.goodsName}</td>
				                <td >${order.money}</td>
				                <td>${order.serviceDate}</td>
				                <td align="center">${order.address_person}</td>
				                <td>${order.address_content}</td>
				                <td align="center">${order.address_tel}</td>
				                <td align="center">
					                <c:if test="${order.flag==1}">
										未处理
									</c:if>
									<c:if test="${order.flag==2}">
										已处理
									</c:if>
									<c:if test="${order.flag==3}">
										 已完成
									</c:if>
									<c:if test="${order.flag==4}">
										 已退单
									</c:if>
									<c:if test="${order.flag==5}">
										派遣中
									</c:if>
									<c:if test="${order.flag==6}">
										阿姨确定
									</c:if>
									<c:if test="${order.flag==7}">
										异常结束
									</c:if>
				                </td>      
				                <td align="center">
				                	<c:if test="${order.flag==1}">
										<a onclick="cancelOrder(${order.id})" class="green" href="javascript: void(0)">取消</a>
									</c:if>
						            <a onclick="lookGoodsType(${order.id})" class="green" href="javascript: void(0)">全部商品</a>
						            <a onclick="lookOrderContent(${order.id})" class="blue" href="javascript: void(0)">查看备注</a>
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
							<jsp:setProperty property="url" value="/pc/pcUserOrder" name="paging"/>
							${paging.pageString}
					<!-- 分页结束 -->
                </div>
        <!-- end 内容 -->
      </div>
     </div>
     <div class="clear"></div>
     <script language="javascript">
	  function lookGoodsType(order_id){
	  	layer.open({
	  		type: 2,
			title: "查看订单全部商品",
			shadeClose: true,
			shade: 0.8,
			maxmin: true, //开启最大化最小化按钮
			area: ['400px', '300px'],
			content: '<%=basePath%>user/commonUserAction?tag=getGoodsTypeByActive&order_id='+order_id+''
	  	});
	  }
	  
	  function lookOrderContent(order_id){
	  	layer.open({
	  		type: 1,
			title: "查看服务备注",
			shadeClose: true,
			shade: 0.8,
			maxmin: true, //开启最大化最小化按钮
			area: ['400px', '300px'],
			content:$("#content_"+order_id+"")
	  	});
	  }
	  
	  function cancelOrder(order_id){
	  	if(confirm("请确认是否退单，退单时您的积分和优惠券的消费将不会退入您的账户")){
	  		$.ajax({
				url:"<%=basePath%>user/cancelOrder", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:{order_id:order_id},         //要传递的数据
				success:function(data){
					alert(data.tip);
					window.location.reload();
				}
			});
	  	}
	  }
	</script>
	
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/foot.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/>
   
  </body>
</html>