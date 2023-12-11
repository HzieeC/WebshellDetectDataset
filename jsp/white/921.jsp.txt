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
		<div class="title_c"><span>添加友情链接/合作商</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c">
		        	<form name="cooperationForm" id="cooperationForm">
		        		<input name="flag" value="${flag }" type="hidden"/>
				        <ul>
					        <li>
						        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
							        <tbody>
								        <tr>
								            <td width="15%" class="label_c"><label><fmt:message key="url" bundle="${messagesBundle}"/>：</label></td>
								            <td width="85%">
								            <input type="text" value="${net.title}" name="url" id="url" class="wid_60"/>
								            </td>
								        </tr>
								        <tr>
											<td class="label_c"><label><fmt:message key="www" bundle="${messagesBundle}"/>：</label></td>
											<td>
												<input type="text" value="${net.title}" name="www" id="name" class="wid_60"/>
											</td>
										</tr>
										<tr>
											<td class="label_c"><label><fmt:message key="order" bundle="${messagesBundle}"/>：</label></td>
											<td>
												<input type="text" type="text" value="${net.title}" name="order" id="integer" class="wid_60"/>
											</td>
										</tr>
										<tr>
											<td class="label_c"><label><fmt:message key="logo" bundle="${messagesBundle}"/>：</label></td>
											<td>
												<div class="butt_smt" ><fmt:message key="select" bundle="${messagesBundle}"/><fmt:message key="file" bundle="${messagesBundle}"/></div>
											</td>
										</tr>
							        </tbody>
						        </table>
					        </li>
				        </ul>
				        <input name="logo" value="${net.logo}" type="hidden">
			        </form>
		        </div>
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
		
		<div class="theme-popover">
		     <div class="theme-poptit">
		          <a href="javascript:;" title="<fmt:message key="close" bundle="${messagesBundle}"/>" class="close">×</a>
		          <h3><fmt:message key="upload" bundle="${messagesBundle}"/></h3>
		     </div>
		     <div class="theme-popbod dform">
		            <iframe src="" width="100%" height="100%" frameborder="0" scrolling="auto" name="main" id="main" allowtransparency="true" ></iframe>
		     </div>
		</div>
		<div class="theme-popover-mask"></div>

		<script>
	       window.onload = function () { 
	            new uploadPreview({ UpBtn:"up_img",DivShow: "imgdiv",ImgShow:"imgShow" });
	        }
	        //修改网站基本信息
			function add(){
				var url=requree_name("url");
				var title=requree_name("www") && requree_length("www",2,20);
				var order=requree_integer("order");
				if(!url){
	            	layer.alert("请输入网址");
	            }else if(!title){
	            	layer.alert("请输入名称，字数限制2~20字");
	            }else if(!order){
	            	layer.alert("请输入排序值，为整数");
	            }else{
		    		var params= $('#cooperationForm').serialize()
			        $.ajax({
						 url:"<%=basePath%>admin/addCooperation", //后台处理程序
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
			
	    </script>
	</fmt:bundle>
<!--end right-->