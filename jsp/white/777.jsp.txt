<%@page language="java" contentType="text/html; charset=utf-8" pageEncoding="UTF-8"%>
<%@page import="org.apache.commons.lang.StringUtils"%>
<%@page import="org.apache.commons.io.FileSystemUtils"%>
<%@include file="common.jsp"%>
<%
	String isAgreeAgreement = request.getParameter("isAgreeAgreement");
	if (StringUtils.equals(isAgreeAgreement, "true")) {
		session.setAttribute("isAgreeAgreement", true);
	} else {
		response.sendRedirect("index.jsp");
		return;
	}
	
	boolean isShopxxXmlCanWrite = isCanWrite(rootPath + "/WEB-INF/classes/shopxx.xml");
	boolean isShopxxPropertiesWrite = isCanWrite(rootPath + "/WEB-INF/classes/shopxx.properties");
	boolean isWebXmlCanWrite = isCanWrite(rootPath + "/WEB-INF/web.xml");
	boolean isUploadCanWrite = isCanWrite(rootPath + "/upload/");
	
	String freeSpaceInfo = "-";
	long freeSpace;
	try {
		freeSpace = FileSystemUtils.freeSpaceKb(rootPath);
	} catch (Exception e) {
		freeSpace = 0;
	}
	if (freeSpace != 0) {
		if (freeSpace < 1024) {
			freeSpaceInfo = "<span class=\"red\">" + freeSpace + "KB</span>";
		} else if (freeSpace <= 1024 && freeSpace < 51200) {
			freeSpaceInfo = "<span class=\"red\">" + (freeSpace / 1024) + "MB</span>";
		} else if (freeSpace >= 51200 && freeSpace < 1048576) {
			freeSpaceInfo = "<span class=\"green\">" + (freeSpace / 1024) + "MB</span>";
		} else if (freeSpace >= 1024 * 1024) {
			freeSpaceInfo = "<span class=\"green\">" + (freeSpace / 1048576) + "GB</span>";
		}
	}
	
	String maxMemoryInfo = "-";
	double maxMemory;
	try {
		maxMemory = Runtime.getRuntime().maxMemory() / 1024 / 1024;
	} catch (Exception e) {
		maxMemory = 0;
	}
	if (maxMemory != 0) {
		if (maxMemory > 128) {
			maxMemoryInfo = "<span class=\"green\">" + maxMemory + "MB</span>";
		} else {
			maxMemoryInfo = "<span class=\"red\">" + maxMemory + "MB</span>";
		}
	}
	
	String isShopxxXmlCanWriteInfo = null;
	if (isShopxxXmlCanWrite) {
		isShopxxXmlCanWriteInfo = "<span class=\"green\">√</span>";
	} else {
		isShopxxXmlCanWriteInfo = "<span class=\"red\">×</span>";
	}
	String isShopxxPropertiesInfo = null;
	if (isShopxxPropertiesWrite) {
		isShopxxPropertiesInfo = "<span class=\"green\">√</span>";
	} else {
		isShopxxPropertiesInfo = "<span class=\"red\">×</span>";
	}
	String isWebXmlCanWriteInfo = null;
	if (isWebXmlCanWrite) {
		isWebXmlCanWriteInfo = "<span class=\"green\">√</span>";
	} else {
		isWebXmlCanWriteInfo = "<span class=\"red\">×</span>";
	}
	String isUploadCanWriteInfo = null;
	if (isUploadCanWrite) {
		isUploadCanWriteInfo = "<span class=\"green\">√</span>";
	} else {
		isUploadCanWriteInfo = "<span class=\"red\">×</span>";
	}
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8" />
<title>检查安装环境 - Powered By SHOP++</title>
<meta name="Author" content="SHOP++ Team" />
<meta name="Copyright" content="SHOP++" />
<link href="css/install.css" rel="stylesheet" type="text/css" />
<script type="text/javascript" src="js/jquery.js"></script>
<script type="text/javascript">
$().ready( function() {
	
	var $encodingInfo = $("#encodingInfo");
	var $previous = $("#previous");
	var $next = $("#next");
	var hasWarn = false;
	
	$.ajax({
		type: "GET",
		cache: false,
		url: "check_encoding.jsp",
		data: {character: "一"},
		dataType: "json",
		beforeSend: function(data) {
			$encodingInfo.text("-");
		},
		success: function(data) {
			if (data.isUTF8) {
				$encodingInfo.html('<span class="green">√<\/span>');
			} else {
				$encodingInfo.html('<span class="red">×<\/span>');
				hasWarn = true;
			}
		}
	});
	
	<%if ((freeSpace != 0 && freeSpace < 51200) || (maxMemory != 0 && maxMemory < 128) || !isShopxxXmlCanWrite || !isShopxxPropertiesWrite || !isWebXmlCanWrite || !isUploadCanWrite) { %>
		hasWarn = true;
	<% } %>
	
	$previous.click( function() {
		window.location.href = "index.jsp";
	});
	
	$next.click( function() {
		if (hasWarn && confirm("您的安装环境有误,可能导致系统安装或运行错误,您确定继续吗?") == false) {
			return false;
		} else {
			window.location.href = "setting.jsp";
		}
	});

});
</script>
</head>
<body>
	<div class="header">
		<div class="title">SHOP++ 安装程序 - 环境检测</div>
		<div class="banner"></div>
	</div>
	<div class="body">
		<div class="bodyLeft">
			<ul class="step">
				<li>许可协议</li>
				<li class="current">环境检测</li>
				<li>系统配置</li>
				<li>系统安装</li>
				<li>完成安装</li>
			</ul>
		</div>
		<div class="bodyRight">
			<table>
				<tr>
					<th>&nbsp;</th>
					<th>基本环境</th>
					<th>推荐环境</th>
					<th>当前环境</th>
				</tr>
				<tr>
					<td>
						<strong>操作系统:</strong>
					</td>
					<td>
						Linux/Unix/Windows ...
					</td>
					<td>
						Linux/Unix/Windows
					</td>
					<td>
						<%=System.getProperty("os.name")%> (<%=System.getProperty("os.arch")%>)
					</td>
				</tr>
				<tr>
					<td>
						<strong>JDK版本:</strong>
					</td>
					<td>
						JDK 1.5 +
					</td>
					<td>
						JDK 1.6
					</td>
					<td>
						<%=System.getProperty("java.version")%>
					</td>
				</tr>
				<tr>
					<td>
						<strong>WEB服务器:</strong>
					</td>
					<td>
						Tomcat
					</td>
					<td>
						Tomcat 6.0
					</td>
					<td>
						<span title="<%=application.getServerInfo()%>">
							<%=StringUtils.substring(application.getServerInfo(), 0, 25)%>
						</span>
					</td>
				</tr>
				<tr>
					<td>
						<strong>数据库:</strong>
					</td>
					<td>
						MySQL/Oracle/SQL Server
					</td>
					<td>
						MySQL 5.0
					</td>
					<td>
						-
					</td>
				</tr>
				<tr>
					<td>
						<strong>磁盘空间:</strong>
					</td>
					<td>
						50MB +
					</td>
					<td>
						300MB +
					</td>
					<td>
						<%=freeSpaceInfo%>
					</td>
				</tr>
				<tr>
					<td>
						<strong>内存大小:</strong>
					</td>
					<td>
						128MB +
					</td>
					<td>
						256MB +
					</td>
					<td>
						<%=maxMemoryInfo%>
					</td>
				</tr>
				<tr>
					<td>
						<strong>字符集编码:</strong>
					</td>
					<td>
						UTF-8
					</td>
					<td>
						UTF-8
					</td>
					<td>
						<span id="encodingInfo"></span>
					</td>
				</tr>
			</table>
			<table>
				<tr>
					<th width="344">
						文件目录
					</th>
					<th width="170">
						所需状态
					</th>
					<th>
						当前状态
					</th>
				</tr>
				<tr>
					<td>
						/WEB-INF/classes/shopxx.xml
					</td>
					<td>
						可写
					</td>
					<td>
						<%=isShopxxXmlCanWriteInfo%>
					</td>
				</tr>
				<tr>
					<td>
						/WEB-INF/classes/shopxx.properties
					</td>
					<td>
						可写
					</td>
					<td>
						<%=isShopxxPropertiesInfo%>
					</td>
				</tr>
				<tr>
					<td>
						/WEB-INF/web.xml
					</td>
					<td>
						可写
					</td>
					<td>
						<%=isWebXmlCanWriteInfo%>
					</td>
				</tr>
				<tr>
					<td>
						/upload/
					</td>
					<td>
						可写
					</td>
					<td>
						<%=isUploadCanWriteInfo%>
					</td>
				</tr>
			</table>
		</div>
		<div class="buttonArea">
			<input type="button" id="previous" class="button" value="上 一 步" />&nbsp;&nbsp;&nbsp;&nbsp;
			<input type="button" id="next" class="button" value="下 一 步" />
		</div>
	</div>
	<div class="footer">
		<p class="copyright">Copyright © 2005-2013 shopxx.net All Rights Reserved.</p>
	</div>
</body>
</html>