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
		        	<form name="roleForm" id="roleForm">
		        		<input type="hidden" name="parentId" value="${parentId}" />
		        		<input type="hidden" name="myId" value="${myId}" />
		        		<input type="hidden" name="is_cms" value="${is_cms}" />
				       
						        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
							        <tbody>
								        <tr>
								            <td width="20%" class="label_c"><label><fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
								            <td width="80%">
								            	<input type="text" value="${modular.title}" name="name" id="name" class="wid_80"/>
								            </td>
								        </tr>
								        <c:if test="${is_cms==1}">
									        <tr>
												<td class="label_c"><label><fmt:message key="sentences10" bundle="${messagesBundle}"/>：</label></td>
												<td>
										            <select name="style" id="type">
											            	<option value="">--请选择--</option>
											            	<option value="1">列表</option>
															<option value="2">单页</option>
											            </select>
												</td>
											</tr>
										</c:if>
										<tr>
											<td class="label_c"><label>url：</label></td>
											<td>
									            <c:if test="${is_cms!='' && is_cms!=null}"><!-- 添加模块时的URL状态 -->
								            		<c:if test="${is_cms==1}"><!-- 当前状态为CMS -->
								            			<c:if test="${parentId==0}"><!-- 一级模块 -->
								            				<input type="text" value="javascript:void(0)" readonly name="url" id="url" class="wid_80"/>
								            			</c:if>
								            			<c:if test="${parentId!=0}"><!-- 非一级模块 -->
								            				<input type="text" value="admin/modularCms" readonly name="url" id="url" class="wid_80"/>
								            			</c:if>
								            		</c:if>
								            		<c:if test="${is_cms==0}"><!-- 当前状态为非CMS -->
								            			<input type="text" value="" name="url" id="url" class="wid_80"/>
								            		</c:if>
								            	</c:if>
								            	<c:if test="${is_cms=='' || is_cms==null}"><!-- 修改模块时的URL状态 -->
								            		<c:if test="${modular.is_not_cms==1}">
									            		<input type="text" value="${modular.url}" readonly name="url" id="url" class="wid_80"/>
								            		</c:if>
								            		<c:if test="${modular.is_not_cms==0}">
									            		<input type="text" value="${modular.url}" name="url" id="url" class="wid_80"/>
								            		</c:if>
								            		
								            	</c:if>
											</td>
										</tr>
										<tr>
											<td class="label_c"><label><fmt:message key="order" bundle="${messagesBundle}"/>：</label></td>
											<td>
									            <input type="text" value="${modular.order}" name="order" id="integer" class="wid_80"/>
											</td>
										</tr>
							        </tbody>
						        </table>
			        </form>
		       
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
			    <li>
			    	<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="add()"/>
			    </li>
		    </ul>	
		</div>
	</div>
	<!--本页主内容结束 -->
	
	</fmt:bundle>
	<script>
		function add(){
			var flag_type=document.getElementById("type");
			var type_s=null;
			var title=requree_name("name") && requree_length("name",2,80);
			if(flag_type!=null){
				type_s=document.getElementById("type").value;
			}
			var url=requree_name("url");
			var order=requree_integer("order");
			
		    if(!title){
		       layer.alert("请输入名称,字数为2到80字！");
		    }else if(flag_type!=null && (type_s==""||type_s==null)){
             	layer.alert("请选择模板类型！");
            }else if(!url){
             	layer.alert("请输入url地址！");
            }else if(!order){
             	layer.alert("请正确输入排序数，规则为整数");
            }else{
		    	var params= $('#roleForm').serialize()
			    $.ajax({
					 url:"<%=basePath%>admin/addModular", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
						 alert(data.tip);
						 parent.window.location.reload();s
					 }
				}); 
		    }
		}
		document.getElementById("type").value="${modular.style}";
		var height = $(window).height();
		$(".middle_cnt_c2").height(height-80);
		$(window).resize(function () {          //当浏览器大小变化时
			    var height = $(window).height();
				$(".middle_cnt_c2").height(height-80);
			});
    	
	</script>