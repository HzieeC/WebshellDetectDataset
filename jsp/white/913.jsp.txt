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
		<div class="title_c"><span>系统模块设置</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c">
			<div class="middle_cnt_cont_d">
				<form name="privaliteForm" id="privaliteForm">
					<div class="toolbar clear">
						<ul class="handlist-left">
							 <li>
						        <input type="button" onclick="addChidren(0,1)" value="<fmt:message key="sentences11" bundle="${messagesBundle}"/>" class="button-4"/>
						     </li>
						     <li>
						        <input type="button" id="btn_open" value="<fmt:message key="all" bundle="${messagesBundle}"/><fmt:message key="open" bundle="${messagesBundle}"/>" class="button-4"/>
						     </li>
						     <li>
						        <input type="button" id="btn_close" value="<fmt:message key="all" bundle="${messagesBundle}"/><fmt:message key="close" bundle="${messagesBundle}"/>" class="button-4"/>
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
					urlstr = "<div><span class='frt1'>"+ o[i]["name"] +"</span><a href='javascript:void(0)' class='orang' onclick='addChidren("+o[i]["id"]+","+o[i]["is_not_cms"]+")'>添加子模块</a><a href='javascript:void(0)' onclick='update("+o[i]["id"]+")' class='green'>修改</a><a href='javascript:void(0)' onclick='deleteModular("+o[i]["id"]+")' class='red'>删除</a><ul>";
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
		function addChidren(id,is_cms){
			layer.open({
		        type: 2,
		        title: '添加模块',
		        maxmin: false,
		        shadeClose: true, //点击遮罩关闭层
		        area : ['500px' , '300px'],
		        content: '<%=basePath%>admin/goAddModular?parentId='+id+'&is_cms='+is_cms+'',
				end:function(index){
					window.location.reload();
				}
		    });
		}
		
		function update(id){
			layer.open({
		        type: 2,
		        title: '修改模块',
		        maxmin: false,
		        shadeClose: true, //点击遮罩关闭层
		        area : ['500px' , '300px'],
		        content: '<%=basePath%>admin/goAddModular?myId='+id+'',
				end:function(index){
					window.location.reload();
				}
		    });
		}
		
		function deleteModular(id){
			if(confirm("请确认是否删除此模块")){
				$.ajax({
					 url:"<%=basePath%>admin/deleteModular", //后台处理程序
					 type:'post',         //数据发送方式
					 dataType:'json',
					 data:{id:id},         //要传递的数据
					 success:function(data){
					 	alert(data.tip);
					 	window.location.reload();
					 }
				}); 
			}
			
			
		}
		
	</script>