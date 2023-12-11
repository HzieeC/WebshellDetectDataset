<%@ page language="java" contentType="text/html; charset=GB2312" pageEncoding="GB2312"%>   
<%@ taglib prefix="s" uri="/struts-tags" %>  
   

   
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">   
   
<html>   
   
    <head>   
   
       <meta http-equiv="Content-Type" content="text/html; charset=GB2312">   
    <link href="./css/style.css" rel="stylesheet" type="text/css">
       <title>用户列表</title>   
 <script language="JavaScript"> function doSearch(){ if(document.all.searchValue.value=="") { alert("请输入查询关键字!"); }else{ window.location.href="userAction!list?queryName="+document.all.searchName.value+"&&queryValue="+document.all.searchValue.value; } } </script>  
    </head>   
   
    <body>   <s:set name="name" value="#session['username']" />
    <s:if test="#name!='null'">
欢迎您<s:property value="#session.username"/>！<br/> 

</s:if>
       <select name="searchName">  <option value="username">用户名</option>  <option value="sex">性别</option>  <option value="hobby">爱好</option><option value="friends">朋友</option></select>   <input type="text" name="searchValue" value="" size="10"/>  <input type="button" value="查询" onClick="doSearch();"> <br/>
         <s:iterator value="pageBean.list">
          <s:property value="id"/>
            <s:property value="username"/>
             <s:property value="hobby"/>
             <s:property value="friends"/>
            <img src="<s:property value="picture"/>" width="100" height="100"/> 
            <a href="viewUser!view?id=<s:property value="id"/>">详细资料</a>
            <br/>
        </s:iterator>
        共<s:property value="pageBean.totalRows"/> 条记录
        共<s:property value="pageBean.totalPages"/> 页
        当前第<s:property value="pageBean.currentPage"/>页
        
        <s:if test="%{pageBean.currentPage == 1}">
            第一页 上一页
        </s:if>
        <s:else>
            <a href="userAction!list?page=1">第一页</a>
            <a href="userAction!list?page=<s:property value="%{pageBean.currentPage-1}"/>">上一页</a>
        </s:else>
        <s:if test="%{pageBean.currentPage != pageBean.totalPages}">
            <a href="userAction!list?page=<s:property value="%{pageBean.currentPage+1}"/>">下一页</a>
            <a href="userAction!list?page=<s:property value="pageBean.totalPages"/>">最后一页</a>
        </s:if>
        <s:else>
            下一页 最后一页
        </s:else>   
    </body>   
   
</html>   