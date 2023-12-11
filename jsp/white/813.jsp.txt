<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
<jsp:useBean id="net" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 网站基本信息-->
<div class="clear footer">
	<ul class="clear list_info">
				<jsp:useBean id="myReceptList" scope="page" class="com.weishang.my.bean.MyBean"/><!--帮助中心的菜单 -->
				<jsp:setProperty property="host" value="<%=basePath%>" name="myReceptList"/>
				<c:forEach items="${myReceptList.receptAndCms}" var="recept" varStatus="status">
					<li><span>${recept.recept.name}</span>
						<ul class=" clear list_info_oline">
							<c:forEach items="${recept.receptList}" var="childrenRecept" varStatus="status">
								<li><a href="${childrenRecept.modUrl}">${childrenRecept.name}</a></li>
							</c:forEach>
						</ul>
					</li>
				</c:forEach>
				
				<li class="list_info_ew"><img src="<%=basePath%>template/${folder.tpl.folder}/images/share_icon_gz.jpg"/><p>关注微信公众号</p></li>
				<li class="list_info_ew"><img src="<%=basePath%>template/${folder.tpl.folder}/images/share_icon_sj.jpg"/><p>下载手机版</p></li>
			</ul>
			<div class="clear"></div>
		<ul class="footer_address">
			<li>
				<span class="left address_icon">&nbsp;</span>
				<jsp:useBean id="store" scope="page" class="com.weishang.my.bean.MyBean"/><!-- 门店系统-->
				<p class="left">
					<c:forEach items="${store.storeList}" var="store" varStatus="status">
						${store.name}：${store.address}</br>
					</c:forEach>
				</p>
			</li>
			<li>
				<span class="left phone_icon">&nbsp;</span>
				<p class="left">24小时免费热线电话</br><span class="font_phone">${net.net.tel}</span></p>
			</li>
			<li class="text-right">
				
			</li>
		</ul>
		<div class="clear"></div>
		<div class="footer_links">
			<div class="left">友情链接：</div>
			<ul class="right">
				<jsp:useBean id="coo" scope="page" class="com.weishang.bean.ReceptBean"/><!--友情链接-->
				<jsp:setProperty property="flag" value="2" name="coo"/>
				<jsp:setProperty property="pageNo" value="1" name="coo"/>
				<jsp:setProperty property="pageSize" value="100" name="coo"/>
				<c:forEach items="${coo.cooList}" var="coo" varStatus="status">
					<li>
	                	<a href="${coo.url}" target="_blank">${coo.name}</a>
	                </li>
				</c:forEach>
			</ul>
		</div>
		<div class="clear footer_copyright">
			<p class="left">${net.net.copyright}</p>
			<p class="right">如果您对${net.net.company}有任何意见，欢迎发送联系电话:${net.net.tel}</p>
		</div>
	</div>
	
	
<style>

/* leftsead */
#leftsead{width:161px;height:290px;position:fixed;top:250px;right:0px; z-index:100;}
*html #leftsead{margin-top:258px;position:absolute;top:expression(eval(document.documentElement.scrollTop));}
#leftsead li{width:161px;height:60px;list-style:none;}
#leftsead li img{float:right; }
#leftsead li a{height:49px;float:right;display:block;min-width:47px;max-width:161px;margin-right:0px;}
#leftsead li a .shows{display:block;}
#leftsead li a .hides{margin-right:-143px;cursor:pointer;cursor:hand;}
#leftsead li a.youhui .hides{display:none;position:absolute;right:143px;}
#leftsead li a.youhui .2wm{display:none;position:absolute;right:143px;}
#p2{width:112px;background-color:#A7D2A9;height:47px;margin-left:47px;border:1px solid #8BC48D;text-align:center;line-height:47px}
#p3{width:112px;background-color:#EC9890;height:47px;margin-left:47px;border:1px solid #E6776C;text-align:center;line-height:47px}
#p1{width:47px;height:49px;float:left}
</style>
<!-- 代码部分begin -->
<div id="leftsead">
	<ul>
    	 <li id="btn">
            <a href="javascript:void(0)">
                <div class="hides" style="width:161px;display:none">
	                <img src="<%=basePath%>template/${folder.tpl.folder}/images/ll05.png" width="161" height="49">
	            </div>
	            <img src="<%=basePath%>template/${folder.tpl.folder}/images/l05.png" width="47" height="49" class="shows">
            </a>
        </li>
		
        <li id="btn">
        <a href="http://wpa.qq.com/msgrd?v=3&amp;uin=694510804&amp;site=qq&amp;menu=yes" target="_blank">
            <div class="hides" style="width:161px;display:none">
                <img src="<%=basePath%>template/${folder.tpl.folder}/images/ll04.png" width="161" height="49">
            </div>
            <img src="<%=basePath%>template/${folder.tpl.folder}/images/l04.png" width="47" height="49" class="shows">
        </a>
    </li>
     

        
    </ul>
</div>
<script type="text/javascript" src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js"></script>
<script>

$(document).ready(function(){
    $("#btn a").hover(function(){
            $(this).children("div.hides").show();
            $(this).children("img.shows").hide();
            $(this).animate({marginRight:'114px'},'0'); 
        
    },function(){ 
            	$(this).animate({marginRight:'0px'},'0',function(){
            		$(this).children("div.hides").hide();
					$(this).children("img.shows").show();
            	});
				    
    });
  
    $("#top_btn").click(function(){if(scroll=="off") return;$("html,body").animate({scrollTop: 0}, 600);});

	    //右侧导航 - 二维码
        $(".youhui").mouseover(function(){
            $(this).children(".2wm").show();
        })
        $(".youhui").mouseout(function(){
            $(this).children(".2wm").hide();
        });


});
</script>
<!-- 代码部分end -->