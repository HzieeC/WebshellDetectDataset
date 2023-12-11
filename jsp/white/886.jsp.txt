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
<script src="<%=basePath%>js/ajaxfileupload.js"></script>
<script src="<%=basePath%>js/layer/layer.js"></script>
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		
		<!--右侧内容部分-->
		<div class="middle_cnt_c2" style="padding: 10px 8px;">
			<div class="middle_cnt_cont_d">
				<div class="new_tabel_c" style="border:0;background:none;padding:0;box-shadow:0 0 0 0;">
				
					<form name="goodsImgsForm" id="goodsImgsForm" action="javascript:void(0)" method="post">
						<input name="aunt_id" type="hidden" value="${aunt_id}" id="aunt_id"/>
						<ul>
							<li>
								<table border="0" cellpadding="0" cellspacing="0" class="table_c1">
								 	<tr>
										<td class="label_c" width="20%"><label>图片描述：</label></td>
										<td width="80%">
											<input type="text" name="goodsImgDesc" value=""/>
										</td>
									</tr>
									<tr>
										<td class="label_c"><label>选择图片：</label></td>
										<td><input name="goodsImgFile" id="goodsImgFile" type="file"/></td>
									</tr>
									<tr>
										<td class="label_c"><label>&nbsp;</label></td>
										<td><input type="submit" onclick="uploadImgs()" class="submit_zt" value="提交"/></td>
									</tr>
								</table>
							</li>
							<li>
								<div class="upload_img" id="my_upload_img">
									<c:forEach items="${imgList}" var="img" varStatus="status">
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
		</div>
	</div>
</fmt:bundle>
<script language="javascript">
	var height = $(window).height();
	$(".middle_cnt_c2").height(height-22);
	$(window).resize(function () {          //当浏览器大小变化时
			    var height = $(window).height();
				$(".middle_cnt_c2").height(height-22);
			});
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
	                	addGoodsImgs();
	                },  
	                error:function(data,status,e)  
	                {
	                   layer.alert("图片导入失败，服务器内部错误");
	                 }  
				});
			}
		}
		
		function addGoodsImgs(){//goodsOtherForm
			
				var goods_id=$("#goods_id").val();
				//alert(goods_id);
				$("#goods_img_id").val(goods_id);
				var params= $('#goodsImgsForm').serialize();
				$.ajax({
					url:"<%=basePath%>admin/myAdmin?tag=addAuntImg", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:params,         //要传递的数据
					success:function(data){
						var str="<div id='goods_img_"+data.imgId+"' class='upload_img_li'>";
						str+="<img src='<%=basePath%>"+data.minImg+"'/>";
						str+="<a href=\"javascript:void(0)\" class=\"upload_img_del\" onClick=\"deleteAuntImg("+data.imgId+")\">删除</a>";
						str+="</div>";
						//my_upload_img
						$("#my_upload_img").append(str);
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
</script>