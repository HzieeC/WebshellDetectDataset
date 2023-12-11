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
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
		        	<form name="attributeForm" id="attributeForm">
		        		<input type="hidden" name="type_id" value="${type_id}" />
		        		<input type="hidden" name="attribute_id" value="${attribute_id}" />
				        		<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
						        	<tr>
										<td class="label_c" width="30%"><label><fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
										<td width="70%">
								           	<input type="text"  value="${attribute.name}" name="name" id="name" class="wid_80"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="type" bundle="${messagesBundle}"/>：</label></td>
										<td>
								           	<input type="text" value="${type.name}" name="cat" readOnly class="wid_80"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="order" bundle="${messagesBundle}"/>：</label></td>
										<td>
								           	<input type="text" value="${attribute.order}" name="order" id="integer" class="wid_80"/>
										</td>
									</tr>
									<c:if test="${strList!=null}">
					        		<tr>
										<td class="label_c"><label><fmt:message key="atrra" bundle="${messagesBundle}"/><fmt:message key="grade" bundle="${messagesBundle}"/>：</label></td>
										<td>
								           	<select name="group" id="group">
										            	<option value="0">--请选择--</option>
										            	<c:forEach items="${strList}" var="str" varStatus="status">
										            		<option value="${status.index+1}">${str}</option>
														</c:forEach>
										            </select>
										</td>
									</tr>
									</c:if>
					        		<tr>
										<td class="label_c"><label>能否进行检索：</label></td>
										<td>
								           	<input type="radio" name="indexs" value="0" checked/>不需要
								            	&nbsp;
								            	<input type="radio" name="indexs" value="1" />关键字
								            	&nbsp;
								            	<input type="radio" name="indexs" value="2" />范围
								            	<br>
								            	<span class="cont_z">说明：不需要该属性成为检索商品条件的情况请选择不需要检索，需要该属性进行关键字检索商品时选择关键字检索，如果该属性检索时希望是指定某个范围时，选择范围检索。</span>
										</td>
									</tr>
					        		<tr>
										<td class="label_c"><label>相同属性值的商品是否关联？：</label></td>
										<td>
								           	<input type="radio" name="linked" value="0" checked/>是
								            &nbsp;
								            <input type="radio" name="linked" value="1" />否
										</td>
									</tr>
					       			<tr>
										<td class="label_c"><label>属性是否可选：</label></td>
										<td>
								           	<input type="radio" name="type" value="0" checked/>唯一属性
								            &nbsp;
								            <input type="radio" name="type" value="1" />单选属性
								            &nbsp;
								            <input type="radio" name="type" value="2" />复选属性 
								            <br>
								            <span class="cont_z">说明：选择"单选/复选属性"时，可以对商品该属性设置多个值，同时还能对不同属性值指定不同的价格加价，用户购买商品时需要选定具体的属性值。选择"唯一属性"时，商品的该属性值只能设置一个值，用户只能查看该值。</span>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>该属性值的录入方式：</label></td>
										<td>
								           	<input type="radio" name="input_type" value="0" checked/>手工录入
								            	&nbsp;
								            	<input type="radio" name="input_type" value="1" />从下面的列表中选择（多个值用,隔开）
								            	&nbsp;
								            	<input type="radio" name="input_type" value="2" />多行文本框
										</td>
									</tr>
					        		<tr>
										<td class="label_c"><label>可选值列表：</label></td>
										<td>
								           	<textarea class="wid_80" name="values" id="values">${attribute.values}</textarea>
								           	<br>
								            <span class="cont_z">说明：多个值请用,隔开。</span>
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
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="addGoodsAttribute()"/>
				</li>
			</ul>	
		</div>
		
	</div>
	<!--本页主内容结束 -->
	
		
	</fmt:bundle>
	<script>
		
		function addGoodsAttribute(){
			 var title=requree_name("name") && requree_length("name",2,80);
			 var order=requree_integer("order") && requree_length("order",1,6);
			 var group=document.getElementById("group").value;
			 
             if(!title){
             	layer.alert("属性名为必填项");
             }else if(!order){
             	layer.alert("请设置顺序");
             }else if(group==""||group==null||group==0){
             	layer.alert("请选择分组！");
             }else{
             	var params= $('#attributeForm').serialize();
			    $.ajax({
					 url:"<%=basePath%>admin/addGoodsAttribute", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
						 alert(data.tip);
						 parent.window.location.reload();
					 }
				}); 
             }
		}
		
		var height = $(window).height();
		$(".middle_cnt_c2").height(height-80);
		$(window).resize(function () {          //当浏览器大小变化时
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-80);
		});
		
    	$("#group").val(${attribute.group});
    	$("input[name=input_type]:eq(${attribute.input_type})").attr("checked",'checked');
    	$("input[name=indexs]:eq(${attribute.indexs})").attr("checked",'checked');
    	$("input[name=linked]:eq(${attribute.linked})").attr("checked",'checked');
    	$("input[name=type]:eq(${attribute.type})").attr("checked",'checked');
	</script>