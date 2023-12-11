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
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<div class="middle_cnt_cont_d">
				<div class="toolbar clear">
					<ul class="handlist-left">
						<li class="rightchild" style="float:left">
							<a href="javascript:void(0)" onclick="batchToExamine(1)"  class="button-2 button-add">批量运行</a>	
						</li>
						<li class="rightchild" style="float:left">
							<a href="javascript:void(0)" onclick="batchToExamine(2)"  class="button-2 button-add">批量关闭</a>	
						</li>
					</ul> 
				</div>
				<table border="0" cellspacing="0" cellpadding="0" class="tablelist" width="100%">
				
			      <thead>
			          <tr>
			              <th align="left"><input type="checkbox" name="chk_all" id="chk_all" /></th>
			              <th align="left"><fmt:message key="title" bundle="${messagesBundle}"/></th>
			              <th align="left"><fmt:message key="time" bundle="${messagesBundle}"/></th>
			              <th align="left"><fmt:message key="username" bundle="${messagesBundle}"/></th>
			              <th align="left"><fmt:message key="head" bundle="${messagesBundle}"/></th>
			              <th align="left"><fmt:message key="state" bundle="${messagesBundle}"/></th>
			              <th align=""><center><fmt:message key="operation" bundle="${messagesBundle}"/></center></th>
			          </tr>
			      </thead>
			      
			      <form action="javascript:void(0)" id="dataForm" name="dataForm">
				      <c:forEach items="${commentList}" var="commont" varStatus="status">
					      <tr>
					      	<td><input type="checkbox" name="cid" value="${commont.id}" /></td>
					        <td>${commont.title}</td>
					        <td>${commont.date}</td>
					        <td>${commont.user}</td>
					        <td>
					        	<img src="<%=basePath%>${commont.userImg}" width="100px" height="100px"/>
					        </td>
					        <td>
						        <c:if test="${commont.flag==1}">
						        	运行
						        </c:if>
						        <c:if test="${commont.flag==2}">
						        	关闭
						        </c:if>
					        </td>
					        <td>
					        	<c:if test="${commont.flag==1}">
					        		<a href="javascript:void(0)" onclick="updateComment(${commont.id},2)" class="a1">关闭</a>
					        	</c:if>
					        	<c:if test="${commont.flag==2}">
					        		<a href="javascript:void(0)" onclick="updateComment(${commont.id},1)" class="a1">运行</a>
					        	</c:if>
					        	<input type="hidden" id="${commont.id}" name="${commont.id}" value="${commont.content}"/>
					        	<a href="javascript:void(0)" onclick="look(${commont.id})" class="a1">查看评论详情</a>
					        	<a href="javascript:void(0)" onclick="deleteComment(${commont.id})" class="a1">删除</a>
					        </td>
					      </tr>
				      </c:forEach>
				      <input type="hidden" name="commentFlag" id="commentFlag" value=""/>
			   	  </form>
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
				<jsp:setProperty property="url" value="/admin/getAllComment" name="paging"/>
				${paging.pageString}
		     </ul>
		</div>
	</div>
	</fmt:bundle>
	  <script language="javascript">
	  	$(function(){
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-80);
			$(window).resize(function () {          //当浏览器大小变化时
			    var height = $(window).height();
				$(".middle_cnt_c2").height(height-80);
			});
		})
	  	$("#chk_all").click(function(){
	  		$('input[name="cid"]').attr("checked",this.checked);
		});
		
		var $subBox = $("input[name='cid']"); 
            $subBox.click(function(){
            $("#chk_all").attr("checked",$subBox.length == $("input[name='cid']:checked").length ? true : false);
        });
		
	  	function look(id){
			layer.open({
			  type: 2,
			  title: '评论信息',
			  shadeClose: true,
			  shade: 0.8,
			  area: ['800px', '500px'],
			  content: 'document.getElementById(id).value' //iframe的url
			});
		}
		
		function batchToExamine(flag){
			var chkbs = document.getElementsByName("cid");
			var j = 0; // 用户选中的选项个数
			for(var i=0;i<chkbs.length;i++){
			  if(chkbs[i].checked){
			    j++;
			  }
			}
			if(j==0){
			    alert("请选择评论");
			}else{
			   var params= $('#dataForm').serialize()
			   //document.getElementById("commentFlag").value=flag;
				$.ajax({
					url:"<%=basePath%>admin/batchToExamine?commentFlag="+flag, //后台处理程序
					type:'post',//数据发送方式
					dataType:'json',
					data:params,//要传递的数据
					success:function(data){
						alert(data.tip);
						window.location.reload();
					}
				}); 
			}
		}
		
		function updateComment(id,flag){
			$.ajax({
				url:"<%=basePath%>admin/updateComment", //后台处理程序
				type:'post',//数据发送方式
				dataType:'json',
				data:{id:id,flag:flag},//要传递的数据
				success:function(data){
					alert(data.tip);
					window.location.reload();
				}
			}); 
		}
		
		function deleteComment(cid){
			if(confirm("请确认是否要删除此评论")){
				$.ajax({
					url:"<%=basePath%>admin/deleteComment", //后台处理程序
					type:'post',//数据发送方式
					dataType:'json',
					data:{cid:cid},//要传递的数据
					success:function(data){
						layer.alert(data.tip);
						window.location.reload();
					}
				}); 
			}
		}
		
	  </script>