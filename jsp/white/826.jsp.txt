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
    	        <!-- 单元内容 新增地址 -->
                <div class="user-bd-main add-address">
                    <!-- 标题 -->
                    <div class="user-bd-main-title">新增地址：</div>

                    <!-- 新增地址 表单 -->
                    <div class="add-address-form">
                        <form name="addressForm" id="addressForm" action="javascript:void(0)" method="post">
                            <div class="formRow clearfix">
                                <span class="tag">联系人：</span>
                                <input class="sAddress" name="name" type="text">
                            </div>
                            <div class="formRow clearfix">
                                <span class="tag">身份证号码：</span>
                                <input class="dAddress" name="address" type="text">
                            </div>
                            <div class="formRow clearfix">
                                <span class="tag">联系电话：</span>
                                <input class="phoneNum" name="tel" type="text">
                            </div>
                            <input class="btn-sub" type="submit" onClick="addAddress()" value="保存">
                        </form>
                    </div>

                    <!-- 地址列表 -->
                    <div class="add-address-list">
                        <!-- 标题 -->
                        <div class="title clearfix">地址<!--<a class="btn-add-address" href="javascript: void(0)">新增收货地址</a>--></div>
                        <!-- 表格 -->
                        <table>
                            <tr>
                                <th>联系人</th>
                                <th>身份证号码</th>
                                <th>联系方式</th>
                                <th>操作</th>
                            </tr>
                            <c:forEach items="${addressList}" var="address" varStatus="status">
                            <tr>
                                <td>${address.name}</td>
                                <td>${address.address}</td>
                                <td>${address.tel}</td>
                                <td class="operate">  
                                    <a href="javascript:void(0)" onClick="deleteAddress(${address.id})">删除</a>
                                </td>
                            </tr>
                            </c:forEach> 
                        </table>
                    </div>
                </div>
		<script language="javascript">
			function deleteAddress(address_id){
			   if(confirm("请确认是否删除此地址信息")){
					$.ajax({
							url:"<%=basePath%>wx/wxDeleteAddress", //后台处理程序
							type:'post',         //数据发送方式
							dataType:'json',
							data:{address_id:address_id},         //要传递的数据
							success:function(data){
								alert(data.tip);
								window.location.reload();
							}
					}); 
			   }
			}
			
			function addAddress(){
				var params= $('#addressForm').serialize();
				$.ajax({
					url:"<%=basePath%>pc/pcUserAddAddress", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:params,         //要传递的数据
					success:function(data){
						alert(data.tip);
						window.location.reload();
					}
				}); 
			}
		</script>
        <!-- end 内容 -->
    </div>
     <div class="clear"></div>
    </div>
    
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/foot.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/>
  </body>
</html>