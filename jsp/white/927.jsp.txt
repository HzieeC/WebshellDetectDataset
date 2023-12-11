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
	  <fmt:setLocale value="zh_CN"/>
	  <fmt:setBundle basename="messages" var="messagesBundle"/>
	  <fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>外聘员工</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild">
							<a href="<%=basePath%>admin/goAddAunt" class="button-1 button-add">+<fmt:message key="add" bundle="${messagesBundle}"/></a>
						</li>
						<li class="rightchild">
							<a href="javascript:void(0)" onclick="deleteAunt()" class="button-1 button-add">-<fmt:message key="delete" bundle="${messagesBundle}"/></a>
						</li>
					</ul> 
				</div>
				<form name="auntForm" id="auntForm" action="javascript:void(0)">
	  			<table width="100%" border="0" cellspacing="0" cellpadding="0" class="tablelist">
	  				<thead>
					<tr>
		              	<th><input type="checkbox" name="chekall" id="chekall" value="${type.id}" class="ckAll"/></th>
			            <th>员工姓名</th>
			            <th><fmt:message key="shenfen" bundle="${messagesBundle}"/><fmt:message key="no" bundle="${messagesBundle}"/></th>
			            <th><fmt:message key="tel" bundle="${messagesBundle}"/></th>
			            <th><fmt:message key="birthday" bundle="${messagesBundle}"/></th>
			            <th>录入人</th>
			            <th>状态</th>
			            <th><fmt:message key="operation" bundle="${messagesBundle}"/></th>
		            </tr>
		            </thead>
		            <tbody id="list_chckbox">
		            <c:forEach items="${auntList}" var="aunt" varStatus="status">
				      <tr>
				      	<td width="3%"><input type="checkbox" name="aunt_id" value="${aunt.id}"/></td>
				        <td width="8%"><a href="javascript:void(0)" onclick="lookAunt(${aunt.id})" class="text_dec">${aunt.name}</a></td>
				        <td width="12%">${aunt.no}</td>
				        <td width="8%">${aunt.tel}</td>
				        <td width="8%">${aunt.birthday}</td>
				        <td width="5%">${aunt.decretion_name}</td>
				        <td width="8%">
				        	<c:if test="${aunt.flag ==1}">
				        		已经签订合同
				        	</c:if>
				        	<c:if test="${aunt.flag ==2}">
				        		适合
				        	</c:if>
				        	<c:if test="${aunt.flag ==3}">
				        		未联系
				        	</c:if>
				        	<c:if test="${aunt.flag ==4}">
				        		不合适
				        	</c:if>
				        </td>
				        <td width="16%">
				        	<a onclick="updateAunt(${aunt.id})" href="javascript:void(0)" class="red">修改状态</a>
				        	<a onclick="lookAuntImg(${aunt.id})" href="javascript:void(0)" class="orang">相册</a>
				        	<a onclick="lookCommentList(${aunt.id})" href="javascript:void(0)" class="blue">评论</a>
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
				<jsp:setProperty property="url" value="/admin/goAdminAuntList?flag=${flag}" name="paging"/>
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
		
		 
	  	function updateAunt(aunt_id){
	  		layer.open({
			  type: 2,
			  title: '修改员工信息',
			  shadeClose: true,
			  shade: 0.8,
			  area: ['300px', '220px'],
			  content: '<%=basePath%>/admin/goAddAunt?aunt_id='+aunt_id+'',
				end:function(index){
					window.location.reload();
				}
			}); 
	  	}
	  	
	  	//查看相册
	  	function lookAuntImg(aunt_id){
	  		layer.open({
			  type: 2,
			  title: '查看员工相册',
			  shadeClose: true,
			  shade: 0.8,
			  area: ['600px', '400px'],
			  content: '<%=basePath%>admin/myAdmin?tag=goAuntImg&aunt_id='+aunt_id+'',
				end:function(index){
					window.location.reload();
				}
			});
	  	}
	  	
	  	//查看评论信息
	  	function lookCommentList(aunt_id){
	  		layer.open({
			  type: 2,
			  title: '评论信息',
			  shadeClose: true,
			  shade: 0.8,
			  area: ['800px', '500px'],
			  content: '<%=basePath%>admin/myAdmin?tag=getAuntComment&aunt_id='+aunt_id+'',
				end:function(index){
					window.location.reload();
				}
			});
	  	}

	  	function lookAunt(aunt_id){
	  		layer.open({
			  type: 2,
			  title: '查看详情',
			  shadeClose: true,
			  shade: 0.8,
			  area: ['600px', '400px'],
			  content: '<%=basePath%>/admin/lookAuntDetial?aunt_id='+aunt_id+'' //iframe的url
			}); 
	  	}
	  	

		function deleteAunt(){
			var params= $('#auntForm').serialize()
			if(confirm("请确认是否删除此数据")){
				 $.ajax({
					url:"<%=basePath%>admin/deleteAunt", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:params,         //要传递的数据
					success:function(data){
						layer.alert(data.tip);
						window.location.reload();
					}
				});
			}
		}
		
	  </script>