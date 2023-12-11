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
		<div class="title_c"><span>会员管理</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<form action="<%=basePath%>admin/goSearchUser" method="get" name="usersearch" id="usersearch">
							<li>
								<input type="text" placeholder="请输入电话或用户名、邮箱" value="${tel}" class="search_inputa" name="tel" id="tel"/>
							</li>
							<li>
								<input type="submit"  value="<fmt:message key="search" bundle="${messagesBundle}"/>" class="button-4 button-search"/>
							</li>
						</form>
						<li class="rightchild">
							<a href="javascript:void(0);" onclick="add()" target="main" class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="javascript:void(0);" onclick="updateMoney('扣费','reduce')" target="main" class="button-1 button-add">扣费</a>
						</li>
						
						<li class="rightchild">
							<a href="javascript:void(0);" onclick="updateMoney('充值','add')" target="main" class="button-1 button-add">充值</a>
						</li>
					</ul> 
				</div>
				<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
			      <thead>
			          <tr>
			              <th width="10%"><fmt:message key="name" bundle="${messagesBundle}"/></th>
			              <th width="10%"><fmt:message key="username" bundle="${messagesBundle}"/></th>
			              <th width="10%"><fmt:message key="role" bundle="${messagesBundle}"/></th> 
			              <th width="10%"><fmt:message key="tel" bundle="${messagesBundle}"/></th>
			              <th width="10%"><fmt:message key="email" bundle="${messagesBundle}"/></th>
			              <th width="10%"><fmt:message key="state" bundle="${messagesBundle}"/></th>
			              <th width="8%">余额</th> 
			              <th width="8%">积分</th>
			              <th width="20%"><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
			          </tr>
			      </thead>
			      <tbody id="list_chckbox">
			      <c:forEach items="${userList}" var="user" varStatus="status">
				      <tr>	      
				        <td>${user.name}</td>
				        <td>${user.username }</td>
				        <td>${user.typeName}</td>
				        <td>${user.tel}</td>
				        <td>${user.email}</td>
				        <c:if test="${user.flag==1}">
				        	<td><span class="green">${user.flagName}</span></td>
				        </c:if>
				        <c:if test="${user.flag==2}">
				        	<td><span class="red">${user.flagName}</span></td>
				        </c:if>
				        
				        <td>${user.money}</td>
				        <td>${user.integel}</td>
				        <td>
				        	<div class="tablehand">
					        	<a href="javascript:void(0)" onclick="update(${user.id})" class="red">
					        		<fmt:message key="modify" bundle="${messagesBundle}"/>
					        	</a>
					        	<a href="javascript:void(0)" onclick="lookRecharge('充值记录',${user.id},1)" class="green">
					        		充值记录
					        	</a>
					        	<a href="javascript:void(0)" onclick="lookRecharge('扣费记录',${user.id},2)" class="blue">
					        		扣费记录
					        	</a>
				        	</div>
				        </td>
				      </tr>
			      </c:forEach>
			      </tbody>
			    </table>
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
				<jsp:setProperty property="url" value="/admin/goUser" name="paging"/>
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
		// 添加
		function add(){
			layer.open({
				  type: 2,
				  title: '添加会员',
				  shadeClose: true,
				  shade: 0.8,
				  area: ['600px', '400px'],
				  content: '<%=basePath%>admin/goAddUser',
				end:function(index){
					window.location.reload();
				}
			}); 
		}
		
		function lookRecharge(name,user_id,direction){
	  		layer.open({
	  			  type: 2,
			      title: name,
			      shadeClose: true,
			      shade: false,
			      maxmin: true, //开启最大化最小化按钮
			      area: ['700px', '400px'],
			      content: '<%=basePath%>admin/goUserCapitalTransactions?user_id='+user_id+'&direction='+direction+'',
				end:function(index){
					window.location.reload();
				}
	  		});
	  	}
	  	
	  	function updateMoney(name,action){
			layer.open({
	  			  type: 2,
			      title:name,
			      shadeClose: true,
			      shade: false,
			      maxmin: true, //开启最大化最小化按钮
			      area: ['500px', '350px'],
			      content: '<%=basePath%>admin/goUpdateUserMoney?action='+action+'',
				end:function(index){
					window.location.reload();
				}
	  		});
		}
		
		function update(id){
			layer.open({
				  type: 2,
				  title: '修改会员信息',
				  shadeClose: true,
				  shade: 0.8,
				  area: ['600px', '400px'],
				  content: '<%=basePath%>admin/goAddUser?item='+id+'',
				end:function(index){
					window.location.reload();
				}
			});
		}
	</script>