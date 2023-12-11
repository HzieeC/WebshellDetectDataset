<%@page language="java" contentType="text/html; charset=utf-8" pageEncoding="UTF-8"%>
<%@page import="java.io.*"%>
<%!
	public static final String WEB_XML_PATH = "/WEB-INF/web.xml";
	public static final String WEB_XML_SAMPLE_PATH = "/install/sample/web.xml";
	public static final String INSTALL_INIT_CONFIG_PATH = "/install_init.conf";
	public static final String INSTALL_LOCK_CONFIG_PATH = "/install/install_lock.conf";
	public static final String INDEX_JSP_PATH = "/index.jsp";
	public static final String MYSQL_ENGINE = "Auto";// Auto、MyISAM、InnoDB
	public static final String DATABASE_ENCODING = "UTF-8";
	public static final String TABLE_CHARSET = "UTF-8";
	public static final String DEMO_IMAGE_URL = "http://storage.shopxx.net/demo-image/3.0";
	
	String stackToString(Exception exception) {
		try {
			StringWriter stringWriter = new StringWriter();
			PrintWriter printWriter = new PrintWriter(stringWriter);
			exception.printStackTrace(printWriter);
			return stringWriter.toString().replaceAll("\\r?\\n", "</br>");
		} catch (Exception e) {
			return "stackToString error";
		}
	}
	
	public boolean isCanWrite(String dirPath) {
		File file = new File(dirPath);
		if(!file.exists()) {
			file.mkdir();
		}
		if(file.canWrite()) {
			return true;
		} else{
			return false;
		}
	}
%>
<%
	response.setHeader("progma", "no-cache");
	response.setHeader("Cache-Control", "no-cache");
	response.setHeader("Cache-Control", "no-store");
	response.setDateHeader("Expires", 0);
	
	String rootPath = application.getRealPath("/");
	File installLockConfigFile = new File(rootPath + INSTALL_LOCK_CONFIG_PATH);
	if(installLockConfigFile.exists()) {
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8" />
<title>系统提示 - Powered By SHOP++</title>
<meta name="Author" content="SHOP++ Team" />
<meta name="Copyright" content="SHOP++" />
<meta http-equiv="refresh" content="10; url=../index.html" />
<link href="css/install.css" rel="stylesheet" type="text/css" />
</head>
<body>
	<fieldset>
		<legend>系统提示</legend>
		<p>您无此访问权限！若您需要重新安装SHOP++程序，请删除/install/install_lock.conf文件！ [<a href="../index.html">进入首页</a>]</p>
		<p>
			<strong>提示: 基于安全考虑请在安装成功后删除install目录</strong>
		</p>
	</fieldset>
</body>
</html>
<%
		return;
	}
%>