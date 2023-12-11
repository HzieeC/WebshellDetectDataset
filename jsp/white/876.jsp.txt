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
	<script src="<%=basePath%>js/public.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<script src="<%=basePath%>js/validate.js" type="text/javascript"></script>
	  <fmt:setLocale value="zh_CN"/>
	  <fmt:setBundle basename="messages" var="messagesBundle"/>
	  <fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>促销管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<a href="javascript:void(0)" onclick="javascript:deleteThematicActivity()" class="button-1 button-add">-<fmt:message key="delete" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="javascript:addActive()"  class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
					</ul>
				</div>
				<form name="thematicForm" id="thematicForm" action="javascript:void(0)">
					<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
				      <thead>
				          <tr>
				              <th width="5%"><input type="checkbox" name="chekall" id="chekall" class="ckAll" value="${active.id}"/></th>
				              <th width="25%">标题</th>
				              <th width="15%">名称</th>
				              <th width="5%">状态</th>
				              <th width="10%">发布时间</th>
				              <th width="10%">到期时间</th>
				              <th width="15%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
				          </tr>
				      </thead>
				      <tbody id="list_chckbox">
				      <c:forEach items="${activeList}" var="active" varStatus="status">
					      <tr>
					      	<td><input type="checkbox" name="thematic_id" value="${active.id}"/></td>
					        <td>${active.title}</td>
					        <td>${active.name}</td>
					        <td>
					        	<c:if test="${active.flag==1}">
					        		<span class="red">关闭</span>
					        	</c:if>
					        	<c:if test="${active.flag==2}">
					        		<span class="green">发布</span>
					        	</c:if>
					        </td>
					        <td>${active.startDate}</td>
					        <td>${active.endDate}</td>
					        <td>
					        	<div class="tablehand">
					        		<a href="javascript:void(0)" onclick="goUpdateActive(${active.id})" class="red">修改</a>
					        		<c:if test="${active.flag==2}">
						        		<a href="javascript:void(0)" onclick="updateThematicFlag(${active.id},1)" class="blue">关闭</a>
						        	</c:if>
						        	<c:if test="${active.flag==1}">
						        		<a href="javascript:void(0)" onclick="updateThematicFlag(${active.id},2)" class="green">开启</a>
						        	</c:if>
					        	</div>
					        </td>
					      </tr>
				      </c:forEach>
				      </tbody>
				    </table>
			    </form>
			</div>
		</div>
		
		<!--分页 -->
		<div class="right_bottom_btnlist">
			<ul class="page_num f_l">
				<jsp:useBean id="paging" scope="page" class="com.weishang.bean.Page"/>
				<jsp:setProperty property="user" value="admin" name="paging"/>
				<jsp:setProperty property="crrent" value="${pageNo}" name="paging"/>
				<jsp:setProperty property="suffix" value="" name="paging"/>
				<jsp:setProperty property="sumPage" value="${sum}" name="paging"/>
				<jsp:setProperty property="url" value="/admin/thematic?tag=goActivity" name="paging"/>
				${paging.pageString}
		     </ul>
		</div>
	</div>
	  </fmt:bundle>
	  <script language="javascript">
	  	//全选   
		 $('.ckAll').click(function(){
			$("#list_chckbox :checkbox").each(function(){
				$(this).prop("checked",!!$("#chekall").prop("checked"));
			});
		});
		  function addActive(){
		  	layer.open({
		  		type: 2,
				title: "添加专题活动",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content: '<%=basePath%>admin/thematic?tag=goAddActivity',
				end:function(index){
					window.location.reload();
				}
		  	});
		  }
		  
		  function goUpdateActive(id){
		  	layer.open({
		  		type: 2,
				title: "修改专题活动",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content: '<%=basePath%>admin/thematic?tag=goUpdateThematicMarket&thematic_id='+id+'',
				end:function(index){
					window.location.reload();
				}
		  	});
		  }
		  
		  function deleteThematicActivity(){
		  	var chk=requree_checkbox("thematic_id");
		  	if(chk){
			  	if(confirm("请确认是否删除专题活动信息")){
			  		var params= $('#thematicForm').serialize();
					layer.msg('加载中', {icon: 16,time: 1000},function(){
						$.ajax({
							url:"<%=basePath%>admin/thematic?tag=deleteActivities", //后台处理程序
							type:'post',         //数据发送方式
							dataType:'json',
							data:params,         //要传递的数据
							success:function(data){
								alert(data.tip);
								window.location.reload();
							}
						}); 
					}); 
			  	}
		  	}else{
		  		layer.alert("请至少选中一个活动");
		  	}
		  }
		  
		  function updateThematicFlag(thematic_id,flag){
		  	layer.msg('加载中', {icon: 16,time: 1000},function(){
				$.ajax({
					url:"<%=basePath%>admin/thematic?tag=updateThematicFlag", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:{thematic_id:thematic_id,flag:flag},         //要传递的数据
					success:function(data){
						alert(data.tip);
						window.location.reload();
					}
				}); 
			}); 
		  }
	  </script>