<%@ page language="java" import="java.util.*" contentType="text/html;charset=utf-8" pageEncoding="utf-8"%>
<%@ taglib prefix="s" uri="/struts-tags"%>
<html>
    <head>
        <title>上传成功</title>
        <link href="../css/style.css" rel="stylesheet" type="text/css">
        <link href="<s:url value="/css/main.css"/>" rel="stylesheet"
            type="text/css" />
    </head>

    <body>
        <table class="wwFormTable">
            <tr>

                <td colspan="2">
                    <h4>
                        上传成功
                    </h4>
                </td>
            </tr>

            <tr>
                <td width="72" class="tdLabel">
                    <label for="doUpload_upload" class="label">
                        内容类型:
                    </label>
                </td>
                <td width="195">
                    <s:property value="uploadContentType" />
                </td>
            </tr>

            <tr>
                <td class="tdLabel">
                    <label for="doUpload_upload" class="label">
                        图片:
                    </label>
                </td>
                <td>
         <input type=hidden name=pic value="<s:property value="uploadFileName" />">
         <img src="<s:property value="uploadFileName" />" width="40" height="40">
              <!--<s:property value="uploadFileName" />-->
                </td>
            </tr>


            <tr>
                <td class="tdLabel">
                    <label for="doUpload_upload" class="label">
                        临时文件:
                    </label>
                </td>
                <td>
                    <s:property value="upload" />
                </td>
            </tr>

            <tr>
                <td class="tdLabel">
                    <label for="doUpload_upload" class="label">
                        备注:
                    </label>
                </td>
                <td>
                    <s:property value="fileCaption" />
                </td>
            </tr>


        </table>
        <script>
//alert("上传图片成功")
var random = Math.random();
parent.form.pic.value=document.all.pic.value;
</script>

  </body>
</html>
