  <%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
  <%
	String path = request.getContextPath();
	String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
  %>
   <jsp:useBean id="folder" scope="page" class="com.weishang.bean.ReceptBean"/><!-- 当前正在使用的模板 -->
  <!-- 网页背景层 -->
    <div class="wrapper_bg">
        <!-- 头部 -->
        <!-- end 头部 -->
        <!-- 内容 -->
        <div class="wrap_1200">
