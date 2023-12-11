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
		<!--标题部分-->
		<div class="title_c"><span>添加幻灯片</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c">
					<form name="slideForm" id="slideForm">
		        		<input type="hidden" name="id" value="${slide.id}">
						<ul>
							<li>
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
									<tr>
										<td class="label_c" width="15%"><label><fmt:message key="title" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<input type="text" value="${slide.title}" name="title" id="title" class="wid_60" placeholder="请输入标题"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>url：</label></td>
										<td>
											<input type="text" value="${slide.url}" name="url" id="url" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="order" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${slide.order}" name="order" id="integer" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="img" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<div class="butt_smt" ><fmt:message key="select" bundle="${messagesBundle}"/><fmt:message key="file" bundle="${messagesBundle}"/></div>
										</td>
									</tr>
								</table>
							</li>
						</ul>
					<input name="img" value="${slide.img}" type="hidden">
			        </form>
				</div>
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<li>
			    	<input type="submit" onclick="addSlide()" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter"/>
			    </li>
			    <li>
			    	<input type="button" onclick="history.go(-1)" value="<fmt:message key="back" bundle="${messagesBundle}"/>" class="button-2 vcenter"/>
			    </li>
			</ul>	
		</div>
		
	</div>
	<!--本页主内容结束 -->
	
		
		

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
			
	       window.onload = function () { 
	            new uploadPreview({ UpBtn:"up_img",DivShow: "imgdiv",ImgShow:"imgShow" });
	        }
	        //添加或者修改幻灯片
			function addSlide(){
				var title=requree_name("title") && requree_length("title",2,80);
				var integer=requree_integer("order") && requree_length("order",0,2);
	             if(!title){
	             	layer.alert("标题为必填项,字数限制2~80字");
	             }else if(!integer){
	             	layer.alert("顺序为数字,最大允许输入两位数");
	             }else{
			    	var params= $('#slideForm').serialize()
			        $.ajax({
						 url:"<%=basePath%>admin/addSlide", //后台处理程序
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
	</fmt:bundle>
