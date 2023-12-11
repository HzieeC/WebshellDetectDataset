<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%@ taglib uri="http://java.fckeditor.net" prefix="FCK" %>
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
	<script src="<%=basePath%>js/jquery.tools.min.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<script src="<%=basePath%>js/ajaxfileupload.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<script src="<%=basePath%>js/My97DatePicker/WdatePicker.js"></script>
	<script src="<%=basePath%>js/validate.js" type="text/javascript"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2">
			<script type="text/javascript">
				$(function() {
					$("ul.link_d1").tabs("div.con_d1 > div");
					$("#r1").click();
				});
			</script>
			<div class="middle_cnt_tas">
				<ul class="middle_cnt_tas_t link_d1">
					<li><a href="javascript:void(0)" id="r1">通用信息</a></li>
					<li><a href="javascript:void(0)">活动相册</a></li>
					<li><a href="javascript:void(0)">活动详情</a></li>
					<li><a href="javascript:void(0)">关联商品</a></li>
					<li><a href="javascript:void(0)">关联文章</a></li>
				</ul>
			</div>
			<div class="middle_cnt_cont con_d1">
					<!--通用信息-->
					<div class="middle_cnt_cont_d">
							<div class="new_tabel_c">
								<input name="thematic_id" id="thematic_id" value="${thematic_id}" type="hidden"/>
								<form action="javascript:void(0)" id="thematicBase" name="thematicBase">
								<ul>
									<li>
										<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
											<tr>
												<td width="15%" class="label_c"><label>名称：</label></td>
												<td width="85%">
													<input type="text" value="${thematic.name}" name="name" id="name" data-rule-required="true" class="wid_80"/>
												</td>
											</tr>
											<tr>
												<td width="15%" class="label_c"><label>标题：</label></td>
												<td width="85%">
													<input type="text" value="${thematic.title}" name="title" id="title" data-rule-required="true" class="wid_80"/>
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>关键字：</label></td>
												<td>
													<input type="text" value="${thematic.keywords}" name="keywords" id=""keywords"" class="wid_80"/>
												</td>
											</tr>
											
											<tr>
												<td class="label_c"><label>描述：</label></td>
												<td>
													<textarea name="desc" class="wid_80">${thematic.desc}</textarea>
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>样式表：</label></td>
												<td>
													<input type="text" value="${thematic.style}" name="style" id="style" class="wid_80"/>
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>模板：</label></td>
												<td>
													<input type="text" value="${thematic.template}" name="template" id="template" class="wid_80"/>
												</td>
											</tr>
											<tr id="cu_id_date">
												<td class="label_c"><label>促销日期：</label></td>
												<td>
													<input name="startdate" type="text" value="${thematic.startDate}" class="wid_20" onClick="WdatePicker({minDate:'%y-%M-{%d+1}'})">至
													<input name="enddate" type="text" value="${thematic.endDate}" class="wid_20" onClick="WdatePicker({minDate:'%y-%M-{%d+1}'})">
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>上传商品图片：</label></td>
												<td>
													<div onclick="updateImg()" class="butt_smt" ><fmt:message key="select" bundle="${messagesBundle}"/><fmt:message key="file" bundle="${messagesBundle}"/></div>
												</td>
											</tr>
											<tr>
												<td class="label_c"></td>
												<td>
													<input name="img" type="hidden" value="${thematic.img}"/>
													<input type="submit" onclick="javascriopt:updateThematicBase()" class="submit_zt" value="提交"/>
												</td>
											</tr>
										</table>
									</li>
								</ul>
								</form>
							</div>
						</div>
						<!--商品相册-->
						<div class="middle_cnt_cont_d">
							<div class="new_tabel_c">
								<form name="thematicImgsForm" id="thematicImgsForm" action="javascript:void(0)" method="post">
									<input name="thematic_img_id" type="hidden" id="thematic_img_id"/>
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													<td width="15%" class="label_c"><label>图片描述：</label></td>
													<td width="85%">
														<input type="text" name="goodsImgDesc" class="wid_80" value=""/>
													</td>
												</tr>
												<tr>
													<td class="label_c"><label>选择图片：</label></td>
													<td><input name="goodsImgFile" id="goodsImgFile" type="file"/></td>
													<!--  
														<td><input type="button" class="addBtn_r" id="addBtn_tu" value="添加一行"></td>
													-->
												</tr>
												
												<tr>
													<td>&nbsp;</td>
													<td>
														<input type="submit" onclick="uploadImgs()" class="submit_zt" value="提交"/>
													</td>										
												</tr>
											</table>
										</li>
										
										<li>
											<div class="upload_img" id="my_upload_img">
												<c:forEach items="${thematic.imgList}" var="img" varStatus="status">
													<div id="goods_img_${img.id}" class="upload_img_li">
														<img src="<%=basePath%>${img.small_path}"/>
														<a href="javascript:void(0)" class="upload_img_del" onClick="deleteAuntImg(${img.id})">删除</a>
													</div>
												</c:forEach>	
											</div>
										</li>
									</ul>
								</form>
							</div>
						</div>
						<!--商品相册结束-->
						<!--活动 详情-->
						<div class="middle_cnt_cont_d">
							<div class="new_tabel_c">
								<form name="thematicDescForm" id="thematicDescForm" action="javascript:void(0)" method="post">
									<input name="thematic_desc_id" type="hidden" id="thematic_desc_id"/>
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													
													<td width="15%" class="label_c"><label>活动详情：</label></td>
													<td width="85%">
																	<FCK:editor instanceName="active_desc">
																		<jsp:attribute name="value">
																		<p>${thematic.longdesc}</p>
																		</jsp:attribute>
																		<jsp:body>
																			<FCK:config AutoDetectLanguage="${empty param.code ? true : false}"
																				DefaultLanguage="${empty param.code ? 'en' : param.code}" />
																		</jsp:body>
																	</FCK:editor>
																</td>
															
												</tr>
												<tr>
													<td class="label_c"><label></label></td>
													<td>
														<input type="submit" onclick="uploadDesc()" class="submit_zt" value="提交"/>
													</td>
												</tr>
											</table>
										</li>
									</ul>
									<input name="longDesc" id="longDesc" type="hidden" value=""/>
								</form>
							</div>
						</div>
						<!--活动 详情结束 -->
						
						<!--关联商品-->
						<div class="middle_cnt_cont_d">
							<form name="thematicGoodsForm" id="thematicGoodsForm" action="javascript:void(0)" method="post">
								<input name="thematic_goods_id" type="hidden" id="thematic_goods_id"/>
								<div class="new_tabel_c">
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													<td>
														<select id="category_search" name="category_search">
															<option value="">选择分类</option>
														</select>
														<select name="search_brand">
															<option value="">选择品牌</option>
															<c:forEach items="${brandList}" var="brand" varStatus="status">
															 	<option value="${brand.id}">${brand.name}</option>	
															</c:forEach>
														</select>
														<input type="text" name="search_keyword" class="input_z" placeholder="输入关键字" value=""/>
														
														<button onclick="searchGoods2()" class="buttun_add">搜索</button>
													</td>
													
												</tr>
												<tr>
													<td>
														<div class="s_duoxuan_k" style="width:680px;margin-left:0px; ">
														<span class="title_tips1">可选商品</span>
														<span class="title_tips2">该商品的配件</span>
														<select id="PtNbrs0" name="station_list"  multiple="multiple" class="s_duoxuan_k_lf" onchange="GetOptionAul('PtNbrs')">
														    
														</select>
														<div class="jiage_c">
															价格<input type="text" id="inputval" value=""/>
														</div>
														<input type="button" class="s_duoxuan_k_xuan1" value="右移" onclick="PutRightOneClk('PtNbrs')"/>
														<input type="button" class="s_duoxuan_k_xuan2"  value="左移" onclick="PutLeftOneClk('PtNbrs')"/>
														<select id="PtNbrs1" name="station_list_to1" multiple="multiple" class="s_duoxuan_k_rg">
														    <c:forEach items="${thematic.goodsList}" var="goods" varStatus="status">
														   		<option value="${goods.goodsId}-${goods.shopPrice}">${goods.goodsName}-${goods.shopPrice}</option>
														    </c:forEach>
														</select>
														</div>
													</td>
												</tr>
												<tr>
													<td align="left">
														<input type="submit" onclick="addThematicGoods()" class="submit_zt margin_lf_c" value="提交"/>
													</td>
												</tr>
											</table>
										</li>
									</ul>
								</div>
							</form>
						</div>
						<!--关联商品结束-->
						<!--关联文章-->
						<div class="middle_cnt_cont_d">
							<form name="thematicCmsForm" id="thematicCmsForm" action="javascript:void(0)" method="post">
								<input name="thematic_article_id" type="hidden" id="thematic_article_id"/>
								<div class="new_tabel_c">
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													<td>
														<select name="cmsType" id="cmsType">
															<option>选择分类</option>
														</select>
														<input type="text" name="keywords" class="input_z" placeholder="输入关键字" value=""/>
														<button onclick="searchArticle()" class="buttun_add">搜索</button>
													</td>
												</tr>
												<tr>
													<td>
														<div class="s_duoxuan_k" style="width:680px;margin-left:0px; ">
														<span class="title_tips1">可选文章</span>
														<span class="title_tips2">跟该商品关联的文章</span>
														<select id="station_list_2" name="station_list"  multiple="multiple" class="s_duoxuan_k_lf">
															
														</select>
														<input type="button" class="s_duoxuan_k_xuan3" value="右移" onclick="moveOptions('station_list_2','station_list_to2')"/>
														<input type="button" class="s_duoxuan_k_xuan4"  value="左移" onclick="moveOptions('station_list_to2','station_list_2')"/>
														<select id="station_list_to2" name="station_list_to2" multiple="multiple" class="s_duoxuan_k_rg">
															<c:forEach items="${thematic.cmsList}" var="cms" varStatus="status">
														   		<option value="${cms.id}">${cms.name}</option>
														    </c:forEach>
														</select>
														</div>
													</td>
												</tr>
												<tr>
													<td align="left">
														<input type="submit" onclick="addThematicArticle()" class="submit_zt margin_lf_c" value="提交"/>
													</td>
												</tr>
											</table>
										</li>
									</ul>
								</div>
							</form>
						</div>
						<!--关联文章-->
				</div>
			</div>
		</div>
	</fmt:bundle>
	<script>
		var mlt=${mlt};
		var mlt2=${mlt2};
		function getJson(json,eid){
			for(var o in json){
				$(eid).append("<option value="+json[o].id+">"+json[o].name+"</option>");
				var children=json[o].children;
				if(children!=null){
					getJson(children,eid);
				}
			}
		}

		getJson(mlt,"#category_search");
		getJson(mlt2,"#cmsType");
		
		function searchArticle(){
			var params= $('#thematicCmsForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/myAdmin?tag=searchCmsByMod", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					$("#station_list_2").empty();
					var json=data.json; 
					for(var i=0;i<json.length;i++){
						$("#station_list_2").append("<option value="+json[i].id+" id="+json[i].price+">"+json[i].name+"</option>");
					}
				}
			}); 
		}
		
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
		               $(oldname[i]).clone().appendTo($("#"+to));
		               $(oldname[i]).remove();
		       }        
		    }
		}
		
		
		//添加图册
		function getNewRow_tu(){
      		var btn = $("<input class='delBtn_tu addBtn_r' type='button' value='删除' />");
            var newRow = $("<tr>").append($("<td>").append($("<label>图片描述：</label>")))
                                  .append($("<td>").append($("<input type='text'>")))
								  .append($("<td>").append($("<input type='file' name='goodsImgFile' value=''/>")))
                                  .append($("<td>").append(btn));
           	return newRow; 
        }
		
		$(document).ready(function(){ 
			
			//添加图册
			$("#addBtn_tu").click(function (){ 
				$("#table2").append(getNewRow_tu()); 
			});
			$("#table2").on('click', ".delBtn_tu", function (){
				$(this).parent().parent().remove();
			});
			//添加扩展分类
			$("#add_fenlei").click(function(){ 
				$("#add_fenlei_1").clone(true).appendTo("#add_fenlei_cnt");
			}); 
			
			var height = $(window).height();
			$(".middle_cnt_c2").height(height-32);
			$(window).resize(function () {          //当浏览器大小变化时
			    var height = $(window).height();
				$(".middle_cnt_c2").height(height-32);
			});
		});
		
		function addmThematicBase(){
			var title=requree_length("title",2,80) && requree_sper("title");
			var style=requree_name("style");
			var template=requree_name("template");
			var startdate=requree_name("startdate");
			var enddate=requree_name("enddate");
			
			if(!title){
				layer.alert("标题必须输入，并且长度在2到80之间，不能有特殊字符");
			}else if(!style){
				layer.alert("请输入正确的样式表信息");
			}else if(!startdate){
				layer.alert("请输入开始时间");
			}else if(!enddate){
				layer.alert("请输入结束时间");
			}else if(!template){
				layer.alert("请输入正确的模板信息");
			}else{
				var params= $('#thematicBase').serialize();
				$.ajax({
					url:"<%=basePath%>admin/thematic?tag=addActivity", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:params,         //要传递的数据
					success:function(data){
						if(data.thematic_id>0){
							$("#thematic_id").val(data.thematic_id);
						}
						alert(data.tip);
					}
				}); 
			}
		}
		
		function uploadImgs(){
			var goodsImg=document.getElementById("goodsImgFile").value;
			if(goodsImg==""||goodsImg==null){
				layer.alert("请选择图片");
			}else{
				$.ajaxFileUpload({
					url : "<%=basePath%>admin/uploadCommonFile",
	                secureuri : false,
	                type:'post',         //数据发送方式
	                fileElementId:["goodsImgFile"],
	                dataType : 'json',
	                success:function(data) {
	                	addThematicImgs();
	                },  
	                error:function(data,status,e)  
	                {
	                   layer.alert("图片导入失败，服务器内部错误");
	                 }  
				});
			}
		}
		
		function addThematicImgs(){//goodsOtherForm
			var goods_id=$("#thematic_id").val();
			$("#thematic_img_id").val(goods_id);
			var params= $('#thematicImgsForm').serialize();
			layer.msg('加载中', {icon: 16,time: 1000},function(){
				$.ajax({
					url:"<%=basePath%>admin/thematic?tag=addActivityImg", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:params,         //要传递的数据
					success:function(data){
						var str="<div id='goods_img_"+data.imgId+"' class='upload_img_li'>";
						str+="<img src='<%=basePath%>"+data.minImg+"'/>";
						str+="<a href=\"javascript:void(0)\" class=\"upload_img_del\" onClick=\"deleteAuntImg("+data.imgId+")\">删除</a>";
						str+="</div>";
						$("#my_upload_img").append(str);
					}
				}); 
			}); 
		}
		
		function updateImg(){
			layer.open({
			  type: 2,
			  title: '上传图片',
			  shadeClose: true,
			  shade: 0.8,
			  area: ['400px', '300px'],
			  content: '<%=basePath%>admin/goUploadUrl' //iframe的url
			}); 
		}
		
		function uploadDesc(){
			var oEditor = FCKeditorAPI.GetInstance("active_desc");
			$("#longDesc").attr("value",oEditor.GetXHTML(true));
			var thematic_id=$("#thematic_id").val();
			//alert(goods_id);
			$("#thematic_desc_id").val(thematic_id);
			var params= $('#thematicDescForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/thematic?tag=updateThematicDesc", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					layer.alert(data.tip);
				}
			}); 
		}
		
		function addThematicGoods(){
			var goods_id=$("#thematic_id").val();
			$("#thematic_goods_id").val(goods_id);
			var params= $('#thematicGoodsForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/thematic?tag=addThematicGoods", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					layer.alert(data.tip);
				}
			}); 
		}
		
		function addThematicArticle(){
			var goods_id=$("#thematic_id").val();
			$("#thematic_article_id").val(goods_id);
			var params= $('#thematicCmsForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/thematic?tag=addThematicCms", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					layer.alert(data.tip);
				}
			}); 
		}
		
		function searchGoods(){
			var params= $('#thematicGoodsForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/searchRelationGoods", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					$("#station_list").empty();
					var json=data.json; 
					for(var i=0;i<json.length;i++){
						$("#station_list").append("<option value="+json[i].id+">"+json[i].name+"</option>");
					}
				}
			}); 
		}
		function searchGoods2(){
			var goods_id=$("#goods_id").val();
			$("#goods_parts_id").val(goods_id);
			var params= $('#goodsPartsForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/searchRelationGoods", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					$("#PtNbrs0").empty();
					var json=data.json; 
					for(var i=0;i<json.length;i++){
						$("#PtNbrs0").append("<option value="+json[i].id+" id="+json[i].price+">"+json[i].name+"</option>");
					}
				}
			}); 
		}
		 function deleteAuntImg(id){
    		if(confirm("请确认是否删除此图片")){
    			layer.msg('加载中', {icon: 16,time: 1000},function(){
					$.ajax({
						url:"<%=basePath%>admin/myAdmin?tag=deleteAuntImg", //后台处理程序
						type:'post',         //数据发送方式
						dataType:'json',
						data:{id:id},         //要传递的数据
						success:function(data){
							if(data.status=='200'){
								$("#goods_img_"+id+"").remove();
							}else{
								layer.alert(data.tip);
							}
						}
					});
				}); 
    		}
    	}
    	//关联价格
		function GetOptionAul(str) {
		    if(document.getElementById(str+"0").options.selectedIndex == -1){return false;}
		    while(document.getElementById(str+"0").options.selectedIndex > -1){
		        var id = document.getElementById(str+"0").options.selectedIndex;
				var varitemval =  document.getElementById(str+"0").options[id].id;
				document.getElementById("inputval").value = varitemval;
		        return
		    }
		}
		function PutRightOneClk(str) {
		    if(document.getElementById(str+"0").options.selectedIndex == -1){return false;}
		    while(document.getElementById(str+"0").options.selectedIndex > -1){
		        var id = document.getElementById(str+"0").options.selectedIndex;
				var inputval = document.getElementById("inputval").value;
		        var varitem = new Option(document.getElementById(str+"0").options[id].text+"-"+inputval,document.getElementById(str+"0").options[id].value+"-"+inputval);
				//alert(document.getElementById("inputval").value);
		        document.getElementById(str+"1").options.add(varitem);
		        document.getElementById(str+"0").options.remove(id);
				document.getElementById("inputval").value = "";
		    }
		}
		function PutLeftOneClk(str) {
		    if(document.getElementById(str+"1").options.selectedIndex == -1){return false;}
		    while(document.getElementById(str+"1").options.selectedIndex > -1){
		        var id = document.getElementById(str+"1").options.selectedIndex
				var varitem_value = document.getElementById(str+"1").options[id].value;
				var tem_value=varitem_value.split("-")[0];
				var tem_id=varitem_value.split("-")[1];
		        var varitem = new Option(document.getElementById(str+"1").options[id].text.split("-")[0],tem_value);
				varitem.setAttribute("id",tem_id);
		        document.getElementById(str+"0").options.add(varitem);
		        document.getElementById(str+"1").options.remove(id);
		    }
		}
	</script>