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
	<script src="<%=basePath%>js/public.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>品牌信息</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c">
		        	<form name="typeForm" id="typeForm">
		        		<input type="hidden" name="brand_id" value="${brand_id}" />
				        <ul>
							<li>
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
									<tr>
										<td class="label_c" width="15%"><label><fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<input type="text" value="${brand.name}" name="name" id="name" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="net" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${brand.url}" name="url" id="url" class="wid_60"/>
										</td>
									</tr>
						        	<tr>
										<td class="label_c"><label><fmt:message key="logo" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<div class="butt_smt" ><fmt:message key="select" bundle="${messagesBundle}"/><fmt:message key="file" bundle="${messagesBundle}"/></div>
										</td>
									</tr>
					        		<tr>
										<td class="label_c"><label><fmt:message key="desc" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<textarea class="wid_60" name="desc" id="descript">${brand.desc}</textarea>
										</td>
									</tr>
					     			<tr>
										<td class="label_c"><label><fmt:message key="order" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${brand.order}" name="order" id="integer" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="sentences16" bundle="${messagesBundle}"/>：</label></td>
										<td algin="left">
											<input type="radio" name="show" value="1" checked/>是
								            &nbsp;
								            <input type="radio" name="show" value="0" />否
										</td>
									</tr>
					      			<tr>
										<td class="label_c"><label>注明：</label></td>
										<td class="cont_z" algin="left">
											当品牌下还没有商品的时候，首页及分类页的品牌区将不会显示该品牌。
										</td>
									</tr>
						        </table>
					        </li>
				        </ul>
				        <input name="img" type="hidden" value="${brand.logo}"/>
			        </form>
			    </div>
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<li>
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="addGoodsBrand()"/>
				</li>
			</ul>	
		</div>
		
	</div>
	<!--本页主内容结束 -->
		
	</fmt:bundle>
	<script>
		$('.butt_smt').click(function(){
			layer.open({
			        type: 2,
			        title: '上传logo',
			        maxmin: true,
			        shadeClose: true, //点击遮罩关闭层
			        area : ['450px' , '320px'],
			        content: '<%=basePath%>admin/goUploadUrl'
			    });
		})

		function addGoodsBrand(){
			 var title=requree_name("name") && requree_length("name",2,80);
			 var order=requree_integer("order") && requree_length("order",1,2);
             if(!title){
             	layer.alert("名称为必填项");
             }else if(!order){
             	layer.alert("请设置排序");
             }else{
             	var params= $('#typeForm').serialize();
			    $.ajax({
					 url:"<%=basePath%>admin/addGoodsBrand", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
						 alert(data.tip);
						 window.location.reload();
					 }
				}); 
             }
		}
	
	</script>