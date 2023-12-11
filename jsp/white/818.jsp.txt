<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<!-- 内容主体 -->
            <div class="user_left">
                <!-- 用户菜单 -->
                <div class="user-bd-menu">
                    <div class="user-bd-menu-index">用户中心</div>
                    <dl>
                        <dt><div class="d1">个人资料</div></dt>
                        <dd><a href="<%=basePath%>pc/pcGoUserPass">修改密码</a></dd>
                        <dd><a href="<%=basePath%>pc/pcUserIndex">基本资料</a></dd>
                        <dd><a href="<%=basePath%>pc/pcUserAddress">收货地址</a></dd>
                    </dl>
                    <dl>
                        <dt><div class="d4">订单管理</div></dt>
                        <dd><a href="<%=basePath%>pc/pcUserOrder?flag=1">预订单</a></dd>
                        <dd><a href="<%=basePath%>pc/pcUserOrder?flag=2">订单</a></dd>
                    </dl>
                    <dl>
                        <dt><div class="d3">优惠券</div></dt>
                        <dd><a href="<%=basePath%>pc/pcUsercoupon">我的优惠券</a></dd>
                        <dd><a href="<%=basePath%>pc/pcUserGetCoupon">领取优惠券</a></dd>
                    </dl>
                    <dl>
                        <dt><div class="d2">财务管理</div></dt>
                        <dd><a href="<%=basePath%>pc/pcUserMoney">我的消费记录</a></dd>
                        <dd><a href="<%=basePath%>user/commonUserAction?tag=goUserIntegel">我的积分</a></dd>
                    </dl>
                </div>
            </div>