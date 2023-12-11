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
	<script src="<%=basePath%>js/My97DatePicker/WdatePicker.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>添加员工</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c">
					<form name="auntForm" id="auntForm">
		        		<input type="hidden" name="coupon_id" value="${coupon_id}" />
		        		<input type="hidden" name="saveFlag" id="saveFlag" value="" />
						<ul>
							<li>
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
									<tr>
										<td class="label_c" width="15%"><label>员工<fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<input type="text" value="${aunt.name}" name="name" id="name" class="wid_60" placeholder="请输入姓名"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="shenfen" bundle="${messagesBundle}"/><fmt:message key="no" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${aunt.no}" name="no" id="no" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="tel" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${aunt.tel}" name="tel" id="tel" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="birthday" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${aunt.birthday}" name="birthday" id="birthday" class="wid_60" onClick="WdatePicker({dateFmt:'yyyy-MM-dd'})"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>员工状态：</label></td>
										<td>
											<select name="flag" id="flag">
									            	<option value="">--请选择--</option>
									            	<option value="1">已经签订合同</option>
									            	<option value="2">适合的</option>
									            	<option value="3">未联系</option>
									            	<option value="4">不适合</option>
									            </select>
										</td>
									</tr>
									<tr style="display:none">
										<td class="label_c"><label>合同开始时间：</label></td>
										<td>
											<input type="text" name="con_stardate" id="con_stardate"  value="${aunt.con_stardate}" class="wid_60" onClick="WdatePicker({dateFmt:'yyyy-MM-dd'})"/>
										</td>
									</tr>
									<tr style="display:none">
										<td class="label_c"><label>合同结束时间：</label></td>
										<td>
											<input type="text" name="con_enddate" id="con_enddate"  value="${aunt.con_enddate}" class="wid_60" onClick="WdatePicker({dateFmt:'yyyy-MM-dd'})"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>简短描述：</label></td>
										<td>
											<textarea class="wid_60" name="desc" id="desc"></textarea>
										</td>
									</tr>
									<tr style="display:none">
										<td class="label_c"><label>擅长类型：</label></td>
										<td>
											<select name="type" id="type" onblur="getAuntAttr()">
									            	<option value="">--请选择--</option>
									            	<c:forEach items="${goodsType}" var="type" varStatus="status">
									            		<option value="${type.id }">${type.name}</option>
									            	</c:forEach>
									            </select>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>头像：</label></td>
										<td>
											<div class="butt_smt" ><fmt:message key="select" bundle="${messagesBundle}"/><fmt:message key="file" bundle="${messagesBundle}"/></div>
										</td>
									</tr>
									<tr>
										<td rowspan="2">
											<div class="fullcolumn_li" id="aunt_attr">
						       		
						       				</div>
										</td>
									</tr>
								</table>
							</li>
						</ul>
					<input name="head" value="${aunt.head}" type="hidden"/>
			        </form>
				</div>
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
				<li>
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="addAunt()"/>
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
			
		function getAuntAttr(){
			var type_id=document.getElementById("type").value;
			 $.ajax({
					 url:"<%=basePath%>admin/goAuntAttr", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:{type_id:type_id},         //要传递的数据
					 success:function(data){
					 	//alert(data.tip);
						$("#aunt_attr").html(data.tip);
					 }
				}); 
		}
		
		function addAunt(){
			 var title=requree_name("name") && requree_length("name",2,80);
             var sfzcer=requree_cer("no");
             var phone=requree_iphone("tel");
             var slezr=document.getElementById("flag").value;
             
        	if(!title){
             	layer.alert("姓名为必填项");
             }else if(!sfzcer){
             	layer.alert("请正确填写身份证号");
             }else if(!phone){
             	layer.alert("请填写正确手机号");
             }else if(slezr==""||slezr==null){
             	layer.alert("请选择状态");
             }else{
             	var params= $('#auntForm').serialize();
			    $.ajax({
					 url:"<%=basePath%>admin/addAunt", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
						 layer.alert(data.tip);
					 }
				});
             }
		}
		document.getElementById("role").value="${admin.role}";
		document.getElementById("sex").value="${admin.adminSex}";
	</script>