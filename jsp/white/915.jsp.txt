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
	<script src="<%=basePath%>js/public.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>

	  <fmt:setLocale value="zh_CN"/>
	  <fmt:setBundle basename="messages" var="messagesBundle"/>
	  <fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>订单管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<form action="<%=basePath%>/admin/myAdmin?tag=goGoodsOrderByTel" method="post">
							<li>
								<input type="text" value="${tel}" placeholder="请输入订单电话号码"  class="search_inputa"  name="tel" id="person_tel"/>
							</li>
							<li>
								<input type="submit"  value="<fmt:message key="search" bundle="${messagesBundle}"/>" class="button-4"/>
							</li>
						</form>
					</ul>
				</div>
				<form name="brandForm" id="brandForm" action="javascript:void(0)">
					<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
				      <thead>
				          <tr>
				              <th width="15%"><fmt:message key="no" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="state" bundle="${messagesBundle}"/></th>
				              <th width="10%">金额</th>
				              <th width="10%"><fmt:message key="tel" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="name" bundle="${messagesBundle}"/></th>
				              <th width="10%">身份证号码</th>
				              <th width="10%">服务备注</th>
				              <th width="10%">接送机</th>
				              <th width="15%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
				          </tr>
				      </thead>
				      <c:forEach items="${orderList}" var="order" varStatus="status">
					      <tr>
					        <td>${order.no}</td>
					        <td>
					        	<c:if test="${order.flag==1}">
						        		<span class="orang">未处理</span>
						        </c:if>
						        <c:if test="${order.flag==2}">
						        		<span class="green">已处理</span>
						        </c:if>
						        <c:if test="${order.flag==3}">
						        		<span class="green">已完成</span>
						        </c:if>
						        <c:if test="${order.flag==4}">
						        		<span class="red">已退单</span>
						        </c:if>
						        <c:if test="${order.flag==5}">
						        		<span class="green">派遣中</span>
						        </c:if>
						        <c:if test="${order.flag==6}">
						        		<span class="green">人员确定</span>
						        </c:if>
						        <c:if test="${order.flag==7}">
						        		<span class="red">异常结束</span>
						        </c:if>
					        </td>
					        <td>${order.money}</td>
					        <td>${order.address_tel}</td>
					        <td>${order.address_person}</td>
					        <td>${order.address_content}</td>
					        <td>${order.content}</td>
					        <td>${order.desc}</td>
					        <td>
					        	<div class="tablehand">
					        		<c:if test="${order.flag!=7 && order.flag!=3}">
						        		<a href="javascript:void(0)" onclick="javascript:endOrder(${order.id})" class="green">结束</a>
						        		<a href="javascript:void(0)" onclick="javascript:receiv(${order.id})" class="red">收款</a>
						      		</c:if>
						      		
						      		<c:if test="${order.store_id==null || order.store_id==''}">
					        			<a href="javascript:void(0)" onclick="javascript:distribut(${order.id})" class="green"> 
						        			分配门店
						        		</a>
					        		</c:if>
						        	<a onclick="lookMoneyRecord(${order.id})" href="javascript:void(0)" class="blue">交易记录</a>
						        	<a onclick="lookGoodsRecord(${order.id})" href="javascript:void(0)" class="orang">所有商品</a>
					        	</div>
					        </td>
					      </tr>
				      </c:forEach>
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
				<jsp:setProperty property="url" value="/admin/myAdmin?tag=goGoodsOrderList&type=${type}&flag=${flag}" name="paging"/>
				${paging.pageString}
		     </ul>
		</div>
	</div>	
		
	  </fmt:bundle>
	  <script language="javascript">
	  	function lookWaterOrder(order_id){
	  		layer.open({
		  		type: 2,
				title: "查看订单",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content: '<%=basePath%>admin/lookOrderFlowWater?order_id='+order_id+''
		  	});
	  	}
	  
	  	function distributeAunt(order_id){
	  		layer.open({
		  		type: 2,
				title: "查看订单",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content: '<%=basePath%>amin/goDistributeAunt?order_id='+order_id+''
		  	});
		}
		//交易记录
		function lookMoneyRecord(order_id){
			layer.open({
		  		type: 2,
				title: "查看交易记录",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['600px', '400px'],
				content:'<%=basePath%>admin/goOrderMoneyRecord?order_id='+order_id+''
		  	});
		}
		//商品信息
		function lookGoodsRecord(order_id){
			layer.open({
		  		type: 2,
				title: "查看所有商品",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['600px', '400px'],
				content:'<%=basePath%>admin/myAdmin?tag=getGoodsPojoByOrder&order_id='+order_id+''
		  	});
		}
		
		function lookAuntForOrder(order_id){
			layer.open({
		  		type: 2,
				title: "",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content:'<%=basePath%>admin/goAuntListForOrder?order_id='+order_id+''
		  	});
		  	
		}
		
	  	function distribut(order_id){
	  		layer.open({
		  		type: 2,
				title: "",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['300px', '200px'],
				content:'<%=basePath%>admin/myAdmin?tag=goDistributGoodsOrder&order_id='+order_id+''
		  	});
	  	}
	  	
	  	function sigerConnect(order_id){
	  		layer.open({
		  		type: 2,
				title: "",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content:'<%=basePath%>admin/goUpdateOrder?order_id='+order_id+'&flag=3'
		  	});
	  	}
	  	
	  	function addContent(flag,infoid){
	  		layer.open({
		  		type: 2,
				title: "",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content:'<%=basePath%>/admin/goAddContent?infoid='+infoid+'&flag='+flag+''
		  	});
	  	}
	  	
	  	function lookContent(flag,infoid){
	  		layer.open({
		  		type: 2,
				title: "",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content:'<%=basePath%>/admin/goContentList?infoid='+infoid+'&flag='+flag+''
		  	});
	  	}
	  	function updateServiceNum(order_id){
	  		layer.open({
		  		type: 2,
				title: "",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content:'<%=basePath%>admin/goUpdateOrder?order_id='+order_id+'&flag=2'
		  	});
	  	}
	  	//结束订单
	  	function endOrder(order_id){
	  		layer.open({
		  		type: 2,
				title: "结束订单",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['450px', '300px'],
				content:'<%=basePath%>admin/goUpdateOrder?order_id='+order_id+'&flag=1',
				end:function(index){
					window.location.reload();
				}
		  	});
	  	}
	  	//收款
	  	function receiv(order_id){
	  		layer.open({
		  		type: 2,
				title: "收款",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['450px', '300px'],
				content:'<%=basePath%>admin/goReceivAbles?order_id='+order_id,
				end:function(index){
					window.location.reload();
				}
		  	});
	  	}
	  	
	  	function addGoodsType(){
	  		layer.open({
		  		type: 2,
				title: "",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content:'<%=basePath%>admin/goAddGoodsType'
		  	});
		  	
	  	}
	  	
	  	function updateGoodsType(type_id){
	  		layer.open({
		  		type: 2,
				title: "",
				shadeClose: true,
				shade: 0.8,
				maxmin: true, //开启最大化最小化按钮
				area: ['800px', '500px'],
				content:'<%=basePath%>admin/goAddGoodsType?type_id='+type_id
		  	});
	  	}
	  	
		//删除
		function deleteGoodsBrand(){
			var params= $('#brandForm').serialize()
			if(confirm("请确认是否删除此数据")){
				 $.ajax({
					url:"<%=basePath%>admin/deleteGoodsBrand", //后台处理程序
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