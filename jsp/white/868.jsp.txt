<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<meta http-equiv="pragma" content="no-cache"/>
<meta http-equiv="cache-control" content="no-cache"/>
<meta http-equiv="expires" content="0"/>    
	<link rel="stylesheet" href="<%=basePath%>css/admin_lay.css" />
	<script src="<%=basePath%>js/jquery-1.11.2.min.js"></script>
	<script src="<%=basePath%>js/validate.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<jsp:useBean id="com" scope="page" class="com.weishang.bean.Common"/>
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
	  				<form name="categoryForm" id="categoryForm">
	  					<input name="category_id" id="category_id" value="${category_id}" type="hidden"/>
	  					<input name="parent_id" id="parent_id" value="${parent_id}" type="hidden"/>
	  						<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
						        	<tr>
										<td class="label_c" width="22%"><label>分类名称：</label></td>
										<td width="78%">
								            <input type="text" value="${category.name}" name="name" id="name" class="wid_80"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>数量单位：</label></td>
										<td>
								            <input type="text" value="${category.measure_unit}" name="unit" id="unit" class="wid_80"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>排序：</label></td>
										<td>
								            <input type="text" value="${category.order}" name="order" id="order" class="wid_80"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>是否可用：</label></td>
										<td>
								            <select name="is_show" id="is_show">
												<option value="1">可用</option>
												<option value="0">不可用</option>
											</select>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>筛选属性：</label></td>
										<td>
								            <select onblur="selectTypeAttr()" id="type" name="type">
												<option value="0">请选择商品类型</option>
												<c:forEach items="${typeList}" var="type" varStatus="status">
													<option value="${type.id}">${type.name}</option>
												</c:forEach>
											</select>
											<select name="attr" id="attr">
												<option value="0">请选择商品属性</option>
											</select>
											<span class="tips">筛选属性可在前分类页面筛选商品</span>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>分类的样式表文件：</label></td>
										<td>
								            <input type="text" value="${category.style}" name="style_file" id="style_file" class="wid_80"/>
											<span class="tips">您可以为每一个商品分类指定一个样式表文件。例如文件存放在 template/default/css 目录下则输入：template/default/css/style.css</span>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>标题：</label></td>
										<td>
								            <input type="text" value="${category.title}" name="title" id="title" class="wid_80"/>
										</td>
									</tr>
	  								<tr>
										<td class="label_c"><label>关键字：</label></td>
										<td>
								            <input type="text" value="${category.keywords}" name="keywords" id="keywords" class="wid_80"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>分类描述：</label></td>
										<td>
								            <textarea name="desc" id="desc" class="wid_80">${category.desc}</textarea>
										</td>
									</tr>
							</table>
					</form>
				
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
			    <li>
			    	<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="add_category()"/>
			    </li>
		    </ul>	
		</div>
		<div id="menuTree" class="content menuTree">
				
		</div>
	</div>
	<!--本页主内容结束 -->
	
	</fmt:bundle>
	<script>
		function selectTypeAttr(){
			$("#attr").html("<option value=\"0\">请选择商品属性</option>");
			var type_id=$("#type").val();
			$.ajax({
					 url:"<%=basePath%>admin/goGoodsTypeAttrJson", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:{type_id:type_id},         //要传递的数据
					 success:function(data){
						 for(var i=0;i<data.length;i++){
						 	$("#attr").append("<option value=\""+data[i].id+"\">"+data[i].name+"</option>");
						 }
						 $("#attr").val(${category.filter_attr});
					 }
			});
		}
		
		function add_category(){
			var title=requree_name("name") && requree_length("name",2,80);
			var unit=requree_name("unit") && requree_length("unit",1,10);
			var order=requree_integer("order") && requree_length("order",1,2);
			if(!title){
             	layer.alert("请输入分类名称,字数为2到80字");
            }else if(!unit){
             	layer.alert("请正确输入数量单位，字数限制为1到10字");
            }else if(!order){
             	layer.alert("请正确输入排序数，规则为整数，仅限2位数");
            }else{
				var params= $('#categoryForm').serialize()
				$.ajax({
					 url:"<%=basePath%>admin/addGoodsCategory", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
						 layer.alert(data.tip,{
							  skin:'layui-layer-molv', //样式类名
							  closeBtn: 0
						},function(){
							window.parent.location.reload();
							var index = parent.layer.getFrameIndex(window.name); //获取窗口索引
                        	parent.layer.close(index);
						});
					}
				});
			}
		}
		
		function check_type(){
		    layer.open({
		        type: 1,
		        title: '父级分类',
				closeBtn: 0,
				shadeClose: true,
		        shadeClose: true, //点击遮罩关闭层
		        area : ['400px' , '250px'],
		        content:$('#menuTree'),
  				btn: ['确认'] //按钮
		    });
		}
		$("#is_show").val(${category.is_show});
		$("#type").val(${category.template_file});
		
		var height = $(window).height();
		$(".middle_cnt_c2").height(height-80);
		$(window).resize(function () {          //当浏览器大小变化时
		    var height = $(window).height();
			$(".middle_cnt_c2").height(height-80);
		});
		
	</script>