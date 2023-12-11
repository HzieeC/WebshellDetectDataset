<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE html>
<html>
  <head>
    <base href="<%=basePath%>">
    <jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
    <jsp:useBean id="net" scope="page" class="com.weishang.bean.ReceptBean"/><!--网站基本信息-->
    <title>${net.net.company}</title>
    
	<link href="<%=basePath%>template/${folder.tpl.folder}/css/style_pc.css" rel="stylesheet">
	<link href="<%=basePath%>template/${folder.tpl.folder}/css/index.css" rel="stylesheet">
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.min.js" type="text/javascript"></script>
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/jquery.DB_gallery.js" type="text/javascript"></script>
	<script src="<%=basePath%>template/${folder.tpl.folder}/js/public_pc.js" type="text/javascript"></script>

  </head>
  
  <body>
  <jsp:include  page="/template/${folder.tpl.folder}/page/head_nav_yes.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/head.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/left.jsp"/>
    	 <!-- 右侧内容开始 -->
	    <div class="user-bd-main_r">
	    	<!-- 单元内容开始 -->
                <div class="user-bd-main add-address">
                    <!-- 标题 -->
                    <div class="user-bd-main-title">账户充值</div>

                    <!--表单内容 -->
                    <div class="add-address-form">
                        <form name="userForm" id="userForm" action="<%=basePath%>pc/userRecharge" method="post">
                        	<input name="way" type="hidden" value="${way}"/>
                            <div class="formRow clearfix">
                                <span class="tag">充值的金额：</span>
                                <input class="sAddress" value="" name="price" type="text">
                            </div>
                            <input class="btn-sub" type="submit" value="确认充值">
                        </form>
                    </div>
                 </div>
             <!-- 单元内容结束-->
	    </div>
	    <div class="clear"></div>
	    <!-- 右侧内容结束 -->
   </div>
   <!-- 1200宽度结束-->
   <!-- 背景层结束-->
</div>
		<script language="javascript">
			function updateUserBase(){
				var params= $('#userForm').serialize();
				$.ajax({
					url:"<%=basePath%>wx/wxUpdateUserInfo", //后台处理程序
					type:'post',         //数据发送方式
					dataType:'json',
					data:params,         //要传递的数据
					success:function(data){
						alert(data.tip);
						window.location.reload();
					}
				}); 
			}
		</script>
    <jsp:include  page="/template/${folder.tpl.folder}/page/user/foot.jsp"/>
    <jsp:include  page="/template/${folder.tpl.folder}/page/foot.jsp"/>
  </body>
</html>