<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!DOCTYPE html>
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
		<div class="title_c"><span>基本信息内容</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c">
					<form name="netForm" id="netForm">
						<ul>
							<li>
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
									<tr>
										<td class="label_c" width="15%"><label><fmt:message key="title" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<input type="text" value="${net.title}" name="title" id="title" class="wid_60" placeholder="请输入标题"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="keyword" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<textarea class="wid_60" name="keywords" id="keywords">${net.keywords}</textarea>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="desc" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<textarea class="wid_60" name="descript" id="descript">${net.descript}</textarea>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="company" bundle="${messagesBundle}"/><fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${net.company}" name="company" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="copyright" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" name="copyright"  value="${net.copyright}" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="info" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" name="info"  value="${net.info}" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="tel" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" name="tel"  value="${net.tel}" id="tel" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="logo" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<div class="butt_smt" ><fmt:message key="select" bundle="${messagesBundle}"/><fmt:message key="file" bundle="${messagesBundle}"/></div>
										</td>
									</tr>
								</table>
								<input name="logo" value="${net.logo}" type="hidden">
							</li>
						</ul>
					</form>
				</div>
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<li>
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="updateNet()"/>
				</li>
			</ul>	
		</div>
		
	</div>
	<!--本页主内容结束 -->
	
		
		<script>
	       window.onload = function () { 
	            new uploadPreview({ UpBtn:"up_img",DivShow: "imgdiv",ImgShow:"imgShow" });
	        }
	        //修改网站基本信息
			function updateNet(){
				var title=requree_name("title") && requree_length("title",2,80);
				
				if(!title){
					layer.alert("标题为必填项！");
				}else{
					var params= $('#netForm').serialize()
			        $.ajax({
						 url:"<%=basePath%>admin/updateNet", //后台处理程序
						 type:'post',         //数据发送方式
						 dataType:'json',
						 data:params,         //要传递的数据
						 success:function(data){
						 	layer.alert(data.tip);
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
