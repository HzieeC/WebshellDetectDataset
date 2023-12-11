<%@ page language="java" import="java.util.*" contentType="text/html;charset=GB2312" pageEncoding="GB2312"%>
<%@ taglib prefix="s" uri="/struts-tags" %>   
<html>
    <head>
        <title>文件上传示例</title>
            <link href="../css/style.css" rel="stylesheet" type="text/css">
        <link href="<s:url value="/css/main.css"/>" rel="stylesheet"
            type="text/css" />

    </head>

    <body>

        <s:actionerror />
        <s:fielderror />
        <s:form action="upload" method="POST" enctype="multipart/form-data">
           <s:file name="upload" label="上传的文件" /><br>
           <s:textfield name="fileCaption" label="备注" /><br>
            <s:submit value="上   传"/>
        </s:form>
  </body>
</html>
