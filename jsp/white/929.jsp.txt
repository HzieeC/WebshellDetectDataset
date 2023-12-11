<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
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
		        		<input type="hidden" name="item" value="${item}" />
				        <table border="0" cellpadding="0" cellspacing="0" class="table_c1">
				        	<tr>
								<td class="label_c" width="15%"><label><fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
								<td width="85%">
									<input type="text" value="${userType.name}" name="name" id="name" class="wid_90"/>
								</td>
							</tr>
							 <tr>
								<td class="label_c"><label><fmt:message key="role" bundle="${messagesBundle}"/><fmt:message key="aut" bundle="${messagesBundle}"/>：</label></td>
								<td style="height:105px; position:relative; top:10px;">
									<select id="role_list" name="role_list"  multiple="multiple" style="position:absolute;left:0;top:0;width:150px;height:100px;">
												   <c:forEach items="${cp1}" var="cp1" varStatus="status">
												    <option value="${cp1.id }" selected>${cp1.name }</option>
												   </c:forEach>
												</select>
												<input type="button" style="position:absolute;margin:0;padding:0;  left:170px; top:60px; width:100px; height:30px;line-height:30px;" value="右移" onclick="moveOptions('role_list','role_list_to')"/>
												<input type="button" style="position:absolute;margin:0;padding:0;  left:170px; top:20px; width:100px; height:30px; line-height:30px;"  value="左移" onclick="moveOptions('role_list_to','role_list')"/>
												<select id="role_list_to" name="role_list_to" multiple="multiple"  style=" position:absolute; left:290px;top:0;width:150px;height:100px;">
													<c:forEach items="${cp2}" var="cp2" varStatus="status">
												    <option value="${cp2.id }" selected>${cp2.name }</option>
												   </c:forEach>
												</select>
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
					<input type="submit" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter" onclick="add()"/>
				</li>
			</ul>	
		</div>
		
	</div>
	<!--本页主内容结束 -->
	</fmt:bundle>
	<script>
		// 设置内容层高度
		var height = $(window).height();
		$(".middle_cnt_c2").height(height-80);
		$(window).resize(function () {          //当浏览器大小变化时
			    var height = $(window).height();
				$(".middle_cnt_c2").height(height-80);
		});
		
		//把一个select 中的项移到另一个select中
		function moveOptions(from,to){
		    var oldname=$("#"+from+"  option:selected");
		    if(oldname.length==0){
		        return;
		    }
		    
		    var valueOb = {};
		    $("#" + to).find("option").each(function(){
		        valueOb[String($(this).val())] = $(this);
		    });
		    
		    for( var i =0;i< oldname.length; i++){
		       if(valueOb[String($(oldname[i]).val())] == undefined){
		               $(oldname[i]).clone().appendTo($("#"+to))
		               //$("#role_list_to").find("option[text='"+oldname[i]).val()+"']").attr("selected",true); 
		               $(oldname[i]).remove();
		       }        
		    }
		    
		}
		
		function add(){
		 	 var title=requree_name("name") && requree_length("name",2,80);
		 	 var slezr=document.getElementById("role_list_to").value;
             if(!title){
				layer.alert("角色名称为必填项!");
			 }else if(slezr==""||slezr==null){
             	layer.alert("请为角色分配权限!");
             }else{
	            var params= $('#roleForm').serialize()
			    $.ajax({
					 url:"<%=basePath%>admin/addUserType", //后台处理程序
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
		
	</script>