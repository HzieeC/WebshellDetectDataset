<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%@ taglib uri="http://java.fckeditor.net" prefix="FCK" %>
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
	<script type="text/javascript">
		function FCKeditor_OnComplete(editorInstance) {
			window.status = editorInstance.Description;
		}
	</script>
	
	  <fmt:setLocale value="zh_CN"/>
	  <fmt:setBundle basename="messages" var="messagesBundle"/>
	  <fmt:bundle basename="messagesAllMessage">
	 <div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>信息内容添加</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c">
		        	<form name="cmsForm" id="cmsForm" method="post">
		        		<input type="hidden" name="id" value="${cms.id}">
		        		<input type="hidden" name="modId" value="${modId}">
		        		<ul>
							<li>
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
						        	<tr>
										<td class="label_c" width="15%"><fmt:message key="title" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<input type="text" value="${cms.name}" name="name" id="title" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="keyword" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <input type="text" value="${cms.keyword}" name="keyword" id="keywords" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="desc" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<textarea class="wid_60" name="description" id="descript">${cms.description}</textarea>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="order" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <input type="text" value="${cms.order}" name="order" id="integer" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="source" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <input type="text" value="${cms.suroce}" name="suroce" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="upload" bundle="${messagesBundle}"/><fmt:message key="person" bundle="${messagesBundle}"/>：</label></td>
										<td>
								            <input type="text" value="${user.adminName}" readonly name="person" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="recommend" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<select name="flag" id="type">
									            	<option value="">--请选择--</option>
									            	<option value="0">普通</option>
									            	<option value="1">推荐</option>
									            	<option value="2">最热</option>	
									            </select>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="cover" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<div class="butt_smt" ><fmt:message key="select" bundle="${messagesBundle}"/><fmt:message key="file" bundle="${messagesBundle}"/></div>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><label><fmt:message key="content" bundle="${messagesBundle}"/>：</label></td>
										<td height="250">
											<FCK:editor instanceName="cont">
														<jsp:attribute name="value">
														<p>${cms.content}</p>
														</jsp:attribute>
														<jsp:body>
															<FCK:config AutoDetectLanguage="${empty param.code ? true : false}"
																DefaultLanguage="${empty param.code ? 'en' : param.code}" />
														</jsp:body>
													</FCK:editor>
										</td>
									</tr>
								</table>
							</li>
						</ul>
						<input name="img" id="img" type="hidden" value="${cms.img}">
						<input name="content" id="content" type="hidden">
					</form>
				</div>
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<li>
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="updateCms()"/>
				</li>
			</ul>	
		</div>
		
	</div>
	<!--本页主内容结束 -->
		
  </fmt:bundle>
  <script language="javascript">
  		 //修改网站基本信息
		function updateCms(){
			 	var title=requree_name("name") && requree_length("name",2,80);
				var keyword=requree_name("keyword") && requree_length("keyword",2,80);
				var integer=requree_integer("order") && requree_length("order",1,6);
				var type_z=document.getElementById("type").value;
				if(!title){
					layer.alert("标题为必填项!");
				}else if(!keyword){
	             	layer.alert("请设置关键字!");
	            }else if(!integer){
	             	layer.alert("请输入顺序值，为整数!");
	            }else if(type_z==""||type_z==null){
	             	layer.alert("请选择推荐选项！");
	            }else{
             	var oEditor = FCKeditorAPI.GetInstance("cont");
				$("#content").attr("value",oEditor.GetXHTML(true));
				
			    var params= $('#cmsForm').serialize()
			    $.ajax({
					url:"<%=basePath%>admin/addCms", //后台处理程序
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
			        title: '上传封面',
			        maxmin: true,
			        shadeClose: true, //点击遮罩关闭层
			        area : ['450px' , '320px'],
			        content: '<%=basePath%>admin/goUploadUrl'
			   });
		})
			

  		document.getElementById("type").value="${cms.flag}";
  		
  		
  </script>