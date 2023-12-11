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
		<div class="title_c"><span>权限分配</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c">
			<div class="middle_cnt_cont_d">
			
				<form name="privaliteForm" id="privaliteForm">
					<div class="toolbar clear">
						<ul class="handlist-left">
						     <li>
						        	<select name="roleList" id="role">
						        		<option value="">--请选择--</option>
						        		 <c:forEach items="${roleList}" var="role" varStatus="status">
						        			<option value="${role.id}">${role.name }</option>
						        		</c:forEach>
						        	</select>
						     </li>
						     <li>
						        <input type="button" onclick="add()" value="<fmt:message key="distribut" bundle="${messagesBundle}"/>" class="button-4"/>
						     </li>
						     <li>
						        <input type="button" id="btn_open" value="<fmt:message key="all" bundle="${messagesBundle}"/><fmt:message key="open" bundle="${messagesBundle}"/>" class="button-4"/>
						     </li>
						     <li>
						        <input type="button" id="btn_close" value="<fmt:message key="all" bundle="${messagesBundle}"/><fmt:message key="close" bundle="${messagesBundle}"/>"  class="button-4"/>
						     </li>
						 </ul> 
					</div>
				
					<div id="menuTree" class="content menuTree">
						
					</div>
				</form>
			</div>
		</div>
	</div>
	</fmt:bundle>
	<script language="javascript">
		var json=${mlt};
		/*递归实现获取无级树数据并生成DOM结构*/
		var str = "";
		var forTree = function(o){
			for(var i=0;i<o.length;i++){
				var urlstr = "";
				try{
					urlstr = "<div><input type='checkbox' checked name='mod_id' id='"+ o[i]["id"] +"' value="+ o[i]["id"] +"><span>"+ o[i]["name"] +"</span><ul>";
					str += urlstr;
					if(o[i]["children"] != null){
						forTree(o[i]["children"]);
					}
					str += "</ul></div>";
				}catch(e){}
			}
			return str;
		}
		/*添加无级树*/
		document.getElementById("menuTree").innerHTML = forTree(json);
		/*树形菜单*/
		var menuTree = function(){
			//给有子对象的元素加[+-]
			$("#menuTree ul").each(function(index, element) {
				var ulContent = $(element).html();
				var spanContent = $(element).siblings("span").html();
		        if(ulContent){
					$(element).siblings("span").html("[+] " + spanContent)	
				}
		    });
			$("#menuTree").find("div span").click(function(){
				var ul = $(this).siblings("ul");
				var spanStr = $(this).html();
				var spanContent = spanStr.substr(3,spanStr.length);
				if(ul.find("div").html() != null){
					if(ul.css("display") == "none"){
						ul.show(300);
						$(this).html("[-] " + spanContent);
					}else{
						ul.hide(300);
						$(this).html("[+] " + spanContent);
					}
				}
			})
		}()
		/*展开*/
		$("#btn_open").click(function(){
			$("#menuTree ul").show(300);
			curzt("-");
		})
		/*收缩*/
		$("#btn_close").click(function(){
			$("#menuTree ul").hide(300);
			curzt("+");
		})
		
		function curzt(v){
			$("#menuTree span").each(function(index, element) {
				var ul = $(this).siblings("ul");
		        var spanStr = $(this).html();
				var spanContent = spanStr.substr(3,spanStr.length);
				if(ul.find("div").html() != null){
					$(this).html("["+ v +"] " + spanContent);
				}
		    });	
		}
		$("#menuTree ul").show(300);
		curzt("-");
		
		function add(){
			 $("form :input.required").trigger('blur');
             var numError = $('form .onError').length;
             if(numError){
                return false;
             }else{
		   		var params= $('#privaliteForm').serialize()
			    $.ajax({
					 url:"<%=basePath%>admin/privalite", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:params,         //要传递的数据
					 success:function(data){
						 alert(data.tip);
					 }
				}); 
		   	 }
		}
	</script>