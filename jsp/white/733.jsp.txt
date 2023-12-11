<%@ page language="java" import="java.util.*" pageEncoding="GB2312"%>
<%@ taglib prefix="s" uri="/struts-tags" %>  

<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">   
    <title>产品列表</title>  
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
	<link href="./css/style.css" rel="stylesheet" type="text/css">
 </head>
  
  <body>
  <table cellspacing="10" cellpadding="0" border="0"><tr><td>
 <P align="left"><b>产品列表</b></P>
  <table width="100%" cellspacing="0" border="0" align="center">
  <tr>
      <%int m=0;%>
           <s:iterator value="pageBean.list2"> 
            <%m=m+1;%>  
  <td>
    <s:property value="bigcategory"/>>><s:property value="smallcategory"/>    
            <br>  
             <s:set name="pic" value="pic" />
            <s:if test="#pic==''">           
 <img src="./images/nopic_small.jpg" width="150" height="150" border="0"/>
        </s:if>
         <s:else>           
<a href="<s:property value="pic"/>"><img src="<s:property value="pic"/>" width="150" height="150" border="0"/></a></s:else><br>       
          <font size="4">&yen;</font><font color="red" size="3"><b><s:property value="price"/></b></font>元  <br>
                      <a href="showProduct!get?id=<s:property value="id"/>"> <s:property value="productname"/></a><br>
            <a href="addCart!add?id=<s:property value="id"/>"><img src="./images/addcart.gif" border="0"></a></td>
         <%if ((m%5)==0){
%>
              </tr>
              <%}%>
   </s:iterator>       
      </table> <p  align="right">
               共<s:property value="pageBean.totalRows"/> 条记录
        共<s:property value="pageBean.totalPages"/> 页
        当前第<s:property value="pageBean.currentPage"/>页
        
        <s:if test="%{pageBean.currentPage == 1}">
            第一页 上一页        </s:if>
        <s:else>
            <a href="productsAction2!list?page=1">第一页</a>
            <a href="productsAction2!list?page=<s:property value="%{pageBean.currentPage-1}"/>">上一页</a>        </s:else>
        <s:if test="%{pageBean.currentPage != pageBean.totalPages}">
            <a href="productsAction2!list?page=<s:property value="%{pageBean.currentPage+1}"/>">下一页</a>
            <a href="productsAction2!list?page=<s:property value="pageBean.totalPages"/>">最后一页</a>        </s:if>
        <s:else>
        下一页 最后一页        </s:else>  </p>
     </td></tr></table> 
  </body>
</html>
