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
	<link rel="stylesheet" href="<%=basePath%>css/tanchu.css" />
	<link rel="stylesheet" href="<%=basePath%>css/admin_lay.css" />
	<script src="<%=basePath%>js/jquery-1.11.2.min.js"></script>
	<script src="<%=basePath%>js/validate.js"></script>
	<script src="<%=basePath%>js/public.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	  <fmt:setLocale value="zh_CN"/>
	  <fmt:setBundle basename="messages" var="messagesBundle"/>
	  <fmt:bundle basename="messagesAllMessage">
	  <div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>添加前台导航模块</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c">
		        	<form name="receptForm" id="receptForm">
		        		<input type="hidden" name="parent" value="${parent}">
		        		<input type="hidden" name="id" value="${recept.id}">
		        		<input type="hidden" name="is_self" value="${is_self}">
				       <ul>
							<li>
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
									<tr>
										<td class="label_c" width="15%"><label><fmt:message key="name" bundle="${messagesBundle}"/>：</label></td>
										<td width="85%">
											<input type="text" value="${recept.name}" name="name" id="name" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="title" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${recept.title}" name="title" id="title" class="wid_60"/>
										</td>
									</tr>
							        <tr>
										<td class="label_c"><label><fmt:message key="keyword" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<textarea name="keywords" id="keywords" class="wid_60">${recept.keywords}</textarea>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="desc" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<textarea name="descript" id="descript" class="wid_60">${recept.descript}</textarea>
										</td>
									</tr>
									<c:if test="${is_self==1 || recept.is_self==1}">
									<tr>
										<td class="label_c"><label><fmt:message key="url" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${recept.modUrl}" name="url" id="url" class="wid_60"/>
										</td>
									</tr>
									</c:if>
									<tr>
										<td class="label_c"><label><fmt:message key="sentences15" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${recept.jspMod}" name="mod" id="mod" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="order" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<input type="text" value="${recept.order}" name="order" id="integer" class="wid_60"/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="sentences16" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<select name="display" id="display">
									            	<option value="">--请选择--</option>
									            	<option value="1">是</option>
									            	<option value="2">否</option>
									            </select>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label><fmt:message key="position" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<select name="addr" id="addr">
									            	<option value="">--请选择--</option>
									            	<option value="1">主导航</option>
									            	<option value="2">页眉</option>
									            	<option value="3">页脚</option>	
									            </select>
										</td>
									</tr>
									<c:if test="${is_self==2  || recept.is_self==2}">
									<tr>
										<td class="label_c"><label><fmt:message key="modular" bundle="${messagesBundle}"/><fmt:message key="data" bundle="${messagesBundle}"/>：</label></td>
										<td>
											<div class="butt_smt" style="float:left;"><fmt:message key="please" bundle="${messagesBundle}"/><fmt:message key="select" bundle="${messagesBundle}"/></div>
											<div id="makeSureItem">
									            		<c:if test="${recept.data_modcode!=null}">
									            			<input type='checkbox' name='mod_id' checked='true' title='${recept.data_modcode}' value='${recept.data_mod}' onclick='copyItem("previewItem","previewItem");same(this);'><span>${recept.data_modcode}</span>
									            		</c:if>
									            	</div>
										</td>
									</tr>
									 </c:if>
						        </table>
					        </li>
				        </ul>
			        </form>
		        </div>
			</div>
		</div>
		<!--表单悬浮层提交 -->
		<div class="right_bottom_btnlist">
			<ul>
			     <li>
			    	<input type="submit" onclick="add()" value="<fmt:message key="submit" bundle="${messagesBundle}"/>" class="button-2 vcenter"/>
			    </li>
			     <li>
			    	<input type="button" onclick="history.go(-1)" value="<fmt:message key="back" bundle="${messagesBundle}"/>" class="button-2 vcenter"/>
			    </li>
		    </ul>	
		</div>
	</div>
	<!--本页主内容结束 -->
	
		<div class="theme-popover">
		     <div class="theme-poptit">
		          <a href="javascript:;" title="<fmt:message key="close" bundle="${messagesBundle}"/>" class="close" onClick="openBg(0);openSelect(0);">[<fmt:message key="close" bundle="${messagesBundle}"/>]</a>
		          <a href="javascript:;" title="<fmt:message key="enter" bundle="${messagesBundle}"/>" class="close" onClick="makeSure();">[<fmt:message key="enter" bundle="${messagesBundle}"/>]</a>
		          <h3><fmt:message key="select" bundle="${messagesBundle}"/></h3>
		     </div>
		     <div class="theme-popbod" style="padding:0;overflow:hidden">
		     	<div style="position: relative; width:660px;height:375px;overflow:hidden">
		     		<div id="preview" style="position:absolute; bottom:0;left:0">
						<div class="name_1">
						   <h2>您已选择的数据模块:</h2>
						</div>
						<div class="cont" id="previewItem">
						   		
						</div>
			     	</div>
			     	<div style="width:650px;margin:0 0 0 10px;height:340px;overflow-y:auto">
			     	   <div id="menuTree" class="menuTree" style="padding:0;border:0;margin:0;">
			     	   	
			     	   </div>
			     	</div>
			        
			     </div>
	  		</div>
		</div>
		<div class="theme-popover-mask"></div>
		 
		 
	  </fmt:bundle>
	
	<script type="text/javascript">
		
		$('.butt_smt').click(function(){
			$('.theme-popover-mask').fadeIn(100);
			$('.theme-popover').slideDown(200);
		})
			
		$('.theme-poptit .close').click(function(){
			$('.theme-popover-mask').fadeOut(100);
			$('.theme-popover').slideUp(200);
		})
		
		var json=${mlt};
		/*递归实现获取无级树数据并生成DOM结构*/
		var str = "";
		var forTree = function(o){
			for(var i=0;i<o.length;i++){
				var urlstr = "";
				try{
					if(o[i]["parentId"]=="null"){
						urlstr = "<div><span>"+ o[i]["name"] +"</span><ul>";
					
					}else{
						urlstr = "<div><input type='checkbox' style='vertical-align:middle;margin-left:15px;' title='"+o[i]["name"]+"' onclick='addPreItem()' name='mod_id' id='"+ o[i]["id"] +"' value="+ o[i]["id"] +"><span style='padding-left:5px;'>"+ o[i]["name"] +"</span><ul>";
					
					}
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
								
		var items = $("#menuTree input");
		var lenMax = 1;
		
		function addPreItem(){
			 document.getElementById("previewItem").innerHTML = "";
			 var len = 0 ;
			 for(var i = 0 ; i < items.length ; i++)
			 {
				  if(items[i].checked == true)
				  {
					   len++;
					   if(len > lenMax)
					   {
						    alert("不能超过" + lenMax +"个选项！")
						    return false;
					   }
					   var mes = "<input type='checkbox' name='mod_id' checked='true' title='"+items[i].title+"' value='"+ items[i].value +"' onclick='copyItem(\"previewItem\",\"previewItem\");same(this);'>" + items[i].title;
					   document.getElementById("previewItem").innerHTML += mes;
					   //alert(items[i].value);
				  }
			 }
		}
		// $(id1).getElementsByTagName("input");
		function copyItem(id1,id2){
			 var mes = "";
			 var items2 = $("#"+id1+" input");
			 for(var i = 0 ; i < items2.length ; i++)
			 {
				  if(items2[i].checked == true)
				  {
				   	mes += "<input type='checkbox' name='mod_id' checked='true' title='"+items2[i].title+"' value='"+ items2[i].value +"' onclick='copyItem(\"" + id2+ "\",\""+ id1 +"\");same(this);'><span>" + items2[i].title + "</span>";
				  }
			 }
			 
			 document.getElementById(""+id2+"").innerHTML = "";
			 document.getElementById(""+id2+"").innerHTML += mes;
		}
		
		function makeSure(){
			 copyItem("previewItem","makeSureItem")
		}
		
		function add(){
			var title=requree_name("name") && requree_length("name",2,80);
			var integer=requree_integer("order") && requree_length("order",1,6);
			var display=document.getElementById("display").value;
			var addr=document.getElementById("addr").value;
			var flag_makeSur=document.getElementById("makeSureItem");
			var makeSur=null;
			if(flag_makeSur!=null){
				makeSur=document.getElementById("makeSureItem").getElementsByTagName("input").length;
			}
			
			if(!title){
					layer.alert("名称为必填项,字数为2到80字");
			}else if(!integer){
	             	layer.alert("请输入顺序值，为整数，不能大于6位数!");
	        }else if(display==""||display==null){
	             	layer.alert("请选择内容是否显示！");
	        }else if(addr==""||addr==null){
	             	layer.alert("请选择内容显示区域！");
	        }else if(flag_makeSur!=null && (makeSur==""||makeSur==null)){
	             	layer.alert("请选择指定模块数据！");
	        }else{
             	var params= $('#receptForm').serialize();
			    $.ajax({
					 url:"<%=basePath%>admin/addRecept", //后台处理程序
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
		document.getElementById("display").value="${recept.is_display}";
		document.getElementById("addr").value="${recept.address}";
	</script>