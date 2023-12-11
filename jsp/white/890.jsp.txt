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
		<div class="title_c"><span>门店管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<a href="javascript:void(0)" onclick="javascript:deleteSotre()" class="button-1 button-add">-<fmt:message key="delete" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="<%=basePath%>/admin/goAddStore"  class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
					</ul> 
				</div>
				<form name="sotreForm" id="sotreForm" action="javascript:void(0)">
					<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
				      <thead>
				          <tr>
				              <th width="5%"><input type="checkbox" name="chekall" id="chekall" class="ckAll" value="${type.id}"/></th>
				              <th width="15%"><fmt:message key="name" bundle="${messagesBundle}"/></th>
				              <th width="25%"><fmt:message key="address" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="tel" bundle="${messagesBundle}"/></th>
				              <th width="10%"><fmt:message key="state" bundle="${messagesBundle}"/></th>
				              <th width="8%">会员数</th>
			              	  <th width="8%">总余额</th>
				              <th width="20%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
				          </tr>
				      </thead>
				      <tbody id="list_chckbox">
				      <c:forEach items="${storeList}" var="store" varStatus="status">
					      <tr>
					      	<td><input type="checkbox" name="store_id" value="${store.id}"/></td>
					        <td>${store.name}</td>
					        <td>${store.address }</td>
					        <td>${store.tel}</td>
					        <c:if test="${store.flag==1}">
					        	<td><span class="green">运行</span></td>
					        </c:if>
					         <c:if test="${store.flag==2}">
					        	<td><span class="red">关闭</span></td>
					        </c:if>
					        <td>${store.userNum}</td>
				            <td>${store.moneyNum}</td>
					        <td>
					        	<div class="tablehand">
					        		<a href="<%=basePath%>/admin/goAddStore?store_id=${store.id}" class="red"><fmt:message key="modify" bundle="${messagesBundle}"/></a>
					        		<a href="javascript:void(0)" onclick="lookRecharge('充值报表',${store.id},1)" class="blue">充值报表</a>
					        		<a href="javascript:void(0)" onclick="lookRecharge('收入报表',${store.id},2)" class="yionet">收费报表</a>
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
				<jsp:setProperty property="url" value="/admin/goStore" name="paging"/>
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
		//插看报表
	  	function lookRecharge(name,store_id,direction){
	  		layer.open({
	  			  type: 2,
			      title: name,
			      shadeClose: true,
			      shade: false,
			      maxmin: true, //开启最大化最小化按钮
			      area: ['700px', '400px'],
			      content: '<%=basePath%>admin/goCapitalTransactions?store_id='+store_id+'&direction='+direction+'',
				end:function(index){
					window.location.reload();
				}
	  		});
	  	}
	  	
	  	
		
		function deleteSotre(){
			var params= $('#sotreForm').serialize()
			if(confirm("请确认是否删除此数据")){
				 $.ajax({
					url:"<%=basePath%>admin/deleteSotre", //后台处理程序
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