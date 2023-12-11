<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%@ taglib uri="http://java.fckeditor.net" prefix="FCK" %>
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
	<script src="<%=basePath%>js/jquery.tools.min.js"></script>
	<script src="<%=basePath%>js/validate.js"></script>
	<script src="<%=basePath%>js/ajaxfileupload.js"></script>
	<script src="<%=basePath%>js/layer/layer.js"></script>
	<script src="<%=basePath%>js/My97DatePicker/WdatePicker.js"></script>
	
	<fmt:setLocale value="zh_CN"/>
	<fmt:setBundle basename="messages" var="messagesBundle"/>
	<fmt:bundle basename="messagesAllMessage">
	<div class="middle_cnt">
		<!--标题部分-->
		<div class="title_c"><span>添加商品</span></div>
		<!--右侧内容部分-->
		<div class="middle_cnt_c1">
			<script type="text/javascript">
				$(function() {
					$("ul.link_d1").tabs("div.con_d1 > div");
					$("#r1").click();
				});
			</script>
			<div class="middle_cnt_tas">
					<ul class="middle_cnt_tas_t link_d1">
						<li><a href="javascript:void(0)" id="r1">通用信息</a></li>
						<li><a href="javascript:void(0)">详细描述</a></li>
						<li><a href="javascript:void(0)">其他信息</a></li>
						<li><a href="javascript:void(0)">商品属性</a></li>
						<li><a href="javascript:void(0)">商品相册</a></li>
						<li><a href="javascript:void(0)">关联商品</a></li>
						<li><a href="javascript:void(0)">配件</a></li>
						<li><a href="javascript:void(0)">关联文章</a></li>
					</ul>
				</div>
				<div class="middle_cnt_cont con_d1">
					<!--通用信息-->
						<div class="middle_cnt_cont_d">
							<div class="new_tabel_c">
								<input name="goods_id" id="goods_id" value="${goods.goodsId}" type="hidden"/>
								<form action="javascript:void(0)" id="goodsBase" name="goodsBase">
								<input name="goodsImg" id="goodsImg" value="${goods.goodsImg}" type="hidden"/>
								<input name="goods_update_id" id="goods_update_id" value="${goods.goodsId}" type="hidden"/>
								<ul>
									<li>
										<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
											<tr>
												<td width="15%" class="label_c"><label>商品名称：</label></td>
												<td width="85%">
													<input type="text" value="${goods.goodsName}" name="goods_name" id="goods_name" class="wid_60"/>
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>商品货号：</label></td>
												<td>
													<input type="text" value="${goods.goodsSn}" name="goods_sn" id="goods_sn" class="wid_60"/><br>
													如果您不输入商品货号，系统将自动生成一个唯一的货号。
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>商品分类：</label></td>
												<td>
													<select name="category_id" id="category_one">
														<option value="">请选择商品分类</option>
														
													</select>
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>扩展分类：</label></td>
												<td id="add_fenlei_cnt">
													<button class="buttun_add" id="add_fenlei">添加</button>
													<c:forEach items="${goods.catList}" var="cat" varStatus="status">
														<select id="add_fenlei_${status.index+1}" name="category_extend">
															<option value="">请选择商品分类</option>
														</select>
														
													</c:forEach>
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>商品品牌：</label></td>
												<td>
													<select name="brand_id" id="brand_id">
														<option value="">请选择品牌</option>
														<c:forEach items="${brandList}" var="brand" varStatus="status">
														 	<option value="${brand.id}">${brand.name}</option>	
														</c:forEach>
													</select>
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>预交定金：</label></td>
												<td>
													<input type="text" value="${goods.shopPrice}" name="shop_price" id="shop_price" class="wid_60"/>
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>展示价格：</label></td>
												<td>
													日租<input type="text" class="wid_20" name="user_price" value="${goods.userPrice}">周租<input name="cons_price" type="text" class="wid_20" value="${goods.consPrice}">月租<input name="vip_price" type="text" class="wid_20" value="${goods.vipPrice}"><br>
													会员价格为-1时表示会员价格按会员等级折扣率计算。你也可以为每个等级指定一个固定价格
												</td>
											</tr>
											<!--  
												<tr>
													<td class="label_c"><label>商品优惠价格：</label></td>
													<td>
														<table border="0" id="table1" class="table_add">
															<tr>
																<td>优惠数量</td>
																<td><input type="text" value="" /></td>
																<td>优惠价格</td>
																<td><input type="text" value="" /></td>
																<td><input type="button" id="addBtn" class="addBtn_r" value="添加一行"></td>
															</tr>
														</table>
														购买数量达到优惠数量时享受的优惠价格
													</td>
												</tr>
											-->
											<tr>
												<td class="label_c"><label>市场售价：</label></td>
												<td>
													<input type="text" value="${goods.marketPrice}" name="market_price" class="wid_60">
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>赠送消费积分数：</label></td>
												<td>
													<input type="text" value="${goods.giveIntegral}" name="give_integral" class="wid_60"><br>
													购买该商品时赠送消费积分数,-1表示按商品价格赠送
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>积分购买金额：</label></td>
												<td>
													<input type="text" value="${goods.integral}" name="integral" class="wid_60"><br>
													(此处需填写金额)购买该商品时最多可以使用积分的金额
												</td>
											</tr>
											<tr>
												<td class="label_c"><label>是否设置促销价格：</label></td>
												<td>
													<input type="radio" name="cuxiao" id="cuxiao_id" value="1" id="cuxiao_1" checked="checked"/>不设置
													<input type="radio" name="cuxiao" id="cuxiao_id" value="2" name="" id="cuxiao_2"/>设置促销价格
												</td>
											</tr>
											<tr id="cu_id">
												<td class="label_c"><label>促销价：</label></td>
												<td>
													<input type="text" value="${goods.promotePrice}" name="promote_price" class="wid_60"/>
												</td>
											</tr>
											<tr id="cu_id_date">
												<td class="label_c"><label>促销日期：</label></td>
												<td>
													<input name="promote_start_date" type="text" value="${goods.promoteStartDate}" class="wid_20" onClick="WdatePicker({minDate:'%y-%M-{%d}'})"/>至
													<input name="promote_end_date" type="text" value="${goods.promoteEndDate}" class="wid_20" onClick="WdatePicker({minDate:'%y-%M-{%d}'})"/>
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
													<input type="submit" onclick="javascriopt:updateCommodityBase()" class="submit_zt" value="提交"/>
												</td>
											</tr>
										</table>
									</li>
								</ul>
								</form>
							</div>
						</div>
						<!--详细描述-->
						<div class="pn_tas_mer_t_cont">
							<div class="new_tabel_c">
								<form name="goodsDescForm" id="goodsDescForm" action="javascript:void(0)" method="post">
									<input name="content" type="hidden" id="content"/>
									<input name="goods_desc_id" type="hidden" id="goods_desc_id"/>
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													<td width="10%" class="label_c"><label>详细描述：</label></td>
													<td width="90%">
														<FCK:editor instanceName="goods_desc">
															<jsp:attribute name="value">
															<p>${goods.goodsDesc}</p>
															</jsp:attribute>
															<jsp:body>
																<FCK:config AutoDetectLanguage="${empty param.code ? true : false}"
																	DefaultLanguage="${empty param.code ? 'en' : param.code}" />
															</jsp:body>
														</FCK:editor>
													</td>
												</tr>
												<tr>
													<td class="label_c"></td>
													<td>
														<input onclick="javascript:addGoodsDesc()" type="submit" class="submit_zt" value="提交"/>
													</td>
												</tr>
											</table>
										</li>
									</ul>
								</form>
							</div>
						</div>
						<!--其他信息-->
						<div class="pn_tas_mer_t_cont">
							<div class="new_tabel_c">
								<form name="goodsOtherForm" id="goodsOtherForm" action="javascript:void(0)" method="post">
									<input name="goods_other_id" type="hidden" id="goods_other_id"/>
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													<td width="20%" class="label_c"><label>商品重量：</label></td>
													<td width="80%">
														<input type="text" value="${goods.goodsWeight}" name="goods_weight" class="wid_60"/>
														<select>
															<option>千克</option>
														</select>
													</td>
												</tr>
												<tr>
													<td class="label_c"><label>商品库存数量：</label></td>
													<td>
														<input type="text" name="goods_number" value="${goods.goodsNumber}" class="wid_60"/><br>
														库存在商品为虚货或商品存在货品时为不可编辑状态，库存数值取决于其虚货数量或货品数量。
													</td>
												</tr>
												<tr>
													<td class="label_c"><label>库存警告数量：</label></td>
													<td>
														<input name="warn_number" type="text" value="${goods.warnNumber}" class="wid_60"/>
													</td>
												</tr>
												<tr>
													<td class="label_c"><label>加入推荐：</label></td>
													<td>
														<div class="input_z2"><input id="is_best" type="checkbox" name="is_best" value="1"/>精品</div>
														<div class="input_z2"><input id="is_new" type="checkbox" name="is_new" value="1"/>新品</div>
														<div class="input_z2"><input id="is_hot" type="checkbox" name="is_hot" value="1"/>热销</div>
													</td>
												</tr>
												
												<script language="javascript">
													<c:if test="${goods.isBest!=null&&goods.isBest==1}">
														$("#is_best").attr("checked","checked");
													</c:if>
													<c:if test="${goods.isNew!=null&&goods.isNew==1}">
														$("#is_new").attr("checked","checked");
													</c:if>
													<c:if test="${goods.isHot!=null&&goods.isHot==1}">
														$("#is_hot").attr("checked","checked");
													</c:if>
												</script>
												<tr>
													<td class="label_c"><label>上架：</label></td>
													<td>
														<div class="input_z2">
															<input type="checkbox" name="is_shelves" id="is_shelves" value="1"/>
															打勾表示允许销售，否则不允许销售。
														</div>
													</td>
												</tr>
												<script language="javascript">
													<c:if test="${goods.isShelves!=null&&goods.isShelves==1}">
														$("#is_shelves").attr("checked","checked");
													</c:if>
												</script>
												<tr>
													<td class="label_c"><label>能作为普通商品销售：</label></td>
													<td>
														<div class="input_z2">
															<input type="checkbox" id="is_alone_sale" name="is_alone_sale" value="1"/>
															打勾表示能作为普通商品销售，否则只能作为配件或赠品销售。
														</div>
													</td>
												</tr>
												<script language="javascript">
													<c:if test="${goods.isAloneSale!=null&&goods.isAloneSale==1}">
														$("#is_alone_sale").attr("checked","checked");
													</c:if>
												</script>
												<tr>
													<td class="label_c"><label>是否为免运费商品：</label></td>
													<td>
														<div class="input_z2">
															<input type="checkbox" name="is_freight" value="1"/>
															打勾表示此商品不会产生运费花销，否则按照正常运费计算。
														</div>
													</td>
												</tr>
												<script language="javascript">
													<c:if test="${goods.isFreight!=null&&goods.isFreight==1}">
														$("#is_freight").attr("checked","checked");
													</c:if>
												</script>
												<tr>
													<td class="label_c"><label>商品关键词：</label></td>
													<td>
														<input type="text" name="keywords" value="${goods.keywords}" class="wid_60"/>
													</td>
												</tr>
												<tr>
													<td class="label_c"><label>商品简单描述：</label></td>
													<td>
														<textarea name="goods_brief" class="wid_60">${goods.goodsBrief}</textarea>
													</td>
												</tr>
												<tr>
													<td class="label_c"><label>商家备注：</label></td>
													<td>
														<textarea name="seller_note" class="wid_60">${goods.sellerNote}</textarea><br>
														仅供商家自己看的信息
													</td>
												</tr>
												<tr>
													<td class="label_c"></td>
													<td>
														<input type="submit" onclick="javascript:addGoodsOtherInfo()" class="submit_zt" value="提交"/>
													</td>
												</tr>
											</table>
										</li>
									</ul>
								</form>
							</div>
						</div>
						<!--其他信息结束-->
						<!--商品属性-->
						<div class="pn_tas_mer_t_cont">
							<div class="new_tabel_c">
								<form name="goodsAttrForm" id="goodsAttrForm" action="javascript:void(0)" method="post">
									<input name="goods_attr_id" type="hidden" id="goods_attr_id"/>
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													<td width="20%" class="label_c"><label>商品类型：</label></td>
													<td width="80%">
														<select name="goods_type" id="goods_type" onblur="getGoodsType()">
															<option value="">请选择商品类型</option>
															<c:forEach items="${typeList}" var="type" varStatus="status">
															 	<option value="${type.id}">${type.name}</option>	
															</c:forEach>
														</select>
														<br>
														请选择商品的所属类型，进而完善此商品的属性
													</td>
												</tr>
												<tr>
													<td colspan="2">
														<ul id="goodAttr" class="label_lf_z">
															
														</ul>
													</td>
												</tr>
												<tr>
													<td class="label_c"></td>
													<td>
														<input type="submit" class="submit_zt" onclick="addGoodsAttr()" value="提交"/>
													</td>
												</tr>
											</table>
										</li>
									</ul>
								</form>
							</div>
						</div>
						<!--商品属性结束-->
						<!--商品相册-->
						<div class="pn_tas_mer_t_cont">
							<div class="new_tabel_c">
								<form name="goodsImgsForm" id="goodsImgsForm" action="javascript:void(0)" method="post">
									<input name="goods_img_id" type="hidden" id="goods_img_id"/>
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													<td>
														<table border="0" id="table2" class="table_add">
															<tr>
																<td width="15%" class="label_c"><label>图片描述：</label></td>
																<td width="50%">
																	<input type="text" name="goodsImgDesc" value="" class="wid_80"/>
																</td>
																<td width="35%"><label><input name="goodsImgFile" type="file"/></label></td>
																<!--  
																	<td><input type="button" class="addBtn_r" id="addBtn_tu" value="添加一行"></td>
																-->
															</tr>
														</table>
													</td>
													
												</tr>
												<tr>
													<td align="center">
														<input type="submit" style="margin-left:15% " onclick="uploadImgs()" class="submit_zt" value="提交"/>
													</td>
												</tr>
											</table>
										</li>
										<li>
											<div class="upload_img" id="my_upload_img">
												<c:forEach items="${goods.imgsList}" var="img" varStatus="status">
													<div id="goods_img_${img.goodsGalleryId}" class="upload_img_li">
														<img src="<%=basePath%>${img.thumbUrl}"/>
														<a href="javascript:void(0)" class="upload_img_del" onClick="deleteGoodsImg(${img.goodsGalleryId})">删除</a>
													</div>
												</c:forEach>	
											</div>
										</li>
									</ul>
								</form>
							</div>
						</div>
						<!--商品相册结束-->
						<!--关联商品-->
						<div class="pn_tas_mer_t_cont">
							<div class="new_tabel_c">
								<form name="goodsGoodsForm" id="goodsGoodsForm" action="javascript:void(0)" method="post">
									<input name="goods_goods_id" type="hidden" id="goods_goods_id"/>
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													<td>
														<select class="margin_lf_c" id="add_fenlei_2" name="category_search">
															<option value="">选择分类</option>
														</select>
														<select name="search_brand">
															<option value="">选择品牌</option>
															<c:forEach items="${brandList}" var="brand" varStatus="status">
															 	<option value="${brand.id}">${brand.name}</option>	
															</c:forEach>
														</select>
														<input type="text" name="search_keyword" class="input_z" placeholder="输入关键字" value=""/>
														<button onclick="searchGoods()" class="buttun_add">搜索</button>
													</td>
													
												</tr>
												<tr>
													<td>
														<div class="s_duoxuan_k">
														<span class="title_tips1">可选商品</span>
														<span class="title_tips2">跟该商品关联的商品</span>
														<select id="station_list" name="station_list"  multiple="multiple" class="s_duoxuan_k_lf">
														    
														</select>
														<input type="button" class="s_duoxuan_k_xuan3" value="右移" onclick="moveOptions('station_list','station_list_to')"/>
														<input type="button" class="s_duoxuan_k_xuan4"  value="左移" onclick="moveOptions('station_list_to','station_list')"/>
														<select id="station_list_to" name="station_list_to" multiple="multiple" class="s_duoxuan_k_rg">
														    <c:forEach items="${goods.goodsList}" var="goodsTem" varStatus="status">
															 	<option selected value="${goodsTem.goodsId}">${goodsTem.goodsName}</option>	
															</c:forEach>
														</select>
														</div>
													</td>
												</tr>
												<tr>
													<td align="left">
														<input type="submit" onclick="addGoodsRelation()" class="submit_zt margin_lf_c" value="提交"/>
													</td>
												</tr>
											</table>
										</li>
									</ul>
								</form>
							</div>
						</div>
						<!--关联商品结束-->
						<!--配件-->
						<div class="pn_tas_mer_t_cont">
							<form name="goodsPartsForm" id="goodsPartsForm" action="javascript:void(0)" method="post">
								<input name="goods_parts_id" type="hidden" id="goods_parts_id"/>
								<div class="new_tabel_c">
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													<td>
														<select class="margin_lf_c" id="category_search" name="category_search">
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
														<div class="s_duoxuan_k">
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
														    <c:forEach items="${goods.partsList}" var="parts" varStatus="status">
															 	<option value="${parts.goodsId}-${parts.goodsPrice}">${parts.goodsName}-${parts.goodsPrice}</option>	
															</c:forEach>
														</select>
														</div>
													</td>
												</tr>
												<tr>
													<td align="left">
														<input type="submit" onclick="addGoodsParts()" class="submit_zt margin_lf_c" value="提交"/>
													</td>
												</tr>
											</table>
										</li>
									</ul>
								</div>
							</form>
						</div>
						<!--配件结束-->
						<!--关联文章-->
						<div class="pn_tas_mer_t_cont">
							<form name="goodsArticleForm" id="goodsArticleForm" action="javascript:void(0)" method="post">
								<input name="goods_article_id" type="hidden" id="goods_article_id"/>
								<div class="new_tabel_c">
									<ul>
										<li>
											<table width="100%" border="0" cellpadding="0" cellspacing="0" class="table_c1">
												<tr>
													<td>
														<select class="margin_lf_c" name="cmsType" id="cmsType">
															<option>选择分类</option>
														</select>
														<input type="text" name="keywords" class="input_z" placeholder="输入关键字" value=""/>
														<button onclick="searchArticle()" class="buttun_add">搜索</button>
													</td>
												</tr>
												<tr>
													<td>
														<div class="s_duoxuan_k">
														<span class="title_tips1">可选文章</span>
														<span class="title_tips2">跟该商品关联的文章</span>
														<select id="station_list_2" name="station_list"  multiple="multiple" class="s_duoxuan_k_lf">
															
														</select>
														<input type="button" class="s_duoxuan_k_xuan3" value="右移" onclick="moveOptions('station_list_2','station_list_to2')"/>
														<input type="button" class="s_duoxuan_k_xuan4"  value="左移" onclick="moveOptions('station_list_to2','station_list_2')"/>
														<select id="station_list_to2" name="station_list_to" multiple="multiple" class="s_duoxuan_k_rg">
															<c:forEach items="${goods.articleList}" var="cms" varStatus="status">
															 	<option value="${cms.id}">${cms.name}</option>	
															</c:forEach>
														</select>
														</div>
													</td>
												</tr>
												<tr>
													<td align="left">
														<input type="submit" onclick="addGoodsArticle()" class="submit_zt margin_lf_c" value="提交"/>
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

		getJson(mlt,"#category_one");
		getJson(mlt,"#category_search");
		getJson(mlt2,"#cmsType");
		<c:forEach items="${goods.catList}" var="cat" varStatus="status">
			getJson(mlt,"#add_fenlei_${status.index+1}");
			$("#add_fenlei_${status.index+1}").val(${cat.id});
		</c:forEach>
		$("#category_one").val(${goods.category.id});
		$("#brand_id").val(${goods.brand.id});
		$("#goods_type").val(${goods.goodsType.id});
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
		
		//添加优惠价格多行
		function getNewRow(){
      		var btn = $("<input class='delBtn addBtn_r' type='button' value='删除' />");
            var newRow = $("<tr>").append($("<td>").append($("<font>优惠数量</font>")))
                                  .append($("<td>").append($("<input type='text' value=''>")))
								  .append($("<td>").append($("<font>优惠价格</font>")))
                                  .append($("<td>").append($("<input type='text' value=''>")))
                                  .append($("<td>").append(btn));
           	return newRow; 
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
			//添加优惠价格多行
			$("#addBtn").click(function (){ 
				$("#table1").append(getNewRow()); 
			});
			$("#table1").on('click', ".delBtn", function (){
				$(this).parent().parent().remove();
			});
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
			
			//促销
			$("#cuxiao_1").click(function() {
				$("#cu_id").hide(); 
				$("#cu_id_date").hide();
			});
			$("#cuxiao_2").click(function() {
				$("#cu_id").show(); 
				$("#cu_id_date").show();
			});
			//内容高度设置
			$(window).resize(function(){
			   var height = $(window).height();
		    	$(".middle_cnt_c1").height(height-72);
			});
			var height = $(window).height();
		    $(".middle_cnt_c1").height(height-72);
		});
		
		/**修改产品基本信息*/
		function updateCommodityBase(){
			var title=requree_name("goods_name") && requree_length("goods_name",2,80);
			var goods_sn=requree_name("goods_sn");
			var category_z=document.getElementById("category_one").value;
			var brand_id=document.getElementById("brand_id").value;
			var shop_price=requree_double("shop_price");
			var market_price=requree_double("market_price");
			var give_integral=requree_integer("give_integral");
			var integral=requree_integer("integral");
			
			
			if(!title){
				layer.alert("商品名为必填项，长度为2到80！");
			}else if(!goods_sn){
				layer.alert("货号为必填项！");
			}else if(category_z==""||category_z==null){
             	layer.alert("请选择商品分类");
            }else if(brand_id==""||brand_id==null){
             	layer.alert("请选择品牌");
            }else if(!shop_price){
				layer.alert("请输入预交订金金额！");
			}else if(!market_price){
				layer.alert("请输入市场售价金额！");
			}else if(!give_integral){
				layer.alert("请输入赠送积分数，整数！");
			}else if(!integral){
				layer.alert("请输入积分购买金额，整数！");
			}else{
				var params= $('#goodsBase').serialize();
				layer.msg('加载中', {icon: 16,time: 1000},function(){
					$.ajax({
						url:"<%=basePath%>admin/myAdmin?tag=updateGoodsBaseInfo", //后台处理程序
						type:'post',         //数据发送方式
						dataType:'json',
						data:params,         //要传递的数据
						success:function(data){
							layer.alert(data.tip);
						}
					});
				}); 
			}
		}
		
		$("#cuxiao_id").val(${goods.isPromote});
		
		function addGoodsDesc(){
			var oEditor = FCKeditorAPI.GetInstance("goods_desc");
			$("#content").attr("value",oEditor.GetXHTML(true));
			var goods_id=$("#goods_id").val();
			//alert(goods_id);
			$("#goods_desc_id").val(goods_id);
			var params= $('#goodsDescForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/addGoodsDesc", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					layer.alert(data.tip);
				}
			}); 
		}
		
		function addGoodsOtherInfo(){//goodsOtherForm
			var goods_id=$("#goods_id").val();
			//alert(goods_id);
			$("#goods_other_id").val(goods_id);
			var params= $('#goodsOtherForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/addGoodsOtherInfo", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					layer.alert(data.tip);
				}
			}); 
		}
		
		function addGoodsAttr(){//goodsOtherForm
			var goods_id=$("#goods_id").val();
			//alert(goods_id);
			$("#goods_attr_id").val(goods_id);
			var params= $('#goodsAttrForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/addGoodsAttr", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					layer.alert(data.tip);
				}
			}); 
		}
		
		function uploadImgs(){
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
		
		function addGoodsImgs(){//goodsOtherForm
			var goods_id=$("#goods_id").val();
			//alert(goods_id);
			$("#goods_img_id").val(goods_id);
			var params= $('#goodsImgsForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/addGoodsImg", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					var str="<div id='goods_img_"+data.imgId+"' class='upload_img_li'>";
					str+="<img src='<%=basePath%>"+data.minImg+"'/>";
					str+="<a href=\"javascript:void(0)\" class=\"upload_img_del\" onClick=\"deleteGoodsImg("+data.imgId+")\">删除</a>";
					str+="</div>";
					//my_upload_img
					$("#my_upload_img").append(str);
				}
			}); 
		}
		
		function getGoodsType(){
			var type_id=$("#goods_type").val();
			$.ajax({
				url:"<%=basePath%>admin/goGoodsTypeAttr", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:{type_id:type_id},//要传递的数据
				success:function(data){
					$("#goodAttr").html(data.tip);
				}
			}); 
		}
		
		function searchGoods(){
			//goodsGoodsForm
			var goods_id=$("#goods_id").val();
			//alert(goods_id);
			$("#goods_goods_id").val(goods_id);
			var params= $('#goodsGoodsForm').serialize();
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
		
		function searchArticle(){
			var params= $('#goodsArticleForm').serialize();
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
		
		function addGoodsRelation(){
			var goods_id=$("#goods_id").val();
			//alert(goods_id);
			$("#goods_goods_id").val(goods_id);
			var params= $('#goodsGoodsForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/addGoodsRelation", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					layer.alert(data.tip);
				}
			}); 
		}
		
		function addGoodsParts(){
			var goods_id=$("#goods_id").val();
			$("#goods_parts_id").val(goods_id);
			
			var params= $('#goodsPartsForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/myAdmin?tag=addProductParts", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					layer.alert(data.tip);
				}
			}); 
		}
		
		function addGoodsArticle(){
			var goods_id=$("#goods_id").val();
			$("#goods_article_id").val(goods_id);
			var params= $('#goodsArticleForm').serialize();
			$.ajax({
				url:"<%=basePath%>admin/myAdmin?tag=addCmsByMod", //后台处理程序
				type:'post',         //数据发送方式
				dataType:'json',
				data:params,         //要传递的数据
				success:function(data){
					layer.alert(data.tip);
				}
			}); 
		}
		
		function cloneAttr(){
			var $text = $("#fuzhu_z");
			$("#fuzhu_z").after($text.clone());
		}
		//配件修改价格
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
		
		function updateImg(){
			layer.open({
			  type: 2,
			  title: '上传图片',
			  shadeClose: true,
			  shade: 0.8,
			  area: ['300px', '220px'],
			  content: '<%=basePath%>admin/goUploadUrl' //iframe的url
			}); 
		}
		
    	function deleteGoodsImg(id){
    		if(confirm("请确认是否删除此图片")){
    			layer.msg('加载中', {icon: 16,time: 1000},function(){
					$.ajax({
						url:"<%=basePath%>admin/myAdmin?tag=deleteGoodsImg", //后台处理程序
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